# Reference documentation

DUNE already contains Doxygen comments in core headers and task sources.
The new [source guides](../src/README.md) fill the architecture/module-overview
gap; they do not replace those API comments.

[Doxygen/Doxyfile.in](Doxygen/Doxyfile.in) is configured by the root
[build file](../CMakeLists.txt). [Doxygen/Layout.xml](Doxygen/Layout.xml) and
[Doxygen/Style.css](Doxygen/Style.css) customize the generated reference.
When Doxygen and zip are found, the build exposes `doc` and `doc_package`
targets; output lives under the build's `DUNEGeneratedFiles` directory.

```sh
cmake -S . -B build
cmake --build build --target doc
```

Run from the repository root. The
[documentation helper](../programs/scripts/dune-make-docs.py) can build and
publish HTML separately, but synchronizes its destination with deletion enabled.
[EmacsCodeStyle.el](EmacsCodeStyle.el) and
[EclipseCodeStyle.xml](EclipseCodeStyle.xml) are editor formatting settings,
not runtime dependencies.
