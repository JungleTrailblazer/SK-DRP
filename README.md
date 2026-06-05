# SK-DRP

[![Python](https://img.shields.io/badge/Python-3.8%2B-blue)]()
[![PyTorch](https://img.shields.io/badge/PyTorch-2.x-red)]()
[![CUDA](https://img.shields.io/badge/CUDA-12.1-green)]()
[![License](https://img.shields.io/badge/License-See%20LICENSE.txt-lightgrey)]()
[![Status](https://img.shields.io/badge/Status-Research%20Code%20Release-yellow)]()

This repository provides research code related to the manuscript:

> **Syntax-aware dynamic reasoning with large language models for interpretable knowledge graph question answering**

The associated method, **SK-DRP** short for **Syntax-Knowledge enhanced Dynamic Reasoning with Prompt learning**, studies interpretable knowledge graph question answering through syntax- and knowledge-enhanced question representation, dynamic multi-hop reasoning, and LLM-assisted answer selection.

The current public repository is a cleaned research code release. It mainly contains dataset-specific KGQA scripts, dynamic reasoning components, training and evaluation entry points, and utility modules used in our experimental workflow. It is intended to support code inspection, peer review, and future reproducibility improvements.

Please note that this repository is not yet a fully self-contained one-command reproduction package. Some external resources and auxiliary components, such as benchmark datasets, Freebase-derived files, trained checkpoints, generated LLM outputs, private server paths, and full LLM evidence-generation configurations, are not redistributed in the current release.

## Repository

```text
https://github.com/JungleTrailblazer/SK-DRP
```

## Overview

Knowledge graph question answering aims to answer natural language questions by grounding them in structured knowledge graphs. Compared with purely text-based generation, KGQA can provide explicit entity-relation grounding and more interpretable reasoning traces.

The SK-DRP framework studied in the manuscript contains three main parts:

1. **Syntax- and knowledge-enhanced question representation**, which uses dependency information and KG-oriented knowledge augmentation to improve question understanding.
2. **Dynamic multi-hop reasoning over knowledge graphs**, which performs hop-level path expansion and entity scoring through attention- and gate-based reasoning signals.
3. **LLM-assisted answer selection**, which converts candidate global paths into natural-language evidence and selects final answers from candidate entities.

The current repository focuses on releasing the core KGQA reasoning scripts and dataset-specific experimental pipeline. Additional cleaned materials for preprocessing, configuration, and LLM-assisted evidence generation will be added in future updates.

## Current Release Scope

The current public version includes:

* Dataset-specific scripts for MetaQA, WebQSP, WebQSP_half, and CWQ.
* Training, prediction, demonstration, and evaluation entry points.
* Core KGQA modules for question encoding, relation prediction, entity scoring, hop-level reasoning, and path expansion.
* Utility modules for GRU encoders, schedulers, optimization, metrics, and helper functions.
* Basic dependency information and running notes.

The current public version does **not** include:

* Public benchmark datasets.
* Freebase-derived KG resources.
* Preprocessed local data files.
* Trained checkpoints.
* Generated LLM outputs.
* API keys or proprietary LLM service configurations.
* Private server-specific absolute paths.
* A fully packaged LLM fine-tuning and inference pipeline.
* Complete dataset construction scripts for all external resources.

Users who wish to reproduce the full experimental results should prepare the public datasets and KG resources locally and configure model paths according to their own environment.

## Supported Benchmarks

This repository contains scripts related to the following KGQA benchmarks:

| Dataset     | Description                                                        |
| ----------- | ------------------------------------------------------------------ |
| MetaQA      | Multi-hop question answering over a movie-domain knowledge graph   |
| WebQSP      | Knowledge graph question answering over Freebase-derived resources |
| WebQSP_half | Additional WebQSP variant used in our experimental workflow        |
| CWQ         | ComplexWebQuestions, a compositional multi-hop KGQA benchmark      |

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

## Method Components

The manuscript describes SK-DRP as a modular KGQA framework. The following summary explains the relationship between the method and the current code release.

### 1. Syntax- and Knowledge-Enhanced Representation

The full method uses syntactic dependency information and knowledge-augmented pre-training to improve question representation and KG alignment.

In the manuscript, syntactic information is modeled through a Multi-GRU-style encoder over dependency structures, while knowledge augmentation uses KG-neighborhood linearization, knowledge masking, and contrastive learning.

The current repository includes the main question-encoding and KGQA model scripts used in the experimental workflow. Some cleaned preprocessing resources and auxiliary knowledge-augmentation scripts are not fully packaged in this release.

### 2. Dynamic Multi-hop KG Reasoning

The dynamic reasoning module is the main focus of the released code.

The model performs hop-level reasoning over the knowledge graph by predicting relations, expanding candidate paths, updating entity scores, and aggregating hop-level predictions. This design provides explicit intermediate reasoning signals and supports interpretable path-based answer prediction.

### 3. LLM-Assisted Answer Selection

The manuscript further uses an LLM-assisted stage to verbalize compressed candidate paths and select the final answer from candidate entities.

This stage depends on external LLM resources, generated evidence text, fine-tuning configuration, and local inference settings. The current repository documents this component at a high level, while the full cleaned LLM pipeline and reusable examples are planned for a future update.

## Installation

The scripts were developed with the following environment:

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

Depending on the selected script and dataset, users may also need local paths for pretrained language models, GloVe embeddings, benchmark datasets, KG files, and trained checkpoints.

## Data and External Resources

This repository does not redistribute public benchmark datasets or Freebase-derived resources.

Before running experiments, users should prepare the required resources locally, including but not limited to:

* MetaQA dataset and knowledge graph files.
* WebQSP dataset and Freebase-derived resources.
* CWQ dataset and Freebase-derived resources.
* Pretrained BERT or RoBERTa model identifiers or local directories.
* GloVe pickle file for MetaQA, if required by the selected script.
* Checkpoints for prediction or demonstration scripts.

Suggested environment variables:

```bash
export SK_DRP_ROOT=/path/to/SK-DRP
export BERT_BASE_UNCASED=bert-base-uncased
export ROBERTA_BASE=roberta-base
export GLOVE_PICKLE_PATH=/path/to/glove.840B.300d.pickle
```

## Example Entry Points

The exact commands depend on dataset preparation and local path configuration.

Typical training entry points include:

```bash
python WebQSP/train_hop_final.py --input_dir data/WebQSP
python CWQ/train_final.py --input_dir data/CWQ
python MetaQA/train_final.py --input_dir data/MetaQA
```

For prediction or demonstration, please inspect the corresponding `demo_*.py`, `predict*.py`, and evaluation scripts in each dataset folder.

Checkpoint paths and dataset paths should be configured before running prediction or demonstration scripts.

## Experimental Settings in the Manuscript

The manuscript reports experiments on MetaQA, WebQSP, and CWQ.

The reported experimental setup uses:

* Python 3.8.8
* PyTorch 2.2.1
* CUDA 12.1
* BERT-base-uncased for knowledge-augmented encoding
* Maximum reasoning step of 2 for WebQSP
* Maximum reasoning step of 3 for MetaQA and CWQ
* Entity score threshold of 0.7 during path expansion
* Maximum active local paths of 400
* Llama-2-7B with LoRA for the LLM-based evidence generation setting

These settings are provided for reference. Exact reproduction may require additional dataset preprocessing, Freebase resource construction, checkpoint preparation, and LLM-related configuration.

## Reproducibility Notes

This repository improves transparency by releasing implementation scripts and dataset-specific entry points. However, exact reproduction of all reported results is not guaranteed by this repository alone.

Results may vary depending on:

* Dataset preprocessing.
* Freebase subset construction.
* Entity and relation indexing.
* Pretrained model versions.
* Checkpoint availability.
* Random seed settings.
* CUDA and hardware environment.
* LLM backend and decoding settings.
* Local path configuration.

Users who want to build on this repository should first prepare the public datasets and KG resources, then adapt the scripts to their local environment.

## Planned Updates

We plan to improve this repository with additional cleaned and reusable materials, including:

* More detailed dataset preparation instructions.
* Example configuration files.
* Minimal runnable toy examples.
* Cleaner command-line interfaces.
* Docker or Conda environment support.
* Additional notes for reproducing reported experimental settings.
* Optional scripts for path compression and path-to-text evidence construction.
* Documentation for LLM-assisted answer selection.
* Additional preprocessing scripts where redistribution is possible.

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

This repository is maintained as a research code release for peer review, code inspection, and follow-up work.

Issues and pull requests are welcome for:

* Documentation improvements.
* Environment setup fixes.
* Dataset preparation notes.
* Bug reports.
* Compatibility updates for newer Python, PyTorch, CUDA, or transformer versions.
* Additional benchmark support.
* Suggestions for simplifying the running pipeline.

Because this is a research code release, some paths and external resources may need to be configured manually. If you encounter unclear instructions, please open an issue with your environment details and the command you tried to run.

## License

See `LICENSE.txt` for license information.

## Contact

For questions, bug reports, or documentation suggestions, please open a GitHub issue in this repository.
