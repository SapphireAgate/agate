Agate
=====

Install instructions (adapted from [TaintDroid Build Instructions]
(http://appanalysis.org/download.html)):

**1. Get android-4.3_r1 source code.**

Agate is built on TaintDroid for *android-4.3_r1* tag of the Android source
code. To get the source code you can use the following commands (more details
on [Android source code](https://source.android.com)):

```
 mkdir -p ~/agate-4.3_r1
 cd ~/agate-4.3_r1
 repo init -u https://android.googlesource.com/platform/manifest -b android-4.3_r1
 repo sync
```

Note: At this point, it is recommended that you build Android without any
modifications. This will ensure that any build errors for your environment are
resolved and are not confused with Agate build errors. To build android:

```
. build/envsetup.sh
lunch 1
make -j4
```

**2. Get Agate and TaintDroid source code.**

Copy and paste the following content into
~/agate-4.3_r1/.repo/local_manifests/local_manifest.xml:


```
<manifest>
  <remote name="github" fetch="git://github.com"/>
  <remove-project name="platform/dalvik"/>
  <project path="dalvik" remote="github" name="SapphireAgate/android_platform_dalvik"
      revision="agate-4.3_r1"/>
  <remove-project name="platform/libcore"/>
  <project path="libcore" remote="github" name="SapphireAgate/android_platform_libcore"
      revision="agate-4.3_r1"/>
  <remove-project name="platform/frameworks/base"/>
  <project path="frameworks/base" remote="github" name="TaintDroid/android_platform_frameworks_base"
      revision="taintdroid-4.3_r1"/>
  <remove-project name="platform/frameworks/native"/>
  <project path="frameworks/native" remote="github" name="TaintDroid/android_platform_frameworks_native"
      revision="taintdroid-4.3_r1"/>
  <remove-project name="platform/frameworks/opt/telephony"/>
  <project path="frameworks/opt/telephony" remote="github" name="TaintDroid/android_platform_frameworks_opt_telephony"
      revision="taintdroid-4.3_r1"/>
  <remove-project name="platform/system/vold"/>
  <project path="system/vold" remote="github" name="TaintDroid/android_platform_system_vold"
      revision="taintdroid-4.3_r1"/>
  <remove-project name="platform/system/core"/>
  <project path="system/core" remote="github" name="TaintDroid/android_platform_system_core"
      revision="taintdroid-4.3_r1"/>
  <remove-project name="device/samsung/manta"/>
  <project path="device/samsung/manta" remote="github" name="TaintDroid/device_samsung_manta"
      revision="taintdroid-4.3_r1"/>
  <remove-project name="device/samsung/tuna"/>
  <project path="device/samsung/tuna" remote="github" name="TaintDroid/android_device_samsung_tuna"
      revision="taintdroid-4.3_r1"/>
  <project path="packages/apps/TaintDroidNotify" remote="github" name="TaintDroid/android_platform_packages_apps_TaintDroidNotify"
      revision="taintdroid-4.3_r1"/>
</manifest>

```

Pull the source code and make sure we are working with the right version.

```
cd ~/agate-4.3_r1
repo sync
repo for all dalvik libcore \
              -c 'git checkout -b agate-4.3_r1 --track github/agate-4.3_r1 && git pull'

repo for all frameworks/base frameworks/native frameworks/opt/telephony \
             system/vold system/core device/samsung/manta device/samsung/tuna \
             packages/apps/TaintDroidNotify -c 'git checkout -b taintdroid-4.3_r1 --track github/taintdroid-4.3_r1 && git pull'

```

**3. Get proprietary binaries.**

The Galaxy Nexus, Nexus 4, Nexus 7, and Nexus 10 require proprietary binaries
not included in the AOSP release.
[Download](https://developers.google.com/android/nexus/drivers) the correct
version of these files for your device. For example, we used Nexus 7 tilapia and grouper:

3.a Download binaries for Nexus 7 tilapia (Mobile):

```
cd ~/agate-4.3_r1

wget https://dl.google.com/dl/android/aosp/asus-grouper-jwr66y-d9ad928d.tgz
tar -zxvf asus-grouper-jwr66y-d9ad928d.tgz
./extract-asus-grouper.sh # (view the license and then type "I ACCEPT")

wget https://dl.google.com/dl/android/aosp/broadcom-grouper-jwr66y-af694cc9.tgz
tar -zxvf broadcom-grouper-jwr66y-af694cc9.tgz
./extract-broadcom-grouper.sh # (view the license and then type "I ACCEPT")

wget https://dl.google.com/dl/android/aosp/elan-grouper-jwr66y-2ece01e1.tgz
tar -zxvf elan-grouper-jwr66y-2ece01e1.tgz
./extract-elan-grouper.sh # (view the license and then type "I ACCEPT")

https://dl.google.com/dl/android/aosp/invensense-grouper-jwr66y-f21f0c49.tgz
tar -zxvf invensense-grouper-jwr66y-f21f0c49.tgz
./extract-invensense-grouper.sh # (view the license and then type "I ACCEPT")

https://dl.google.com/dl/android/aosp/nvidia-grouper-jwr66y-b3b0003e.tgz
tar -zxvf nvidia-grouper-jwr66y-b3b0003e.tgz
./extract-nvidia-grouper.sh # (view the license and then type "I ACCEPT")

https://dl.google.com/dl/android/aosp/nxp-grouper-jwr66y-f5d295e4.tgz
tar -zxvf nxp-grouper-jwr66y-f5d295e4.tgz
./extract-nxp-grouper.sh # (view the license and then type "I ACCEPT")

https://dl.google.com/dl/android/aosp/widevine-grouper-jwr66y-a0b9cafc.tgz
tar -zxvf widevine-grouper-jwr66y-a0b9cafc.tgz
./extract-widevine-grouper.sh # (view the license and then type "I ACCEPT")
```

3.a Download binaries for Nexus 7 grouper (Wi-Fi):

```
cd ~/agate-4.3_r1

wget https://dl.google.com/dl/android/aosp/asus-grouper-jwr66y-d9ad928d.tgz
wget tar -zxvf asus-grouper-jwr66y-d9ad928d.tgz
./extract-asus-grouper.sh # (view the license and then type "I ACCEPT")

wget https://dl.google.com/dl/android/aosp/broadcom-grouper-jwr66y-af694cc9.tgz
tar -zxvf broadcom-grouper-jwr66y-af694cc9.tgz
./extract-broadcom-grouper.sh # (view the license and then type "I ACCEPT")

wget https://dl.google.com/dl/android/aosp/elan-grouper-jwr66y-2ece01e1.tgz
tar -zxvf elan-grouper-jwr66y-2ece01e1.tgz
./extract-elan-grouper.sh # (view the license and then type "I ACCEPT")

wget https://dl.google.com/dl/android/aosp/invensense-grouper-jwr66y-f21f0c49.tgz
tar -zxvf invensense-grouper-jwr66y-f21f0c49.tgz
./extract-invensense-grouper.sh # (view the license and then type "I ACCEPT")

wget https://dl.google.com/dl/android/aosp/nvidia-grouper-jwr66y-b3b0003e.tgz
tar -zxvf nvidia-grouper-jwr66y-b3b0003e.tgz
./extract-nvidia-grouper.sh # (view the license and then type "I ACCEPT")

wget https://dl.google.com/dl/android/aosp/nxp-grouper-jwr66y-f5d295e4.tgz
tar -zxvf nxp-grouper-jwr66y-f5d295e4.tgz
./extract-nxp-grouper.sh # (view the license and then type "I ACCEPT")

wget https://dl.google.com/dl/android/aosp/widevine-grouper-jwr66y-a0b9cafc.tgz
tar -zxvf widevine-grouper-jwr66y-a0b9cafc.tgz
./extract-widevine-grouper.sh # (view the license and then type "I ACCEPT")
```

**4. Build Agate.**

First, ensure TaintDroid is activated. Create a *buildspec.mk* file and define
some variables so that TaintDroid will build properly. 

```
cd ~/agate-4.3_r1
vim buildspec.mk 

# Enable core taint tracking logic (always add this)
WITH_TAINT_TRACKING := true

# Enable taint tracking for ODEX files (always add this)
WITH_TAINT_ODEX := true

# Enable taint tracking in the "fast" (aka ASM) interpreter (recommended)
WITH_TAINT_FAST := true

# Enable additional output for tracking JNI usage (not recommended)
#TAINT_JNI_LOG := true

# Enable byte-granularity tracking for IPC parcels
WITH_TAINT_BYTE_PARCEL := true
```

Next, add the TaintDroidNotify application to the build. Open
~/agate-4.3_r1/build/target/product/core.mk and add TaintDroidNotify to the end of the
PRODUCT_PACKAGES list.

```
PRODUCT_PACKAGES += \
                    BasicDreams \
                    ...
                    voip-common \
                    TaintDroidNotify

```

Build android with the following commands:

```
. build/envsetup.sh
lunch <target> #(replace <target> with correct value for your device)
make clean
make -j4
```

**5. Flash the image on the device.**

```
adb reboot-bootloader
sleep 3
fastboot flash boot boot.img
fastboot flash system system.img
fastboot flash userdata userdata.img
fastboot reboot
```


