# Coordinates

Coordinate-frame and geodesy helpers: body-fixed transformations, general
2-D bearing/range operations, WGS-84 calculations, UTM projection, and the
World Magnetic Model. [`Coordinates.hpp`](../Coordinates.hpp) collects these
headers. [`General.hpp`](General.hpp) contains inline operations such as
`toPolar`, `getBearingAndRange`, and `displace`; the specialized conversion
implementations are in `General.cpp`, `UTM.cpp`, and `WMM.cpp`.

The package builds on [`Math/Matrix.hpp`](../Math/Matrix.hpp) and bridges
coordinate calculations with IMC state messages. Refer to the inline Doxygen
comments for units and frame assumptions.
