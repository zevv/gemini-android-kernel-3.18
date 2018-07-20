
Fork of the Gemini Android 3.18 Linux kernel as published by Planet Computers.

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
