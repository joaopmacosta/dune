# IO

Defines the handle abstraction and readiness polling used by system I/O.
[`Handle`](Handle.hpp) is the common native-handle interface;
[`HandleBuilder`](HandleBuilder.hpp) constructs handles and [`Poll`](Poll.hpp)
waits for I/O readiness. [`IO.hpp`](../IO.hpp) includes these entry points.

Network TCP/UDP sockets build on `IO::Handle`, so this package is below the
network layer rather than a competing socket API. The public headers document
the readiness and handle contracts.
