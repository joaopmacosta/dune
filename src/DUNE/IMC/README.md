# IMC

The message model and wire-format layer. `Definitions` supplies message
types; `Message`, `Factory`, and `MessageList` provide message construction
and handling; `Serialization`, `Parser`, and `Packet` cover encoding and
packet parsing; `Bus`, `AddressResolver`, and `Blob` support message
distribution and payloads. [`IMC.hpp`](../IMC.hpp) is the package umbrella.

Tasks dispatch these messages, and transports such as Iridium and radio
consume them at the network boundary. Serialization's documented routines
define the wire representation; use those comments and message definitions
for field-level details.
