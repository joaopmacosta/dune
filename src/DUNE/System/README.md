# System

Cross-platform process and host services: resource queries, system errors,
dynamic library loading, and environment access. [`System.hpp`](../System.hpp)
is the section umbrella; the concrete implementations are split across
`Resources.cpp`, `DynamicLoader.cpp`, and `Environment.cpp`.

This layer backs higher-level DUNE components that need OS facilities while
keeping platform details behind shared interfaces. The interface comments
document return values and error behavior.
