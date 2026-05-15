# SK-DRP

This repository contains the cleaned implementation scripts for the manuscript **"Syntax-aware dynamic reasoning with large language models for interpretable knowledge graph question answering"**.

The repository is prepared to support peer review and reproducibility. It contains source scripts, configuration notes, dependency information, and entry points for MetaQA, WebQSP, and CWQ experiments. Public benchmark datasets, Freebase resources, model checkpoints, API keys, generated LLM outputs, and server-specific paths are not included.

## Repository link in the manuscript

Before submission, replace the placeholder below with the final public GitHub URL:

```text
https://github.com/USERNAME/SK-DRP
```

## Contents

- `MetaQA/`: scripts for MetaQA data processing, training, and prediction.
- `WebQSP/`: scripts for WebQSP data loading, training, and evaluation.
- `CWQ/`: scripts for CWQ data loading, training, and evaluation.
- `WebQSP_half/`: additional WebQSP variant scripts.
- `utils/`: shared GRU, scheduler, metric, and optimizer utilities.
- `requirements.txt`: Python package requirements.
- `GITHUB_UPLOAD_GUIDE.md`: step-by-step upload guide.

## Dependencies

Install dependencies with:

```bash
pip install -r requirements.txt
```

The scripts were developed with Python 3.8.8, PyTorch 2.2.1, and CUDA 12.1. Exact results may vary with hardware, dataset preprocessing, pretrained-model versions, and LLM backend.

## Data and model paths

Before running, replace local paths in the scripts with your own paths for:

- MetaQA, WebQSP, and CWQ datasets.
- Freebase subset resources for WebQSP and CWQ.
- Pretrained BERT/RoBERTa model identifiers or local directories.
- GloVe pickle file for MetaQA, if used.
- Checkpoint paths for prediction or demos.

Suggested environment variables:

```bash
export SK_DRP_ROOT=/path/to/this/repository
export BERT_BASE_UNCASED=bert-base-uncased
export ROBERTA_BASE=roberta-base
export GLOVE_PICKLE_PATH=/path/to/glove.840B.300d.pickle
```

## Example entry points

The exact commands depend on dataset preparation. Typical entry points are:

```bash
python WebQSP/train_hop_final.py --input_dir data/WebQSP
python CWQ/train_final.py --input_dir data/CWQ
python MetaQA/train_final.py --input_dir data/MetaQA
```

For prediction, inspect the corresponding `demo_*.py` and `predict*.py` scripts in each dataset folder and set checkpoint paths accordingly.

## Reproducibility notes

The code release improves transparency but does not fully package every external artifact needed to reproduce all reported numbers. Users still need to obtain the public benchmark datasets, prepare Freebase-derived resources following the paper, install pretrained language models, and configure the LLM path-to-text generation and answer-selection pipeline.

## License

See `LICENSE.txt`.
