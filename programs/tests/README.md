# Core-library tests

Each `test_*.cpp` is a standalone test program; [Test.hpp](Test.hpp) provides the
shared assertion/reporting helper. Coverage includes IMC serialization,
coordinates and mathematics, algorithms/codecs, filesystem/network services,
timing and concurrency, database access, and host-resource behavior.

The root [CMakeLists.txt](../../CMakeLists.txt) recursively registers these
programs with CTest only when configured with `-DTESTS=1`. They link to
[dune-core](../../src/DUNE/README.md), not the complete vehicle task graph.

```sh
cmake -S . -B build -DTESTS=1
cmake --build build --parallel 4
ctest --test-dir build --output-on-failure
```

Run from the repository root. Use CTest's `-R` selector for a named test.
Passing these tests is not equivalent to validating a hardware configuration
or every onboard control/maneuver behavior. The
[IMC generator](../generators/README.md) also maintains generated IMC tests here.
