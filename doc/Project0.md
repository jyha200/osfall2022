# Project 0
* Assigned: 2022-09-08
* Due: 2022-09-15
* Help document: https://github.com/jyha200/osfall2022/tree/master/presentations
* **If you need to contact us, please send an email to jyha@dcslab.snu.ac.kr.**

## Introduction
This assignment will help you setup your kernel development environment. This project includes building kernel image from source code and Installing Qemu. After this project, you have setup all the necessary tools for the upcoming projects.

This is an individual project. You have to take a screenshots of terminal of the appropriate steps it took to run your emulator. Send the screenshots via etl before the deadline.
The final screenshot should look like:

![qemu start](/doc/booting_tizen.png)

## Your Device

We will use QEMU to emulate device environment.
In this project we will guide you step by step on how to do that.

## Development Environment

To build kernel images and run the emulator, development environment must be prepared. There are number of ways to do this. Choose the most appropriate one for you.

**WARNING: Linux source tree has files whose names differ in case only. This means that you MUST use a _case sensitive_ filesystem when you work on your projects. Common case _insensitive_ filesystems include NTFS (Windows) and HFS+ (Mac); please avoid these when working with the source code. You do not have to worry about this if you stick to the setup guide we provided below.**

### Setup Ubuntu

You can install Ubuntu side-by-side with existing OS on your machine, or erase existing OS and install Ubuntu on it, or you can use Virtual machine to run ubuntu operating system (know that with VM you will allocate limited resources and that will slow down the runtime of your project) (Learn more [#1](https://ubuntu.com/tutorials/install-ubuntu-desktop#1-overview) [#2](https://help.ubuntu.com/community/WindowsDualBoot) [#3](https://ubuntu.com/tutorials/create-a-usb-stick-on-windows#1-overview))

The following releases are supported:
* 18.04 LTS Bionic Beaver (recommanded)
* 16.04 LTS Xenial Xerus (and its point releases)
* 14.04 LTS Trusty Tahr (and its point releases)
* 17.10.1 Artful Aardvark

Then install the following packages: ccache, gcc-aarch64-linux-gnu, unrpm, pv
```bash
sudo apt-get install ccache gcc-aarch64-linux-gnu obs-build pv -y
```

## Compiling the Kernel & Running the emulator 

Now it's time to actually build the kernel image and run the emulator.

* If you use Virtual Machine, don't forget to execute commands in VM.

### Getting Kernel Source

You clone the kernel source using `git`. Execute the following command.
```bash
git clone https://github.com/jyha200/tizen-5.0-rpi3/
```

Or if you've [registered your ssh key to GitHub](https://help.github.com/articles/connecting-to-github-with-ssh/), you can execute the following command.
```bash
git clone git@github.com:jyha200/tizen-5.0-rpi3.git
```

You don't have to type in GitHub username and password if you're cloning repository over ssh.

### Compiling the Kernel and Making images

Walk into the kernel source.
```bash
cd tizen-5.0-rpi3
```

And just type
```bash
./build-rpi3-arm64.sh
```
to compile the kernel.

Then, make boot images with the following shell script (you need sudo this time).
```bash
sudo ./scripts/mkbootimg_rpi3.sh
```
** You may turn off the automatic opening of mounted folders with `dconf-editor`.

Ensure that two new image files (`modules.img`, `boot.img`) are created.

## Run QEMU

Start by installing QEMU
```bash
sudo apt-get install qemu
```
or 
```bash
sudo apt-get install qemu-system-aarch64
```
* download the script and config file from this link
[https://github.com/jyha/osfall2022/tree/master/src](https://github.com/jyha200/osfall2022/tree/master/src)
* Move `qemu.sh` to the Tizen kernel directory `(ex. /home/dcslab/osfall2020-team1/)`
* Create a directory called `tizen-image` where the Tizen kernel directory is located `(ex. /home/dcslab/)`
* Replace the existing `arch/arm64/configs/tizen_bcmrpi3_defconfig file` with downloaded file (back up the existing file)
* After building the kernel and using the existing scripts to create the kernel images `boot.img`, `modules.img` files, move to the `tizen-image` directory
* Create the remaining img files by decompressing the `tizen-unified_20181024.1_iot-headless-2parts-armv7l-rpi3.tar.gz` of the kernel directory into the `tizen-image` directory.
```(ex) tar xvzf tizen-unified_20181024.1_iot-headless-2parts-armv7l-rpi3.tar.gz -C /home/dcslab/tizen-image```
* By executing the script `qemu.sh` you can access to the tizen shell `(ID:root  PW:tizen)`
* Take a screenshots of your process and submit it to etl.

* (참고) tizen-image 디렉터리를 만들지 않고 qemu.sh 스크립트의 img 파일 경로 (ex. file=../tizen-image/rootfs.img)를 해당 img 파일이 있는 경로로 직접 바꿔주셔도 됩니다.
* (2) Troubleshooting
추가로, 위 과정대로 진행해도 쉘로 진입하지 못하고 터미널에 아무것도 보이지 않을 수 있습니다. qemu의 스탠다드 콘솔출력 문제를 참고하여 진행하시길 바랍니다.

## We're Here to Help You

Any troubles? Discussions on [issue board](https://github.com/jyha200/osfall2022/issues) is more than welcome. Remember, we are here to help you.

Start early, ask for help if needed, and most importantly, have fun!
