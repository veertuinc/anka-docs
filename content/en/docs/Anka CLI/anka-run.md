

---
date: 2019-12-12T00:00:00-00:00
title: "Anka RUN"
linkTitle: "Anka RUN"
weight: 3
description: >
  `anka run` command provides a container like interface to work with the Anka VMs directly from the host where they are running.  
---

## Execute operation inside Vm from the host with RUN
Use `anka run` command, similar to `docker run` to execute operation inside the Vm from the host where it's running. Returns 125 on timeout.
```
anka run --help
Usage: anka run [OPTIONS] VM_NAME COMMAND [ARGS]...

  Run commands inside VM environment

Options:
  -w, --workdir PATH              Working directory inside the VM
  -v, --volumes-from, --volume PATH
                                  Mount host directory (current directory by default) into VM . '--volumes-from' is
                                  deprecated form
  -n, --no-volumes-from, --no-volume
                                  Use this flag to prevent implicit mounting of current folder (see --volume
                                  option). '--no-volumes-from' is deprecated form
  -E, -e, --env                   Inherit environment variables. '-e' is deprecated form
  -f, --env-file PATH             Provide environment variables from file
  -N, --wait-network              Wait till guest network interface up
  -T, --wait-time                 Wait for guest time sync
  --help                          Show this message and exit.
```


### Examples of anka run usage

`anka run sierrav40c1 xcodebuild -sdk iphonesimulator -scheme Kickstarter-iOS build` will mount the default directory from the host into the sierrav40c1 Vm and execute build.

`anka run -w /Applications VMNAME ls -l` will pipe the results of `ls -l` from the VM's `/Applications` directory.

`anka run -v . VMNAME ls` will mount the host current directory inside the VM, execute `ls`, pipe the results and unmount.

`anka run -v . VMNAME xcodebuild ...` will mount the current directory from the developer machine(host) to the VM and execute an `xcodebuild` command and pipe the results back.

`anka run sudo ...` executes commands inside the VM with `sudo` privileges. For instance:

`anka run VMNAME cp -R simpledir /Users/anka` will copy the current host directory to the VM at /Users/anka location

`anka run -n sierrav40c1 xcodebuild -sdk iphonesimulator -scheme Kickstarter-iOS build` will not mount the current host directory and execute build in the VM current directory

```
$ anka run VMNAME sudo whoami
root
```
`anka run`, when run in default mode with no flags, mounts the current folder from the host into the VM and executes commands on this mount point as current directory.

It's also possible to explicitly specify mount directory inside VM, using this syntax -v /host/folder:/mnt/path, e.g:

```
anka run -v $PWD:/tmp/mountpoint_1 VMNAME pwd

```

Currently only single volume could be specified in the run command. If you need more than one folder shared between host and the VM, use `anka mount/unmount` commands.

```
anka mount VMNAME ~/Library/MobileDevice/Provisioning\ Profiles/ /Users/anka/Library/MobileDevice/Provisioning\ Profiles/
anka run VMNAME xcodebuild -exportOptionsPlist exportInfo.plist archive
```
***Note***VM should be running for mount command to work.

If you need more configuration changes to the VM, before anka run execution, you could start the VM with corresponding parameters, or write the parameters to VM configuration with `anka modify` command.

### Working with environment variables using anka run

anka run command doesn't use .profile, .bash_profile by itself. To configure the environment variables, you can pass it with `anka run -f environment.txt`, where environment.txt is a text file in the form VARIABLE=VALUE, one variable per line.

To inherit entire host's environment, use `anka run -E` command, but in this case existing guests variables will not be overridden by host's variables.

In order to use environment variables from VMs `.bash_profile`, execute the following command via intermediate bash.

`anka run ios-temp bash -c 'source ~/.bash_profile; ./iPhone/make_build -b dev -c 1 -d ./build -e Mobile -z'`


### Few things to note

`anka run` doesn't support TTY mode, but you could easily use POSIX streams as with regular bash tool.

`anka run -n VNMANE whoami > /dev/null`  

`cat file.txt | anka run -n VMNAME md5`




