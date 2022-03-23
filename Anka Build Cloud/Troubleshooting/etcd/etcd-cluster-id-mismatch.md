---
title: "ETCD cluster won't start and fails with \"request sent was ignored by remote peer due to cluster ID mismatch\""
weight: 1
---

## Scenario

Starting your etcd cluster throws an error like:

```json
{"level":"warn","ts":"2022-03-09T01:06:57.767Z","caller":"rafthttp/stream.go:653","msg":"request sent was ignored by remote peer due to cluster ID mismatch","remote-peer-id":"99fbb86961c11d8f","remote-peer-cluster-id":"d0d0a4fb77ca1d5f","local-member-id":"5892b00848500dd8","local-member-cluster-id":"464487e7a78e118c","error":"cluster ID mismatch"}
```

## Common Causes

1. All etcd instances are not starting at the same or a close enough time frame. ETCD is highly sensitive to delays in the members that are used to start the cluster.

## Solution

Remove any readiness or aliveness probes or anything that could get in the way of the instances starting. Here is an example of a Kubernetes statefulset for an etcd cluster which is confirmed to work:

```yaml
---
# Source: etcd/templates/statefulset.yaml
apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: anka-etcd
  labels:
    app.kubernetes.io/name: etcd
    app.kubernetes.io/instance: anka-etcd
spec:
  selector:
    matchLabels:
      app.kubernetes.io/name: etcd
      app.kubernetes.io/instance: anka-etcd
  serviceName: anka-etcd-headless
  podManagementPolicy: Parallel
  replicas: 3
  updateStrategy:
    type: RollingUpdate
  template:
    metadata:
      labels:
        app.kubernetes.io/name: etcd
        app.kubernetes.io/instance: anka-etcd
      annotations:
        prometheus.io/path: '/metrics'
        prometheus.io/port: '2379'
        prometheus.io/scrape: 'true'
    spec:      
      affinity:
        podAntiAffinity:
          preferredDuringSchedulingIgnoredDuringExecution:
          - podAffinityTerm:
              labelSelector:
                matchExpressions:
                - key: app
                  operator: In
                  values:
                  - etcd
              topologyKey: topology.kubernetes.io/zone
            weight: 100
          - podAffinityTerm:
              labelSelector:
                matchExpressions:
                - key: app
                  operator: In
                  values:
                  - etcd
              topologyKey: kubernetes.io/hostname 
            weight: 99    
      securityContext:
        fsGroup: 1001
        runAsUser: 1001
      containers:
        # Variables to populate static cluster
        - name: anka-etcd
          image: docker.io/bitnami/etcd:3.5.2-debian-10-r31
          imagePullPolicy: "IfNotPresent"
          lifecycle:
            preStop:
              exec:
                command:
                  - /opt/bitnami/scripts/etcd/prestop.sh
          resources:
            limits:
              cpu: 256m
              memory: 1Gi
            requests:
              cpu: 256m
              memory: 1Gi
          env:
            - name: BITNAMI_DEBUG
              value: "false"
            - name: MY_POD_IP
              valueFrom:
                fieldRef:
                  fieldPath: status.podIP
            - name: MY_POD_NAME
              valueFrom:
                fieldRef:
                  fieldPath: metadata.name
            - name: MY_NAMESPACE
              valueFrom:
                fieldRef:
                  fieldPath: metadata.namespace
            - name: ETCDCTL_API
              value: "3"
            - name: ETCD_ON_K8S
              value: "yes"
            - name: ETCD_START_FROM_SNAPSHOT
              value: "no"
            - name: ETCD_DISASTER_RECOVERY
              value: "no"
            - name: ETCD_NAME
              value: "$(MY_POD_NAME)"
            - name: ETCD_DATA_DIR
              value: /bitnami/etcd/data
            - name: ETCD_LOG_LEVEL
              value: "info"
            - name: ETCD_AUTH_TOKEN
              value: "simple"
            - name: ETCD_ADVERTISE_CLIENT_URLS
              value: "http://$(MY_POD_NAME).anka-etcd-headless.anka-etcd.svc.cluster.local:2379,http://anka-etcd.anka-etcd.svc.cluster.local:2379"
            - name: ETCD_LISTEN_CLIENT_URLS
              value: "http://0.0.0.0:2379"
            - name: ETCD_INITIAL_ADVERTISE_PEER_URLS
              value: "http://$(MY_POD_NAME).anka-etcd-headless.anka-etcd.svc.cluster.local:2380"
            - name: ETCD_LISTEN_PEER_URLS
              value: "http://0.0.0.0:2380"
            - name: ETCD_INITIAL_CLUSTER_TOKEN
              value: "etcd-cluster-k8s"
            - name: ETCD_INITIAL_CLUSTER_STATE
              value: "new"
            - name: ETCD_INITIAL_CLUSTER
              value: "anka-etcd-0=http://anka-etcd-0.anka-etcd-headless.anka-etcd.svc.cluster.local:2380,anka-etcd-1=http://anka-etcd-1.anka-etcd-headless.anka-etcd.svc.cluster.local:2380,anka-etcd-2=http://anka-etcd-2.anka-etcd-headless.anka-etcd.svc.cluster.local:2380"
            - name: ETCD_CLUSTER_DOMAIN
              value: "anka-etcd-headless.anka-etcd.svc.cluster.local"
            - name: ALLOW_NONE_AUTHENTICATION
              value: "yes"
            - name: ETCD_AUTO_COMPACTION_MODE
              value: "periodic"
            - name: ETCD_AUTO_COMPACTION_RETENTION
              value: "1"
          ports:
            - name: client
              containerPort: 2379
              protocol: TCP
            - name: peer
              containerPort: 2380
              protocol: TCP
          volumeMounts:
            - name: data
              mountPath: /bitnami/etcd
  volumeClaimTemplates:
    - metadata:
        name: data
      spec:
        accessModes:
          - "ReadWriteOnce"
        resources:
          requests:
            storage: "4Gi"
```

## Still experiencing problems?

Talk to us! we are available via [slack](https://slack.veertu.com/) or [email](mailto:support@veertu.com)
