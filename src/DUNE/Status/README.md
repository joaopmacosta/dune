# Status

Defines the shared status-code and status-message vocabulary.
[`Codes.hpp`](Codes.hpp) contains status identifiers and
[`Messages.hpp`](Messages.hpp) defines their message representations;
[`Status.hpp`](../Status.hpp) is the umbrella.

The task runtime uses status codes when reporting task state, and IMC
messages carry related state across components. The headers are the source
of truth for individual codes and message fields.
