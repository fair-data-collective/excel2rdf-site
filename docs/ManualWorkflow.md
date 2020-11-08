# Manual Workflow 

This page describes the manual process of creating a vocabulary of controlled terms in Excel, 
converting that vocabulary into a semantic standard known as SKOS, 
and if desired, submitting the result into the BioPortal ontology repository (or any OntoPortal-compatible repository).

To perform this process, you don't have to set up a GitHub repo or other automated system. 
Instead you can perform each step manually, inspecting the results and making necessary changes 
before going on to the next step. 

Once you have a baseline ontology and confirm the process works as expected, 
you may want to automate the process,
Instructions for automating the process are given in the next page.

## Choosing your template

The core of the transformation is engineered by software from the SKOS Play site, https://labs.sparna.fr/skos-play/,
and in particular its Excel-to-SKOS conversion software at https://labs.sparna.fr/skos-play/convert. 
Developer Thomas Francart of Sparna Labs developed this sophisticated tool 
to address a wide range of vocabulary representation and needs, 
and it is available both as a web service (described here) and an installable app file (following page).

To begin, the user needs to choose an appropriate spreadsheet for representing their vocabularies. 
There are 8 examples on the SKOS Play web site, each illustrating different features of the system. 
Examples 4 and 7 are not intended to produce rigorous SKOS outputs, and can be ignored.

We also provide spreadsheet examples in this repository, addressing typical scenarios. 
In particular, we provide additional metadata suitable to describe your SKOS vocabulary with appropriate metadata.
Each template contains a description of the purpose for which the template is intended.

## Converting your file to SKOS: SKOS Play

Once you have chosen your desired template, you can edit its content, 
then submit it to the [SKOS Play site for conversion](https://labs.sparna.fr/skos-play/convert).
The site produces a SKOS file in various formats—we recommend Turtle (TTL) for readability—
and displays the results in a window. 
You can copy this text and paste it into a file, whose extension should be .ttl for the Turtle format
and .rdf for the XML/RDF format.

## Validation

Several SKOS validation sites exist, in case you want to perform validations or quality checks beyond what SKOS Play provides.
For example, [SKOSify](https://github.com/NatLibFi/Skosify) and [qSKOS](https://github.com/cmader/qSKOS) are two examples of such validators.

## Submission to BioPortal or OntoPortal

[BioPortal](https://bioportal.bioontology.org) allows anyone to submit an ontology.  
Simply visit the [Ontologies page]((https://bioportal.bioontology.org/ontologies) and click on the Submit New Ontology button.
(The site requests that ontologies that are not of value for public use be submitted as private resources,
but there are no constraints on ontology submissions based on their subject.)

When you submit your ontology, be sure to select the SKOS option for ontology type. 
The ontology will still parse if the OWL format is selected, but will not show the SKOS concepts as classes on the BioPortal site,
so they will not be visible.
You can verify your terms are visible to the public by visiting the Classes tab and viewing the tree that's shown in that tab.
 
## Making Private Ontology Externally Accessible

If you want your ontology to be visible to an external system (like the [CEDAR Workbench](https://metadatacenter.org),) 
and your ontology Viewing Restrictions field is set to Private,
you will have to explicitly give your external application or user permissions to access it with their own API key or account. 
To do so for the CEDAR Workbench, enter `cedar`, `cedar-mjd`, `cedar-test`, and `cedar-public` in the Viewing Restrictions field for those accounts.
It takes as many as 7 hours for the ontology to be detected in CEDAR once it is published by BioPortal.


