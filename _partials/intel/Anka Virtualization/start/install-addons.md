When running Big Sur, addons are not installed by default. These addons require manual installation steps.

In order to install addons, you need to start the Anka VM with the `-uv` options:

```bash
❯ anka start -uv 10.15.7
+-----------------------+--------------------------------------+
| uuid                  | c0847bc9-5d2d-4dbc-ba6a-240f7ff08032 |
+-----------------------+--------------------------------------+
| name                  | 10.15.6 (base:port-forward-22)       |
+-----------------------+--------------------------------------+
. . .

❯ anka run -n 10.15.7 bash -c "ls -alht /Volumes/"
total 0
drwxr-xr-x   4 root  wheel   128B Nov  9 14:16 .
lrwxr-xr-x   1 root  wheel     1B Nov  9 14:15 Macintosh HD -> /
drwxr-xr-x@  4 anka  staff   136B Nov  6 12:51 Anka Guest Addons 2.2.3(118.6755654)
drwxr-xr-x  22 root  admin   704B Sep 25 11:48 ..
```

You can now go into the VM and manually install addons using the installer package inside of the `Anka Guest Addons` mounted disk.