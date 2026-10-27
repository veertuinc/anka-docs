The push stores the changes since the base tag. The result is a thin tag. The base tag stays out of the manifest, so you can apply the tool tag onto another VM. It works only with an OCI registry.

With `push_mode` `2`, that same push stores the file changes. The macOS base disk stays out of the manifest.
