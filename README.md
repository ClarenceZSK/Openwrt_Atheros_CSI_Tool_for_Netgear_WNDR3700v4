# OpenWRT_Atheros_CSI_Tool_for_Netgear_WNDR3700v4
We modified the openWRT source code that embeds Atheros CSI tool to let it work for Netgear WNDR3700v4, which is not supported by [the release](https://github.com/xieyaxiongfly/Atheros_CSI_tool_OpenWRT_src) of Dr. Yaxiong Xie.

If you are interested in Atheros CSI tool, please refer to [the maintenance page](http://pdcc.ntu.edu.sg/wands/Atheros/) for more information.

## Install OpenWRT
### Install from binary firmware



%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%
%%% Below is the README from official OpenWRT github
%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%

This is the buildsystem for the OpenWrt Linux distribution.

Please use "make menuconfig" to choose your preferred
configuration for the toolchain and firmware.

You need to have installed gcc, binutils, bzip2, flex, python, perl, make,
find, grep, diff, unzip, gawk, getopt, subversion, libz-dev and libc headers.

Run "./scripts/feeds update -a" to get all the latest package definitions
defined in feeds.conf / feeds.conf.default respectively
and "./scripts/feeds install -a" to install symlinks of all of them into
package/feeds/.

Use "make menuconfig" to configure your image.

Simply running "make" will build your firmware.
It will download all sources, build the cross-compile toolchain,
the kernel and all choosen applications.

To build your own firmware you need to have access to a Linux, BSD or MacOSX system
(case-sensitive filesystem required). Cygwin will not be supported because of
the lack of case sensitiveness in the file system.
