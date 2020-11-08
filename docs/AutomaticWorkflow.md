# Automatic Workflow
Here we describe how you can automatize the workflow described in [Manual Workflow](../ManualWorkflow) page.

We advise you to read this page and afterwards fork and update the following Github repository in accordance to your vocabulary:

https://github.com/fair-data-collective/excel2rdf-template


The above repository contains all necessary files and configuration to automatically convert and validate your Excel vocabularies.


## Structuring Gihtub repository
Here we shortly describe how one could structure a Github repository which will be used for:

- Maintaining (and version controlling)
- Converting and
- Validating your controlled vocabularies

Here is an example of the Github repository structure, produced by running `tree` command on [excel2rdf-template]((https://github.com/fair-data-collective/excel2rdf-template)) repository:

```bash
├── .github
│   └── workflows
│       └── excel2rdf.yml
├── LICENSE
├── README.md
├── logs
│   ├── conversion.log
│   └── validation.log
├── vocabulary.ttl
└── vocabulary.xlsx
```

The key parts of this structure are following files:
- `vocabulary.xlsx` is the Excel template for building controlled vocabulary that will serve as input for `xls2rdf` tool
- `vocabulary.ttl` is the resulting file generated when running `xls2rdf` on `vocabulary.xlsx`
- `xls2rdf` produces logs that are saved in `conversion.log` file
- `validation.log` contains logs that are produced when running `qSKOS` on `vocabulary.ttl` to validate the converted vocabulary
- `excel2rdf.yml` contains configuration of Github action that automatize the workflow described in [Manual Workflow](../ManualWorkflow) page.

## Setting up Github actions

Considering that you opt for the above structure of your Github repository the following is an example of how to setup Github action via `excel2rdf.yml`:

```bash
name: Excel to RDF conversion

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

jobs:
  build-test-deploy-vocabulary:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v2

      - name: prepare # pulls repository and dowloads xls2rdf and qSKOS
        run: |
          git config user.name github-actions
          git config user.email github-actions@github.com
          git pull
          curl -L https://github.com/cmader/qSKOS/releases/download/2.0.3/qSKOS-cmd.jar -o qSKOS.jar
          curl -L https://github.com/sparna-git/xls2rdf/releases/download/2.1.1/xls2rdf-app-2.1.1-onejar.jar -o xls2rdf.jar

      - name: build # converts vocabulary.xlsx to vocabulary.ttl
        run: |
          java -jar xls2rdf.jar convert -i ./ontology/vocabulary.xlsx -o ./ontology/vocabulary.ttl -l en
          mv xls2rdf.log ./ontology/logs/

      - name: test # tests vocabulary.ttl against set of qSKOS tests
        run: |
          java -jar qSKOS.jar analyze -dc mil,bl ./ontology/vocabulary.ttl -o ./ontology/logs/qSKOS.log

      - name: deploy # comitts changes to the repository
        run: |
          rm qSKOS.jar
          rm xls2rdf.jar
          git add .
          git diff-index --quiet HEAD || git commit -m "generated new .ttl from .xlsx file"
          git push
```

