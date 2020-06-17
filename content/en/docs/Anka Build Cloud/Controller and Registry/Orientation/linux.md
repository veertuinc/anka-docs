

---
title: "Linux"
linkTitle: "Linux"
weight: 1
description: >
  Where to find installed files and directories on Linux.
---


## Anka Controller Docker container
### Default Ports
80

### Binaries and scripts
`/usr/bin/anka-controller`

### Configuration files
Configuration is done through docker-compose file or through environment variables.

### Logs
Logs will be written to:  
`/var/log/anka-controller`  
It's also possible to get the logs through `docker logs` command.   
Assuming your container is called `anka-controller-1`:  
```shell
docker logs --follow anka-controller-1
```

### Data
No data is saved on the container.

## Anka Registry Docker container
### Default Ports
8089
### Binaries and scripts
`/usr/bin/anka-registry`

### Configuration files
Configuration is done through docker-compose file or through environment variables.

### Logs
Logs will be written to:  
`/var/log/anka-registry`  
It's also possible to get the logs through `docker logs` command.  
Assuming your container is called `anka-registry-1`:  
```shell
docker logs --follow anka-registry-1
```

### Data
Registry data will be written to:
`/mnt/vol`


## Anka Controller Deb package
### Default Ports
80
### Binaries and scripts
Application Binary:
`/usr/bin/anka-controller`
Start/Stop daemon:  
`/etc/init.d/anka-controller`
### Configuration files
`/etc/anka-controller/controller.conf`

### Logs
`/var/log/anka-controller`

### Data
No data is saved.

## Anka Registry Deb package
### Default Ports
80
### Binaries and scripts
Application Binary:  
`/usr/bin/anka-registry`  
Start/Stop daemon:  
`/etc/init.d/anka-registry`  

### Configuration files
`/etc/anka-registry/reg.conf`

### Logs
`/var/log/anka-registry`

### Data
Data will be written to:  
`/var/anka`







