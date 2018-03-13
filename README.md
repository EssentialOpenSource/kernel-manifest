### How to build kernel/module prebuilt for Android build

* Init repo:
```
repo init -u https://github.com/EssentialOpenSource/kernel-manifest -b refs/tags/OPM1.180104.092
```
* Sync repo:
```
repo sync -j4 -c
```
* Download the latest [Android NDK](https://developer.android.com/ndk/downloads/index.html) and extract in a toolchain folder
* Export global variables
```
export ARCH=arm64
export CROSS_COMPILE=`pwd`/toolchain/android-ndk-r16b/toolchains/aarch64-linux-android-4.9/prebuilt/linux-x86_64/bin/aarch64-linux-android-
```
* Build the kernel (For PH-1)
```
cd kernel
make mata_defconfig
make -j32
```
* Build the WiFi module
```
cd ../qcacld-3.0/
make -j32
${CROSS_COMPILE}strip --strip-debug wlan.ko
```
* Use qcacld-3.0/wlan.ko and arch/arm64/boot/Image.gz-dtb as prebuilt for your Android boot image
