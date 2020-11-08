# Introduction

It's hard to build up a strong SKOS vocabulary that is well maintained and well distributed.

We've put together a small suite of tools that lets you maintain the vocabulary in a straightforward Excel list (or lists!),
then automatically converting those lists into well-defined ontologies in the SKOS (Simple Knowledge Organization System) format.

By following the best standards and practices of SKOS,
this tool suite makes it easy to produce and distribute advanced semantic content, without having to understand SKOS directly yourself.

The SKOS format is compatible with the required SKOS format for BioPortal and OntoPortal repositories,
making it easy to access your terms through those systems and through the [CEDAR Workbench](https://metadatacenter.org) metadata system.

We use the following tools to manage this conversion:

- Excel spreadsheets
- [SKOS Play](https://labs.sparna.fr/skos-play/convert) (web site or downloaded service)
- [qSKOS](https://github.com/cmader/qSKOS) (for RDF verification)
- [BioPortal](https://bioportal.bioontology.org/)
- GitHub Actions for workflow automatization

Click on the [Tools](../Tools) link to learn more about the items in this list.

You don't have to have GitHub to use the manual version of this workflow.
However, if you opt for maintaining your controlled vocabulary on Github, then it is rather straightforward to automatize the workflow using [Github actions](https://github.com/features/actions). This can make the process of managing your vocabulary nearly trivial. To see the actual process of the workflow automatization click on the [Automatic Workflow](../AutomaticWorkflow) link.
