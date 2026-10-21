# Entities

Entity helpers attach identity and state to task-owned components.
[`BasicEntity`](BasicEntity.hpp) handles entity information queries, while
[`StatefulEntity`](StatefulEntity.hpp) adds state handling;
`ParameterizedEntity` and `EntityUtils` support parameterized entity setup.
[`Entities.hpp`](../Entities.hpp) collects the public interfaces.

The classes bind and dispatch IMC messages through `Tasks::Recipient` and
`Tasks::Context`; the task runtime owns and uses these helpers. See the header
comments for binding and lifecycle details.
