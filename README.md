DUNE: Unified Navigation Environment
======================================

DUNE: Unified Navigation Environment is a runtime environment for unmanned systems on-board software. It is used to write generic embedded software at the heart of the system, e.g. code or control, navigation, communication, sensor and actuator access, etc. It provides an operating-system and architecture independent platform abstraction layer, written in C++, enhancing portability among different CPU architectures and operating systems.

## Reading the code

Start with the [source architecture and module map](src/README.md). Each task
family and core-library section has a short guide explaining its responsibility
and its connections to neighboring modules. These guides complement, rather
than replace, the existing Doxygen comments in the C++ sources.

Supporting sections:

* [Runtime configuration](etc/README.md) selects tasks, parameters and profiles.
* [Build system](cmake/README.md) detects platforms and optional dependencies.
* [Programs and development tools](programs/README.md) contains utilities,
  generators and tests.
* [Web interface](www/README.md) displays runtime state through the HTTP task.
* [Reference documentation](doc/README.md) explains the existing API-doc setup.
* [Container environment](docker/README.md), [translations](i18n/README.md),
  [assets](assets/README.md) and [bundled dependencies](vendor/README.md)
  support building, packaging and operating the system.

[![Build Status](https://travis-ci.org/LSTS/dune.svg?branch=master)](https://travis-ci.org/LSTS/dune)
[![Build status](https://ci.appveyor.com/api/projects/status/tdcdgyf408u4y0ng?svg=true)](https://ci.appveyor.com/project/zepinto/dune) 
