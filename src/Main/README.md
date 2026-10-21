# Process entry points

This section connects command-line execution to the
[core daemon](../DUNE/Daemon.hpp) and the configured
[tasks](../DUNE/Tasks/README.md).

* [Daemon.cpp](Daemon.cpp) builds the `dune` executable. It parses configuration,
  profile, vehicle-name and parameter-dump options, registers statically linked
  tasks, sets signal handling and runs the daemon. Shutdown joins tasks; daemon
  failure can abort the process, and a requested reboot is handled here.
* [Launcher.cpp](Launcher.cpp) builds `dune-launcher`. On POSIX it starts the
  neighboring `dune` executable and restarts it after a signal-induced crash;
  it exits after normal daemon termination. It is a process wrapper, not the
  task scheduler.
* [StaticTasks.cpp.cmake](StaticTasks.cpp.cmake) is the template for the generated
  factory-registration source. [CMake task discovery](../../cmake/Tasks.cmake)
  supplies the constructors for tasks whose build dependencies are available.
* [Memory.hpp](Memory.hpp) supports the daemon's optional memory-locking mode;
  [Assets.rc](Assets.rc) supplies Windows executable resources.

The runtime's configuration and web roots come from the task context and can be
overridden by command-line options. See the
[configuration guide](../../etc/README.md) before selecting a vehicle profile.
