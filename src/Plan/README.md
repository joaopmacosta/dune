# Plan

Mission storage, generation and execution are separate `Tasks::Task` services.
The principal chain is `PlanGeneration` → `PlanSpecification`/`PlanDB` →
`PlanControl` → `VehicleCommand` → [maneuver execution](../Maneuver/README.md).
The [vehicle supervisor](../Supervisors/Vehicle/Task.cpp) owns vehicle mode and
maneuver-command acceptance; the engine monitors its replies.

| Task | Purpose and important interfaces |
| --- | --- |
| [DB](DB/Task.cpp) | Persistent plan database using DUNE's SQL/SQLite support. Handles `PlanDB` storage/query/delete operations and supplies stored specifications to `PlanControl` start requests; also observes `PowerOperation`. |
| [Engine](Engine/Task.cpp) | Executes `PlanSpecification` maneuver graphs and actions. Consumes `PlanControl`, vehicle/maneuver and entity activation feedback plus state/fuel; issues `VehicleCommand`/logging requests and publishes `PlanControlState`, plan specifications/statistics and request replies. |
| [Generator](Generator/Task.cpp) | Expands configured `PlanGeneration` templates into maneuver graphs, using current position, announcements and LBL configuration where needed. Stores generated plans with `PlanDB` and optionally starts them with `PlanControl`. |

`RegisterManeuver` tells Engine which executors are available. Entity activation
is part of plan actions and calibration, so sensor/payload activation failures
can affect execution. [Autonomy](../Autonomy/README.md) supplies event/text/T-REX
requests, while [Transports](../Transports/README.md) carries remote requests and
plan/state reports.
