# Building an Application

This section describes the build process.

## Building Locally

## Building with Docker

We can run `make` in the container to build Freetribe.

```
docker compose exec freetribe make clean
docker compose exec freetribe make
```

The final binary will be at `freetribe/cpu/build/cpu.bin`.

This includes the DSP firmware, which is sent to the Blackfin by the CPU, via SPI.

I will add documentation for setting up the build environment locally,
but you can probably work it out from the commands in the Dockerfile.

## Makefile Options
