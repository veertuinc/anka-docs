You can find the installation packages on the [veertu.com website](https://veertu.com/getting-started-anka-trials/). Once downloaded, double click the .pkg to start the installation process.

![installer with pkg](/images/anka-virtualization/installer.png)

> **Nested Virtualization**
>
> If you need Nested Virtualization to run Docker inside of the VM, you can find it by clicking the `Customize` button on the **Installation Type** stage of the interactive installer or adding the following to the installer command in your terminal: `sudo installer -applyChoiceChangesXML nanka.xml -pkg AnkaVirtualization.pkg -target /`
>
> When you generate your VM Template, be sure to enable it with: `anka modify {vmNameOrUUID} set nested 1`
>
> Only Docker is supported at this time