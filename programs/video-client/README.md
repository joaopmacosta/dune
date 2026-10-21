# Video client

This optional Qt application receives and displays video using core media and
network services. [Main.cpp](Main.cpp) creates the application from remote
address, remote port and local port arguments.
[VideoClient](VideoClient.hpp) handles the client widget and
[GraphicsScene](GraphicsScene.hpp) supplies its scene.

[Program.cmake](Program.cmake) creates the `video-client` target only when
`DUNE_USING_QT5` is available. It links to
[dune-core](../../src/DUNE/README.md) and the detected system dependencies.
It is a standalone viewer, not the browser interface or an onboard camera
driver; see [Vision](../../src/Vision/README.md) for acquisition tasks.
