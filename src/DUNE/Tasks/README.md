# Tasks

The runtime for independently scheduled DUNE components.
[`Task`](Task.hpp) is the task base, [`Context`](Context.hpp) provides its
environment, and [`Manager`](Manager.hpp) starts/stops tasks. Factory,
creator, recipient/consumer, parameter-table/parser, and source-filter
interfaces support construction, configuration, and message delivery.
[`Tasks.hpp`](../Tasks.hpp) is the public umbrella.

`Task` ties together concurrency, IMC messages, status codes, parsers,
entities, and timing. [`CustomMessageFilters/`](CustomMessageFilters/Factory.hpp)
supplies the entity-state-change and log-book-entry filters, selected through
its factory, within this same dispatch model. Its purpose is covered here
because the package is included by the task umbrella. Method-level lifecycle
and dispatch contracts are documented inline.
