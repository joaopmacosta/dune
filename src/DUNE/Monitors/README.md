# Monitors

Monitoring helpers for AIS proximity, delayed triggers, medium state, motor
and servo current/position, and vertical limits. [`Monitors.hpp`](../Monitors.hpp)
collects the public headers. These components evaluate or track conditions
for higher-level task code; for example, the servo monitor task consumes the
package.

Use the corresponding monitor header for thresholds, state, and message
contracts; this overview only maps the component family.
