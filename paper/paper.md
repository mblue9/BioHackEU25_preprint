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
- name: Helge Hecht
  orcid: "0000-0001-6744-996X"
  affiliation: 2
- name: Vincent J Carey
  orcid: "0000-0003-4046-0063"
  affiliation: 3
- name: Maria A Doyle
  orcid: "0000-0003-4847-8436"
  affiliation: 4
- name: Alban Gaignard
  orcid: "0000-0002-3597-8557"
  affiliation: 5
- name: Hervé Ménager
  orcid: "0000-0002-7552-1009"
  affiliation: 6, 7
- name: Júlia Mir
  orcid: "00000-0001-6104-9260"
  affiliation: 8
- name: Claire Rioualen
  orcid: "0000-0002-7684-8679"
  affiliation: 7
---
# Abstract

Identifying the most appropriate bioinformatics tool for a task remains challenging across multiple domains. 
Annotating tools with EDAM ontology terms (e.g. topics, operations, input / output data and formats) can help 
but manual annotation is labour-intensive, error-prone, and difficult to scale, 
particularly given the high rate of first-time package developers in academic environments. 

At BioHackathon Europe 2025, our team explored how Large Language Models (LLMs) can assist this process through the Model Context Protocol (MCP),
an emerging open standard that specifies how LLMs call external functions, using metabolomics as a domain use case. 
We developed an MCP-based workflow that grounds tool descriptions in the EDAM ontology (Ison et al., 2013), 
improving reproducibility and semantic precision. Two core modules, entry-point specification and semantic text segmentation,
were completed during the hackathon, while additional mapping, validation, and reporting functions were outlined for follow-up development. 
Benchmarking integrated with the BioChatter framework (Lobentanzer et al., 2025) demonstrated that MCP-assisted models 
outperform unconstrained baselines on initial tests using metabolomics packages from bio.tools (Ison et al., 2019). 

Ongoing work will expand benchmarking datasets, refine term-mapping logic, and extend the workflow to proteomics, 
supporting scalable, ontology-driven annotation across the ELIXIR ecosystem.

# Introduction

## Background

[Metabolomics](https://elixir-europe.org/communities/metabolomics), 
[fluxomics](https://elixir-europe.org/internal-projects/commissioned-services/fluxomics-workflows), 
and [proteomics](https://elixir-europe.org/communities/proteomics) face ongoing interoperability challenges despite shared platforms and analytical frameworks
(e.g. MS-based workflows and bioimaging pipelines).  

While these communities have advanced standardization through [EDAM-annotated Bioconductor packages](https://doi.org/10.37044/osf.io/dsgnw_v1) 
(Rioualen et al., 2025), several gaps persist:  

1. Manual tool annotation in [bio.tools](https://bio.tools) is labour-intensive and often inconsistent.

2. Tool discoverability for experimentalists remains limited.

3. Integration across ELIXIR services (Galaxy, WorkflowHub, BioContainers) requires strengthening, 
as identified in [ELIXIR commissioned services](https://elixir-europe.org/internal-projects/commissioned-services/proteomics-pipelines) 
and a related [white paper](https://f1000research.com/articles/6-875/v1) (Viscaíno et al., 2017).

This  project was initiated at the [2025 ISMB CollaborationFest](https://doi.org/10.12688/f1000research.169977.1): 
[Improving how we describe and discover Bioinformatics tools](https://github.com/mblue9/biocedam-cofest-2025). 
As a result, a first draft of the EDAM MCP server was developed to improve LLM-based annotations. 

The MCP is an emerging open standard for structured interaction between LLMs and external tools. 
It defines how models can call external functions—such as ontology lookups or API queries—instead of relying purely on text generation. 
In our context, MCP acts as a control layer ensuring that annotations derived from natural language remain 
grounded in the EDAM ontology and are both reproducible and machine-readable.

## Objectives

This project aims to enhance the accessibility and interoperability of metabolomics and proteomics tools 
by leveraging Large Language Models (LLMs) for automated, ontology-driven annotation.  

We focus on integrating the [EDAM ontology](https://doi.org/10.7490/f1000research.1118900.1) with the MCP framework to provide structured, 
reproducible annotations of bioinformatics resources.

Key objectives include:

- Developing an LLM-based querying system for the bio.tools API.  

- Strengthening interoperability between omics tools, ELIXIR services, and Bioconductor packages.  

- Facilitating user-friendly, structured search and guided workflow usage within Galaxy’s [Metabolomics](https://workflow4metabolomics.usegalaxy.fr/) 
and [Proteomics](https://proteore.org/) environments.  

# Results

## Overview of the BioHackathon results

![xx](figures/Fig1_biohack25_poster.png)

![xx](figures/Fig2_llm_hallucination.png)

## EDAM-MCP workflow

![xx](figures/Fig3_edam_mcp_workflow.png)

## Use case: Metabolomics packages

## Benchmarking

![xx](figures/Fig4_mcp_baseline_model_comparison.png)

# Discussion 

# Data availability

# Acknowledgements

# References
