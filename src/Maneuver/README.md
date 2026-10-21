# Maneuver

Maneuver executors translate mission instructions into references for
[Control](../Control/README.md). [Plan/Engine](../Plan/Engine/Task.cpp) requests
execution through the [vehicle supervisor](../Supervisors/Vehicle/Task.cpp).
Most tasks derive from [DUNE::Maneuvers::Maneuver](../DUNE/Maneuvers/Maneuver.cpp):
the base registers supported maneuvers, handles `StopManeuver` and
`PathControlState`, publishes `ManeuverControlState`, and manages `ControlLoops`.
Navigation feedback normally arrives as `EstimatedState`.

| Task | Purpose and important interfaces |
| --- | --- |
| [CommsRelay](CommsRelay/Task.cpp) | Executes `CommsRelay` by positioning between two announced systems; tracks their `Announce` positions and issues `DesiredPath`, including waiting loiters. |
| [CompassCalibration](CompassCalibration/Task.cpp) | Executes a calibration loiter with optional pitch variation. Uses state, attitude and magnetic samples, publishes `DesiredPath`/`DesiredPitch` and magnetic calibration results. |
| [CoverArea](CoverArea/Task.cpp) | Converts a `CoverArea` polygon into coverage rows and executes them with `DesiredPath`, advancing on path-controller feedback. |
| [FlyByCamera](FlyByCamera/Task.cpp) | Disabled prototype that sets `DesiredHeading` to the received local yaw plus a fixed offset; it is not an image-based tracking pipeline. |
| [FollowReference/AUV](FollowReference/AUV/Task.cpp) | Executes `FollowReference` using an authorized source's `Reference` position/speed/Z requests. Issues `DesiredPath` and reports `FollowRefState`, with arrival/loiter and source-timeout handling. |
| [FollowReference/UAV](FollowReference/UAV/Task.cpp) | Aircraft reference-following variant with default flight speed/height and loiter behavior; publishes `DesiredPath`/`FollowRefState` and detects source timeouts. |
| [FollowSystem](FollowSystem/Task.cpp) | Follows another system from `Announce` or `RemoteState` positions, issuing updated `DesiredPath` and waiting loiters. |
| [FollowTarget](FollowTarget/Task.cpp) | Disabled, incomplete target-following prototype. Reads `Target`, `EstimatedState` and stream velocity and computes circular-path geometry, but does not dispatch flight-control references. |
| [FollowTrajectory](FollowTrajectory/Task.cpp) | Extends `Maneuvers::FollowTrajectory` to track timed waypoints, adjusting `DesiredSpeed` for arrival timing; the base issues path references. |
| [Multiplexer](Multiplexer/Task.cpp) | Hosts local handlers for Idle, Goto, Launch, Loiter, StationKeeping, YoYo, Rows, FollowPath, Elevator, PopUp, Dislodge, ScheduledGoto, Takeoff, Land, Drop, Sample and StationKeepingExtended. Shares maneuver lifecycle/path feedback; handlers are helper classes, not separate tasks. |
| [RowsCoverage](RowsCoverage/Task.cpp) | Executes `RowsCoverage` with row spacing derived from coverage settings and filtered altitude; publishes `DesiredPath` for successive survey legs. |
| [Teleoperation](Teleoperation/Task.cpp) | Enables the teleoperation control loop for `Teleoperation` and finishes on `TeleoperationDone`; actual operator mixing belongs to Control's remote-operation tasks. |

## Formation tasks

These include specialized maneuver bases and standalone periodic coordinators.
`Coordinator`, `FormCollAvoid` and `Test` are disabled in their `Task.cmake`;
their presence here is not a claim that they are enabled on a vehicle.

| Task | Purpose and important interfaces |
| --- | --- |
| [VehicleFormation/Coordinator](VehicleFormation/Coordinator/Task.cpp) | `Tasks::Periodic` formation coordinator: handles `Formation`, leader/vehicle state, plan requests and entity activation; relays selected commands/state under configured system aliases. |
| [VehicleFormation/FormCollAvoid](VehicleFormation/FormCollAvoid/Task.cpp) | `Tasks::Periodic` formation controller with collision-avoidance calculations. Uses formation/leader and vehicle state to issue roll/speed/Z commands and formation monitoring/evaluation messages. |
| [VehicleFormation/SMC](VehicleFormation/SMC/Task.cpp) | Extends `Maneuvers::VehicleFormation` with sliding-mode formation control; base handles `EstimatedState`/`RemoteState` and path/speed dispatch. |
| [VehicleFormation/Swarm](VehicleFormation/Swarm/Task.cpp) | Extends `Maneuvers::BasicSwarm` with Wi-Fi/acoustic synchronization and `UamRxFrame` acknowledgments; launches a fallback plan through `PlanControl` when the leader is lost. |
| [VehicleFormation/Test](VehicleFormation/Test/Task.cpp) | `Tasks::Periodic` diagnostic scaffold: logs received `EstimatedState` and exposes formation parameters; its periodic body does not implement a formation controller. |
