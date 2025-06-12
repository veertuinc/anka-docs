

Once a VM has been tagged, it becomes a "VM Template". The Tag can be thought of as a commit in a git repository. The commit will always be there, even if you were to later on remove the code you added to the repo/codebase in that specific commit. The tag will always be a part of that Template going forward, even if you remove the data later on from the VM. This is important to consider when using the [Anka Build Cloud Registry]({{< relref "anka-build-cloud/_index.md" >}}).

{{< hint info >}}
Clones are not automatically tagged.
{{< /hint >}}

```bash
❯ anka list | grep test
| test (v1)                                 | ff06aa5b-0825-4f86-b5d0-c1cdb39fcedf | Jan 25 13:15:10 2022 | stopped |

❯ anka clone --tag v1 test test3                  
8a4e0033-29b4-4c29-8a0c-51fa53093d1c

❯ anka list | grep test         
| test3                                     | 8a4e0033-29b4-4c29-8a0c-51fa53093d1c | Feb 3 12:01:34 2022  | stopped |
| test (v1)                                 | ff06aa5b-0825-4f86-b5d0-c1cdb39fcedf | Jan 25 13:15:10 2022 | stopped |
```

In the above example, `test3` is a shallow clone of `test` and has a new UUID. The underlying layers are shared with `test` and will be used when `test3` is started.

#### Our Recommendation for Templates

If you're managing multiple templates for multiple teams and projects, you want to share as many underlying layers in the hierarchy of VM Templates as possible. To do this, we typically recommend:

1. Create a VM with macOS `13.0.1` and also name it that. The, push or locally tag it as `vanilla`.
2. Start the `13.0.1` VM and add the dependencies that everyone would need (typically git and brew). Stop the VM and [modify to add port forwarding]({{< relref "anka-virtualization-cli/command-line-reference.md#modify-nameuuid-port" >}}). Push/locally tag this as `brew+git+portforwarding22`.
3. Clone from `13.0.1` (with the `brew+git+portforwarding22` tag) and create `13.0.1-xcode14.1`. Start it and install Xcode. Stop it and push it with any tag name. I use `v1` usually.
4. Clone from `13.0.1-xcode14.1` and create `13.0.1-xcode14.1-{projectNameHere}-v1` which you'll install all of that projects dependencies in and push with any tag name.

This allows everything to share the underlying layers that are the same (since they're all cloned from `13.0.1`) and optimize disk space. You can then just pull the last VM template in the hierarchy and create a new one when needed, telling the teams to point to the new one when ready.

If it helps, here is it visually:

```
13.0.1 (stopped)  | 
                  | -> clone -> 13.0.1-xcode14.1 (stopped) |
                  |                                        | -> clone -> 13.0.1-xcode14.1-project1-v1 (with fastlane-v1.X) (suspended)
                  |                                        | -> clone -> 13.0.1-xcode14.1-project2-v1 (with fastlane-v2.X) (suspended)
                  |                                        | -> clone -> 13.0.1-xcode14.1-project2-v2 (with fastlane-v2.X) (suspended)
                  |
                  | -> clone -> 13.0.1-xcode13.4.1 (stopped) -> clone -> 13.0.1-xcode13.4.1-project3-v1 (suspended)
```

{{< hint warning >}}
**ARM USERS:** Suspending will currently stop the VM. It will show as `suspended`, regardless.
{{< /hint >}}

It's important to know that when you tag a VM you cannot remove the data from the layers that make up the VM unless you do a full clone. When you go to clone a VM or create a new tag, it may be beneficial to come from the tag/layers before you last made changes so that you don't include the data/layers and disk usage with the new tag you're creating.
