---
title: 'Type-safe accession and browsing of OBO formatted Ontologies in validation packages'
title_short: 'OboProvider'
authors:
  - name: Kevin Schneider
    orcid: 0000-0002-2198-5262
    affiliation: 1
affiliations:
  - name: Computational Systems Biology, RPTU Kaiserslautern-Landau, Kaiserslautern, Germany
    index: 1
date: 25 June 2025
---

# Summary

This project developed an F# type provider for ontologies, aiming to improve the integration of ontology terms into ARC validation workflows.  
Instead of parsing ontology files at runtime and navigating lists of terms programmatically, the type provider exposes ontology terms as named static properties.  
This approach enables enhanced tooling support such as autocompletion, static type checking, and code navigation in development environments.

# Introduction

ARCs (Annotated Research Contexts) are research data management containers that include both data and metadata descriptors.  
Built on top of established standards for experimental metadata (ISA) and computational provenance and execution metadata (CWL), this framework supports generic metadata annotation capabilities that can be adapted to specific domains through the use of ontologies.

A common domain-specific objective is the validation of an ARC — for example, checking whether it adheres to domain-specific ontology usage or employs the correct data formats.  
ARCs can be validated using **validation packages**, which define a set of requirements that an ARC must satisfy to be considered valid for a particular purpose.  
ARC validation packages are implemented as scripts that can be executed in a CI/CD pipeline.  

Ontologies are essential for validating both structural and semantic metadata.  
Existing approaches for loading ontologies into ARC validation packages rely on parsing formats such as OBO files. While these provide access to the required data, they lack strong IDE support.  
Type providers — a feature of the F# programming language — allow developers to generate types at compile time based on external data sources.  
An OBO type provider offers a declarative and discoverable way to reference ontology terms in code, reducing errors and improving the developer experience.

# Results

Type providers for all `[Term]` and `[Typedef]` stanzas in OBO files were implemented, allowing developers to access them as strongly typed properties.
An example can seen in the `sample.fsx` file, where ontology terms from `some_go_terms.obo` (a small subset from the go ontology) are accessed using the type provider.
The type provider generates types for each term, enabling static type checking and autocompletion in IDEs.
An example can be seen in this screenshot, where the generated type for the following obo term is shown:

```obo
[Term]
id: TGMA:0000010
name: adult scape
def: "The first or basal segment of the antenna." [ISBN:0-937548-00-6]
comment: Fig 02,05,06,07,08 Abbr: Sc in ISBN:0-937548-00-6.
synonym: "antennal scape" RELATED [ISBN:0-937548-00-6]
synonym: "antennal sclerite" RELATED [ISBN:0-937548-00-6]
synonym: "basal antennal segment" RELATED [ISBN:0-937548-00-6]
synonym: "basal joint" RELATED [ISBN:0-937548-00-6]
synonym: "basal segment" RELATED [ISBN:0-937548-00-6]
synonym: "first antennal segment" RELATED [ISBN:0-937548-00-6]
synonym: "first segment" RELATED [ISBN:0-937548-00-6]
is_a: TGMA:0001835 ! compound organ component
relationship: part_of TGMA:0000007 ! adult antenna
```

Here is a screenshot of the generated type in an IDE, showing the generated tooltip for this term

![tooltip](./tooltip.png)

And here is what the synonym accession can look like in an IDE, showing the synonym text available for the term

```fsharp
go.``adult scape``.Synonyms
|> List.map (fun x -> x.Text)
```

![synonyms](./synonyms.png)
