```shell
> sudo anka modify 10.15.4 set custom-variable --help
Usage: anka modify set custom-variable [OPTIONS] KEY VALUE

  configure variables

Options:
  --help  Show this message and exit.
```

You can set the following custom variables:

* boot-args                   - NVRAM boot arguments
* hw.uuid                     - Hardware UUID
* hw.serial                   - Serial Number (system)
* hw.manufacturer             - SMBIOS parameter (Reserved)
* hw.product                  - SMBIOS parameter (Reserved)
* hw.family                   - SMBIOS parameter (Reserved)
* hw.board                    - SMBIOS parameter (Reserved)