You can find the installation packages on the [veertu.com website](https://veertu.com/download-anka-develop/). Once downloaded, double click the .pkg to start the installation process.

![installer with pkg](/images/anka-virtualization/installer.png)

> **Nested Virtualization**
>
> If you need Nested Virtualization to run Docker inside of the VM, you can find it by clicking the `Customize` button on the **Installation Type** stage of the interactive installer.
>
> Or, **if you're on Catalina or lower with SIP disabled**, by adding the `applyChoiceChangesXML` to the installer command in your terminal: `sudo installer -applyChoiceChangesXML nanka.xml -pkg AnkaVirtualization.pkg -target /`.
>
> **nanka.xml**
>  ```xml
>  <?xml version="1.0" encoding="UTF-8"?>
>  <!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
>  <plist version="1.0">
>  <array>
>    <dict>
>      <key>attributeSetting</key>
>      <integer>1</integer>
>      <key>choiceAttribute</key>
>      <string>selected</string>
>      <key>choiceIdentifier</key>
>      <string>nested</string>
>    </dict>
>  </array>
>  </plist>
>  ```
>
> Finally, when you generate your base VM Template, be sure to enable it with: `anka modify {vmNameOrUUID} set nested 1`
>
> Internally, only Docker is supported at this time.