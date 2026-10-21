# Compression

Provides codec interfaces, selection, and stream/file adapters. Start with
[`Factory`](Factory.hpp) to map method names/extensions or create a
`Compressor`/`Decompressor`; [`FilterInput`](FilterInput.hpp),
[`FilterOutput`](FilterOutput.hpp), `FileInput`, and `FileOutput` connect the
codecs to streams and files. [`Compression.hpp`](../Compression.hpp) exposes
the package umbrella.

The implemented methods include gzip, zlib, and bzip2. The corresponding
implementations include zlib and bzip2 headers, so those codec backends depend
on their native libraries when enabled by the build. API details and failure
behavior are documented in the interface and exception headers.
