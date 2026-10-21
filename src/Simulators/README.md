# Simulators

Substitutes for hardware and vehicle/environment models. The usual chain is
actuation → `VSIM` or `UAV` → `SimulatedState` → synthetic sensor messages →
[Navigation](../Navigation/README.md) → [Control](../Control/README.md).
Use matching entity labels and origin configuration instead of running hardware
and simulator publishers indiscriminately on the same inputs.

Tasks derive from `Tasks::Periodic` except event-driven `AIS`, `AcousticModem`,
`CDC3`, `Docking`, `IMU`, `LBL`, `Leaks`, `Reporter` and `Servos`, which derive
from `Tasks::Task`. Not all simulators are isolated: acoustic/AIS communication
uses networking, and the Iridium substitute contacts a configured server.

| Task | Purpose and important interfaces |
| --- | --- |
| [AcousticModem](AcousticModem/Task.cpp) | Networked acoustic-link substitute using UDP multicast `SimAcousticMessage`; consumes `UamTxFrame`/`UamTxRange`, models configured link behavior and returns receive/range/status messages. |
| [AIS](AIS/Task.cpp) | Generates/exchanges vessel `AisInfo` over UDP, with static and moving-vessel reports and GPS context; supplies collision-avoidance traffic without a physical AIS receiver. |
| [CDC3](CDC3/Task.cpp) | Generates synthetic CDC3 acoustic protocol commands/reports as `UamRxFrame`, using `EstimatedState` for position-dependent packets. |
| [CTD](CTD/Task.cpp) | Converts `SimulatedState` into synthetic temperature, conductivity, depth, pressure, salinity and sound-speed measurements with configured randomization. |
| [DepthSensor](DepthSensor/Task.cpp) | Publishes `Depth` from the current `SimulatedState`. |
| [Docking](Docking/Task.cpp) | Models a moving docking target from configured geometry and GPS origin; tests vehicle `SimulatedState` against docking tolerances and logs success, without a dedicated docking-result publication. |
| [DVL](DVL/Task.cpp) | Produces bottom/water velocity measurements (`GroundVelocity`, `WaterVelocity`) from `SimulatedState`. |
| [Environment](Environment/Task.cpp) | Produces bottom/forward `Distance` from simulated position and configured bathymetry/obstacle geometry; uses GPS to establish its origin. |
| [Gaussian](Gaussian/Task.cpp) | Evaluates a spatial Gaussian field at `SimulatedState` and publishes a configured IMC scalar message; not a vehicle-motion model. |
| [GPS](GPS/Task.cpp) | Publishes synthetic `GpsFix` and optional `GroundVelocity`/`EulerAngles`; GPS availability depends on simulated depth, and GPS input establishes the origin. |
| [Heave](Heave/Task.cpp) | Generates periodic sinusoidal vertical `Displacement` from configured wave amplitude/frequency, suitable for the wave estimator. |
| [IMU](IMU/Task.cpp) | Converts `SimulatedState` into attitude, acceleration, angular velocity and inertial increments, with configurable noise/bias. |
| [Iridium](Iridium/Task.cpp) | Accepts `IridiumMsgTx`, posts encoded payloads to a configured HTTP/Ripples endpoint and returns `IridiumTxStatus`, including empty-queue status. It does not implement a physical SBD modem. |
| [LBL](LBL/Task.cpp) | Uses configured beacons, GPS origin and simulated position to generate noisy `LblRange` or `UamRxRange`; can wait for acoustic ranging requests and caches `LblConfig`. |
| [Leaks](Leaks/Task.cpp) | Applies `LeakSimulation` to named reserved entities, reporting normal/failure `EntityState` for leak-monitoring tests. |
| [Motor](Motor/Task.cpp) | Converts `SetThrusterActuation` to filtered synthetic `Rpm`; supplies RPM feedback for controllers without a motor driver. |
| [Reporter](Reporter/Task.cpp) | Encodes `EstimatedState` into simulated acoustic reports and injects them as `UamRxFrame`. |
| [Servos](Servos/Task.cpp) | Converts `SetServoPosition` into `ServoPosition`, optionally limiting angle/rate; uses simulated-state timing for updates. |
| [StreamVelocity](StreamVelocity/Task.cpp) | Generates `EstimatedStreamVelocity` with a configured stream generator, optionally dependent on `SimulatedState`; feeds vehicle and guidance models. |
| [Target](Target/Task.cpp) | Generates a moving `Target` from configured initial coordinates, height and speed limits. |
| [UAV](UAV/Task.cpp) | Aircraft motion model driven by pitch/roll/speed/Z references or actuator commands; initializes from GPS and publishes `SimulatedState`. |
| [UAVAutopilot](UAVAutopilot/Task.cpp) | Converts accepted `SimulatedState` into autopilot-like `EstimatedState`, `IndicatedSpeed`, `TrueSpeed` and stream velocity; GPS supplies origin context. |
| [USBL](USBL/Task.cpp) | Computes noisy USBL range/bearing/elevation from simulated position and transducer geometry, encoding delayed data as `DevDataBinary`; this is not direct `UsblFixExtended` output. |
| [VSIM](VSIM/Task.cpp) | Vehicle dynamics using the local VSIM model library/factory. Consumes servo position, thruster actuation, stream velocity and GPS origin; publishes `SimulatedState` and caches origin changes. |
