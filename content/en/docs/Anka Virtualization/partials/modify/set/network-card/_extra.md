> `--vlan` is useful if using a VLAN and bridged mode

> [INDEX] is always 0 since Anka doesn’t support more than 1 NIC per VM.

> [bridged] For Bonjour to work between host and guest, you'll need to run `sudo defaults write /Library/Preferences/com.apple.mDNSResponder.plist NoMulticastAdvertisements -bool NO`