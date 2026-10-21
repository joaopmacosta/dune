# Power

Power-board drivers and board-specific sequencing. The usual interface is named
`PowerChannelControl` requests plus `QueryPowerChannelState`/`PowerChannelState`;
[Supervisors](../Supervisors/README.md) coordinates system-level shutdown and
payload power. Measurements feed [Monitors](../Monitors/README.md).
Board channels, ADC conversions and entity names are configuration-specific.

All tasks derive from `Tasks::Task` except `BATMANv2`, which uses
[`Hardware::BasicDeviceDriver`](../DUNE/Hardware/BasicDeviceDriver.cpp) for
connection/power lifecycle. Several serial boards use DUNE's LUCL protocol and
runtime-selected ADC message types, so their outputs are not limited to the
concrete IMC types declared locally.

| Task | Purpose and important interfaces |
| --- | --- |
| [APD](APD/Task.cpp) | Serial power-distribution board: switches/queries channels, publishes configured ADC measurements and firmware `VersionInfo`. |
| [BATMANv2](BATMANv2/Task.cpp) | Battery-monitor driver over a configured device URI; reports pack/cell `Voltage`, `Current`, `Temperature`, optional `FuelLevel` and firmware version. |
| [CPMB](CPMB/Task.cpp) | Serial CPMB power/measurement board; handles channel control/state queries and publishes calibrated ADC measurements, including voltage/current. |
| [CPMBH](CPMBH/Task.cpp) | CPMBH board variant with its own channel/ADC handling; exposes the same power-channel command/state and calibrated measurement interfaces. |
| [CPMBv2](CPMBv2/Task.cpp) | CPMB v2 board variant; controls named power channels and publishes calibrated ADC measurements and version information. |
| [DOAMv1](DOAMv1/Task.cpp) | Serial DOAM board control and monitoring, including power channels, ADC/temperature and watchdog handling; coordinates slave heartbeat, activation parameters and `PowerOperation`. |
| [DOAMv2](DOAMv2/Task.cpp) | Coordinates configured CPU/camera/strobe channels and slave activation from heartbeat/power-operation events; emits `PowerChannelControl` and `SetEntityParameters` rather than implementing the v1 board protocol. |
| [LUEMB](LUEMB/Task.cpp) | Serial LUEMB power/LED board: channel control/state, named LED brightness queries/settings, calibrated measurements and firmware reporting. |
| [MCBv2](MCBv2/Task.cpp) | I2C MCB v2 interface using MCP23017 support; handles power channels and optional `LcdControl`, and reports board-triggered shutdown through `PowerOperation`. |
| [OPCON](OPCON/Task.cpp) | Board-specific supervision reacting to `EntityState`, `RSSI` and power-channel commands; emits power-down/abort operations and entity parameter changes. |
| [PCTLv2](PCTLv2/Task.cpp) | Serial power controller with named channel/LED control and queries, ADC/temperature reporting, watchdog and low-voltage/emergency shutdown signaling via `PowerOperation`. |
| [PSIMAR](PSIMAR/Task.cpp) | ESCC-connected PSU interface: handles channel control/state queries and publishes current and firmware version. |
| [WRPD](WRPD/Task.cpp) | Device-URI power-board interface for ESC, 12 V and 5 V rails; publishes channel state and voltage/current telemetry, and signals shutdown with `PowerOperation`. |
