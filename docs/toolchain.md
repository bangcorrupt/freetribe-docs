# Installing the Toolchain

This section describes how to install the toolchain
for building and debuggong Freetribe applications.
See [Building an Application](building.md) for more
details on the build system, and
[Attaching a Debugger](debugging.md) for how to run and debug an application.


## Installing Locally

## Installing with Docker

If you have `docker compose` installed on your local machine,
change to the `freetribe` directory and build the docker image:

```
docker compose build
```

Then start a container:

```
docker compose up -d
```

The current working directory will be mounted in the container at `/freetribe`.






