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

### MIDI Service

- Serial MIDI input and output via TRS port.
- Register callbacks for each type of message received.
- Send simple messages such as note and CC.

### Display Service

- Set or clear a pixel anywhere in the vast 128x64 dot-matrix.
- Control backlight RGB (binary).

### Panel Service

- Register callbacks for all of the panel controls.
- Set and toggle LEDs, with brightness control for those with support.

### System service

- Aggregates useful things like print and timing.

### DSP Service

- Send commands to the Blackfin DSP and receive feedback.

## DSP Kernel

The DSP kernel receives commands from the CPU and processes audio frames.
A user defined module runs in the audio callback,
with an interface similar to many plugin formats.

## Planned Features

Some of these are in progress, most should be possible.

### High speed DSP Control

- CPU EMIFA is connected to DSP HostDMA with a 16 bit parallel interface.
- (Currently Freetribe uses SPI to control DSP).

### DMA Driver

- Implement CPU DMA driver and update device drivers.

### USB Driver

- Port TinyUSB.

### SD Card Driver

- Port FatFS.

### DSP Block Processing

- Implement block processing using DMA linked descriptors.

### Cache and Memory Protection

- Currently everything runs in system mode with no cacheing.

### Dynamic Linking

- CPU app and DSP module are currently compiled into the kernels.
- Implement as DLL to allow runtime switching of apps/modules/plugins.

### Preemptive Scheduling

- Port FreeRTOS.

### MicroPython

- As FreeRTOS task.

### Sync Ports

- Probably debounced GPIO, haven't checked.
