# Features

From a user application perspective, Freetribe is currently light on features.
Most of the hardware initialisation is complete, with driver stacks for much
of the system. Built on this is a set of services providing a high-level
interface to the device. Some basic examples are provided, showing how to
integrate user application code with the Freetribe kernel.

## CPU Kernel

The CPU kernel initialises the hardware,
processes communication with the outside world
and controls the Blackfin DSP via a serial command interface.

- Serial MIDI input and output via TRS port.
- Set or clear a pixel anywhere in the vast 128x64 dot-matrix.
- Control backlight RGB (binary).
- Register callbacks for all of the panel controls.
- Set and toggle LEDs, with brightness control for those with support.
- Send commands to the Blackfin DSP and receive feedback.

## DSP Kernel

The DSP kernel receives commands from the CPU and processes audio frames.
A user defined module runs in the audio callback,
with an interface similar to many plugin formats.

- Process audio based on control input.

## Planned Features

Some of these are in progress, most should be possible.

- High speed DSP control via EMIFA/HostDMA.
- DMA support for peripheral drivers.
- USB driver.
- SD card driver.
- DSP block processing
- Cache and memory protection.
- Dynamic Linking of apps and modules.
- Preemptive sheduling using FreeRTOS.
- Embedded LUA and MicroPython.
- Support for sync ports.
