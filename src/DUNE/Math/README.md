# Math

Numerical primitives used across control, navigation, coordinates, and
simulation: matrices, quaternions/Euler angles, angle helpers, derivatives,
moving averages, FIR and robust filters, grids, optimization, and QP solving.
[`Math.hpp`](../Math.hpp) is the main umbrella.

[`Random.hpp`](Random.hpp) exposes the nested [`Random/`](Random/Factory.hpp)
generator family (factory, threaded generator, and implementations including
MT19937); it is included by the math umbrella, so a separate nested overview
is unnecessary. Inline APIs document conventions such as angle units and
matrix operations.
