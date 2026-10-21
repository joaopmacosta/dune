# Utils

General-purpose support types: byte and circular buffers, byte copying,
FIFO, option parsing, state machines, strings, tuples, XML, and utility
functions. [`Utils.hpp`](../Utils.hpp) is the umbrella; individual headers
document their API contracts.

Nested helpers are also exported by the umbrella: `Actuators/` contains
actuator-related types, including current and RPM models for the T200/T500
families ([`Current/T200`](Actuators/Models/Current/T200.hpp),
[`RPM/T200`](Actuators/Models/RPM/T200.hpp), via `Actuators.hpp`) that map
PWM/current values using voltage-dependent tables;
`Codecs/` contains [coded estimated-state](Codecs/CodedEstimatedState.hpp)
and [reference](Codecs/CodedReference.hpp) messages (via `Codecs.hpp`).
These focused directories are covered here without duplicating their API
documentation. `Utils` is shared by task, transport, and driver code.
