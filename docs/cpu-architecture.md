# CPU Firmware

`freetribe/cpu/src`

The CPU firmware controls the full system, initialising the other hardware,
processing user input and providing feedback.
The firmware is split into kernel and user tasks.
The separation is currently in name only, memory protection is not yet supported.

## Kernel

`freetribe/cpu/src/kernel/knl_*`

The kernel orchestrates a set of services, built on device and peripheral drivers.
A simple API is provided, hiding the complexity of the underlying system.
The rest of this section looks at each layer, from the hardware up.

### Hardware

`freetribe/cpu/src/kernel/hardware/hw_*`

Macros and definitions for accessing CPU registers.
These files are from Texas Instruments Starterware.
We should not have to modify anything at this layer.

### HAL

`freetribe/cpu/src/kernel/hal/hal_*`

Functions for accessing CPU registers.
These files are from Texas Instruments Starterware.
It is unlikely that we will need to modify anything at this layer.

### Peripheral

`freetribe/cpu/src/kernel/peripheral/per_*`

The peripheral layer uses HAL functions to initialise and control CPU peripherals,
such as UART or SPI. Interrupt Service Routines may execute callback handlers,
registered by the layer above. Peripheral drivers should be self-contained and
deal with a single peripheral. An exception may be DMA transfers,
which are currently unsupported.

### Device

`freetribe/cpu/src/kernel/device/dev_*`

The device layer uses peripheral drivers to provide access to system devices,
such as flash memory or the LCD.
Device drivers may use multiple peripheral drivers, for example,
using SPI to control the LCD and GPIO to control the backlight.

### Service

`freetribe/cpu/src/kernel/service/svc_*`

The service layer uses device drivers to provide services to the kernel.
Examples include MIDI processing and handling the control panel.
Services are implemented as non-blocking state machines and should
do as little as possible each time they are invoked.

### API

## User

### App
