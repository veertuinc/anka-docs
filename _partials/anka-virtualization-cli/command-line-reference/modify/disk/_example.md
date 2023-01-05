```bash
❯ anka show 12.6 disk size
128GiB

❯ anka modify 12.6 disk -s 200G

❯ anka run 12.6 bash -c "df -h"
Filesystem       Size   Used  Avail Capacity iused      ifree %iused  Mounted on
/dev/disk2s1s1  123Gi   14Gi  106Gi    12%  502068 1106561040    0%   /

❯ anka run 12.6 bash -c "diskutil apfs resizeContainer disk0s2 0"
Started APFS operation
Aligning grow delta to 77,309,411,328 bytes and targeting a new physical store size of 208,855,367,680 bytes
Determined the maximum size for the targeted physical store of this APFS Container to be 208,855,370,752 bytes
Resizing APFS Container designated by APFS Container Reference disk2
The specific APFS Physical Store being resized is disk0s2
Verifying storage system
Using live mode
Performing fsck_apfs -n -x -l /dev/disk0s2
Checking the container superblock
. . .
Verifying volume object map space
The volume /dev/rdisk2s6 appears to be OK
Verifying allocated space
The container /dev/disk0s2 appears to be OK
Storage system check exit code is 0
Growing APFS Physical Store disk0s2 from 131,545,956,352 to 208,855,367,680 bytes
Modifying partition map
Growing APFS data structures
Finished APFS operation

❯ anka run 12.6 bash -c "df -h"
Filesystem       Size   Used  Avail Capacity iused      ifree %iused  Mounted on
/dev/disk2s1s1  195Gi   14Gi  177Gi     8%  502068 1860793720    0%   /

```