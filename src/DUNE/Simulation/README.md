# Simulation

Vehicle simulation helpers. [`UAV.hpp`](UAV.hpp) defines `UAVSimulation`
models with configurable degrees of freedom and vehicle dynamics; the
implementation is in [`UAV.cpp`](UAV.cpp). It builds on DUNE math matrices
and task context.

This package is not included by [`DUNE.hpp`](../DUNE.hpp), so include its
header directly. Simulator and vehicle-formation task code are nearby
consumers. The constructor and state documentation is in `UAV.hpp`.
