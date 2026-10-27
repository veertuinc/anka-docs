This is the default in Anka 3.10.x. The push includes the base tag. Without `--thin`, the result is a full tag. A push to the Anka Registry always works this way.

{{< hint info >}}
Remember, both an OCI registry and the Anka Registry try to optimize disk usage. They reuse the disk images that the registry already has.
{{< /hint >}}

Example: `27.0.1-arm64:vanilla` is already on a host. You `anka pull` cmake onto that VM, then push the result as `27.0.1-arm64:cmake`.

On an Anka Registry or an OCI registry, `cmake` is a new tag whose base is the `vanilla` tag. The push reuses the `vanilla` tag the registry already has, so that tag stays unchanged. The registry stores the `cmake` tag on top of it.
