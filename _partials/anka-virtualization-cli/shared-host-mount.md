Starting in 3.9.0, we've added support for sharing host directories as inside of the VM.

### High level

- Supports multiple VMs mounting the same folder from host
- Supports `anka mount` + `anka unmount`
- Supports `anka modify {vm} mount` + `anka modify delete mount` so that you automatically mount the same location no matter what host is running the VM Template.
- Survives VM reboots

### Command line reference

{{< include file="_partials/anka-virtualization-cli/command-line-reference/mount/_index.md" >}}
{{< include file="_partials/anka-virtualization-cli/command-line-reference/unmount/_index.md" >}}
{{< include file="_partials/anka-virtualization-cli/command-line-reference/modify/mount/_index.md" >}}
{{< include file="_partials/anka-virtualization-cli/command-line-reference/modify/delete/mount/_index.md" >}}
{{< include file="_partials/anka-virtualization-cli/command-line-reference/show/mount/_index.md" >}}

### Examples

**IMPORTANT:** The folders you mount are all available under `/Volumes/My Shared Files` in the VM. This is not something we can change and is enforced by Apple.

```bash
❯ anka run test bash -c "ls -alht /Volumes/"
total 0
drwxr-xr-x   4 root  wheel   128B May  4 08:59 .
lrwxr-xr-x   1 root  wheel     1B May  4 08:59 Macintosh HD -> /
drwxr-xr-x  22 root  wheel   704B Apr  6 01:10 ..
drwxr-xr-x   1 anka  staff     0B Dec 31  1969 My Shared Files

❯ anka run test bash -c "ls -alht /Volumes/My\ Shared\ Files/"
total 0
drwxr-xr-x  4 root  wheel   128B May  4 08:59 ..
drwxr-xr-x  1 anka  staff     0B Dec 31  1969 .

❯ anka mount test ~/
/Volumes/My Shared Files/nathanpierce

❯ anka run test bash -c "ls -alht /Volumes/My\ Shared\ Files/"
total 16
drwxr-xr-x@ 245 anka  staff   7.7K May  4  2026 nathanpierce
drwxr-xr-x    4 root  wheel   128B May  4 08:59 ..
drwxr-xr-x    1 anka  staff     0B Dec 31  1969 .

❯ echo "hello" > ~/testfile1

❯ cat ~/testfile1
hello

❯ anka run test bash -c "cat /Volumes/My\ Shared\ Files/nathanpierce/testfile1"
hello
```

You can see what folders are mounted with:

```bash
❯ anka mount test
+------+---------------------+-------------------+---------------------------------------+
| fsid | host_path           | guest_folder_name | guest_path                            |
+------+---------------------+-------------------+---------------------------------------+
| 1    | /Users/nathanpierce | nathanpierce      | /Volumes/My Shared Files/nathanpierce |
+------+---------------------+-------------------+---------------------------------------+

❯ anka show test mount
+---------------------+---------------------------------------+
| host_path           | guest_path                            |
+---------------------+---------------------------------------+
| /Users/nathanpierce | /Volumes/My Shared Files/nathanpierce |
+---------------------+---------------------------------------+
```

You can disable the automounting of the volume that holds the folders in the VM globally in the host by changing `anka config mount_prestart_automount_device` to `0`:

```bash
❯ anka config | grep automount
| mount_prestart_automount_device | 1 
```