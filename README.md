# ez-Wheel SWD® services

## Overview

This repository provides the documentation of the APIs provided in the linux debian package **swd-services**.
These APIs can be used to control and configure the SWD® (Safety Wheel Drive) powered by [ez-Wheel](https://www.ez-wheel.com).

| <img src="https://www.ez-wheel.com/storage/image-product/visuels-swd-core-2-0-0.png" width="45%"></img> | <img src="https://ez-wheel.com/storage/image-product/ezswd125im-31102023-photo-hd-det.png" width="50%"></img> | <img src="https://www.ez-wheel.com/storage/image-product/roue-electrique-swd-150-2-0-0-0.png" width="45%"></img>       |
| ------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------- |
| [SWD® Core](https://www.ez-wheel.com/en/safety-gear-motor) <br />Safety gear motor                      | [SWD® 125](https://ez-wheel.com/en/safety-wheel-drive-swd-125) <br /> Medium duty Safety Wheel Drive |[SWD® 150](https://www.ez-wheel.com/en/swd-150-safety-wheel-drive) <br /> Heavy duty Safety wheel drive                      | 

Users should regularly inform themselves about updates of this driver (Activating GitHub notifications with "Watch", 'All activity' button on top of this page).

## Prerequisites

- SWD® drive
- Ubuntu 22.04 jammy or
- Ubuntu 20.04 focal
- CAN bus communication established bewteen the PC and SWD®

## Linux debian package

It is available for the following platforms:

- *ARM 32-bits*, i.e. **armhf** debian packages (only for Ubuntu 20.04 focal)
- *ARM 64-bits*, i.e. **arm64** debian packages, since version 3.3.1
- *AMD 64-bits*, i.e. **amd64** debian packages, for x86 machines

## Installation

In order to install **swd-services** with apt (Advanced Packaging Tool), you need to add ez-Wheel repository to your Apt sources configuration file as sudo in: */etc/apt/sources.list*. Type the following command:

For  Ubuntu 20.04 (Focal):

```shell
echo "deb http://packages.ez-wheel.com:8081/ubuntu focal main" | sudo tee -a /etc/apt/sources.list
```

For  Ubuntu 22.04 (Jammy):

```shell
echo "deb http://packages.ez-wheel.com:8081/ubuntu/ jammy main" | sudo tee -a /etc/apt/sources.list
```

Then, download and add the GPG key. Type the following command:

```shell
wget -qO - http://packages.ez-wheel.com:8081/archive.key | sudo apt-key add -
```

Now, you should be able to install the package using apt:

```shell
sudo apt update && sudo apt install swd-services
```

The libraries are now installed in your linux environment in */opt/ezw/usr/*.

## Start using swd-services

### Configure CAN bus on Linux socket (Optional if already configured)

```shell
sudo ip link set down can0
sudo ip link set can0 up type can bitrate 1000000 restart-ms 100
sudo ip link set can0 txqueuelen 1000
```

### Configure D-Bus session (Optional if already one exists)

```shell
unset LD_LIBRARY_PATH 
/usr/bin/dbus-launch > /tmp/SYSTEMCTL_dbus.id
```

### Start ezw-smc-service

```shell
export $(cat /tmp/SYS*.id) 
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/opt/ezw/usr/lib 
/opt/ezw/usr/bin/ezw-smc-service /opt/ezw/usr/etc/ezw-smc-core/drive_config.ini
```

*Success if you can see those logs:*

- *"Initialize PDO success."*
- *"Initializing dbus service!"*

## Examples

### Script using ezw-smc-service

The python 'remote' script can be used to control a SWD® drive:

```shell
/opt/ezw/usr/sbin/remote.py smc_drive
```

*Keyboard commands are listed at the end.*

### Script using ezw-smc-core

The c++ code 'DiffDriveController.cpp' can be used to control a pair of  SWD® drives as a diffrential kinematic platform.



The python 'swd_xxxx_x_commissioning' scripts can be used to configure a SWD® drive:

```shell
swd_left_4_commissioning.py
swd_right_5_commissioning.py
```

*Note that, if you are using SWD® diff_drive_controller you might have to stop it before commissioning SWD®.*
