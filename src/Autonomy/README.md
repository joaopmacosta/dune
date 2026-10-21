# Autonomy

High-level event and operator integrations, above
[plan execution](../Plan/README.md) and [maneuvers](../Maneuver/README.md).
All three tasks derive from `Tasks::Task`. They issue IMC requests rather than
direct actuator commands; plan and vehicle supervisors still handle execution.

| Task | Purpose and important interfaces |
| --- | --- |
| [OnEvent](OnEvent/Task.cpp) | Samples a configured IMC message field, applying thresholds and consecutive-sample confirmation. A confirmed event can trigger `Abort`, `PlanGeneration` or acoustic notification through `AcousticOperation`; message type and field are configuration-driven. |
| [TextActions](TextActions/Task.cpp) | Interprets `TextMessage` commands to abort, start/resume or generate plans, change parameters and request reports/restarts. Tracks vehicle, plan, position and fuel state; routes replies through `TransmissionRequest` according to the origin. |
| [TREX](TREX/Task.cpp) | Integrates an external T-REX executive using `TrexOperation`/`TrexToken`. Creates/stops a `FollowReference` plan through `PlanControl`, watches executive heartbeats and vehicle/plan state, publishes `VehicleLinks`, and manages the auxiliary CPU power channel. Its build rule allows Linux and Darwin. |

Text and executive traffic can arrive through
[Transports](../Transports/README.md). `TREX`'s generated reference-following
plan is executed by the corresponding Maneuver task, not by this integration.
