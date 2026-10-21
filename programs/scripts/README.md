# Maintenance and operational scripts

These helpers automate repository or deployment workflows rather than
participating in the IMC task bus.

| Scripts | Responsibility and dependencies |
| --- | --- |
| [dune-create-task.py](dune-create-task.py), [dune-create-test-task.py](dune-create-test-task.py) | Scaffold task/test source using DUNE lifecycle and registration conventions. Generated placeholders still need real behavior and documentation. |
| [dune-maintainer-checks.py](dune-maintainer-checks.py), [dune-clang-format.sh](dune-clang-format.sh) | Maintenance checks and source formatting. |
| [dune-create-changelog.py](dune-create-changelog.py), [dune-create-params.py](dune-create-params.py) | Produce changelog/parameter artifacts. |
| [dune-extract-elabels.py](dune-extract-elabels.py) | Extract configured entity labels for the translation pipeline. |
| [dune-make-docs.py](dune-make-docs.py) | Configure a temporary `ref-docs` build, run the documentation target and synchronize generated HTML to a destination. |
| [dune-cdash-build.rb](dune-cdash-build.rb) | Drive a CDash-oriented build workflow. |
| [dune-flightgear.sh](dune-flightgear.sh) | FlightGear simulation helper. |
| [dune-mobile-inet.sh](dune-mobile-inet.sh), [dune-storage-ro-rw.sh](dune-storage-ro-rw.sh) | Deployment helpers for mobile connectivity and storage mount modes. |

The scaffolding connects to [task discovery](../../cmake/Tasks.cmake);
documentation connects to [Doxygen](../../doc/README.md), and entity-label
extraction feeds [i18n](../../i18n/README.md). Read arguments and paths before
running scripts: documentation publishing uses `rsync --delete`, and deployment
helpers can change host networking or filesystem state.
