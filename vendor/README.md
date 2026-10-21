# Bundled dependencies

[libraries/](libraries/) contains third-party code integrated with DUNE's build:
compression (bzip2, zlib, LZ4), hashing, SQLite, JPEG, Eigen, JSON, MAVLink, AIS,
geomagnetic-model support, and platform/device helpers such as pthreads-win32,
RTIMULib and TLSF. [matlab/](matlab/) holds bundled MATLAB tooling.

These are dependencies of first-party core/task modules, not additional DUNE
task families. The root [CMakeLists.txt](../CMakeLists.txt) and
[library configuration fragments](../cmake/Libraries/) determine which
implementations are built/used; inclusion here does not mean every dependency
is active in every build.

Keep upstream licenses and documentation with their code. Existing third-party
guides, such as [RTIMULib's README](libraries/RTIMULib/README.md), remain the
reference for those libraries; the DUNE module guides explain first-party
integration rather than duplicating vendor documentation.
