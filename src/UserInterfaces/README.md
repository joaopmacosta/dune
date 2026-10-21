# UserInterfaces

Local buttons, displays and status indicators. All tasks derive from
`Tasks::Task`. Hardware adapters publish operator events or consume display/state
messages; `MantaPanel` connects those primitives to communications and shutdown.
Device paths, GPIOs, button IDs and display/power names must match the platform.

| Task | Purpose and important interfaces |
| --- | --- |
| [Buttons](Buttons/Task.cpp) | Reads configured buttons from a `Hardware::Buttons` event device and publishes `ButtonEvent`. |
| [ButtonsRPI](ButtonsRPI/Task.cpp) | Polls Raspberry Pi GPIO inputs, using configured GPIO character-device or legacy access, and publishes press/release `ButtonEvent`. |
| [LCD](LCD/Task.cpp) | I2C character-display driver consuming `LcdControl`; manages backlight GPIO/power and idle timeout, optionally issuing `PowerChannelControl`. |
| [LEDs](LEDs/Task.cpp) | Maps `VehicleState`, selected `EntityState` failures and `PowerOperation` to blink patterns through GPIO, parallel-port, emulator or message backends. |
| [MantaPanel](MantaPanel/Task.cpp) | Button-driven local operations panel. Uses `ButtonEvent` to select systems, range/abort acoustically and manage shutdown; drives `LcdControl` and uses acoustic/transmission feedback and power commands. |

[Power/MCBv2](../Power/MCBv2/Task.cpp) also has board-specific LCD handling.
Communication operations are implemented by [Transports](../Transports/README.md),
and vehicle/system power sequencing by
[Supervisors](../Supervisors/README.md), rather than by the generic button driver.
