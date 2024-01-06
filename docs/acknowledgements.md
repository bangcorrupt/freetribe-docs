# Acknowledgements

Freetribe would be almost impossible without other open-source projects.
The CPU drivers are based on [StarterWare](https://www.ti.com/tool/STARTERWARE-SITARA) by Texas Instruments.
The hardware abstraction, build environment and code examples
provided the stepping-stone needed to get started.

In much the same way, the DSP drivers are based on [monome/aleph](https://github.com/monome/aleph).
This showed how to initialise the Blackfin processor and configure peripherals.
They also provide a public domain DSP library, with many of the difficult
maths problems packaged into convenient unit generators.

MIDI input parsing is based on [mikromodular/libmidi](https://github.com/mikromodular/libmidi),
with sysex/binary conversion borrowed from the [Arduino MIDI library](https://github.com/FortySevenEffects/arduino_midi_library/blob/master/src/MIDI.cpp) by Francois Best.

[UGUI](https://github.com/deividAlfa/UGUI) and [micromenu](https://github.com/abcminiuser/micromenu-v2) provide a graphical interface for user applications.

Special thanks to countless stackoverflow users.
