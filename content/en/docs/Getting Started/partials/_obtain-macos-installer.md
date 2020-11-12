There are multiple ways to obtain the installer .app file for Mac OSX that we'll detail for you below:

1. If you have a pending upgrade to the next minor or patch version of Mac OS:
    - Within `Preferences -> Software Update -> Advanced`, make sure `Download new updates when available` is checked but `Install macOS updates` is not. While you're still within `Software Update`, click `Update Now` but do not install the next version (Restart) until after you've created the Anka VM or the Install .app under /Applications will be deleted.
    - You can also use the App Store to download the installer.
2. On Catalina, you can use `softwareupdate`: `sudo softwareupdate --fetch-full-installer --full-installer-version 10.15.6`
3. Have your local IT department provide a network volume or download links.
4. Use our [Getting Started script](https://github.com/veertuinc/getting-started#create-vm-templatebash), but run it with `./create-vm-template.bash --no-anka-create`
3. Have your local IT department provide a network volume or download links.
