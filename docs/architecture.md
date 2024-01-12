# System Overview

This section takes a deeper look at the Freetribe system architecture.

There are two separate firmwares, one for the ARM CPU and the other for the Blackfin DSP.
The code is built in layers, from peripheral drivers up to user applications.
Each layer consumes from the layer below and provides for the layer above.
