


### Install Anka CLI

#### Download latest Anka PKG
Download the file from [Anka Build Download page](https://veertu.com/download-anka-build/).
```shell
curl -L -o Anka.pkg https://veertu.com/downloads/ankabuild-latest
```

#### Install Anka PKG
cd to the directory containing the file (if you are in another directory), then run:
```shell
sudo installer -pkg Anka.pkg -target
```

#### Verify installation
Verify the installation by running `anka version` command.
```shell 
anka version

Anka Build Basic version 2.1.2 (build 112)
```

#### License activation
Activate your license using the anka license command and your license key:
```
sudo anka license activate <key>
```

#### Download MacOS installer
##### Using the App Store
Use the App Store to download a MacOS install package of your choosing.  
Here's a link to the MacOS Mojave install page - <a href="https://itunes.apple.com/us/app/macos-mojave/id1398502828?ls=1&mt=12" target="_blank"> https://itunes.apple.com/us/app/macos-mojave/id1398502828?ls=1&mt=12 </a>.

After the download is complete the files will be at `/Applications/Install macOS Mojave.app` (different MacOS versions will be called differently). 

##### Using a script
You can use the *"installinstallmacos.py"* script from this <a href="https://github.com/munki/macadmin-scripts" target="_blank"> Github repository </a> (requires python installed).  

Download and run the script:  
```shell
curl --fail --silent -L -O https://raw.githubusercontent.com/munki/macadmin-scripts/master/installinstallmacos.py
sudo chmod +x installinstallmacos.py
sudo ./installinstallmacos.py --raw
```

The script downloads a disk image or a dmg, so further steps are necessary in order to create an Anka VM.
Create a directory for the app:  
```shell
mkdir -p /tmp/app
```
Attach the image using *hdiutil* (assuming `$IMAGE_PATH` is the path to the image you downloaded):    
```shell
hdiutil attach "$IMAGE_PATH" -mountpoint /tmp/app
```
Copy the files to the `Applications` folder
```shell
sudo cp -r "/tmp/app/Applications/Install macOS Mojave.app" /Applications/
```
Detach the image:
```shell
sudo hdiutil detach /tmp/app -force
```
Delete the downloaded Image:
```shell
rm -f "$IMAGE_PATH"
```


After downloading the MacOS application you can now create your vm!


### Create the VM
Use `anka create` command to create macOS VMs from the `.app` installer app.  

```shell
anka create --app /Applications/Install\ macOS\ Mojave.app/ mojave-base
```

The VM creation should take around 30 minutes or so.  

