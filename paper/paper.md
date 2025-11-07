---
title: 'BioHackEU25 report: Improving package annotation in metabolomics and proteomics via robust, ontology-driven LLM integration'
title_short: 'BioHackEU25 #17: improving package annotation'
tags:
  - Bioinformatics
  - EDAM
  - metabolomics
authors:
  - name: First Author
    affiliation: 1
    role: Writing – original draft
  - name: Last Author
    orcid: 0000-0000-0000-0000
    affiliation: 2
    role: Conceptualization, Writing – review & editing
affiliations:
  - name: First Affiliation
    index: 1
  - name: ELIXIR Europe
    ror: 044rwnt51
    index: 2
date: 7 November 2025
cito-bibliography: paper.bib
event: BH25EU
biohackathon_name: "BioHackathon Europe 2025"
biohackathon_url:   "https://biohackathon-europe.org/"
biohackathon_location: "Berlin, Germany, 2025"
group: Project 13
# URL to project git repo --- should contain the actual paper.md:
git_url: https://github.com/biohackrxiv/publication-template
# This is the short authors description that is used at the
# bottom of the generated paper (typically the first two authors):
authors_short: First Author \emph{et al.}
---


# Introduction

## Background
Metabolomics, fluxomics and proteomics face interoperability challenges despite shared platforms and analysis tools (e.g. MS-based workflows, bioimaging processing). While these communities have advanced standardization through EDAM-annotated Bioconductor packages, key gaps remain: (1) manual tool annotation in bio.tools is labor-intensive and inconsistent, (2) discoverability for experimentalists needs improvement, and (3) integration across ELIXIR services (Galaxy, WorkflowHub, Biocontainers) requires strengthening (as stated in this commissioned service and this white paper).

## Objectives
We seek to enhance the accessibility and interoperability of metabolomics and proteomics tools by leveraging Large Language Models (LLMs) and automated annotations. The first steps will be to develop an LLM-based querying system for the bio.tools API and strengthen the integration between omics tools and ELIXIR services, as well as the large collection of related Bioconductor packages. Additionally, the project aims to improve end-user accessibility by facilitating structured queries and guided workflow usage, particularly through platforms like Galaxy’s Metabolomics and Proteomics environments.

- Implement two layers of an abstract process (as prototyped in EDAM-MCP):
  - Identify whether a term exists in EDAM (if so, return)
  - If not, suggest a term (including where it should go in EDAM)
- Cater to user groups for facilitating annotation for users or automatically (bio.tools, Bioconductor, nf-core, Galaxy, Application scenarios: single user wants to annotate concrete instance; ecosystem maintainer wants to automate annotations (human-in-the-loop)
- Use case: metabolomics
  - Core set of well-annotated tools - Collect, Formalise, Introduce into benchmark
- Benchmark the utility of the developed functions / MCP on the domain dataset
  - Q&A of example packages and the questions a user might have for them

# Results

## Use case: Metabolomics packages
We identified ~20 metabolomics tools that are well-annotated in bio.tools for use in testing EDAM annotations (listed in this spreadsheet). Some of these bio.tools entries were manually annotated/improved with EDAM terms during the hackathon. We manually created questions and answers about two of these tools for the benchmarking. Examples question-answer pairs for the xcms tool are here: https://github.com/edamontology/edammcp/blob/main/benchmark/xcms/qa.tsv
Next step is to create a script that can be run against biotools API for a given package and generates Q&A datasets for that package.

## EDAM-MCP workflow
The challenge of applying an MCP-driven agent is to clearly and unambiguously instruct the agent with a multi-step task in order to reduce probabilistic problems in the copilot behaviour. We aim to structure the EDAM MCP approach into:
an entry point that explains available functions to the copilot
a set of deterministic functions that allow the copilot to flexibly apply the instructions
To operationalize this approach, the EDAM MCP workflow introduces a structured process that separates high-level planning from low-level execution. After the entry point provides the copilot with task context and a concise map of available functions, the agent performs tool discovery and ranking using semantic metadata such as EDAM operations, data types, and constraints. This enables the agent to select the most relevant tools without requiring full parameter schemas upfront.
Once the plan is formed, the agent retrieves detailed JSON signatures only for shortlisted tools and executes them through deterministic function calls. This design minimizes ambiguity, reduces probabilistic decision-making, and ensures reproducibility across multi-step tasks. By leveraging EDAM for semantic alignment, the workflow supports flexible orchestration while maintaining clarity and control. A graphical overview of these modules and their interactions is provided in https://github.com/edamontology/edammcp/issues/37, it contains a graphical overview of the modules, also below.

![EDAM-MCP workflow](figures/edam_mcp_workflow.png) 

*Segmentation*
During the workflow design, an optional Segmentation step was introduced to break large free text input into chunks-  topics and keywords. This ensures clarity in planning and reduces ambiguity in multi-step execution. In addition, a Commonsense validation layer is planned to check the segmented steps for logical consistency and feasibility before execution, acting as a safeguard against nonsensical or incomplete plans.

*Mapping of EDAM branches*
In our initial approach, the MCP server returned EDAM terms regardless of EDAM branch type (Operation, Topic, Format, Data). For instance, if we provide a text description with a few sentences covering both Data and Operation, we may have imbalanced results in terms of concept types and we may also retrieve EDAM Topics. To overcome this potential issue, we are investigating if we can split the mapping work into more specific tasks.  We began work on searching for only Operations (PR https://github.com/edamontology/edammcp/pull/51). Topics, Data, Formats will come in a future PR if this one is accepted. We still need to decide between four single mapping tools (for operations, data, topics, formats) or one combined mapping tool relying on these sub-mapping tasks. 

## Benchmarking
Prior to the hackathon, we had outlined scenarios for benchmarking the EDAM-MCP workflow in a GitHub issue: https://github.com/edamontology/edammcp/issues/20
During the hackathon, we started work on a programmatic approach, integrated with the biochatter benchmarking framework, that can evaluate the MCP based on the questions created from the well-annotated packages: https://github.com/edamontology/edammcp/tree/main/benchmark
The automatically created benchmark questions are then transformed into the idiomatic biochatter benchmark format and executed in the biochatter suite, comparing naive model baselines to MCP-supported models. Merged in https://github.com/biocypher/biochatter/pull/321
Preliminary results indicate that most models benefit from having the MCP support, even though the suite is far from complete. Claude Haiku is an exception, performing quite well on the (very simple) initial development benchmark of 10 questions. The benchmark should be continuously updated to inform the development of EDAM MCP. Further, EDAM MCP should be versioned to clarify which version was used to achieve which performance (this needs to be recorded in the benchmark results).

![MCP baseline improvement summary](figures/mcp_baseline_improvement_summary.png) 
![MCP baseline model comparison](figures/mcp_baseline_model_comparison.png) 
![MCP baseline overall comparision summary](figures/mcp_baseline_overall_comparison.png) 
![MCP baseline subtask comparison](figures/mcp_baseline_subtask_comparison.png) 




![Poster from mid-week reporting](figures/biohack25_poster.png) 


# Discussion

...

## Acknowledgements

...

## References
