# Database

Small SQLite-backed connection and statement interfaces. [`Connection`](Connection.hpp)
opens in-memory or file databases and provides execution and transaction
operations; [`Statement`](Statement.hpp) represents prepared statements.
[`Database.hpp`](../Database.hpp) is the section umbrella.

`Connection.hpp` exposes SQLite's `sqlite3` handle type, and `Connection.cpp`
implements the native connection operations. One nearby consumer is
[`SituationalAwareness/ENCManager`](../SituationalAwareness/ENCManager.hpp),
which reads nautical depth data through this API. Method contracts and error
reporting are documented in the headers.
