# Power

Contains the power [`Model`](Model.hpp), exposed by
[`Power.hpp`](../Power.hpp). Construct it from `Parsers::Config`, validate
its model parameters, then use `computeMotionEnergy`, `computePayloadEnergy`,
`computeHotelEnergy`, and `computeIMUEnergy` to estimate consumption in Wh.
The model assumes positive speed; its direct dependency is
[`Parsers::Config`](../Parsers/Config.hpp).

The public header documents inputs, outputs, and configuration fields. This
small section is an offline calculation helper, not a device driver.
