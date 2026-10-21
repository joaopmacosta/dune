# Maneuvers

Reusable motion-pattern and maneuver logic: trajectory following, station
keeping, loiter patterns (circular and figure-eight), elevation, rows/stages,
vehicle formation, and swarm helpers. [`Maneuver.hpp`](Maneuver.hpp) is the
base interface and [`Maneuvers.hpp`](../Maneuvers.hpp) collects the package.

These are maneuver building blocks used by task-level vehicle behavior; they
are not the task manager itself. `Control` supplies related controller
primitives. Each maneuver header documents its inputs and behavior.
