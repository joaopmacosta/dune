# Vision

Camera acquisition, image logging and trigger/power control. These tasks are
device interfaces, not a general perception pipeline. Most derive from
`Tasks::Task`; `FrameGrabber` uses `Tasks::Periodic`.

Image-file producers use [Logging](../Transports/Logging/Task.cpp)'s
`LoggingControl` session identity to place captures with vehicle logs; they do
not all publish images over IMC. Navigation/activation context and camera/strobe
power are supplied by neighboring [Navigation](../Navigation/README.md),
[Supervisors](../Supervisors/README.md) and [Power](../Power/README.md) tasks.

| Task | Purpose and important interfaces |
| --- | --- |
| [DFK51BG02H](DFK51BG02H/Task.cpp) | Network camera capture using local GVCP/GVSP, white-balance and auto-exposure helpers; configures camera/strobe and records frames under the current logging session. |
| [FrameGrabber](FrameGrabber/Task.cpp) | Acquires frames from a platform video device, JPEG-compresses them and publishes `CompressedImage`; `ImageTxSettings` adjusts transmission/image settings. |
| [Lumenera](Lumenera/Task.cpp) | HTTP/MJPEG camera control and capture with local HTTP/logging helpers; uses `EstimatedState`, entity activation feedback and `LoggingControl`, and manages camera/LED power. |
| [PhotoTrigger](PhotoTrigger/Task.cpp) | Triggers photographs by frequency or navigation-derived footprint overlap/distance. Pulses a named `PowerChannelControl` output and records `LogBookEntry`; it does not capture the image itself. |
| [PointGrey](PointGrey/Task.cpp) | Camera/strobe acquisition and image saving with navigation metadata and logging-session context. Platform-gated FlyCapture/OpenCV/Exiv2 implementation; build metadata marks it proprietary and checks these libraries on ARM. |
| [UAVCamera](UAVCamera/Task.cpp) | Activation-driven camera power control through `PowerChannelControl`; exposes AXIS/FLIR geometry parameters but does not implement frame acquisition in this task. |
| [UI2210MGL](UI2210MGL/Task.cpp) | uEye camera capture with exposure/gain/AOI configuration and logging-session image output. Local `CaptureUeye` and OpenCV support are required; build rules check uEye headers and OpenCV. |
