# Parsers

Readers and writers for DUNE configuration and external data formats,
including NMEA, PD4, plan configuration, and HDF5, plus generic string
reader/writer bases. [`Parsers.hpp`](../Parsers.hpp) lists the available
interfaces; [`NMEAReader`](NMEAReader.hpp) validates sentences and converts
fields, while `NMEAWriter` and `NMEASentence` handle output/representation.

The HDF5 reader's h5cpp integration is optional in the build configuration.
Parser exceptions and inline API comments describe malformed input and
conversion behavior. Parsers are consumed by task and simulator code.
