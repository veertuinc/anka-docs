

### General & Commonly used (standalone registry)
{{< include file="_partials/anka-build-cloud/configuration-reference/registry/general&commonlyused(standaloneregistry)/notice.md" >}}
| ENV | Type | Description | Default Value |
| --- | :---: | --- | :---: |
| ANKA_BASE_PATH | (string) | Set the registry data's base path | . |
| ANKA_IMAGE_DIR_PATH | (string) | Set the path to put images directory (relative to base) | images_dir |
| ANKA_LISTEN_ADDR | (string) | The address and port to listen on (format: "address:[port]"). |  |
| ANKA_STATE_FILE_DIR_PATH | (string) | Set the path to put the state files directory (relative to base) | state_file_dir |
| ANKA_VM_DIR_PATH | (string) | Set the path to put vm directory (relative to base) | vm_dir |
| ANKA_VM_LIST_CACHE_TTL | (duration) | Template information cache TTL | 30s |
