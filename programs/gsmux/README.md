# GSMUX tools

[gsmux.cpp](gsmux.cpp) implements `dune-gsmux`, which takes a device and output
folder. [gsmux-tsv.cpp](gsmux-tsv.cpp) implements `dune-gsmux-tsv`, which takes a
recorded binary file and produces tabular output.

[Filter.hpp](Filter.hpp) and [Fields.def](Fields.def) hold the local filtering
and field definitions. Both tools reuse core I/O, time and utility services;
they are standalone acquisition/conversion programs, not transport tasks.
[Program.cmake](Program.cmake) adds both executables to the build/install list.
Check their `main` functions for exact arguments before accessing a device.
