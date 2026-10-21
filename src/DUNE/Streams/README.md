# Streams

Terminal output and multiplexed output helpers. [`Terminal.hpp`](Terminal.hpp)
provides the terminal interface; [`OutputMultiplexer.hpp`](OutputMultiplexer.hpp)
and `OutputMultiplexerBuffer` combine or buffer output destinations. Note that
[`Streams.hpp`](../Streams.hpp) includes `Terminal.hpp`; include the
multiplexer headers directly when needed.

The HTTP server uses the stream utilities for response/output handling.
Implementation details and platform behavior are documented in the relevant
headers.
