# LineageOS 17.1 (Android 10 Go) for the Samsung Galaxy S (GT-I9000)

Local manifest for building LineageOS 17.1 with the Android 10 Go configuration
for the Galaxy S (`galaxysmtd`, Linux 3.0 kernel).

## Getting the source

```bash
repo init -u https://github.com/LineageOS/android.git -b lineage-17.1 --git-lfs
mkdir -p .repo/local_manifests
curl -o .repo/local_manifests/galaxysmtd.xml \
    https://raw.githubusercontent.com/MaxSignal/local_manifests/lineage-17.1/galaxysmtd.xml
repo sync -c
```

## Building

The usual LineageOS 17.1 build environment (64-bit Linux, OpenJDK from the
tree, about 200 GB of disk).

```bash
source build/envsetup.sh
lunch lineage_galaxysmtd-userdebug
m systemimage recoveryimage updater s1q_blkpg simg2img signapk
device/samsung/galaxysmtd/releasetools/build_release.sh
```

`brunch` does not produce a usable package for this device. The release script
builds the kernel (`kernel/samsung/aries`, `s1q_galaxys_defconfig`, with the
recovery and first stage ramdisks embedded) and writes to `$OUT/release`:

- `lineage-17.1-<date>-UNOFFICIAL-galaxysmtd-kernel.tar.md5`: kernel for Odin (PDA)
- `zImage`: the same kernel for Heimdall (`--KERNEL`)
- `lineage-17.1-<date>-UNOFFICIAL-galaxysmtd.zip`: installer for the recovery
  in that kernel

Optional microG installer zip (APKs from https://github.com/microg/GmsCore/releases):

```bash
m updater signapk
device/samsung/galaxysmtd/releasetools/build_microg.sh \
    com.google.android.gms-<version>.apk com.android.vending-<version>.apk
```
