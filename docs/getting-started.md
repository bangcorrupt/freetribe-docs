# Getting Started

### Examples

The main demo uses the volume knob to control attenuation of audio pass-through,
while sending MIDI CC and printing to the display. An LED blinks at regular intervals.

Future examples will deal separately with individual features,
building up to a more complex application.

## Building

Create a codespace from the Freetribe Github repo, then run:

```
make clean && make
```

There will be a lot of warnings about incompatible types and implicit declarations, but there should be no errors.

Alternatively, if you have `docker compose` installed on your local machine,
change to the `freetribe` directory and build the docker image:

```
docker compose build
```

Then start a container:

```
docker compose up -d
```

The current working directory will be mounted in the container at `/freetribe`.

Now we can run `make` in the container to build Freetribe.

```
docker compose exec freetribe make clean
docker compose exec freetribe make
```

The final binary will be at `freetribe/cpu/build/cpu.bin`.

This includes the DSP firmware, which is sent to the Blackfin by the CPU, via SPI.

I will add documentation for setting up the build environment locally,
but you can probably work it out from the commands in the Dockerfile.

## Debugging

See the [Hacktribe Debrick Guide](https://github.com/bangcorrupt/hacktribe/wiki/Debrick#rpi-and-openocd) for more details on getting a debugger attached.

### CPU

For the CPU, a JLink EDU and JLinkGDBServer works well.
We can also use a Raspberry Pi and OpenOCD.

If using JLink, connect the debugger to USB host,
then power electribe using modified power switch. Then run:

```
JLinkGDBServer -device am1802 -speed 0
```

If using Raspberry Pi, OpenOCD will exit if the electribe is not powered,
but the electribe will boot if OpenOCD is not attached,
so we need some trickery. First power up electribe, then run OpenOCD,
then power off electribe with OpenOCD still running.
Then power up electribe again before restarting OpenOCD.
It is a lot easier to use a JLink.

Once you have a gdb server attached to the CPU,
change directory to `freetribe/cpu` and run:

```
arm-none-eabi-gdb
```

The commands in `freetribe/cpu/.gdbinit` should connect to the gdb server,
load the symbols from `cpu/build/cpu.elf` and run the firmware.
You may need to edit the port number in `.gdbinit`. OpenOCD uses `3333` by default,
with JLinkGDBServer using `2331`.

If all is well, you should hit a breakpoint at `main`.  
Other useful breakpoints may be `knl_init`, `knl_run`, `app_init` and `app_run`.

### DSP

For the DSP, I've only managed to get the official debugger from Analog Devices working.  
The ADSP-ICE-1000 costs around £180 (thank you sponsors) and is very much a one-trick pony.
I will try again to get the Raspberry Pi set up with the DSP,
but I'm not sure if the Analog Devices OpenOCD fork is even supposed to support this.

The good news is, the DSP firmware is loaded by the CPU.
So if you can deal without debugging you can still load and run your code.

If you have a debugger for the DSP, first run the binary on the CPU.
By the time the CPU reaches `app_run` the DSP kernel will be running.

Change directory to `freetribe/dsp` and run:

```
sudo openocd-bfin -s /usr/share/openocd-bfin/scripts \
                  -f /usr/share/openocd-bfin/scripts/interface/ice1000.cfg \
                  -f /usr/share/openocd-bfin/scripts/target/bf527.cfg
```

Then in another shell in the same directory:

```
bfin-elf-gdb
```

The commands in `freetribe/dsp/.gdbinit` should connect to the gdb server,
load the symbols from `freetribe/dsp/build/bfin.elf` and
set a breakpoint at `module_process`, the audio processing callback.

I will try to add new sysex functions to Hacktribe,
so we can load and execute code without needing a debugger.
