# Navigation

Navigation algorithms and sensor helpers: basic navigation state, beam
filtering, compass calibration, Kalman filtering, ranging, stream estimation,
and USBL utilities. [`Navigation.hpp`](../Navigation.hpp) is the umbrella.

The implementations depend on shared math types and connect to sensor/state
messages where needed. These headers are algorithm components for navigation
tasks rather than task implementations; consult their Doxygen comments for
state and measurement assumptions.
