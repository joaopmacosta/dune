# Navigation

Navigation tasks turn [sensor](../Sensors/README.md) measurements into state for
[Control](../Control/README.md), [Maneuver](../Maneuver/README.md) and
[Monitors](../Monitors/README.md). `EstimatedState` is the principal output.
GPS/GNSS navigation implementations are alternatives selected by configuration;
their entity labels identify the intended sensor sources.

`AUV/Navigation` and `General/ROV` extend
[`Navigation::BasicNavigation`](../DUNE/Navigation/BasicNavigation.cpp). This
base consumes attitude/inertial, depth/altitude, GPS, DVL, LBL, USBL and RPM
measurements; handles validity/rejection; and publishes `EstimatedState`,
`NavigationUncertainty` and `NavigationData`. Thus their interfaces are larger
than the explicit bindings in their own `Task.cpp`. Other tasks derive from
`Tasks::Task`, except periodic `General/GNSS`.

| Task | Purpose and important interfaces |
| --- | --- |
| [Attitude/EKF](Attitude/EKF/Task.cpp) | Attitude/gyro-bias EKF using `AngularVelocity` or `EulerAnglesDelta` prediction and acceleration/magnetic updates; publishes `EulerAngles`. Configured source labels and mounting/calibration matter. |
| [AUV/Navigation](AUV/Navigation/Task.cpp) | `BasicNavigation` AUV Kalman filter with inertial alignment and sensor fusion; additionally watches `EntityActivationState`, publishes `AlignmentState` and adjusts configured entity parameters. |
| [AUV/Ranger](AUV/Ranger/Task.cpp) | Schedules acoustic beacon ranging from `LblConfig`; sends `UamTxFrame` requests and converts modem `UamRxRange` replies into `LblRange` for navigation. |
| [General/Estimator](General/Estimator/Task.cpp) | Estimates wave encounter frequency and corrected heave from `Displacement` and `EulerAngles`; publishes `Frequency` and `Displacement`, not vehicle position. |
| [General/GNSS](General/GNSS/Task.cpp) | Prioritized multi-source GNSS navigation with depth/altitude and attitude/rate/velocity inputs; publishes `EstimatedState`, rejected-fix diagnostics and configured satellite notifications. |
| [General/GPSNavigation](General/GPSNavigation/Task.cpp) | GPS-based state estimation from selected `GpsFix`, `GroundVelocity`, `EulerAngles` and `AngularVelocity`; publishes `EstimatedState` and `GpsFixRejection`. |
| [General/LBL](General/LBL/Task.cpp) | Estimates LBL beacon locations using vehicle/GPS/attitude and `LblRange`; publishes `LblEstimate` and updated `LblConfig`, caching configuration through `CacheControl`. Its build metadata marks it proprietary. |
| [General/NavManager](General/NavManager/Task.cpp) | Selects among configured GPS units and attitude sources, incorporating angular rate and altitude; publishes `EstimatedState`, fix rejection and satellite status notifications. |
| [General/ROV](General/ROV/Task.cpp) | `BasicNavigation` ROV Kalman-filter implementation; relies on the base's measurement subscriptions and navigation publications rather than declaring separate IMC consumers. |
