> ***NOTE***  
> Perform the following steps on the machines you intend to be Nodes.
> "Nodes" are the host machines you want to run the Anka VMs. You can use any Apple hardware for this.

### Install the Anka CLI

#### Download the latest Anka PKG
```shell
curl -L -o Anka.pkg https://veertu.com/downloads/ankabuild-latest
```
- You can find the various Anka Build packages on the [Anka Build download page](https://veertu.com/download-anka-build/).

#### Install the Anka PKG
```shell
sudo installer -pkg Anka.pkg -tgt /
```

#### Verify the installation
```shell 
anka version
```
- The output should be similar to `Anka Build Basic version 2.1.2 (build 112)`.

#### Activate your Anka license
```
sudo anka license activate <key>
```