> Host: ubuntu 18.04

> Phone: Xiaomi Max 3

> Toolchain: aarch64-linux-android-4.9 From Android P

Setup Build Environment
My environment for build aosp and debug, not only for kernel

```
sudo apt-get install git ccache automake flex lzop bison \
gperf build-essential zip curl zlib1g-dev zlib1g-dev:i386 \
g++-multilib python-networkx libxml2-utils bzip2 libbz2-dev \
libbz2-1.0 libghc-bzlib-dev squashfs-tools pngcrush \
schedtool dpkg-dev liblz4-tool make optipng maven libssl-dev \
pwgen libswitch-perl policycoreutils minicom libxml-sax-base-perl \
libxml-simple-perl bc libc6-dev-i386 lib32ncurses5-dev \
x11proto-core-dev libx11-dev lib32z-dev libgl1-mesa-dev xsltproc unzip
```

Next , let us compile kernel
```
$ git clone https://github.com/MiCode/Xiaomi_Kernel_OpenSource.git -b nitrogen-o-oss nitrogen-o-oss
$ cd nitrogen-o-oss
$ mkdir out
$ export ARCH=arm64
$ export SUBARCH=arm64
$ export CROSS_COMPILE=/code/android-p/prebuilts/gcc/linux-x86/aarch64/aarch64-linux-android-4.9/bin/aarch64-linux-android-
```

```
make O=out nitrogen_user_defconfig
make -j$(nproc) O=out
```

After that, you can find many compiled files in the out directory.
You can find the kernel in this directory.
`out/arch/arm64/boot`

If you want to flash this,you must unpack boot.img,replace kernel, repack boot.img.
Some device need dtbo.img, so you must repack dtbo.img.

Here we focus only on compiling the kernel.
Please go to xda or Google search how to repack boot.img and repack dtbo.img

Sometims,  you need compile wlan and audio(sdm845) modules if you find wifi and audio not work after flash boot.img

Someone maybe find compile error  like 
` error: msm_isp.h: No such file or directory`

In android source code (caf) ,  It is already setup environment.  like O=out/target/product/{TARGET_PRODUCT}/obj/kernel/,  toolchain=   ARCH=arm64/arm/x86/mips

it is no error with compile kernel