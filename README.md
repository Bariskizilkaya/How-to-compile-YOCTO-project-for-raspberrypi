# How-to-compile-YOCTO-project-for-raspberrypi

Install dependencies
```
sudo apt-get install gawk wget git-core diffstat unzip texinfo gcc-multilib build-essential chrpath socat cpio python python3 python3-pip python3-pexpect xz-utils debianutils iputils-ping libsdl1.2-dev xterm
```
Create folder for yocto
```
$ mkdir yocto
$ cd yocto
$ mkdir sources
$ cd sources
```
Pull yocto project
```
git clone git://git.yoctoproject.org/poky -b dunfell
```
Download Raspberry-pi meta data
```
git clone git://git.yoctoproject.org/meta-raspberrypi -b dunfell
```
Download mate data for open embedded
```
git clone https://git.openembedded.org/meta-openembedded -b dunfell
```
Run the oe-init-build-env
```
cd yocto
cd poky
source ./oe-init-build-env
```
Add the raspberrypi to local.conf
```
nano ./poky/build/conf/local.conf
```
find the MACHINE line and change to the raspberrypi
```
MACHINE ??= “raspberrypi”
```
change bblayers.conf mine is looking like this

under the ./poky/build/conf/bblayers.conf directory
```
# POKY_BBLAYERS_CONF_VERSION is increased each time build/conf/bblayers.conf
# changes incompatibly
POKY_BBLAYERS_CONF_VERSION = “2”

BBPATH = “${TOPDIR}”
BBFILES ?= “”

BBLAYERS ?= ” \
/home/pinhan/yocto2/poky/meta \
/home/pinhan/yocto2/poky/meta-poky \
/home/pinhan/yocto2/poky/meta-yocto-bsp \
/home/pinhan/yocto2/meta-raspberrypi \
/home/pinhan/yocto2/meta-openembedded/meta-oe \
/home/pinhan/yocto2/meta-openembedded/meta-multimedia \
/home/pinhan/yocto2/meta-openembedded/meta-networking \
/home/pinhan/yocto2/meta-openembedded/meta-python \
“
```
start compilation
```
bitbake core-image-minimal
```
after the successful

![image](https://github.com/user-attachments/assets/ae6d09d6-014d-4e0f-aac7-661fc7de2f56)


bzip2 -d -f tmp/deploy/images/raspberrypi/core-image-sato-raspberrypi.wic.bz2

You can burn the .wic file like this
```
sudo dd bs=4M if=core-image-minimal-raspberrypi.wic of=/dev/sdd conv=fsync
```
You can find the device
```
sudo fdisk -l
```
This is mine

![image](https://github.com/user-attachments/assets/a85ccdba-6aac-4e3c-bf50-6c09fded2528)


if you want to add apt commant to the image open the local.conf and add there lines
```
CORE_IMAGE_EXTRA_INSTALL += "apt"
EXTRA_IMAGE_FEATURES ?= “package-management”
```
and then
```
bitbake rpi-basic-image
```
