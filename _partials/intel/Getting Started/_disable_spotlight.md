```bash
❯ anka run 12.1.0 bash -c "set -exo pipefail; \
echo \"Disable indexing\" && \
sudo defaults write ~/.Spotlight-V100/VolumeConfiguration.plist Exclusions -array \"/Volumes\" && \
sudo defaults write ~/.Spotlight-V100/VolumeConfiguration.plist Exclusions -array \"/Network\" && \
sudo killall mds && \
sleep 60 && \
sudo mdutil -a -i off && \
sudo mdutil -a -i off && \
sudo rm -rf /.Spotlight-V100/*"
+ echo 'Disable indexing'
+ sudo defaults write /Users/anka/.Spotlight-V100/VolumeConfiguration.plist Exclusions -array /Volumes
Disable indexing
+ sudo defaults write /Users/anka/.Spotlight-V100/VolumeConfiguration.plist Exclusions -array /Network
+ sudo killall mds
+ sleep 60
+ sudo mdutil -a -i off
2022-01-10 05:39:03.164 mdutil[1083:6592] mdutil disabling Spotlight: / -> kMDConfigSearchLevelFSSearchOnly
2022-01-10 05:39:03.273 mdutil[1083:6592] mdutil disabling Spotlight: /System/Volumes/Data -> kMDConfigSearchLevelFSSearchOnly
/:
	Indexing disabled.
/System/Volumes/Data:
	Indexing disabled.
+ sudo mdutil -a -i off
2022-01-10 05:39:03.815 mdutil[1085:6610] mdutil disabling Spotlight: / -> kMDConfigSearchLevelFSSearchOnly
2022-01-10 05:39:03.856 mdutil[1085:6610] mdutil disabling Spotlight: /System/Volumes/Data -> kMDConfigSearchLevelFSSearchOnly
/:
	Indexing disabled.
/System/Volumes/Data:
	Indexing disabled.
+ sudo rm -rf '/.Spotlight-V100/*'
```

Then, ensure there is no heavy usage from mds:

```bash
❯ anka run 12.1.0 bash -c "ps aux | grep mds"
anka              1091   0.0  0.0 34123248    984   ??  S     5:39AM   0:00.00 bash -c ps aux | grep mds
root               865   0.0  1.3 34024748 110880   ??  Ss    5:38AM   0:18.58 /System/Library/Frameworks/CoreServices.framework/Frameworks/Metadata.framework/Versions/A/Support/mds_stores
root               863   0.0  0.4 33740376  34864   ??  Ss    5:38AM   0:12.59 /System/Library/Frameworks/CoreServices.framework/Frameworks/Metadata.framework/Support/mds
anka              1093   0.0  0.0 34122720    684   ??  S     5:39AM   0:00.00 grep mds
```
