# Getting Started

This section shows how to get started with the Freetribe API. 
The minimal example code shows how to blink an LED on the panel 
using the Freetribe API's non-blocking delay function. 


## Prerequisites

Before we can actually run our app, we will need to 
[install the toolchain](toolchain.md) and [attach a debugger](debugging.md).
For now, we can work in a codespace on Github, 
and continue with this section to get an idea of how the Freetribe API works.  

Create a codespace on the `main` branch of the Freetribe Github repo,
then create a directory for the app and a file for the code:

```
mkdir cpu/src/apps/blink
touch cpu/src/apps/blink.c
```

Open `blink.c` in the editor of your choice.  The codespace includes VSCode and neovim.

## Minimal Example

First, we must include the Freetribe API:

``` c
#include "freetribe.h"
```

This gives us access to all the functions our 
application should need for interacting with the device.

Next, there are 2 functions we should override.
The first, `app_init()`, runs once when our app starts. 
This is a good place to register callbacks for events we are interested in, 
and do any initialisation required by external libraries.

In this example, we initialise a static global variable to hold the start time of our delay. 
The `app_init()` function takes no arguments and returns `t_status`, an integer error code.

``` c
// 1 second in microseconds.
#define DELAY_TIME 1000000 

static uint32_t g_start_time; 

t_status app_init(void) {

    // Set start time.
    g_start_time = ft_get_delay_current();

    return SUCCESS;
}
```

<br/>

The second function, `app_run()`, is called continuously in the main loop, 
after `app_init()` has completed. 
In this example, we toggle an LED on the panel if 1 second has passed, 
and reset the delay start time. 

The `app_run` function takes no arguments and returns nothing.

``` c
void app_run(void) {

    // Wait for delay.
    if (ft_delay(g_start_time, DELAY_TIME)) {

        // Toggle LED.
        ft_toggle_led(LED_TAP);

        // Reset start time.
        g_start_time = ft_get_delay_current();

    }
}
```

This is all the code we need to blink an LED at regular intervels, 
everything else is taken care of by the Freetribe kernel.  It is
important that any code we write is non-blocking, as everything 
is running in the same loop as the kernel.
The full listing of `blink.c` is reproduced at the bottom of this page. 

## Building

Build with `make`, passing the name of our app directory in the APP environment variable:

```
make clean && make APP=blink
```

There will be a lot of warnings about incompatible types and implicit declarations, but there should be no errors.
See [Buiding an Application](building.md) for more information about the build system.

## Output Files

The final output file, `freetribe/cpu/build/cpu.elf`, includes the CPU kernel and our app.
It also includes the DSP kernel and audio processing module, as an array sent to the DSP during boot.
We will explore the other files in this directory in future [tutorials](tutorial.md).


## Next Steps

Once you have [set up the toolchain](toolchain.md), move on to [attaching a debugger](debugging.md) 
to see how to run this simple example app.
After that, work through the [Freetribe Tutorial](tutorial.md) to explore more of the Freetribe API.

## `blink.c`

``` c
// Freetribe: Minimal Example
// License: AGPL-3.0-or-later

#include "freetribe.h"

// 1 second in microseconds.
#define DELAY_TIME 1000000 

static uint32_t g_start_time; 

t_status app_init(void) {

    // Set start time.
    g_start_time = ft_get_delay_current();

    return SUCCESS;
}

void app_run(void) {

    // Wait for delay.
    if (ft_delay(g_start_time, DELAY_TIME)) {

        // Toggle LED.
        ft_toggle_led(LED_TAP);

        // Reset start time.
        g_start_time = ft_get_delay_current();

    }
}
```
