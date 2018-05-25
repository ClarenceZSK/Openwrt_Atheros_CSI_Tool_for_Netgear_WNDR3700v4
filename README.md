# OpenWRT_Atheros_CSI_Tool_for_Netgear_WNDR3700v4
We modified the openWRT source code that embeds Atheros CSI tool to let it work for Netgear WNDR3700v4, which is not supported by [the release](https://github.com/xieyaxiongfly/Atheros_CSI_tool_OpenWRT_src) of Dr. Yaxiong Xie.

If you are interested in Atheros CSI tool, please refer to [the maintenance page](http://pdcc.ntu.edu.sg/wands/Atheros/) for more information.

## Install OpenWRT
### Install from binary firmware
You need the system firmware to install OpenWRT on your device. Please obtain the firmware for Netgear WNDR3700v4 from our [GitHub](). There are only two firmware in this Github folder:
* openwrt-ar71xx-nand-wndr3700v4-ubi-factory.img --- it is used when you want to upgrade from the origin system to OpenWRT for the first time.
* openwrt-ar71xx-nand-wndr3700v4-squashfs-sysupgrade.tar --- it is used when your device has already been flashed with OpenWRT and you want to update the system running on it.

The device runs the system provided by the vendors, please use the web browser. First of all, connect your PC with the router LAN port via ethernet cable. After buiding connection with the router, open your web browser and visit IP "192.168.1.1". Now, you are actually visitng the WebUI provided by the vendor. Find the "Firmware Upgrade" option and use it to upgrade the firmware (choose openwrt-ar71xx-nand-wndr3700v4-ubi-factory.img).

If your device has already been flashed with OpenWRT, you can use the WebUI provided by OpenWRT called [Luci](https://wiki.openwrt.org/doc/techref/luci) to upgrade the firmware (choose openwrt-ar71xx-nand-wndr3700v4-squashfs-sysupgrade.tar).

### Install from source code
This repo is the source code. You can compile our modified OpenWRT system. After the compilation, you will get the firmware for your device. Follow the steps described above, you can get your device capable of extracting CSI.

#### Prerequisite
Before stating to build, we need to install some necessary packages by:
```
$ sudo apt-get update
$ sudo apt-get install sudo apt-get install git python sed wget cvs subversion git-core coreutils unzip texi2html  
texinfo docbook-utils gawk python-pysqlite2 diffstat help2man make gcc g++ build-essential g++ desktop-file-utils     
chrpath flex libncurses5 libncurses5-dev zlib1g-dev pkg-config gettext libxml-simple-perl guile-1.8 cmake libssl-dev
xsltproc fastjar mercurial pngcrush imagemagick tcl binutils bzip2 perl grep diffutils openjdk-7-jdk zlib1g zlib1g-dbg
zlib-bin zlibc zlib-gst ccache distcc gcc-multilib g++-multilib bin86 libtool gawk
```
#### Download and update
To begin with, you must operate as a normal user: (NOT ROOT). First of all, use git to download the source code:
```
$ git clone https://github.com/ClarenceZSK/Openwrt_Atheros_CSI_Tool_for_Netgear_WNDR3700v4.git
```
Please note that in order to avoid the error like “cannot find the file” showing up, you’d better git the source code to /home/$(username) directory, and we will take this path as default for all later commands. Enter folder containing the source code:
```
$ cd ~/Openwrt_Atheros_CSI_Tool_for_Netgear_WNDR3700v4
```
Download and install all available "feeds":
```
$ ./scripts/feeds update -a
$ ./scripts/feeds install -a
```
NOTE If you got error "bash: ./scripts/feeds: Permission denied" when running above commands, add exectuable to all .sh file and feeds by:
```
$ find . -name "*.sh" -exec chmod +x {} \;
$ chmod +x -R scripts
```
#### Configure
Type the following command and enter the configuration mode:
```
$ make menuconfig
```
The GUI is as shown in following figure:
![](https://camo.githubusercontent.com/4a1486c1e78242af80f52fc95844acdf2c574cf4/687474703a2f2f706463632e6e74752e6564752e73672f77616e64732f41746865726f732f696d616765732f6d656e75636f6e6669672e706e67)
When you search the target system, you cannot find **wndr3700v4** in the menu "Target Profile (Default Profile (all drivers))". Instead, you will see target _wndr3700_. But it only supports _wndr3700v1_ and _wndr3700v2_. Plese note that you need to first select **Subtarget  (Generic)** and select **generic devices with NAND flash**. Then return to the menu shown in the above figure, enter the "Target Profile" again, you can find and select **wndr3700v4** target.

**Please** remember to compile the sendData and recvCSI by entering the CSI configuration as following figure shows:
![](https://camo.githubusercontent.com/40b92ef85a54a90ab39dc4f95bea51e6e1371e1c/687474703a2f2f706463632e6e74752e6564752e73672f77616e64732f41746865726f732f696d616765732f6d616b654353492e706e67)
Then select the recvCSI and sendData as built-in modules as following figure shows:
![](https://camo.githubusercontent.com/c4894e38fc55d8a4171967ab873aea91878ea501/687474703a2f2f706463632e6e74752e6564752e73672f77616e64732f41746865726f732f696d616765732f726563764353492e706e67)
**NOTE** If you want to use the injector mode, please select the lorcon as the built-in module. To select lorcon, please enter the Library section as following figure shows:
![](https://camo.githubusercontent.com/9310e8989f1b854606426952b7fd3918bd829481/687474703a2f2f706463632e6e74752e6564752e73672f77616e64732f41746865726f732f696d616765732f6c69625f6d616b652e706e67)
and then find and select loron:
![](https://camo.githubusercontent.com/a57347f460571f8acf1afab880e2cd9d1bdb6581/687474703a2f2f706463632e6e74752e6564752e73672f77616e64732f41746865726f732f696d616765732f6c6f72636f6e2e706e67)
**note** There is a conflict between two modules: _libustream-openssl_ and _libustream-mbedtls_. Please exclude one of them in the Library section.

#### Compile
Build by:
```
$ make
```
The first build takes few hours. On the multicore machine, you can use the make's –j option to speed up the building procedure. If you want to see what is going on during the building procedure, or you want to see an error detail, you can use the environment variable V:
```
$ make -j1 V=s
```
When the compilation is over, you will find the system images in the **~/Openwrt_Atheros_CSI_Tool_for_Netgear_WNDR3700v4/bin/targets/ar71xx/nand** directory.

The final step is to follow the aforementioned steps to flash our compiled firmware into your device. After the installation, you can check whether our CSI tool is available on your device by:
```
$ lsmod | grep 'ar9003_csi'
```
This kernel module “ar9003_csi” is crucial for CSI collection because it helps create CSI-detectable packets on the transmitter, and obtain CSI data on the receiver. If you get the message like this:
![](https://camo.githubusercontent.com/900b176fe93188e2dc77ae5e9a2b185d1e5cfe6f/687474703a2f2f706463632e6e74752e6564752e73672f77616e64732f41746865726f732f696d616765732f4353495f6d6f64756c652e706e67)
Now you have got the CSI availability of Netgear WNDR3700v4.

This is an extension of the Atheros CSI Tool in OpenWRT developed by Dr. Yaxiong Xie. Please refer to his [GitHub](https://github.com/xieyaxiongfly/Atheros_CSI_tool_OpenWRT_src) for more information.
