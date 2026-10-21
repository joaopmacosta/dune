# Container build environment

[Dockerfile](Dockerfile) prepares a Debian Bookworm environment with a C/C++
toolchain, CMake, Python, Doxygen and debugging/editing tools. It creates
`/dune/source` and `/dune/build`, sets the build directory as the working
directory, and adds it to `PATH`. It does not compile DUNE or start a vehicle.

[docker-compose.yml](docker-compose.yml) builds that image and mounts the
repository at `/dune/source` and the host directory `../../build` at
`/dune/build` (paths relative to this Compose file). Its TTY/restart settings
provide a development container, not a defined DUNE runtime service.

Inside the container, use the [CMake workflow](../cmake/README.md), with
`/dune/source` as the source directory and `/dune/build` as the build directory.
Hardware device access and a safe runtime configuration must be arranged
separately; the Compose file does not provide device mappings.
