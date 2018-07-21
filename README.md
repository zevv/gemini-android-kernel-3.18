
This is a fork of the Gemini Android 3.18 Linux kernel as published by Planet
Computers. (Which really deserves a big "thank you!", it is really great that
Planet was able to deliver!)

This fork has some small fixes and workarounds for bugs/issues on the Gemini
PDA. 

Some of the code is shared with (stolen from) the kernel maintained by the
Gemian project at https://github.com/gemian/gemini-linux-kernel-3.18.

I provide boot images which can be installed with the Gemini flash tool at
https://github.com/zevv/gemini-android-kernel-3.18/releases. These work for me
but might break your device, use at your own risk.

Done:

- Fixed out of phase audio output when on playing on speaker. When no accessory
  is attached the phase of the left channel is now inverted before data is sent
  to the codec.

- Fix keyboard ghosting. The keyboard driver now keeps track of key presses and
  releases and makes a good effort to generate the right key codes of a chord
  when releasing the first keys (Thanks Nathan)

Whishlist:

- Fix EMC noise on headphones when not playing. It might be possible to power
  down the appropriate analog circuits, or keep the codec powered on. Needs
  more investigation.

Can not fix:

- Stereo output on speakers. It seems that mono downmixing is done in userspace
  before the audio is sent to the driver, so there is nothing the kernel can
  do here.


# Building

To cross compile the kernel on x86_64:

Download the kernel source:

```bash
$ git clone git@github.com:zevv/gemini-android-kernel-3.18.git
```

Download a prebuilt gcc:

```bash
$ git clone https://android.googlesource.com/platform/prebuilts/gcc/linux-x86/aarch64/aarch64-linux-android-4.9 -b nougat-release --depth 1
```

Get the `mkbootimg` tool:

```bash
$ git clone https://github.com/osm0sis/mkbootimg.git
$ make -C mkbootimg
```

Building the kernel:

```bash
make O=../build -C gemini-android-kernel-3.18 ARCH=arm64 aeon6797_6m_n_defconfig
make O=../build -C gemini-android-kernel-3.18 ARCH=arm64 CROSS_COMPILE=../aarch64-linux-android-4.9/bin/aarch64-linux-android- -j8
```

To create the boot image, run `mkbootimg` with a bunch of options. You will
need the kernel image you just compiled, but also the ramdisk cpio to go into
the image. I extracted the ramdisk image from my own mmcblk0p22 partition, you
can find it here:

  http://zevv.nl/mmcblk0p22-ramdisk.gz

Now run:

```bash
./mkbootimg/mkbootimg \
    --kernel build/arch/arm64/boot/Image.gz-dtb \
    --ramdisk mmcblk0p22-ramdisk.gz \
    --base 0x40078000 \
    --second_offset 0x00e88000 \
    --cmdline "bootopt=64S3,32N2,64N2 buildvariant=user" \
    --kernel_offset 0x00008000 \
    --ramdisk_offset 0x04f88000 \
    --tags_offset 0x03f88000 \
    --pagesize 2048 \
    --board 1528859406 \
    --hash sha1 \
    --os_version 7.1.1 \
    --os_patch_level 2017-11 \
    -o linux_boot.img
```

And with some luck you will end up with `linux_boot.img`

# Installing

Install using the planet computers flashtool. Just load any scatter file,
untick all the boxes except for the 'boot' partition, and point the image to
the file you just generated. Click 'download' and restart the Gemini to load
the new image.
