# Control

Controllers turn maneuver/path references and navigation feedback into lower-level
references or actuator commands. [Maneuver](../Maneuver/README.md) typically
produces `DesiredPath`; [Navigation](../Navigation/README.md) produces
`EstimatedState`; [Actuators](../Actuators/README.md) consumes the final commands.
These are configurable alternatives, not a requirement to run every controller.

Shared base classes in [DUNE/Control](../DUNE/Control) supply important interfaces:

- `PathController` consumes `DesiredPath`, `EstimatedState`, `ControlLoops`,
  speed/Z references, braking and navigation uncertainty; it publishes
  `PathControlState` and manages control-loop/Z requests.
- `BasicAutopilot` handles state, heading/rate, pitch, Z, velocity and loop
  references for attitude/depth controllers.
- `BasicRemoteOperation` handles `Teleoperation`, `RemoteActions`, action
  registration and loop ownership. Its subclasses implement vehicle mixing.
- `UAV/LOS` uses `ProxyPathController`; other tasks below derive directly from
  `Tasks::Task`, except periodic `AUV/Diving` and `UAV/Height`.

## Surface and underwater vehicles

| Task | Purpose and important interfaces |
| --- | --- |
| [ASV/Conventional/Heading](ASV/Conventional/Heading/Task.cpp) | PID heading control from `DesiredHeading` and `EstimatedState` to rudder `SetServoPosition`, gated by yaw `ControlLoops`. |
| [ASV/Conventional/Speed](ASV/Conventional/Speed/Task.cpp) | Converts `DesiredSpeed` in m/s, RPM or percentage into `SetThrusterActuation`; uses state/RPM feedback and honors `Brake` and speed-loop control. |
| [ASV/Conventional/RemoteOperation](ASV/Conventional/RemoteOperation/Task.cpp) | `BasicRemoteOperation` mixer for one thruster and rudder; also handles remote logging and power-down actions. |
| [ASV/Differential/HeadingAndSpeed](ASV/Differential/HeadingAndSpeed/Task.cpp) | Combines heading and speed control into two thruster commands using `EstimatedState`/`Rpm`; handles `Abort` and loop enablement. |
| [ASV/Differential/RemoteOperation](ASV/Differential/RemoteOperation/Task.cpp) | `BasicRemoteOperation` mixer for differential port/starboard `SetThrusterActuation`. |
| [AUV/Allocator](AUV/Allocator/Task.cpp) | Allocates `DesiredControl` torques to fin `SetServoPosition`, using speed/RPM and servo feedback; reports `AllocatedControlTorques`. Connects attitude controllers to servo drivers. |
| [AUV/Attitude](AUV/Attitude/Task.cpp) | `BasicAutopilot` attitude/depth control producing `DesiredControl` torques and pitch references; allocator performs the subsequent fin conversion. |
| [AUV/Diving](AUV/Diving/Task.cpp) | Supervises dive initiation from depth/altitude, speed, state and RPM; forces `DesiredPitch`/`DesiredSpeed` during the dive-start sequence. |
| [AUV/LMI](AUV/LMI/Task.cpp) | Alternative `BasicAutopilot` using LMI controller gains to produce `DesiredControl` roll/pitch/yaw torques for the allocator. |
| [AUV/Speed](AUV/Speed/Task.cpp) | Speed controller supporting m/s, RPM and percentage references; uses state/RPM feedback and outputs `SetThrusterActuation`, with abort/brake handling. |
| [AUV/RemoteOperation](AUV/RemoteOperation/Task.cpp) | `BasicRemoteOperation` maps operator input to `DesiredSpeed`, `DesiredHeadingRate`, `DesiredPitch` or torque references for downstream AUV controllers. |
| [ROV/Depth](ROV/Depth/Task.cpp) | `BasicAutopilot` vertical controller converting depth/heave references into vertical `SetThrusterActuation`; also publishes `DesiredVelocity` heave references. |
| [ROV/HorizontalPlane](ROV/HorizontalPlane/Task.cpp) | `BasicAutopilot` horizontal motion/yaw controller and thruster allocation; publishes thruster commands plus control/rate diagnostics. |
| [ROV/RemoteOperation](ROV/RemoteOperation/Task.cpp) | `BasicRemoteOperation` ROV mixer with depth/heading references and optional `Distance`-based tracking; supplies direct thruster commands or references to the ROV controllers. |

## Path following and aircraft

| Task | Purpose and important interfaces |
| --- | --- |
| [Path/CAS](Path/CAS/Task.cpp) | `PathController` combining ILOS guidance with model-predictive collision avoidance. Uses `AisInfo`, chart `ENCAwareness`, wind/current and wave inputs to adjust `DesiredHeading`; supports remote enable/disable actions. C++17 build rule. |
| [Path/ILOS](Path/ILOS/Task.cpp) | `PathController` integral line-of-sight guidance producing `DesiredHeading` for yaw control. |
| [Path/LOSnSMC](Path/LOSnSMC/Task.cpp) | `PathController` combining line-of-sight guidance and sliding-mode bank control; uses `IndicatedSpeed` and `EstimatedStreamVelocity`, publishing `DesiredRoll`. |
| [Path/PurePursuit](Path/PurePursuit/Task.cpp) | `PathController` pure-pursuit guidance converting path tracking errors into `DesiredHeading`. |
| [Path/VectorField](Path/VectorField/Task.cpp) | `PathController` vector-field guidance producing `DesiredHeading` for line and loiter tracking. |
| [UAV/Ardupilot](UAV/Ardupilot/Task.cpp) | MAVLink bridge for ArduPilot: sends IMC path/control, takeoff/land and calibration commands; translates autopilot telemetry into navigation, airspeed, power and mode messages plus `PathControlState`. |
| [UAV/PX4](UAV/PX4/Task.cpp) | MAVLink bridge for PX4 mission/path, takeoff/land/loiter and calibration handling. Publishes navigation, mode/arming/VTOL, power and path-progress messages. |
| [UAV/Height](UAV/Height/Task.cpp) | Converts `DesiredZ` and estimated height/airspeed into `DesiredVerticalRate`, managing the vertical-rate loop through `ControlLoops`. |
| [UAV/LOS](UAV/LOS/Task.cpp) | `ProxyPathController` line-of-sight aircraft guidance using `IndicatedSpeed` to produce bank (`DesiredRoll`) references. |
| [UAV/RemoteOperation](UAV/RemoteOperation/Task.cpp) | `BasicRemoteOperation` maps operator actions to `DesiredRoll`, `DesiredSpeed` and `DesiredVerticalRate` for an aircraft controller/autopilot. |

## Actuator adapters

| Task | Purpose and important interfaces |
| --- | --- |
| [Actuators/AntennaTracker](Actuators/AntennaTracker/Task.cpp) | Uses system `EstimatedState` positions and local `EulerAngles` to point an antenna with pan/tilt `SetControlSurfaceDeflection`; the FLIR PTU driver accepts these references. |
| [Actuators/Thruster/PWM](Actuators/Thruster/PWM/Task.cpp) | Converts normalized `SetThrusterActuation` into `SetPWM`, with PWM/current feedback and calibration limits; a hardware PWM driver performs output. |
