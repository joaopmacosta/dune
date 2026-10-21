# Translations

[dune.pot](dune.pot) is the gettext message template;
[pt_PT/](pt_PT/) contains Portuguese translation data.
[entity_labels.txt](entity_labels.txt) makes configured entity names available
to the extraction pipeline.

[cmake/I18N.cmake](../cmake/I18N.cmake) defines `i18n_texts`, `i18n_extract`,
`i18n_update` and `i18n_compile` workflows when the relevant tools are available.
Extraction reads source strings and entity labels from
[configuration](../etc/README.md); compilation supplies runtime catalogs.
[Main](../src/Main/README.md) initializes the language through the core I18N
helpers and task context's translation directory.
