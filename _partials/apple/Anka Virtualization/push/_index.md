```shell
> anka push --help
usage: push [options] vmid

   Push a VM to the registry

arguments:
  vmid                     VM to push

options:
  -t,--tag <val>           Set the tag name to push
  -v,--remote-vm <val>     Registry template to push the tag onto
  -d,--description <val>   Set textual description of the tag
  -f,--force               Forcefully push, regardless of a tag already existing
  -l,--local               Commit the template without pushing it to the Registry
  -s,--shallow             Include all the changes of an older tags
  -q,--quiet               Do not show progress
  -r,--registry <val>      Sets an alternate registry
```
