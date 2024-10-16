# CPU Firmware

`freetribe/cpu/src/*`

The CPU firmware controls the full system, initialising the other hardware,
processing user input and providing feedback.
The firmware is split into kernel and user tasks.
The separation is currently in name only, memory protection is not yet supported.

## Kernel

`freetribe/cpu/src/kernel/knl_*`

The kernel orchestrates a set of services, built on device and peripheral drivers.
A simple API is provided, hiding the complexity of the underlying system.
The rest of this section looks at each layer, from the peripheral drivers up.

### Peripheral

`freetribe/cpu/src/kernel/peripheral/per_*`

Peripheral drivers initialise and control CPU peripherals,
such as UART or SPI. Interrupt Service Routines may execute callback functions,
registered by the layer above. Peripheral drivers should be self-contained and
deal with a single peripheral. An exception may be DMA transfers,
which are currently unsupported.

### Device

`freetribe/cpu/src/kernel/device/dev_*`

Device drivers use peripheral drivers to provide access to system devices,
such as flash memory or the LCD. Received data is queued and dealt with on
demand by the layer above. Device drivers may use multiple peripheral drivers,
for example, using SPI to control the LCD and GPIO to control the backlight.

### Service

`freetribe/cpu/src/kernel/service/svc_*`

The service layer uses device drivers to provide high level services to the kernel.
Examples include MIDI processing and handling the control panel.
Services are implemented as non-blocking state machines and should
do as little as possible each time they are invoked.

### API

`freetribe/cpu/src/kernel/api/ft_*`

The API provides an interface to everything needed by user code.
Function names should be human friendly, with reduced sets of parameters where possible.

## User

`freetribe/cpu/src/user/usr_*`

The user task runs any code provided as an app.
There are two functions we override, `app_init()` and `app_run()`,
see [Getting Started](getting-started.md) for more.

### App

`freetribe/cpu/src/apps/*`

Apps are whatever we make them. We override the `app_init()` and `app_run()` functions
from the user task and call functions from the API.
Apps that process audio must have a corresponding module in the DSP firmware.

See the [Freetribe Tutorial](tutorial.md) for more.
