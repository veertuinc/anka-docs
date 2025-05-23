

Once a VM has been tagged, it becomes a "VM Template". The VM template & tag's state cannot be permanently modified unless you create a new tag, post-changes. This is very reminiscent of how `git commit` works. You can execute commands and modify the state of the VM after tagging it, but it will not save the changes until you create a new tag. This is important to consider when using the [Anka Build Cloud Registry]({{< relref "anka-build-cloud/_index.md" >}}) since it will only push the state of the VM when the tag was created, not after.

In summary, when cloning a tagged VM you have two options:

1. Shallow Clone from the current VM state, regardless of the state when it was tagged (`anka clone {source} {clone}`).
2. Shallow Clone the state of a VM template & tag by targeting the tag by name (`anka clone --tag {tagName} {source} {clone}`), regardless of what has been done to it since tagging.

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

Now, there is another registry structure/approach you can use to fix a team to a specific UUID/Template in the registry and avoid having to change anything in the team's workflow/CI job files. Otherwise you'll have to change the UUID in your team's workflow/CI job files to the new one each time there is a new Template/Tag created with dependencies. Not a huge problem if you use version control, but we want to at least recommend it as an approach.

1. Create empty VMs for each team: `anka create TeamA`
1. Push those empty VMs to the registry with `anka push --tag empty TeamA`. Note the `empty` tag. This creates a single tag Template as placeholder in the registry. As long as you don't delete the tag in the registry, the Template/UUID will always remain.
1. Now that an empty Template/Tag exist in the registry, you can use `anka push --remote-vm TeamA {new-template-name} -t dependencies-installed` to push the new template to the registry **onto the existing Template and therefore changing to match it's UUID**.

Now you can use `revert` APIs or CLI commands to delete the last `--remote-vm` pushed tag from the TeamA Template, going back to the `empty` tag. After reverting, you can push a brand new tag with updated dependencies at any time without needing to change the UUID in your team's workflow/CI job files. Be careful that you don't revert the `empty` tag or you'll lose the Template. It also allows you to benefit from shared layers as long as you have still, at some point, cloned everything from the same base Template.

{{< hint warning >}}
**ARM USERS:** Suspending will currently stop the VM. It will show as `suspended`, regardless.
{{< /hint >}}
