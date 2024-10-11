# DSP Firmware

`freetribe/dsp/src/*`

The DSP kernel receives commands from the CPU and processes audio frames.
A user defined module runs in the audio callback, with an interface similar
to many plugin formats. The code here is less developed, but will gradually
be made more similar to the CPU firmware.

## Kernel

`freetribe/dsp/src/kernel/knl_*`

Similar to the CPU firmware, the kernel should orchestrate a set of services,
built on device and peripheral drivers.
The rest of this section looks at each layer, from the hardware up.

### Peripheral

`freetribe/dsp/src/kernel/peripheral/per_*`

The peripheral layer accesses hardware registers directly to initialise and
control DSP peripherals, such as SPI or SPORT (for i2s).
Currently, Interrupt Service Routines push directly to queues in the peripheral driver.
Queues should be moved up to a device layer,
with ISRs executing optional callback functions.
Peripheral drivers should be self-contained and
deal with a single peripheral and its associated DMA channels.

### Device

`freetribe/dsp/src/kernel/device/dev_*`

The device layer does not currently exist. This is where we should handle peripheral
interrupts, pushing data to queues and providing access for the service layer above.

### Service

`freetribe/dsp/src/kernel/service/svc_*`

The service layer uses drivers to provide services to the kernel.
For example, the CPU command service parses and handles messages received
from the CPU via SPI. Services are implemented as non-blocking state
machines and should do as little as possible each time they are invoked.

## Module

`freetribe/dsp/src/modules/*`

Modules are whatever we make them. We override functions to provide initialisation,
audio processing, and parameter change handling. The `module_process()` function
is called for each audio sample, block processing is not yet supported. The `module_set_param()`
function is called by the CPU command service when the appropriate message is received.

What's currently called 'module' will be refactored into an 'app', similar to the
CPU firmware. An app could host a set of modules, providing a patchable
virtual modular system.

See the [Freetribe Tutorial](tutorial.md) for more.
