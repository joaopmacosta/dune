# Build configuration

These modules implement the platform/dependency detection used by the root
[CMakeLists.txt](../CMakeLists.txt). They produce configuration headers, select
task implementations and connect source code to system or bundled libraries.

| Section | Role |
| --- | --- |
| [Toolchain.cmake](Toolchain.cmake) | Coordinates compiler, architecture and operating-system checks. |
| [Compiler.cmake](Compiler.cmake), [CXXLibrary.cmake](CXXLibrary.cmake), [Headers.cmake](Headers.cmake), [Functions.cmake](Functions.cmake), [Types.cmake](Types.cmake) | Detect usable compiler features, headers, functions and types for portable core code. |
| [Architecture.cmake](Architecture.cmake), [OperatingSystem.cmake](OperatingSystem.cmake), [SystemLibraries.cmake](SystemLibraries.cmake) | Identify the target and its required platform libraries. |
| [Programs.cmake](Programs.cmake) | Discover build-time tools such as Python and documentation tools. |
| [Libraries/](Libraries/) | Detect or configure optional SDKs and libraries, including camera, image, Qt and other integrations. |
| [Tasks.cmake](Tasks.cmake) | Discover `Task.cmake` files, check `TASK_REQUIRES`, and build enabled tasks statically or as `.ddt` modules. |
| [IMC.cmake](IMC.cmake) | Define explicit specification-download and IMC regeneration targets. |
| [I18N.cmake](I18N.cmake) | Extract, update and compile gettext catalogs. |
| [Version.cmake](Version.cmake) | Generate version information for the core library. |
| [Macros.cmake](Macros.cmake), [MinGW.cmake](MinGW.cmake) | Shared configuration helpers and Windows cross-build support. |

The core compiles from [src/DUNE](../src/DUNE/README.md). Task discovery also
looks for `private*` and `user*` extensions; missing optional SDKs can disable
individual tasks rather than the entire runtime. Check configure output for
the enabled/disabled task lists.

The [Linux CI workflow](../.github/workflows/linux.yml) configures with
`-DDEBUG=1 -DTESTS=1`, builds, and invokes CTest. The standard local shape is:

```sh
cmake -S . -B build -DDEBUG=1 -DTESTS=1
cmake --build build --parallel 4
ctest --test-dir build --output-on-failure
```

Run from the repository root. These commands build/test DUNE; they do not
start vehicle tasks. See [runtime configuration](../etc/README.md).
