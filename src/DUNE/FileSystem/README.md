# FileSystem

Path, directory, locking, and filesystem exception helpers. The public
include point is [`FileSystem.hpp`](../FileSystem.hpp); [`Path.hpp`](Path.hpp)
and [`Directory.hpp`](Directory.hpp) cover path manipulation and directory
operations, while [`FileLock.hpp`](FileLock.hpp) provides file locking.

The package wraps platform filesystem behavior for shared DUNE code. The HTTP
request handler is one nearby consumer. Consult the interface comments for
platform-specific semantics and error cases.
