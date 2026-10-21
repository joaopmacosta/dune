# Command-line utilities

These C++ programs link to [dune-core](../../src/DUNE/README.md) and are included
in the normal build/install. Their argument conventions are defined in each
source file; they are separate tools, not onboard task implementations.

| Tools | Purpose and dependencies |
| --- | --- |
| [dune-sendmsg](dune-sendmsg.cpp), [dune-activate](dune-activate.cpp) | Send IMC messages or activation requests to running systems; use message/network services. |
| [dune-lsfreplay](dune-lsfreplay.cpp), [dune-lsf2jpg](dune-lsf2jpg.cpp) | Replay recorded LSF messages or extract logged image data; relate to runtime logging and IMC serialization. |
| [dune-config-check](dune-config-check.cpp) | Parse INI files and report include/require or missing entity-label problems. It prints per-file results; its exit status alone is not a complete validation verdict. |
| [dune-wgs84](dune-wgs84.cpp) | Coordinate calculations using the core WGS84 helpers. |
| [dune-md5](dune-md5.cpp), [dune-sha256](dune-sha256.cpp) | Calculate hashes using core algorithm implementations. |
| [dune-lucl-talk](dune-lucl-talk.cpp), [dune-lucb](dune-lucb.cpp), [dune-uctk](dune-uctk.cpp), [dune-stm](dune-stm.cpp) | Interact with embedded controllers through the corresponding hardware/protocol helpers. |

Message/activation and controller tools can affect running hardware. Inspect
their destination/device arguments before use. See the
[configuration guide](../../etc/README.md) and
[transport tasks](../../src/Transports/README.md) for their runtime neighbors.
