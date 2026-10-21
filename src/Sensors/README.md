# Sensors

Hardware acquisition and sensor-side conversion. Outputs feed
[Navigation](../Navigation/README.md), [Monitors](../Monitors/README.md),
[Control](../Control/README.md) and logging in
[Transports](../Transports/README.md). Select the device URI/serial settings,
mounting transform, calibration and entity labels for the actual installation.
Not every task here publishes measurements: some configure devices, export
navigation as NMEA, or aggregate historical data.

Tasks named **BDD** below derive from
[`Hardware::BasicDeviceDriver`](../DUNE/Hardware/BasicDeviceDriver.cpp), which
supplies connection/recovery, activation/power and logging hooks and shared
state/sound-speed inputs. Others derive from `Tasks::Task` unless marked
periodic. The tables list important interfaces, not every health/debug message.

## Position, attitude and motion

| Task | Purpose and important interfaces |
| --- | --- |
| [GPS](GPS/Task.cpp) | **BDD** NMEA receiver using a reader/parser over the configured device URI. Publishes `GpsFix` and supported attitude/angular-rate data; `DevDataText`/`IoEvent` carry reader input/events. |
| [EmulatedGPS](EmulatedGPS/Task.cpp) | Exports fresh `EstimatedState` as configurable NMEA sentences on a serial port, optionally synchronized by `Pulse`; it does not synthesize an IMC GPS measurement. |
| [V104setup](V104setup/Task.cpp) | Serial setup utility that sends the V104 `$JBAUD,19200` command once, then waits; no navigation messages are published. |
| [IFOG](IFOG/Task.cpp) | ESCC-connected inertial unit: applies mounting rotation and publishes `EulerAnglesDelta`/`VelocityDelta` plus firmware information. Linux-only, proprietary build metadata. |
| [LIMU](LIMU/Task.cpp) | **BDD** inertial unit providing angular velocity, acceleration, attitude, magnetic field, increments and temperature; accepts magnetic calibration updates and supports raw diagnostics. |
| [MTi](MTi/Task.cpp) | Serial Xsens MTi acquisition publishing `EulerAngles`, `Acceleration`, `AngularVelocity` and `Temperature`; accepts synchronization `Pulse`. |
| [Microstrain3DMGX1](Microstrain3DMGX1/Task.cpp) | Serial MicroStrain attitude/inertial driver publishing attitude, acceleration, angular rate and magnetic field; applies mounting rotation and magnetic calibration. |
| [Microstrain3DMGX3](Microstrain3DMGX3/Task.cpp) | **BDD** 3DM-GX3 variant publishing attitude/inertial/magnetic measurements and accepting hard-iron calibration updates. |
| [OS4000](OS4000/Task.cpp) | **BDD** compass/inertial driver publishing attitude, acceleration, magnetic field and temperature; handles `DevCalibrationControl` with `DevCalibrationState` progress. |
| [WT901C](WT901C/Task.cpp) | **BDD** UART IMU parser publishing acceleration, angular velocity, magnetic field and attitude through its generic dispatch helper; accepts magnetic hard-iron corrections. |
| [RaspberryPiSenseHatv2](RaspberryPiSenseHatv2/Task.cpp) | Uses RTIMULib and the Sense HAT LED matrix; publishes configured IMU/environment measurements and can consume selected `EulerAngles` for display. |
| [SonTekArgonaut](SonTekArgonaut/Task.cpp) | Serial Doppler velocity acquisition publishing `GroundVelocity`, `WaterVelocity`, beam/bottom `Distance` and `Temperature`, with mounting position/orientation configuration. |
| [WaterLinkedDVL](WaterLinkedDVL/Task.cpp) | **BDD** TCP/UART DVL driver; its local Driver publishes `GroundVelocity` and raw/filtered altitude/beam `Distance`. `VehicleMedium` and operator settings govern acoustics. |

## Sonar and acoustic devices

| Task | Purpose and important interfaces |
| --- | --- |
| [Edgetech2205](Edgetech2205/Task.cpp) | **BDD** network sonar with separate TCP command/data links, subsystem configuration and time synchronization. Writes JSF sonar logs and uses navigation/logging context rather than publishing ordinary `SonarData` pings. |
| [Imagenex837B](Imagenex837B/Task.cpp) | **BDD** multibeam sonar acquisition with 837/83P frame helpers, navigation/logging integration, raw device data and `SonarData` publication. |
| [Imagenex852](Imagenex852/Task.cpp) | **BDD** echo-sounder acquisition publishing `Distance`/`SonarData`; handles medium and `UamTxStatus` context to coordinate acoustic operation. |
| [Imagenex872](Imagenex872/Task.cpp) | **BDD** sidescan driver pinging both channels and publishing `SonarData`, with configured data/balance gain. |
| [Imagenex881A](Imagenex881A/Task.cpp) | Serial scanning sonar with sound-speed updates, power-channel control and configurable scan geometry; publishes `Distance` and `SonarData`. |
| [MLBL](MLBL/Task.cpp) | Serial acoustic/LBL interface for beacon configuration/ranging and compact vehicle reports/commands. Consumes navigation, fuel, plan, medium and sound-speed inputs; publishes `LblRange` and decoded control/report messages. |
| [MLBLTracker](MLBLTracker/Task.cpp) | Serial tracker-side acoustic interface handling `AcousticOperation`/`AcousticSystemsQuery`, sending vehicle position and decoding remote state/ranging information; advertises its service. |

## Water properties and analog acquisition

| Task | Purpose and important interfaces |
| --- | --- |
| [AIM104MultiIO](AIM104MultiIO/Task.cpp) | Periodic direct-I/O ADC acquisition through the local Driver; averages channels and publishes configured IMC scalar message types. |
| [CyclopsC7](CyclopsC7/Task.cpp) | Converts SADC voltage/readings into configured calibrated Cyclops measurements, accounting for gain; optionally controls sensor power and uses `VehicleMedium` context. |
| [DMS](DMS/Task.cpp) | Serial multichannel detection sensor; publishes channel `Voltage` and baseline/scaled `DmsDetection` readings using its local protocol driver. |
| [Keller](Keller/Task.cpp) | **BDD** pressure sensor publishing `Pressure`, `Depth` and `Temperature`; GPS/medium inputs support pressure-zeroing behavior. |
| [MetrecX](MetrecX/Task.cpp) | **BDD** multiprobe acquisition with configured digital/analog channel messages, pH/redox support and pressure-to-depth conversion using navigation latitude; can use an external temperature input. |
| [MiniSVS](MiniSVS/Task.cpp) | Serial sound-velocity sensor publishing `SoundSpeed` with missing-data watchdog handling. |
| [OEMX](OEMX/Task.cpp) | Serial multiparameter water sensor with configured probe mounts; publishes conductivity, temperature, pressure/depth, salinity, sound speed and turbidity as available. |
| [SADC](SADC/Task.cpp) | **BDD** serial ADC with gain configuration/switching; publishes `SadcReadings` and channel `Voltage`, supplying Cyclops conversion tasks. |
| [SW100](SW100/Task.cpp) | **BDD** water sensor publishing depth, temperature, conductivity and salinity; uses vehicle/medium state for calibration and publishes `DepthOffset`. |
| [XchangeSV](XchangeSV/Task.cpp) | **BDD** sound-velocity acquisition from a configured device URI; publishes `SoundSpeed` and monitors input timeout. |
| [XR620CTD](XR620CTD/Task.cpp) | Serial CTD parser publishing conductivity, temperature, pressure, depth, salinity and filtered sound speed; optionally publishes raw `DevDataBinary`. |

## Weather, host and reporting utilities

| Task | Purpose and important interfaces |
| --- | --- |
| [AIS](AIS/Task.cpp) | **BDD** AIS/NMEA decoder using libais. Publishes raw `DevDataText` and vessel `RemoteSensorInfo`; this is distinct from the simulator's `AisInfo` output. |
| [DataStore](DataStore/Task.cpp) | Aggregates navigation, CTD, sonar and log/plan events into `HistoricTelemetry`, `HistoricCTD`, `HistoricSonarData` and `HistoricEvent` for the transport-side historical store. |
| [Genesys](Genesys/Task.cpp) | Periodic TCP interface to an addressed Genesys supply, polling `Voltage` and `Current`. |
| [GillWindObserverII](GillWindObserverII/Task.cpp) | Serial wind sensor parser publishing `WindSpeed`. |
| [SCH311X](SCH311X/Task.cpp) | Periodic sysfs temperature acquisition for three configured sensor entities; publishes `Temperature`. |
| [ThermalZone](ThermalZone/Task.cpp) | Periodic Linux-style sysfs thermal-zone reader publishing `Temperature` under a configured entity. |
| [VantagePro2](VantagePro2/Task.cpp) | Serial weather-station acquisition publishing temperature, humidity and wind; combines `EstimatedState` with wind for `AbsoluteWind` and optional vertical profiles. |
| [WifiRSSI](WifiRSSI/Task.cpp) | Polls wireless link quality locally or through configured SSH access and publishes `RSSI`; does not implement a network transport. |
