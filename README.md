### How to build kernel/module prebuilt for Android build

* Init repo:
```
repo init -u https://github.com/EssentialOpenSource/kernel-manifest -b refs/tags/PPR1.180610.091
```
* Sync repo:
```
repo sync -j4 -c
```
* Download the latest [Android NDK](https://developer.android.com/ndk/downloads/index.html) and extract it in a toolchain folder
* Download [clang-4053586](https://android.googlesource.com/platform/prebuilts/clang/host/linux-x86/) and extract it in the toolchain folder
* Export global variables
```
export ARCH=arm64
export CROSS_COMPILE=`pwd`/toolchain/android-ndk-r16b/toolchains/aarch64-linux-android-4.9/prebuilt/linux-x86_64/bin/aarch64-linux-android-
# Build with CLANG
export PATH=`pwd`/toolchain/clang-4630689/bin:$PATH
export LD_LIBRARY_PATH=`pwd`/toolchain/clang-4630689/lib64:$LD_LIBRARY_PATH
export CLANG_TRIPLE=aarch64-linux-gnu-
```
* Build the kernel (For PH-1)
```
cd kernel
make CC=clang mata_defconfig
make CC=clang -j32
```
* Build the WiFi module
```
cd ../qcacld-3.0/
make CC=clang -j32
${CROSS_COMPILE}strip --strip-debug wlan.ko
```
* Use qcacld-3.0/wlan.ko and arch/arm64/boot/Image.gz-dtb as prebuilt for your Android boot image

## Build Essential PH-1 kernel with KASAN/KCOV support

To allow KASAN testing and fuzzing (like with syzkaller), you can compile
PH-1 kernel with mata_kasan_defconfig

```
cd kernel
make CC=clang mata_kasan_defconfig
make CC=clang -j32
```

KCOV, KASAN and additional debug configuration flags (like kernel memory leak detector)
are enabled
