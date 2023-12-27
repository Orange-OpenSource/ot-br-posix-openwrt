# Package OTBR for OpenWRT

Repository to allow adding an openwrt feed for OTBR (https://github.com/openthread/ot-br-posix/tree/main/etc/openwrt/openthread-br)

This is based on the sha1 de7cd7b20

Based on the openwrt branch of the OTBR project (https://github.com/openthread/ot-br-posix/tree/openwrt)

# Build
## Prepare OpenWRT environment
```
git clone https://github.com/openwrt/openwrt.git
# enter the OpenWRT directory
cd openwrt
# update and install feeds
./scripts/feeds update -a
./scripts/feeds install -a
# select your preferred configuration for the toolchain, target system & firmware packages.
make menuconfig
# make the OpenWRT project
make
```

## Package and Install otbr package
```
Add ot-br-posix's feeds
echo "src-git openthread https://github.com/Orange-OpenSource/ot-br-posix-openwrt.git;main" >> feeds.conf
./scripts/feeds update openthread
./scripts/feeds install -a -p openthread
# in "Network" column, select "openthread" option
make menuconfig
# package the project
make package/openthread/compile
# the .ipk file will be in ./bin/packages/mips_24kc/otbrpackage/openthread_xxx.ipk
# copy the .ipk file into OpenWRT device, and install
opkg install openthread_xxx.ipk
# then can use openthread on OpenWRT
otbr-agent -d7 -v /dev/ttyUSB0 115200
```
