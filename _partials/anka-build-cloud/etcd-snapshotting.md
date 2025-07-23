

#### ETCD Snapshotting

It's always a good idea to take frequent snapshots of the etcd data. This helps if you have to recover from data loss or a crash of your Build Cloud host. In this guide, we'll not only show you how to take a snapshot, but also how to restore it to a different etcd.

The official ETCD snapshotting docs can be found at https://etcd.io/docs/v3.5/op-guide/recovery/#snapshotting-the-keyspace.

##### Taking a snapshot

```bash
docker exec -it anka-etcd /bin/bash -c "ETCDCTL_API=3 etcdctl snapshot save snapshot.db"
docker cp anka-etcd:/snapshot.db ./
```

Output should be something like:

```bash
{"level":"info","ts":1646315443.450814,"caller":"snapshot/v3_snapshot.go:68","msg":"created temporary db file","path":"snapshot.db.part"}
{"level":"info","ts":1646315443.4519353,"logger":"client","caller":"v3/maintenance.go:211","msg":"opened snapshot stream; downloading"}
{"level":"info","ts":1646315443.451973,"caller":"snapshot/v3_snapshot.go:76","msg":"fetching snapshot","endpoint":"127.0.0.1:2379"}
{"level":"info","ts":1646315443.4534197,"logger":"client","caller":"v3/maintenance.go:219","msg":"completed snapshot read; closing"}
{"level":"info","ts":1646315443.5064054,"caller":"snapshot/v3_snapshot.go:91","msg":"fetched snapshot","endpoint":"127.0.0.1:2379","size":"41 kB","took":"now"}
{"level":"info","ts":1646315443.5067463,"caller":"snapshot/v3_snapshot.go:100","msg":"saved","path":"snapshot.db"}
Snapshot saved at snapshot.db
```

##### Recovering from snapshot

Let's say that you've created a brand new server and unpacked the docker package's tar.gz.

The default data directory/volume the `docker-compose.yml` sets for etcd is `/var/etcd-data`. Before you start, you want to restore the snapshot from the server/host level into the data directory for etcd:

{{< hint info >}}
We include the etcdctl binary for you to use inside of the package.
{{< /hint >}}

```bash
cd ~/anka-controller-registry-1.22.0-5dc750f1/
./etcd/etcdctl snapshot restore --data-dir /var/etcd-data ./snapshot.db
```

Output should look something like:

```bash
2022-03-03T16:04:16+02:00	info	snapshot/v3_snapshot.go:251	restoring snapshot	{"path": "snapshot.db", "wal-dir": "etcd-data/member/wal", "data-dir": "etcd-data", "snap-dir": "etcd-data/member/snap", "stack": "go.etcd.io/etcd/etcdutl/v3/snapshot.(*v3Manager).Restore\n\t/tmp/etcd-release-3.5.1/etcd/release/etcd/etcdutl/snapshot/v3_snapshot.go:257\ngo.etcd.io/etcd/etcdutl/v3/etcdutl.SnapshotRestoreCommandFunc\n\t/tmp/etcd-release-3.5.1/etcd/release/etcd/etcdutl/etcdutl/snapshot_command.go:147\ngo.etcd.io/etcd/etcdctl/v3/ctlv3/command.snapshotRestoreCommandFunc\n\t/tmp/etcd-release-3.5.1/etcd/release/etcd/etcdctl/ctlv3/command/snapshot_command.go:128\ngithub.com/spf13/cobra.(*Command).execute\n\t/home/remote/sbatsche/.gvm/pkgsets/go1.16.3/global/pkg/mod/github.com/spf13/cobra@v1.1.3/command.go:856\ngithub.com/spf13/cobra.(*Command).ExecuteC\n\t/home/remote/sbatsche/.gvm/pkgsets/go1.16.3/global/pkg/mod/github.com/spf13/cobra@v1.1.3/command.go:960\ngithub.com/spf13/cobra.(*Command).Execute\n\t/home/remote/sbatsche/.gvm/pkgsets/go1.16.3/global/pkg/mod/github.com/spf13/cobra@v1.1.3/command.go:897\ngo.etcd.io/etcd/etcdctl/v3/ctlv3.Start\n\t/tmp/etcd-release-3.5.1/etcd/release/etcd/etcdctl/ctlv3/ctl.go:107\ngo.etcd.io/etcd/etcdctl/v3/ctlv3.MustStart\n\t/tmp/etcd-release-3.5.1/etcd/release/etcd/etcdctl/ctlv3/ctl.go:111\nmain.main\n\t/tmp/etcd-release-3.5.1/etcd/release/etcd/etcdctl/main.go:59\nruntime.main\n\t/home/remote/sbatsche/.gvm/gos/go1.16.3/src/runtime/proc.go:225"}
2022-03-03T16:04:16+02:00	info	membership/store.go:141	Trimming membership information from the backend...
2022-03-03T16:04:16+02:00	info	membership/cluster.go:421	added member	{"cluster-id": "cdf818194e3a8c32", "local-member-id": "0", "added-peer-id": "8e9e05c52164694d", "added-peer-peer-urls": ["http://localhost:2380"]}
2022-03-03T16:04:16+02:00	info	snapshot/v3_snapshot.go:272	restored snapshot	{"path": "snapshot.db", "wal-dir": "etcd-data/member/wal", "data-dir": "etcd-data", "snap-dir": "etcd-data/member/snap"}
```

Once restored, you can now start with `docker-compose up -d` and you will see a similar list of instances and nodes as the previous controller.

{{< hint info >}}
Be sure that the FQDN or IP you're using for the Anka Nodes to join now points to the new machine or else they will need to be disjoined and rejoined to the new server.
{{< /hint >}}
