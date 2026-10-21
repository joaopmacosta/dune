# Source architecture

DUNE runs independently configured C++ tasks inside an onboard process.
Tasks exchange typed IMC messages through the local message bus; transports
connect that bus to other systems, including Neptus. A dependency between tasks
is usually a message or configured entity name, not a direct C++ call.

## Runtime flow

1. [Main](Main/README.md) parses options and starts the core daemon.
2. [DUNE](DUNE/README.md) provides task management, parameters, entities,
   IMC serialization, message delivery and portable platform services.
3. [Configuration](../etc/README.md) chooses task instances and execution profiles.
   A task being compiled does not mean it is enabled in a running vehicle.
4. Drivers and simulators publish observations; navigation estimates vehicle
   state; maneuvers request motion; controllers produce actuator commands.
   Supervisors and monitors oversee execution and report faults.

## Task families

| Section | Responsibility and main neighbors |
| --- | --- |
| [Sensors](Sensors/README.md) | Device observations consumed by navigation, monitors and logging. |
| [Navigation](Navigation/README.md) | State estimation from sensor messages, consumed by maneuvers and control. |
| [Plan](Plan/README.md) | Plan storage, generation and execution, connected to maneuver execution and vehicle supervision. |
| [Maneuver](Maneuver/README.md) | Maneuver execution and guidance, connected to plan commands, state estimates and control loops. |
| [Control](Control/README.md) | Vehicle guidance/control and actuator allocation; some UAV tasks bridge external autopilots. |
| [Actuators](Actuators/README.md) | Hardware command drivers below control and payload functions. |
| [Autonomy](Autonomy/README.md) | Event/text-triggered actions and higher-level decision-making integration. |
| [Supervisors](Supervisors/README.md) | Vehicle, entity, power and contingency coordination. |
| [Monitors](Monitors/README.md) | Health, limits and environmental checks that inform supervision and operators. |
| [Power](Power/README.md) | Board-specific power switching and telemetry beneath power supervision. |
| [Transports](Transports/README.md) | Message communication, discovery, recording, replay and remote services. |
| [Simulators](Simulators/README.md) | Synthetic vehicle/sensor/environment messages for simulation profiles. |
| [Vision](Vision/README.md) | Camera acquisition and triggering, with device-specific dependencies. |
| [UserInterfaces](UserInterfaces/README.md) | Onboard buttons, indicators and displays driven by runtime state. |

Family guides catalog their individual tasks. Core section guides describe
reusable libraries; do not confuse, for example, the
[navigation library](DUNE/Navigation/README.md) with
[navigation tasks](Navigation/README.md).

## Finding an implementation

Task directories contain `Task.cpp` and `Task.cmake`. The latter declares build
requirements; the former defines parameters, message bindings, handlers and
lifecycle callbacks. Names such as `Sensors.GPS` map to namespace/directory
paths; configuration can create multiple instances with a `/name` suffix.
See [task registration](../cmake/Tasks.cmake) and the
[task library](DUNE/Tasks/README.md) for the shared machinery.
