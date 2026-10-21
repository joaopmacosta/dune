# Onboard web interface

[index.html](index.html) and [index.js](index.js) implement DUNE's browser
dashboard. The page polls runtime messages and logbook entries, renders
vehicle/task/sensor state, and exposes power-channel actions.

* [js/HTTP.js](js/HTTP.js) supplies browser HTTP requests.
* [js/Sections.js](js/Sections.js), [js/BasicSection.js](js/BasicSection.js) and
  the other section widgets render state, sensors, announcements and logs.
* [js/Power.js](js/Power.js) invokes `dune/power/channel/...` endpoints.
* [css/](css/) and [images/](images/) provide presentation resources.

The backend is the [HTTP transport task](../src/Transports/HTTP/Task.cpp),
which connects HTTP requests to runtime data/actions. This is not a standalone
static website: loading the HTML without its DUNE backend cannot supply live
`dune/state/messages.js` or `dune/state/logbook.js` responses.
The [main executable](../src/Main/README.md) supports overriding the web root;
the root build installs this directory alongside runtime configuration.

Power controls can affect real hardware. Treat access to the dashboard as
operational access, not simply read-only monitoring.
