# SituationalAwareness

[`ENCManager`](ENCManager.hpp) queries nautical-chart depth soundings around
a location, along a transect, or within a corridor, and can write selected
soundings to CSV. Its implementation is in [`ENCManager.cpp`](ENCManager.cpp).
The section umbrella is [`SituationalAwareness.hpp`](../SituationalAwareness.hpp).

`ENCManager` uses `Database::Connection` for its chart database; this is the
concrete bridge between this domain helper and the SQLite-backed
[`Database`](../Database/README.md) package. See the header for query methods
and their arguments.
