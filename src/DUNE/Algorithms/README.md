# Algorithms

General-purpose encoding, checksums, digests, and seawater calculations.
The section umbrella is [`Algorithms.hpp`](../Algorithms.hpp); its headers
include `Base64`, CRC-8/16, Fletcher and XOR checksums, MD5, SHA-256, and
`UNESCO1983`. `CRC32.hpp` is also present as a direct-include API but is not
listed by that umbrella. The digest implementations are in matching `.cpp`
files; the inline headers document each call's input and output contract.

This package is a low-level dependency used within DUNE rather than a
task-facing subsystem. Its interfaces use standard C++ data types and DUNE
configuration; the umbrella does not introduce a separate runtime service.
