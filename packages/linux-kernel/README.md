# About this fork

This branch is dedicated for me personally to build vyos image that add support for my hardware.
You're welcome to share me the insturction to add support of your hardware, if the instruction is clear, I'll include it here.

## Current list of hardware support by me

Below are the lsit of hardware that I tested, but the patch I apply here may add support for more that the list.

* 2023/03/24 Quectel RM500U-CN (2c7c:0900) # 5G module (I don't recommend this for anyone intend to add 5G to vyos. it will show up as usb0 which vyos can't control)
* 2023/03/24 COMFAST CF-953AX (3574:6211) # WiFi AX module with chipset MT7921U this device id is added in Linux 6.3.X+, this will be removed once 6.3.X or above become LTS.

# About

VyOS runs on a custom Linux Kernel (which is 4.19) at the time of this writing.
This repository holds a Jenkins Pipeline which is used to build the Custom
Kernel (x86_64/amd64 at the moment) and all required out-of tree modules.

VyOS does not utilize the build in Intel Kernel drivers for its NICs as those
Kernels sometimes lack features e.g. configurable receive-side-scaling queues.
On the other hand we ship additional not mainlined features as WireGuard VPN.

## Kernel

The Kernel is build from the vanilla repositories hosted at <https://git.kernel.org>.
VyOS requires two additional patches to work which are stored in the patches/kernel
folder.

### Config

The Kernel configuration used is [x86_64_vyos_defconfig](x86_64_vyos_defconfig)
which will be copied on demand during the Pipeline run into the `arch/x86/configs`i
direcotry of the Kernel source tree.

Other configurations can be added in the future easily.

### Modules

VyOS utilizes several Out-of-Tree modules (e.g. WireGuard, Accel-PPP and Intel
network interface card drivers). Module source code is retrieved from the
upstream repository and - when needed - patched so it can be build using this
pipeline.

In the past VyOS maintainers had a fork of the Linux Kernel, WireGuard and
Accel-PPP. This is fine but increases maintenance effort. By utilizing vanilla
repositories upgrading to new versions is very easy - only the branch/commit/tag
used when cloning the repository via [Jenkinsfile](Jenkinsfile) needs to be
adjusted.
