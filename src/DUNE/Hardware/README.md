# Hardware

Low-level device interfaces and protocol helpers, including serial/I²C,
GPIO, PWM, CAN, modem, button, and device-driver abstractions. The section
umbrella is [`Hardware.hpp`](../Hardware.hpp); it also exports the embedded
protocol packages below. Drivers in `src/` use these interfaces for concrete
hardware access.

Nested components: [`GPIO/`](GPIO/GPIOInterface.hpp) provides character-device
and legacy sysfs backends; [`LUCL/`](LUCL/Protocol.hpp) and
[`UCTK/`](UCTK/Parser.hpp) contain protocol parsers and bootloader helpers;
[`STM/`](STM/Interface.hpp) provides an interface and firmware-update helper.
These are grouped here because their APIs are included by the umbrella.
OS/device-specific capabilities are selected by the build and platform
configuration.
