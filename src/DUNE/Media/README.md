# Media

Image and video helpers: JPEG compression, video capture, IIDC-1394 capture,
Bayer decoding, and the MJPG encoder. [`Media.hpp`](../Media.hpp) is the
public include point; [`MJPG/Encoder.hpp`](MJPG/Encoder.hpp) writes the
encoder's AVI/MJPG chunks and is included by that umbrella.

The optional hardware/codec paths are build-configured: the repository has
separate JPEG, DC1394, and V4L2 checks. Thus an available header does not by
itself guarantee a backend is enabled. Capture and compression interfaces
document their supported input formats.
