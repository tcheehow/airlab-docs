# Clone Edison Base Inage

The instructions are based on [companion](https://github.com/ArduPilot/companion)/[Intel\_Edison](https://github.com/ArduPilot/companion/tree/master/Intel_Edison)/[Debian](https://github.com/ArduPilot/companion/tree/master/Intel_Edison/Debian)/**1\_create\_base\_image.txt**

```text
# on host:
TIMESTAMP=`date '+%Y%m%d%H%M%S'`
APSYNC_TOFLASH="apsync-edison-$TIMESTAMP"
cp -a edison-ToFlash $APSYNC_TOFLASH
pushd $APSYNC_TOFLASH
rm edison-image-edison.ext4 edison-image-edison.hddimg

# on host (e.g.)
HOST_IP=10.0.1.116

# on host:
nc -w 5 -l $HOST_IP 9876 | dd status=progress >edison-image-apsync-boot.hddimg
# on Edison:
time (sudo dd if=/dev/mmcblk0p7 | nc $HOST_IP 9876) # ~14s

# on host:
nc -w 5 -l $HOST_IP 9876 | dd status=progress >edison-image-apsync-root.ext4
# on Edison:
time (sudo dd if=/dev/mmcblk0p8 | nc $HOST_IP 9876) # ~6 minutes

# on host:
nc -w 5 -l $HOST_IP 9876 | dd status=progress >edison-image-apsync-home.ext4
# on Edison:
time (sudo dd if=/dev/mmcblk0p10 | nc $HOST_IP 9876) # ~5 minutes
```

