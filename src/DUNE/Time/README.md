# Time

Clock and elapsed-time utilities: `Clock`, delays and periodic delays,
broken-down time, deltas, counters, formatting, and conversion helpers.
[`Time.hpp`](../Time.hpp) collects the core APIs; [`Format.hpp`](Format.hpp)
is available as a direct include for formatting.

Time counters and delays are used by task scheduling and protocol code; the
interfaces document clock sources, units, and wait behavior. The package
provides primitives rather than a separate timer service.
