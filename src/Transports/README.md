# Transports

IMC network/device links, routing, persistence and data services.
`Announce`/`Discovery` establish presence; transports carry selected bus messages.
`CommManager` handles higher-level `TransmissionRequest` routing/status, with
specialized acoustic, satellite, SMS and on-demand TCP paths below it.
Actual delivery depends on configured addresses, bindings, filters and deadlines.

Most tasks derive from `Tasks::Task`. `Serial`, `TCP/Client` and `TCP/Server`
instead use [`Tasks::SimpleTransport`](../DUNE/Tasks/SimpleTransport.cpp) for
common bus-to-stream framing and inbound dispatch. `SMS` uses
[`Hardware::BasicDeviceDriver`](../DUNE/Hardware/BasicDeviceDriver.cpp) for
connection/power lifecycle. `HTTP` also implements its local `RequestHandler`.

## Discovery, networking and routing

| Task | Purpose and important interfaces |
| --- | --- |
| [Announce](Announce/Task.cpp) | Collects local `AnnounceService` and `EstimatedState`, periodically sending system `Announce` presence over configured multicast/broadcast destinations and the bus. |
| [Discovery](Discovery/Task.cpp) | Listens for network announcements, decodes IMC packets and dispatches discovered `Announce` onto the local bus. |
| [UDP](UDP/Task.cpp) | Bidirectional IMC UDP transport with static destinations, announcement-derived peers, message/rate filters and configurable communication restrictions. Advertises its service. |
| [TCP/Client](TCP/Client/Task.cpp) | `SimpleTransport` connection to a configured TCP server; parses inbound IMC and forwards selected outbound bus messages. |
| [TCP/Server](TCP/Server/Task.cpp) | `SimpleTransport` TCP listener for IMC clients, with optional `AnnounceService` publication. |
| [TCPOnDemand](TCPOnDemand/Task.cpp) | Sends individual `TCPRequest` messages to requested destinations with retries and reports `TCPStatus`; distinct from a continuously connected TCP transport. |
| [Serial](Serial/Task.cpp) | `SimpleTransport` IMC framing/parsing over a configured serial port. |
| [SerialOverTCP](SerialOverTCP/Task.cpp) | Raw serial-to-TCP byte bridge for attached equipment; does not decode and publish an IMC sensor stream itself. |
| [CommManager](CommManager/Task.cpp) | Routes `TransmissionRequest` by requested/available communication mean and deadline; translates lower-level status to `TransmissionStatus`, manages fragmentation and handles legacy acoustic/SMS requests. |
| [Fragments](Fragments/Task.cpp) | Reassembles incoming `MessagePart`, dispatches decoded IMC messages and requests missing parts through `MessagePartControl`/`TransmissionRequest`. Complements CommManager's outgoing fragment handling. |
| [HTTP](HTTP/Task.cpp) | Embedded HTTP server with worker/request-handler helpers for web resources and monitored IMC/logbook data; accepts configured message traffic and advertises an HTTP service. |
| [FTP](FTP/Task.cpp) | File-transfer server with local session helpers and configurable control/data ports; advertises its service through `AnnounceService`. |
| [MobileInternet](MobileInternet/Task.cpp) | Manages serial cellular-modem/PPP connectivity, optional forwarding/Dynamic DNS and modem power state. Linux-only build rule; not an IMC packet transport by itself. |

## Acoustic, satellite and radio

| Task | Purpose and important interfaces |
| --- | --- |
| [Evologics](Evologics/Task.cpp) | TCP acoustic-modem driver: translates `UamTxFrame` to modem operations and publishes frame/range/status and USBL data. Uses GPS, sound speed and medium context. |
| [Seatrac](Seatrac/Task.cpp) | Serial acoustic-modem/USBL driver: handles `UamTxFrame`, receives frames/ranges/USBL results and optionally AHRS/environment telemetry; medium and magnetic calibration affect operation. |
| [UAN](UAN/Task.cpp) | Acoustic network protocol above modem `Uam*` messages. Handles `AcousticRequest`, compact reports, decoded commands and `AcousticStatus`; converts USBL position/angle information into fixes and advertises acoustic services. |
| [IridiumSBD](IridiumSBD/Task.cpp) | Physical SBD modem driver over a configured I/O port. Consumes `IridiumMsgTx`, publishes `IridiumMsgRx`/`IridiumTxStatus`, and manages modem events, sessions and medium-dependent operation. |
| [Iridium](Iridium/Task.cpp) | Satellite application codec/reporting layer: decodes `IridiumMsgRx` into IMC/text/asset information and sends device updates, announcements and state using `TransmissionRequest`. Separate from the SBD hardware driver. |
| [IridiumOperation](IridiumOperation/Task.cpp) | Configurable satellite message subscription/periodic-forwarding service with persistent-message helpers, entity/plan handling and received-operation processing. Uses `TransmissionRequest`/`TransmissionStatus`; C++17 build rule. |
| [SMS](SMS/Task.cpp) | `BasicDeviceDriver` GSM SMS service accepting `SmsRequest`, returning `SmsStatus` and polling the mailbox; its Driver emits received `TextMessage` and modem RSSI/version information. |
| [Radio](Radio/Task.cpp) | Serial telemetry-radio service with selectable radio-driver helpers, compact vehicle reports and `TelemetryMsg` exchange; handles reporting requests, power state and communication-system queries. |

`UAN` sits above `Evologics`, `Seatrac` or an appropriate modem substitute;
it is not another serial modem driver.
[Autonomy/TextActions](../Autonomy/TextActions/Task.cpp) can act on decoded text;
[Supervisors/Reporter](../Supervisors/Reporter/Task.cpp) schedules report requests.

## Logs, cache and historical data

| Task | Purpose and important interfaces |
| --- | --- |
| [Cache](Cache/Task.cpp) | Stores configured IMC messages and snapshots on `CacheControl`; reloads and dispatches persisted messages in configured order at startup. |
| [DataStore](DataStore/Task.cpp) | Stores/queries historical samples and routes them to gateways over configured links. Handles `HistoricData`/`HistoricDataQuery` and transmission acknowledgments; complements Sensors/DataStore's sample generation. |
| [LogBook](LogBook/Task.cpp) | Retains `LogBookEntry` records and answers `LogBookControl` retrieval/clear operations; separate from binary logging. |
| [Logging](Logging/Task.cpp) | Writes selected IMC traffic as binary LSF logs with configured compression/rates, entity/version metadata and flush policy. Handles `LoggingControl`, shutdown and remote log-restart requests; reports log identity to consumers. |
| [LoggingDigest](LoggingDigest/Task.cpp) | Produces a sampled LSF digest from configured messages and entity information, following `LoggingControl` log sessions. |
| [Replay](Replay/Task.cpp) | Reads recorded IMC logs and dispatches selected messages with timing, skip and speed controls from `ReplayControl`; coordinates replay log naming via `LoggingControl`. |
