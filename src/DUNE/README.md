# DUNE library

`src/DUNE` is the reusable C++ foundation for DUNE task modules, transports,
drivers, and simulators. [`DUNE.hpp`](DUNE.hpp) is the broad public umbrella;
the section umbrellas are narrower include points. `Config.hpp` and
`Version.hpp` are generated as part of the build, while `Units.hpp` supplies
the shared unit definitions.

## Sections

- [Algorithms](Algorithms/README.md) · [Compression](Compression/README.md) · [Concurrency](Concurrency/README.md)
- [Control](Control/README.md) · [Coordinates](Coordinates/README.md) · [Database](Database/README.md)
- [Entities](Entities/README.md) · [FileSystem](FileSystem/README.md) · [Hardware](Hardware/README.md)
- [IMC](IMC/README.md) · [IO](IO/README.md) · [Maneuvers](Maneuvers/README.md)
- [Math](Math/README.md) · [Media](Media/README.md) · [Monitors](Monitors/README.md)
- [Navigation](Navigation/README.md) · [Network](Network/README.md) · [Parsers](Parsers/README.md)
- [Plans](Plans/README.md) · [Power](Power/README.md) · [Simulation](Simulation/README.md)
- [SituationalAwareness](SituationalAwareness/README.md) · [Status](Status/README.md) · [Streams](Streams/README.md)
- [System](System/README.md) · [Tasks](Tasks/README.md) · [Time](Time/README.md) · [Utils](Utils/README.md)

Use the API comments in the linked headers for method-level contracts; these
pages describe the role of each package and its connections rather than
restate those comments. `Simulation` is a direct-include package and is not
included by `DUNE.hpp`.
