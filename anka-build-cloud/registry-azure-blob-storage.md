---
date: 2019-12-12T00:00:00-00:00
title: "Registry Azure Blog Storage"
description: >
  Configuring your Registry to use Azure Blog Storage
toc_hide: true
---

By default, the Anka Build Cloud Registry uses normal files and directories to store VM Templates. As an alternative, especially when you need to make your Registry Highly Available, you can use Azure Blob Storage. It can be enabled using a feature flag described below.

## Prerequisites

- Data Lake Storage Gen2 NFS v3 enabled storage


## Usage with Docker

Inside of your docker-compose, include:

```yaml
    env_file:
       - azure.env
```

Then, inside of `azure.env`, put:

```bash
# Azure blob storage settings
ANKA_USE_BACKEND_PLUGIN=true

# Azure Data lake storage files and logs settings
ANKA_LOG_SERVER_BACKEND_TYPE=azure
# supports sharedKey or file
AZURE_AUTH_TYPE=sharedKey
# in case using a file (from az cli)
# AZURE_AUTH_LOCATION=
# in case using sharedKey
AZURE_KEY=
# required regardless of AUTH_TYPE
AZURE_ACCOUNT=

# Registry Azure Data Lake Storage gen2 configuration options

#REGISTRY_BLOB_IMAGES_DIR=images                            # Folder for storing images
#REGISTRY_BLOB_STATEFILES=statefiles                        # Folder for storing state files
#REGISTRY_BLOB_VMS=vms                                      # Folder for storing VM files 
#REGISTRY_BLOB_FS=regvmdata                                 # Container name for storing all data
#REGISTRY_BLOB_TIMEOUT=90                                   # Timeout for communicating with ADLS in seconds

#REGISTRY_BLOB_PARALLEL_DOWNLOADS=4                         # Number of goroutines to download in parallel from ADLS (bytes)
#REGISTRY_BLOB_DOWNLOAD_BUFFER_SIZE=16777216                # Buffer size for downloading from ADLS (bytes)
#REGISTRY_BLOB_DOWNLOAD_BUFFER_CHUNK_SIZE=4194304           # The amount every routine downloads (bytes)
#REGISTRY_BLOB_UPLOAD_WORKERS=1                             # Number of workers to upload small files
#REGISTRY_BLOB_BIG_FILE_UPLOAD_WORKERS=16                   # Number of workers to upload big files 
#REGISTRY_BLOB_UPLOAD_CHUNK_SIZE=8388608                    # Chunk size for uploading to ADLS
#REGISTRY_BLOB_UPLOAD_BUFFER_SIZE=33554432                  # Buffer size for upload to ADLS
#REGISTRY_BLOB_UPLOAD_FLUSH_THRESHOLD=104857600             # The amount of bytes written to flush

```
