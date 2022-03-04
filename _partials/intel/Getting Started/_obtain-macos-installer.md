There are multiple ways to obtain the installer .app file for Mac OSX that we'll detail for you below:

The recommended approach is using `softwareupdate` in your terminal:

```bash
❯ softwareupdate --list-full-installers
Finding available software
Software Update found the following full installers:
* Title: macOS Monterey, Version: 12.2.1, Size: 12155426708K, Build: 21D62
* Title: macOS Monterey, Version: 12.2, Size: 12156325205K, Build: 21D49
* Title: macOS Monterey, Version: 12.1, Size: 12157035487K, Build: 21C52
* Title: macOS Monterey, Version: 12.0.1, Size: 12128428704K, Build: 21A559
* Title: macOS Big Sur, Version: 11.6.4, Size: 12439328867K, Build: 20G417
* Title: macOS Big Sur, Version: 11.6.3, Size: 12435122667K, Build: 20G415
* Title: macOS Big Sur, Version: 11.6.2, Size: 12433351292K, Build: 20G314
* Title: macOS Big Sur, Version: 11.6.1, Size: 12428472512K, Build: 20G224
* Title: macOS Big Sur, Version: 11.5.2, Size: 12440916552K, Build: 20G95
* Title: macOS Catalina, Version: 10.15.7, Size: 8248985973K, Build: 19H15
* Title: macOS Catalina, Version: 10.15.7, Size: 8248854894K, Build: 19H2
* Title: macOS Catalina, Version: 10.15.6, Size: 8248781171K, Build: 19G2021

❯ softwareupdate --fetch-full-installer --full-installer-version 12.2.1 
Scanning for 12.2.1 installer
Install finished successfully

❯ ls /Applications | grep Monterey 
Install macOS Monterey.app
```

Other options are:

1. If you have a pending upgrade to the next minor or patch version of Mac OS:
    - Within `Preferences -> Software Update -> Advanced`, make sure `Download new updates when available` is checked but `Install macOS updates` is not. While you're still within `Software Update`, click `Update Now` but do not install the next version (Restart) until after you've created the Anka VM or the Install .app under /Applications will be deleted.
    - You can also use the App Store to download the installer.
2. Have your local IT department provide a network volume or download links.
3. Use our [Getting Started script](https://github.com/veertuinc/getting-started#create-vm-templatebash), but run it with `./create-vm-template.bash --no-anka-create`
4. [MIST - macOS Installer Super Tool](https://github.com/ninxsoft/Mist)
