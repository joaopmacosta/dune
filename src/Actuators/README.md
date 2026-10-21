# Actuators

Hardware-facing motor, servo, lighting and camera-position drivers. Vehicle
controllers in [Control](../Control/README.md) produce their commands; sensor
feedback is also used by [Monitors](../Monitors/README.md). IDs, entity labels,
channel names and calibration must match the selected vehicle configuration.
The links below lead to each task's implementation.

Most drivers derive directly from `Tasks::Task`; `AMC`, `Broom`, `FLIRPTU`,
`MicroCamD`, `SingleSIMCT01` and `Torqeedo` use `Tasks::Periodic` for polling.
These tasks operate hardware rather than deciding vehicle-level motion.

| Task | Purpose and important interfaces |
| --- | --- |
| [AMC](AMC/Task.cpp) | Serial motor controller: converts `SetThrusterActuation` to motor commands and publishes `Rpm`, `Current`, `Voltage` and `Temperature` per motor. |
| [Broom](Broom/Task.cpp) | Serial Broom motor driver with selectable thrust-control mode. Accepts `SetThrusterActuation` and `DesiredSpeed`; reports motor/controller electrical, temperature and RPM feedback. |
| [Dice](Dice/Task.cpp) | Converts port/starboard `SetThrusterActuation` into `SetPWM`; optionally estimates `Rpm` from configured motor models, duty cycle and `Voltage`. Connects to a PWM output task rather than writing PWM hardware itself. |
| [FLIRPTU](FLIRPTU/Task.cpp) | Controls a serial FLIR pan/tilt unit from `SetControlSurfaceDeflection` IDs `p` and `t`; pairs with the antenna tracker in Control. |
| [LED4R](LED4R/Task.cpp) | Serial LED driver handling named `SetLedBrightness`/`QueryLedBrightness`, returning `LedBrightness` and publishing supply measurements. |
| [MCD4R](MCD4R/Task.cpp) | Serial MCD4R board driver with laser brightness and remote-action controls; publishes channel `Voltage`/`Current` and registers supported remote actions. |
| [MicroCamD](MicroCamD/Task.cpp) | Serial camera pan/tilt/zoom control from `RemoteActions` and `CameraZoom`; publishes camera orientation as `EulerAngles`. |
| [PWM](PWM/Task.cpp) | Direct I/O-port PWM servo output: accepts `SetServoPosition` and reports commanded `ServoPosition`. Linux-only build rule. |
| [RaspberryPiPWM](RaspberryPiPWM/Task.cpp) | Maps `SetPWM` IDs to configured Linux PWM chip/channel outputs. Requires C++17 according to its build rule. |
| [SCRTv4](SCRTv4/Task.cpp) | LUCL serial servo controller: consumes `SetServoPosition`, reports `ServoPosition` and servo `Current`, and exposes device diagnostics. |
| [SIMCT01](SIMCT01/Task.cpp) | Drives multiple serial SIMCT01 thrusters using configured logical/hardware addresses; consumes `SetThrusterActuation` and publishes per-thruster `Current`/`Rpm`. |
| [SingleSIMCT01](SingleSIMCT01/Task.cpp) | Polling driver for one serial SIMCT01 thruster; selects its configured actuation ID and reports `Current`/`Rpm`. |
| [Torqeedo](Torqeedo/Task.cpp) | CAN interface for Torqeedo propulsion and power channels. Consumes thruster and power-channel commands; reports channel state, RPM, battery fuel and electrical/temperature measurements. |
