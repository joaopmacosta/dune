# Plans

Plan-related calculations: [`TimeProfile`](TimeProfile.hpp) models timing,
[`Progress`](Progress.hpp) tracks progress, and
[`SpeedModel`](SpeedModel.hpp) provides speed estimates. All three are
exported through [`Plans.hpp`](../Plans.hpp).

These are small planning primitives used by maneuver/task code, not a plan
parser or execution engine; plan configuration parsing lives in
[`Parsers`](../Parsers/README.md). See each header for its specific model and
units.
