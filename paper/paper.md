---
title: 'Improving package annotation in metabolomics and proteomics via robust, ontology-driven LLM integration'
title_short: 'Package annotation via LLM integration'
date: "10 April 2026"
output: pdf_document
affiliations:
- name: "Institute of Computational Biology, Computational Health Center, Helmholtz Center, Munich, Germany"
  ror:  00cfam450
  index: 1
- name: "RECETOX, Faculty of Science, Masaryk University, Kotlářská 2, 611 37 Brno, Czechia"
  ror: 032hzqh22
  index: 2
- name: "Channing Division of Network Medicine, Mass General Brigham, Harvard Medical School, Boston MA, 02115 USA"
  index: 3
- name: "Limerick Digital Cancer Research Centre, School of Medicine, University of Limerick, V94 T9PX, Ireland"
  ror: 00a0n9e72
  index: 4
- name: "University of Nantes, Nantes, France"
  ror: 03gnr7b55
  index: 5
- name: "Institut Pasteur, Université Paris Cité, Bioinformatics and Biostatistics Hub, 75015, Paris, France"
  ror: 0495fxg12
  index: 6
- name: "IFB-core, French Institute of Bioinformatics (IFB), CNRS, INSERM, INRAE, CEA, 94800 Villejuif, France"
  ror: 045f7pv37
  index: 7
- name: "Centre for Genomic Regulation, The Barcelona Institute of Science and Technology, Dr. Aiguader 88, Barcelona 08003, Spain"
  ror: 03wyzt892
  index: 8
tags:
- bioinformatics
- ontology
- biohackeu25
- Bioconductor
- EDAM
- ELIXIR
- metabolomics
- proteomics
cito-bibliography: paper.bib
event: BH25EU
biohackathon_name: BioHackathon Europe, Barcelona, Spain, 2025
biohackathon_url: "https://biohackathon-europe.org/"
biohackathon_location: Berlin, Germany, 2025
group: Project 13
git_url: https://github.com/mblue9/BioHackEU25_preprint
authors_short: Sebastian Lobentanzer \emph{et al.}
authors:
- name: Sebastian Lobentanzer
  orcid: "0000-0003-3399-6695"
  affiliation: 1
  role: Co-lead, hacking, writing.
- name: Helge Hecht
  orcid: "0000-0001-6744-996X"
  affiliation: 2
  role: Co-lead, curation, hacking, writing.
- name: Vincent J Carey
  orcid: "0000-0003-4046-0063"
  affiliation: 3
  role: Hacking.
- name: Maria A Doyle
  orcid: "0000-0003-4847-8436"
  affiliation: 4
  role: Hacking, writing.
- name: Alban Gaignard
  orcid: "0000-0002-3597-8557"
  affiliation: 5
  role: Hacking.
- name: Hervé Ménager
  orcid: "0000-0002-7552-1009"
  affiliation: 6, 7
  role: Hacking, writing.
- name: Júlia Mir
  orcid: "0000-0001-6104-9260"
  affiliation: 8
  role: Online hacking.
- name: Claire Rioualen
  orcid: "0000-0002-7684-8679"
  affiliation: 7
  role: Co-lead, hacking, writing.
---
# Abstract

Identifying the most appropriate bioinformatics tool for a task remains challenging across multiple domains. 
Annotating tools with EDAM ontology terms (e.g. topics, operations, input / output data and formats) can help 
but manual annotation is labour-intensive, error-prone, and difficult to scale, 
particularly given the high rate of first-time package developers in academic environments. 

At BioHackathon Europe 2025, our team explored how Large Language Models (LLMs) can assist this process through the Model Context Protocol (MCP),
an emerging open standard that specifies how LLMs call external functions, using metabolomics as a domain use case. 
We developed an MCP-based workflow that grounds tool descriptions in the EDAM ontology [@citesAsDataSource:ison_edam_2013], 
improving reproducibility and semantic precision. Two core modules, entry-point specification and semantic text segmentation,
were completed during the hackathon, while additional mapping, validation, and reporting functions were outlined for follow-up development. 
Benchmarking integrated with the BioChatter framework [@extends:lobentanzer_platform_2025] demonstrated that MCP-assisted models 
outperform unconstrained baselines on initial tests using metabolomics packages from bio.tools [@extends:ison_edam_2019]. 

Ongoing work will expand benchmarking datasets, refine term-mapping logic, and extend the workflow to proteomics, 
supporting scalable, ontology-driven annotation across the ELIXIR ecosystem.

# Introduction

## Background

[Metabolomics](https://elixir-europe.org/communities/metabolomics), 
[fluxomics](https://elixir-europe.org/internal-projects/commissioned-services/fluxomics-workflows), 
and [proteomics](https://elixir-europe.org/communities/proteomics) face ongoing interoperability challenges despite shared platforms 
and analytical frameworks (e.g. MS-based workflows and bioimaging pipelines).  

While these communities have advanced standardization through [EDAM-annotated  Bioconductor  packages](https://doi.org/10.37044/osf.io/dsgnw_v1) 
[@citesAsRecommendedReading:rioualen_biohackeu24_2025], several gaps persist:  

1. Manual tool annotation in [bio.tools](https://bio.tools) is labour-intensive and often inconsistent.

2. Tool discoverability for experimentalists remains limited.

3. Integration across ELIXIR services (Galaxy, WorkflowHub, BioContainers) requires strengthening, 
as identified in [ELIXIR commissioned services](https://elixir-europe.org/internal-projects/commissioned-services/proteomics-pipelines) 
and a related [white paper](https://f1000research.com/articles/6-875/v1) [@cites:vizcaino_community_2017].

This  project was initiated at the [2025\ ISMB\ CollaborationFest](https://doi.org/10.12688/f1000research.169977.1): 
[Improving how we describe and discover Bioinformatics tools](https://github.com/mblue9/biocedam-cofest-2025). 
As a result, a first draft of the EDAM-MCP server was developed to improve LLM-based annotations. 

The MCP is an emerging open standard for structured interaction between LLMs and external tools. 
It defines how models can call external functions—such as ontology lookups or API queries—instead of relying purely on text generation. 
In our context, MCP acts as a control layer ensuring that annotations derived from natural language remain 
grounded in the EDAM ontology and are both reproducible and machine-readable.

## Objectives

This project aims to enhance the accessibility and interoperability of metabolomics and proteomics tools 
by leveraging LLMs for automated, ontology-driven annotation.  

We focus on integrating the [EDAM  ontology](https://edamontology.org) with the MCP framework to provide structured, 
reproducible annotations of bioinformatics resources.

Key objectives include:

- Developing an LLM-based querying system for the bio.tools API.  

- Strengthening interoperability between omics tools, ELIXIR services, and Bioconductor packages.  

- Facilitating user-friendly, structured search and guided workflow usage within Galaxy’s [Metabolomics](https://workflow4metabolomics.usegalaxy.fr/) 
and [Proteomics](https://proteore.org/) environments.  

# Results

## Overview

During the hackathon, we extended the EDAM-MCP framework to improve ontology-driven annotation through structured, 
deterministic LLM interactions (Figure 1). Early experiments showed that unconstrained LLMs frequently hallucinate EDAM terms, 
producing annotations that are syntactically plausible but semantically invalid (Figure 2). MCP’s modular, 
function-based control mitigates this by enforcing explicit task definitions and reproducible behaviour.

![Mid-week reporting poster used during BioHackathon Europe 2025 to communicate early design decisions, 
illustrate EDAM branches, and motivate the use of the Model Context Protocol to mitigate LLM hallucinations 
during ontology-driven tool annotation.](figures/Fig1_biohack25_poster.png)

![Example of hallucination: operation_3225 is actually “Variant classification”.](figures/Fig2_llm_hallucination.png)

We planned a two-layered process:

- Check whether a term already exists in EDAM; if so, return it.  

- If no match exists, suggest a new term with its proposed position in the ontology.  

The initial use case focused on metabolomics, chosen because of the wide range of software it includes. We aimed to (1) identify 
a representative set of well-annotated tools, (2) create realistic Q&A benchmarks for annotation quality, and (3) evaluate 
the MCP workflow on this domain dataset.

## EDAM-MCP workflow

Applying MCP to annotation required us to design a workflow that distinguishes planning (deciding what to do) from execution (carrying it out). 
This ensures reproducible, logically consistent mappings and reduces the probabilistic behaviour typical of unconstrained LLMs.

The workflow begins with an entry point that provides the AI agent with a structured overview of available functions and options. 
Two key components were implemented and tested during the hackathon:

- `get_workflow_summary()` (called `describe_workflow()` during development) - defines the MCP entry point, summarizing the workflow and listing available functions, expected inputs/outputs, and configurable parameters.  

- `segment_text()` - determines whether an input should be processed as a single concept or split into multiple semantically coherent segments to improve mapping precision.

The remaining modules were designed conceptually and opened as GitHub issues for ongoing work:

- `map_to_edam()` - main mapping function linking text to EDAM ontology terms using embeddings and ontology traversal.  

- `commonsense_check()` - validation layer to assess mapping plausibility and adjust overly generic or overly specific terms [Issue #41](https://github.com/edamontology/edammcp/issues/41). 

- `merge_results()`, `report_summary()`, and `update_opts()` - functions to aggregate results, summarise mappings, and enable parameter re-runs 
([Issue #42](https://github.com/edamontology/edammcp/issues/42), [Issue #43](https://github.com/edamontology/edammcp/issues/43), [Issue #44](https://github.com/edamontology/edammcp/issues/44)).

A schematic view of the workflow and its interactions is shown below and detailed in [Issue #37](https://github.com/edamontology/edammcp/issues/37) 
(Figure 3).

![Schematic view of the workflow and its interactions.](figures/Fig3_edam_mcp_workflow.png)

We also considered how the MCP handles EDAM branches (Operations, Topics, Data, Formats). 
One of the key elements in the implementation of these functions is the logical constraints inferred from choices of concepts 
in the different EDAM branches, which dictate whether for instance a given output EDAM format is compatible with a chosen EDAM operation. 
Initial testing showed that querying all branches simultaneously produced imbalanced results, 
so we began developing specialised sub-mappers—starting with an Operations-only mapper ([PR #51](https://github.com/edamontology/edammcp/pull/51))—to improve control and benchmarking accuracy.


## Use case: Metabolomics packages

We identified over 60 metabolomics tools in bio.tools, serving as a basis for testing EDAM annotations ([Supplementary Table S1](https://github.com/mblue9/BioHackEU25_preprint/tree/main/paper/S1_tools_metabolomics_from_biotools.xlsx)). These tools varied in annotation quality, so several were manually reviewed and updated during the hackathon. 

For benchmarking, we generated question-answer datasets for two of the selected tools (*xcms* and *recetox-aplcms*), 
which are available at: https://github.com/edamontology/edammcp/blob/main/benchmark.

A prototype script was created to automate dataset generation. 
It queries the bio.tools API for a given package and produces structured Q&A datasets suitable for benchmarking annotation accuracy.

## Benchmarking

To assess the MCP’s contribution, we integrated a programmatic benchmarking framework with [BioChatter](https://github.com/biocypher/biochatter). 
Benchmark data and scripts are available in the [EDAM-MCP repository](https://github.com/edamontology/edammcp/tree/main/benchmark). 
The pipeline we constructed automatically converts the Q&A datasets, 
based on well-annotated metabolomics packages, into BioChatter’s benchmark format and compares model outputs with and without MCP guidance. 
The setup was merged in [BioChatter PR #321](https://github.com/biocypher/biochatter/pull/321).

Preliminary results (based on a small, 10-question development benchmark) indicate that MCP-supported models 
generally outperform baseline LLMs in producing accurate EDAM annotations (Figure 4). 
An exception was Claude Haiku, which performed well even without MCP—likely reflecting the simplicity of the dataset. 
Future work will extend the benchmark to larger datasets and introduce versioned MCP releases for result traceability. 
We will also include secondary measures, such as the cost of model runs, in the more comprehensive benchmark. 
Before the more extensive benchmarking, a first stable version of the MCP server should be attempted to facilitate consistent measurements. 
This includes decisions on the function scope of the MCP server and technical challenges 
such as the sub-mapping of individual branches described above.

![Evaluation on 2 examples shows significantly better performance over the baseline LLM.](figures/Fig4_mcp_baseline_model_comparison.png)

# Discussion 

Future plans include extending both the technical scope and practical applicability of the EDAM-MCP workflow. 
The approach is intended to support individual curators via human-in-the-loop annotation, 
as well as ecosystem maintainers seeking automated, large-scale annotation pipelines across resources such as bio.tools and Bioconductor. 
Specifically, we aim to:

- Integrate new metabolomics and proteomics concepts into EDAM.

- Expand the ground-truth dataset.

- Improve term-suggestion logic and integration with other EDAM components.

- Enhance text-based annotation from diverse sources (e.g. package metadata, documentation).

# Data availability

All scripts and materials developed during the hackathon are available in the [EDAM-MCP GitHub repository](https://github.com/edamontology/edammcp).  

The repository also includes benchmarking data, workflow specifications, and issue tracking for ongoing development.

# Acknowledgements

ThisworkwascarriedoutduringtheELIXIRBioHackathonEurope2025,heldinBerlin,Germany,
and organised by [ELIXIR](https://elixir-europe.org/). CR is part of the Institut Français de Bioinformatique (IFB, UAR 3601), 
funded by the Programme d’Investissements d’Avenir through the Agence Nationale de la Recherche (ANR-11-INBS-0013). 
This project also benefited from the Chan Zuckerberg Initiative EOSS6 grants "Software for Science: 
Ontological resource tagging and discovery for Bioconductor" (2024-342819 and 2024-342820). 
HH thanks the RECETOX Research Infrastructure (No LM2023069) financed by the Ministry of Education, Youth and Sports, 
and the Operational Programme Research, Development and Education (the CETOCOEN EXCELLENCE project No. CZ.02.1.01/0.0/0.0/17_043/0009632) 
for supportive background. This work was also supported from the European Union’s Horizon 2020 research 
and innovation program under grant agreement No 857560 (CETOCOEN Excellence). Views and opinions expressed are however those of the author(s) 
only and do not necessarily reflect those of the European Union or HADEA. Neither the European Union nor the granting authority 
can be held responsible for them.

# References