# Supervisors

Vehicle lifecycle, recovery and resource coordination. Supervisors connect
[Plan](../Plan/README.md), [Maneuver](../Maneuver/README.md),
[Monitors](../Monitors/README.md), [Power](../Power/README.md) and communication
services without replacing their device-level implementations.

`Autonaut`, `AUV/Assist`, `AUV/LostComms`, `Entities`, `StratoPIWatchdog`,
`UAV/LostComms` and `Vehicle` derive from `Tasks::Periodic`; the other tasks
derive from `Tasks::Task`. Configured system/entity names and power-channel
names determine which resources are supervised.

| Task | Purpose and important interfaces |
| --- | --- |
| [Autonaut](Autonaut/Task.cpp) | Registers arm/disarm `RemoteActions` and generates/starts/stops service loiter plans through `PlanControl`, using navigation and vehicle/plan state. |
| [AUV/Assist](AUV/Assist/Task.cpp) | Detects unsuccessful surfacing/ascent from depth-rate, RPM, medium, GPS and communications context; requests a dislodge behavior through `PlanGeneration`. |
| [AUV/LostComms](AUV/LostComms/Task.cpp) | Monitors heartbeat, medium and vehicle/plan state; manages a stored lost-communications plan and starts it through `PlanControl` after the configured timeout. |
| [Delegator](Delegator/Task.cpp) | Represents a remote slave task locally: coordinates power and activation, queries/forwards entity information and parameters, and mirrors slave health/activation feedback. |
| [Entities](Entities/Task.cpp) | Sets configured entities' activation parameters according to idle, plan-execution or remote-operation `VehicleState`. Unlike Monitors/Entities, this task changes activation policy. |
| [Grounding](Grounding/Task.cpp) | Checks GPS surroundings and commanded `DesiredPath` against a configured nautical-chart database; publishes depth/static-obstacle `ENCAwareness` used by Control/Path/CAS. C++17 build rule. |
| [Power](Power/Task.cpp) | Coordinates system/slave shutdown and abort sequences from `PowerOperation`/`PowerChannelControl`, including configured host commands and the main power channel. |
| [PowerManager](PowerManager/Task.cpp) | Controls the `Radio` power channel using `VehicleMedium`, `Rpm` and `PowerChannelState`: suppresses radio power while moving underwater and restores it at the surface or after stopped-RPM confirmation. |
| [Reporter](Reporter/Task.cpp) | Schedules `ReportControl` requests for configured acoustic, radio and SMS reporting; client/dispatcher helpers coordinate tickets with reporting tasks. Does not itself operate a modem. |
| [SlaveCPU](SlaveCPU/Task.cpp) | Powers a named slave on during activation and waits for a fresh, synchronized `Heartbeat`; deactivation optionally requests graceful `PowerOperation` shutdown before switching its channel off. |
| [StarLink](StarLink/Task.cpp) | Cycles a configured satellite-radio power channel on/off according to timing parameters, issuing `PowerChannelControl`. |
| [StratoPIWatchdog](StratoPIWatchdog/Task.cpp) | Drives the Strato Pi hardware watchdog GPIO heartbeat/enable pins and reports a timeout input as entity failure. |
| [UAV/LostComms](UAV/LostComms/Task.cpp) | Uses heartbeat and current plan/medium context to retrieve a configured fallback plan through `PlanDB` and start it with `PlanControl` on communications loss. |
| [Vehicle](Vehicle/Task.cpp) | Central vehicle-mode/maneuver supervisor. Consumes `VehicleCommand`, `Abort`, control-loop and entity-monitoring feedback; starts/stops maneuvers, publishes `VehicleState`/`Aborted` and coordinates safety/calibration transitions. |
