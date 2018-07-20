
Fork of the Gemini Android 3.18 Linux kernel as published by Planet Computers.

This fork has some small fixes and workarounds for bugs/issues on the Gemini
PDA. 

Some of the code is shared with (stolen from) the kernel maintained by the
Gemian project at https://github.com/gemian/gemini-linux-kernel-3.18.

I provide boot images which can be installed with the Gemini flash tool at
https://github.com/zevv/gemini-android-kernel-3.18/releases. These work for me
but might break your device, use at your own risk.

Done:

- Fixed out of phase audio output when on playing on speaker
- Fix keyboard ghosting (Thanks Nathan)

Whishlist:

- Fix EMC noise on headphones when not playing

