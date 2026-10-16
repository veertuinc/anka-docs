You can prepare a build VM on one producer host, or on each runner with an Ankafile.

{{< hint info >}}
`anka create` and `from darwin:` use the stored `.base_<ProductBuild>` tag from above. The first create on a host downloads the IPSW. The next create for that ProductBuild on the same host clones that tag. A runner that only pulls the finished template receives macOS inside that template.
{{< /hint >}}
