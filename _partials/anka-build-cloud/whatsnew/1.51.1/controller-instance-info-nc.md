Starting in 1.51.1, you can now get Controller Instance specific information under `nc -U /var/run/anka` in the VM. This includes the instance ID, Template ID (vmid), and tag.

```
❯ anka run mgmtManaged-26.6.2-arm64-jre21-jenkins-Nathans-MacBook-Pro-2.local-1788790252859991000 nc -U /var/run/anka
instance_id: 0c33db41-8b38-4b89-6dc6-43af41b2f68d
vmid: c0847bc9-5d2d-4dbc-ba6a-240f7ff08032
uuid: 77110bab-1b71-481f-b0d8-5c4d5d3ba23b
tag: v1
version: 3.9.2
license: com.veertu.anka.entplus,h:255
name: mgmtManaged-26.6.2-arm64-jre21-jenkins-Nathans-MacBook-Pro-2.local-1788790252859991000
```
