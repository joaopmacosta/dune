# Control

Reusable control components rather than complete task implementations.
[`Control.hpp`](../Control.hpp) exposes path and proxy-path controllers,
basic autopilot/remote-operation helpers, `DiscretePID`, vehicle models,
altitude and bottom tracking, and estimator/filter utilities. See the
individual headers for algorithm inputs and state conventions.

These components are building blocks for vehicle task and maneuver code;
their interfaces connect to DUNE math and message/task types where needed.
The umbrella is the convenient include point when using several control
helpers together.
