# Network

Address and URL types, TCP/UDP sockets, interface discovery, TDMA, upstream
TCP serving, and fragmented-message helpers. [`Network.hpp`](../Network.hpp)
is the umbrella. The socket classes build on [`IO::Handle`](../IO/Handle.hpp)
and its polling support.

Nearby consumers include the HTTP request handler, communication manager,
and radio/Iridium transports. Socket and fragment APIs document buffer,
address, and lifecycle details in their headers.
