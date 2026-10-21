# Programs and development tools

Unlike the runtime [task families](../src/README.md), these are standalone
executables or build/development scripts. Most C++ tools reuse
[dune-core](../src/DUNE/README.md), including IMC parsing, coordinates, logs and
hardware interfaces.

| Section | Purpose and connections |
| --- | --- |
| [utils](utils/README.md) | Installed command-line tools for messages, logs, configuration and hardware protocols. |
| [tests](tests/README.md) | Core-library tests registered with CTest when `TESTS` is enabled. |
| [generators](generators/README.md) | IMC/status source and address-table generation. |
| [scripts](scripts/README.md) | Task scaffolding, maintenance, documentation and operational helpers. |
| [gsmux](gsmux/README.md) | GSMUX capture and tabular conversion programs. |
| [video-client](video-client/README.md) | Optional Qt-based video receiver/display. |

The C++ files directly in this directory are specialist/offline tools:
log filtering and replay analysis, travel distance and energy calculations,
calibration, USBL evaluation, photo triggering and plan/row experiments.
Inspect each program's `main` and argument checks before use; they do not share
one command-line interface.

The root [build file](../CMakeLists.txt) creates targets named after these
top-level source files but marks them `EXCLUDE_FROM_ALL`; build one explicitly
with `cmake --build build --target <name>`. Utilities in `utils/` are part of
the normal build, while GSMUX and video-client have separate build fragments.
