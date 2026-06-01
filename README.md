# SK-DRP

[![Python](https://img.shields.io/badge/Python-3.8%2B-blue)]()
[![PyTorch](https://img.shields.io/badge/PyTorch-2.x-red)]()
[![CUDA](https://img.shields.io/badge/CUDA-12.1-green)]()
[![License](https://img.shields.io/badge/License-See%20LICENSE.txt-lightgrey)]()
[![Status](https://img.shields.io/badge/Status-Research%20Release-yellow)]()

**SK-DRP** is an open-source research codebase for **Syntax-Knowledge enhanced Dynamic Reasoning with Prompt learning for interpretable knowledge graph question answering**.

This repository contains the cleaned implementation scripts for the manuscript:

> **Syntax-aware dynamic reasoning with large language models for interpretable knowledge graph question answering**

The project is designed to support peer review, reproducibility, and future research on LLM-assisted multi-hop reasoning over knowledge graphs.

## Repository

```text
https://github.com/JungleTrailblazer/SK-DRP
```

## Overview

Knowledge graph question answering (KGQA) aims to answer natural language questions by grounding them in structured knowledge graphs. Compared with pure language-model-based question answering, KGQA can provide more reliable factual grounding and more interpretable reasoning traces.

However, existing KGQA systems still face several challenges:

* Complex and compositional questions are difficult to represent accurately.
* Multi-hop reasoning paths are often not sufficiently transparent.
* Local triple-level reasoning may ignore global path-level evidence.
* LLM-assisted KGQA systems may introduce additional cost, latency, and hallucination risks.

**SK-DRP** addresses these challenges through a modular framework that combines:

1. Syntax- and knowledge-enhanced question representation.
2. Dynamic multi-hop path reasoning over knowledge graphs.
3. LLM-assisted answer selection based on compressed global reasoning paths.

The repository provides source scripts, configuration notes, dependency information, and dataset-specific entry points for experiments on **MetaQA**, **WebQSP**, and **ComplexWebQuestions (CWQ)**.

## Key Features

* Syntax-aware question representation using dependency-based encoding.
* Knowledge-enhanced language-model representations aligned with KG triples.
* Dynamic reasoning module for explicit hop-level path expansion.
* Candidate path scoring and answer prediction over knowledge graphs.
* LLM-assisted global path verbalization and answer selection.
* Dataset-specific scripts for MetaQA, WebQSP, and CWQ.
* Research-oriented implementation for reproducibility and future extension.

## Supported Benchmarks

This repository contains scripts for the following KGQA benchmarks:

| Dataset | Description                                                        |
| ------- | ------------------------------------------------------------------ |
| MetaQA  | Multi-hop question answering over a movie-domain knowledge graph   |
| WebQSP  | Knowledge graph question answering over Freebase-derived resources |
| CWQ     | Complex multi-hop question answering with compositional questions  |

## Repository Structure

```text
SK-DRP/
├── MetaQA/             # MetaQA data processing, training, prediction, and evaluation scripts
├── WebQSP/             # WebQSP data loading, training, prediction, and evaluation scripts
├── WebQSP_half/        # Additional WebQSP variant scripts
├── CWQ/                # CWQ data loading, training, prediction, and evaluation scripts
├── utils/              # Shared GRU, scheduler, metric, optimizer, and helper utilities
├── requirements.txt    # Python package requirements
├── LICENSE.txt         # License information
└── README.md           # Project documentation
```

## Method Summary

SK-DRP follows a modular architecture consisting of three main components.

### 1. Syntax- and Knowledge-Enhanced Question Representation

The model first builds enhanced question representations by combining syntactic and knowledge-aware features.

The syntax-aware component uses dependency information to capture structural relations between question tokens. The knowledge-enhanced component uses knowledge masking and contrastive learning over KG-derived textual representations to improve alignment between natural language questions and structured KG entities and relations.

### 2. Dynamic Reasoning over Knowledge Graphs

The dynamic reasoning module performs explicit multi-hop path expansion. At each reasoning hop, the model generates an indicator vector from the enhanced question representation and uses it to guide local path matching, entity scoring, and path expansion.

This design allows the reasoning process to be inspected step by step, improving interpretability compared with purely embedding-based answer selection.

### 3. LLM-Assisted Answer Selection

After dynamic reasoning, SK-DRP obtains candidate global paths and candidate answer entities. Candidate paths are compressed and transformed into natural-language evidence. A fine-tuned LLM then selects the final answer from the candidate answer set using the generated evidence.

This module is used for global path-level reasoning and answer selection rather than for every path-expansion step, reducing repeated LLM calls during graph traversal.

## Installation

The scripts were developed with:

* Python 3.8.8
* PyTorch 2.2.1
* CUDA 12.1

Install the dependencies with:

```bash
pip install -r requirements.txt
```

A virtual environment is recommended:

```bash
python -m venv skdrp_env
source skdrp_env/bin/activate
pip install -r requirements.txt
```

For Windows PowerShell:

```powershell
python -m venv skdrp_env
.\skdrp_env\Scripts\Activate.ps1
pip install -r requirements.txt
```

## Data and External Resources

This repository does **not** redistribute public benchmark datasets, Freebase-derived resources, pretrained model checkpoints, API keys, generated LLM outputs, or server-specific paths.

Before running experiments, users need to prepare the following resources locally:

* MetaQA dataset and knowledge graph files.
* WebQSP dataset and Freebase-derived resources.
* CWQ dataset and Freebase-derived resources.
* Pretrained BERT or RoBERTa model identifiers or local directories.
* GloVe pickle file for MetaQA if required by the selected script.
* Checkpoints for prediction or demonstration scripts.
* LLM backend or locally fine-tuned model paths for evidence generation and answer selection.

Suggested environment variables:

```bash
export SK_DRP_ROOT=/path/to/SK-DRP
export BERT_BASE_UNCASED=bert-base-uncased
export ROBERTA_BASE=roberta-base
export GLOVE_PICKLE_PATH=/path/to/glove.840B.300d.pickle
```

For Windows PowerShell:

```powershell
$env:SK_DRP_ROOT="D:\path\to\SK-DRP"
$env:BERT_BASE_UNCASED="bert-base-uncased"
$env:ROBERTA_BASE="roberta-base"
$env:GLOVE_PICKLE_PATH="D:\path\to\glove.840B.300d.pickle"
```

## Example Entry Points

The exact commands depend on dataset preparation and local path configuration.

Typical training entry points are:

```bash
python WebQSP/train_hop_final.py --input_dir data/WebQSP
python CWQ/train_final.py --input_dir data/CWQ
python MetaQA/train_final.py --input_dir data/MetaQA
```

For prediction or demonstration, inspect the corresponding `demo_*.py`, `predict*.py`, and evaluation scripts in each dataset folder. Checkpoint paths should be configured before running prediction scripts.

## Reproducibility Scope

This repository is a cleaned research release. It improves transparency by making the implementation scripts and experiment entry points publicly available.

However, the repository does not fully package every external artifact required to reproduce all reported numbers directly. Exact results may vary depending on:

* Dataset preprocessing.
* Freebase resource construction.
* Pretrained language model versions.
* LLM backend and generation settings.
* Hardware environment.
* Random seeds.
* Checkpoint availability.
* Local path configuration.

Users who want to reproduce the experiments should obtain the public benchmark datasets, prepare Freebase-derived resources, configure pretrained model paths, and follow the experiment settings described in the associated manuscript.

## Experimental Setting

The main experiments are conducted on MetaQA, WebQSP, and CWQ.

The implementation uses:

* Python 3.8.8
* PyTorch 2.2.1
* CUDA 12.1

The dynamic reasoning module expands candidate paths over the knowledge graph and produces entity scores. The LLM-assisted answer-selection module uses compressed candidate paths as evidence for final answer selection.

For detailed experimental settings, please refer to the associated manuscript.

## Citation

If you use this repository in your research, please cite the associated manuscript:

```bibtex
@misc{yuan2026skdrp,
  title        = {Syntax-aware dynamic reasoning with large language models for interpretable knowledge graph question answering},
  author       = {Yuan, Ling and Wu, Bicheng and Yang, Yangyang},
  year         = {2026},
  note         = {Manuscript under review}
}
```

Please update this BibTeX entry after the manuscript is formally published.

## Authors

* Ling Yuan
* Bicheng Wu
* Yangyang Yang

## Maintenance and Contributions

This repository is maintained as a research codebase for peer review, reproducibility, and follow-up work.

Issues and pull requests are welcome for:

* Documentation improvements.
* Environment setup fixes.
* Dataset preparation notes.
* Bug reports.
* Compatibility updates for newer Python, PyTorch, CUDA, or transformer versions.
* Additional benchmark support.
* Suggestions for simplifying the running pipeline.

Because this is a research release, some paths and external resources may need to be configured manually. If you encounter unclear instructions, please open an issue with your environment details and the command you tried to run.

## Roadmap

Planned improvements include:

* More detailed dataset preparation instructions.
* Example configuration files.
* Minimal runnable toy examples.
* Docker or Conda environment support.
* Cleaner command-line interfaces.
* Additional notes for reproducing reported experimental settings.
* Optional scripts for preparing path-to-text evidence examples.

## License

See `LICENSE.txt` for license information.

## Contact

For questions, bug reports, or documentation suggestions, please open a GitHub issue in this repository.
