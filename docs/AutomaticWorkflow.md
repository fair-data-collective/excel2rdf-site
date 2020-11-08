# Automatic Workflow
Here we describe how you can automatize the workflow described in [Manual Workflow](../ManualWorkflow) page.

We advise you to read this page and afterwards fork and update the following Github repository in accordance to your vocabulary:

https://github.com/fair-data-collective/excel2rdf-template


The above repository contains all necessary files and configuration to automatically convert and validate your Excel vocabularies.


## Structuring Gihtub repository
Here we shortly describe the structure of [excel2rdf-template](https://github.com/fair-data-collective/excel2rdf-template) repository which can be used for:

- Maintaining (and version controlling)
- Converting and
- Validating your Excel controlled vocabularies

The structure is as following:

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
The most important files of this repository are:
- `vocabulary.xlsx` is the Excel template for building controlled vocabulary that will serve as input for `xls2rdf` tool
- `vocabulary.ttl` is the resulting file generated when running `xls2rdf` on `vocabulary.xlsx`
- `xls2rdf` produces logs that are saved in `conversion.log` file
- `validation.log` contains logs that are produced when running `qSKOS` on `vocabulary.ttl` to validate the converted vocabulary
- `excel2rdf.yml` contains configuration of Github action that automatize the workflow described in [Manual Workflow](../ManualWorkflow) page.

## Setting up Github actions

In accordance to the above structure we set up the Github action via `excel2rdf.yml`, which is placed in `.github/workflows/` folder (it must be there for Github to automatically picks it up and execute it).

The content of `excel2rdf.yml` file looks like this

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
          java -jar xls2rdf.jar convert -i ./vocabulary.xlsx -o ./vocabulary.ttl -l en
          mv xls2rdf.log ./logs/conversion.log

      - name: test # tests vocabulary.ttl against set of qSKOS tests
        run: |
          java -jar qSKOS.jar analyze -dc mil,bl ./vocabulary.ttl -o ./logs/validation.log

      - name: deploy # commits changes to the repository
        run: |
          rm qSKOS.jar
          rm xls2rdf.jar
          git add .
          git diff-index --quiet HEAD || git commit -m "generated new .ttl from .xlsx file"
          git push
```

