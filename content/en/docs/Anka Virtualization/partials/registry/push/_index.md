```shell
> sudo anka registry push --help
Usage: anka registry push [OPTIONS] VM_ID [TAGNAME]...

  Push a VM template (and tag) to the Cloud Registry

Options:
  -t, --tag TEXT          Set the tag name to push
  -d, --description TEXT  Set the description of the tag
  -v, --remote-vm TEXT    The name of a registry template you want to push the local template (and tag) onto
  -l, --local             Assign a tag to your local template and avoid pushing to the Registry
  --help                  Display usage information
```
