# Monitors

State assessment, fault detection and reporting. Inputs come chiefly from
[Sensors](../Sensors/README.md), [Navigation](../Navigation/README.md),
[Actuators](../Actuators/README.md) and [Plan](../Plan/README.md).
Entity health feeds [Supervisors](../Supervisors/README.md); reports normally
use [Transports](../Transports/README.md) rather than opening a modem directly.
Some monitors also take corrective actions, so they are not all passive.

Tasks derive from `Tasks::Task` unless noted. `Emergency`, `FuelLevel`,
`MantaFuelLevel`, `Medium`, `OperationalLimits` and `Uptime` are periodic;
`NavigationLight` instead uses `Monitors::AISProximity` for target proximity.

| Task | Purpose and important interfaces |
| --- | --- |
| [Boot](Boot/Task.cpp) | Sends delayed boot notifications through `TransmissionRequest`, including configured text/entity-state reporting. |
| [Clock](Clock/Task.cpp) | Uses valid `GpsFix` UTC samples to synchronize the system clock; publishes `ClockControl`, requests logging start, and optionally invokes a hardware-clock synchronization command. |
| [Collisions](Collisions/Task.cpp) | Detects anomalous acceleration using state, RPM, braking and medium context; publishes `Collision` and reports entity health. |
| [CPUScaling](CPUScaling/Task.cpp) | Applies configured host CPU governor/frequency settings using Linux commands/sysfs; no vehicle sensor or control-message interface. |
| [Emergency](Emergency/Task.cpp) | Builds emergency/lost-communications reports from abort, heartbeat, vehicle/plan, position and fuel inputs; sends configured SMS/satellite notifications through `TransmissionRequest`. |
| [Entities](Entities/Task.cpp) | Aggregates watched `EntityState` messages under `MonitorEntityState` requests and publishes `EntityMonitoringState` for vehicle supervision. |
| [FuelLevel](FuelLevel/Task.cpp) | Estimates battery `FuelLevel` from voltage/current/temperature and vehicle/entity activity; configured battery models and source entity labels determine the estimate. |
| [HostResources](HostResources/Task.cpp) | Linux host/process resource accounting; publishes `CpuUsage` and `RamUsage` and reports resource-related entity health. Linux-only build rule. |
| [Log](Log/Task.cpp) | Enforces logging-duration/storage policy using `LoggingControl` and `StorageUsage`; requests log start/stop and applies the configured storage-limit procedure. |
| [MantaFuelLevel](MantaFuelLevel/Task.cpp) | Manta battery monitoring using supply voltage/charging current; publishes `FuelLevel`, displays warnings via `LcdControl` and can request power-down. |
| [Medium](Medium/Task.cpp) | Determines `VehicleMedium` from water-property, GPS, navigation and airspeed inputs, including missing-data/unknown-medium handling. |
| [NavError](NavError/Task.cpp) | Measures position discrepancy on GPS reacquisition and traveled-distance/error ratio; emits `Distance` diagnostics using path, GPS, medium and vehicle context. |
| [NavigationLight](NavigationLight/Task.cpp) | Changes a configured light entity parameter according to AIS target proximity; uses `EntityParameters`/`SetEntityParameters` and sends satellite change notifications. |
| [OperationalLimits](OperationalLimits/Task.cpp) | Stores/replies to `OperationalLimits`, persists updates with `CacheControl`, and checks `EstimatedState` against configured depth, altitude, speed, vertical-rate and area limits. Violations affect entity health. |
| [Payload](Payload/Task.cpp) | Watches configurable payload message streams/timeouts and forwards selected samples over satellite using `TransmissionRequest`; message types are selected at runtime. |
| [Servos](Servos/Task.cpp) | Compares `SetServoPosition`, measured `ServoPosition` and `Current` to detect position/current faults through entity state. |
| [StateReport](StateReport/Task.cpp) | Builds compact state transmissions from navigation, fuel and plan/vehicle context; requests satellite transmission and expands received `StateReport` into `AssetReport`. |
| [Thrusters](Thrusters/Task.cpp) | Detects high-current thruster faults using actuation/current/medium context; can cycle a configured `PowerChannelControl` channel and send satellite notifications. |
| [Uptime](Uptime/Task.cpp) | Reads host uptime and reports it as entity status; no separate uptime telemetry message is dispatched. Linux-only build rule. |
| [ZeroTier](ZeroTier/Task.cpp) | Checks ZeroTier connectivity/service state, optionally restarting the service or modem power channel and sending satellite notifications. Linux-only build rule. |
