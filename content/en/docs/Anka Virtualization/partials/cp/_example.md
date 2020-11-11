```bash
❯ time anka cp ~/VirtualBox\ VMs/cloud-client-side-ha-1/cloud-client-side-ha-1.vdi 10.15.6:/Users/anka/
anka --debug cp  10.15.6:/Users/anka/  0.86s user 20.60s system 4% cpu 8:53.85 total
```
Compare the ~9 minutes it takes for `anka cp` to how much it takes to transfer the same file with `scp`:

```bash
❯ time scp -P 10000 -v ~/VirtualBox\ VMs/cloud-client-side-ha-1/cloud-client-side-ha-1.vdi anka@192.168.0.110:/Users/anka/
scp -P 10000 -v  anka@192.168.0.110:/Users/anka/  110.61s user 60.88s system 22% cpu 12:46.30 total
```