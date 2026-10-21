# Source generators

These Python tools keep protocol definitions, embedded specification data,
tests and address tables synchronized. They are explicit maintenance tools;
normal runtime message exchange uses their generated C++ output.

* [imc_download.py](imc_download.py) clones a selected IMC specification revision
  into a destination directory. It removes an existing destination first.
* [imc_code.py](imc_code.py) generates message/type code in
  [DUNE/IMC](../../src/DUNE/IMC/README.md).
* [imc_blob.py](imc_blob.py) embeds the protocol specification.
* [imc_tests.py](imc_tests.py) generates protocol tests under
  [programs/tests](../tests/README.md).
* [imc_addresses.py](imc_addresses.py) generates the shared
  [IMC address table](../../etc/common/imc-addresses.ini).
* [imc/](imc/) contains shared Python parsing/emission helpers.
* [status_codes.py](status_codes.py) generates status-code artifacts from
  [Codes.def](../../src/DUNE/Status/Codes.def).

[cmake/IMC.cmake](../../cmake/IMC.cmake) defines `imc_download`, `imc` and
`imc_force` targets. The latter two write into the source tree and should be
used only when intentionally updating the protocol. The root build also
defines the `status` target. Review generated changes together with their
specification inputs rather than editing generated definitions independently.
