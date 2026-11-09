# Network Intrusion Detection with Deep Learning

A comparative study of convolutional, residual, and diffusion-based approaches to network intrusion detection using the **CICIDS2017** dataset. This repository contains data-preparation notebooks, supervised classification experiments, and an adaptation of **IMDiffusion** for anomaly detection.

## Overview

The project explores how different representations and learning paradigms affect intrusion detection in network-flow data:

- **2D CNN (15 selected features):** a compact feature representation padded to a 4 × 4 grid and processed with a PyTorch CNN.
- **2D CNN (64 PCA components):** a higher-dimensional representation reduced to 64 components, reshaped to 8 × 8, and classified with a TensorFlow/Keras CNN.
- **ResNet-1D:** a PyTorch residual convolutional network trained for binary classification on tabular network-flow features.
- **IMDiffusion:** a diffusion-based anomaly-detection approach, adapted from the original IMDiffusion implementation and evaluated on CICIDS data.

**Scope:** The CNN and ResNet experiments are supervised classifiers; IMDiffusion follows a different, unsupervised anomaly-detection approach. Their scores should not be interpreted as directly comparable without accounting for label definitions, preprocessing, and evaluation protocols.

## Repository structure

```text
.
├── Preprocess_CICIDS2017_Download.ipynb  # Download and initial data preparation
├── preprocess_CICIDS_15features.ipynb    # Prepare features and pickle datasets
├── cnn2d_15Features(main Theme).ipynb    # PyTorch 2D CNN, 15 features + padding
├── cnn2d_64Features.ipynb                # Keras 2D CNN, 64 PCA components
├── resnet1d.ipynb                        # PyTorch 1D residual classifier
└── IMDiffusion/
    ├── config/base.yaml                  # Diffusion/training configuration
    ├── dataset.py                        # Dataset loading
    ├── diff_models.py                    # Diffusion model components
    ├── main_model.py                     # Model and training utilities
    ├── exe_machine.py                    # Training entry point
    ├── evaluate_machine_window_middle.py # Inference entry point
    ├── compute_score.py                  # Threshold scoring
    ├── ensemble_proper.py                # Evaluation/aggregation
    └── ensemble_residual/CICIDS.csv      # Saved CICIDS evaluation output
```

The archive also contains generated checkpoints, IDE metadata, and Python caches; these are not required to understand the source code and can be excluded from the public repository.

## Dataset

The experiments use the **CICIDS2017** network-intrusion dataset, including an improved CICIDS2017 distribution accessed in the download notebook through KaggleHub.

1. Run `Preprocess_CICIDS2017_Download.ipynb` to obtain and combine the source CSV files.
2. Use `preprocess_CICIDS_15features.ipynb` to prepare the selected-feature data and serialize train/test arrays for the PyTorch experiments.
3. Update the dataset paths in each notebook before execution. Several paths currently refer to Google Colab, Google Drive, or a local Linux directory.

An existing preprocessed dataset was also linked in the original project notes: [preprocessed CICIDS2017 files on Google Drive](https://drive.google.com/drive/folders/1P-k3BaHqmmvr0ZcVRuQqAyvgbyzGgD-c?usp=sharing). Availability and contents of this external folder are not guaranteed.

**Data handling:** The notebooks include IP-address conversion, label encoding, scaling, and train/test splitting. Check each notebook's actual feature selection and preprocessing sequence before running or comparing experiments. In particular, fit learned preprocessing transformations on training data only when constructing a new leakage-free benchmark.

Large datasets and generated pickle files are not included in this repository.

## Models

| Model | Framework | Representation | Task |
| --- | --- | --- | --- |
| 2D CNN — 15 features | PyTorch | 15 features + 1 padding value → 4 × 4 | Supervised classification |
| 2D CNN — 64 components | TensorFlow / Keras | PCA → 64 components → 8 × 8 | Supervised classification |
| ResNet-1D | PyTorch | 1D network-flow feature vector | Supervised binary classification |
| IMDiffusion | PyTorch | Multivariate feature sequences/windows | Unsupervised anomaly detection |

The two CNN experiments use different input representations and implementations. The 64-component experiment uses **PCA**, rather than simply selecting 64 original features.

## Getting started

### 1. Clone the repository

```bash
git clone https://github.com/<your-username>/<your-repository>.git
cd <your-repository>
```

Replace the placeholders with your repository details.

### 2. Install dependencies

Use a Python environment with Jupyter. The notebooks and scripts use packages including:

```bash
python -m pip install jupyter numpy pandas scipy scikit-learn matplotlib seaborn \
  torch tensorflow pyyaml tqdm kagglehub
```

This is a **convenience dependency list**, not a tested lockfile. Package versions, CUDA support, and additional imports may need adjustment for your environment.

### 3. Run the experiments

Open the relevant notebook, update the file paths, and run its cells in order:

```bash
jupyter notebook
```

Suggested order:

1. `Preprocess_CICIDS2017_Download.ipynb`
2. `preprocess_CICIDS_15features.ipynb` (for pickle-based experiments)
3. One of the model notebooks

The notebooks were developed with environment-specific paths and are **not guaranteed to run without modification**.

### IMDiffusion notes

The `IMDiffusion/` folder is based on the [original IMDiffusion project](https://github.com/alirznmt/IMDiffusion) / its associated research implementation. Its scripts expect dataset pickle files in the structure configured by the code (notably under `data/Machine/`) and dataset-specific feature dimensions.

The included `exe_machine.py` and evaluation script contain dataset-specific branches. **CICIDS is not fully registered in every feature-dimension branch**, so these entry points may require modification before a clean CICIDS run. Do not assume the generic upstream command will run unchanged for CICIDS.

Configuration parameters are in `IMDiffusion/config/base.yaml`. The included configuration specifies a 50-step diffusion process; training settings and experiment loops may also be overridden in the Python scripts.

## Evaluation

The classification notebooks compute metrics such as accuracy, precision, recall, F1-score, classification reports, and confusion matrices. The IMDiffusion workflow includes anomaly scoring and threshold-based evaluation.

A saved IMDiffusion output is available at `IMDiffusion/ensemble_residual/CICIDS.csv`. Its recorded aggregate values are:

| Metric | Value |
| --- | ---: |
| Precision | 0.7857 |
| Recall | 0.1367 |
| F1-score | 0.2328 |

These values are reproduced from the supplied CSV artifact, **not independently rerun or validated**. They represent that saved experiment only, not a comprehensive benchmark across all four models. No cross-model ranking is asserted here.

## Reproducibility and limitations

- Input datasets and some preprocessed pickle files are external to the repository.
- Several notebook paths are specific to the original development environment.
- Some IMDiffusion scripts require CICIDS-specific configuration changes.
- Random splits, scaling procedures, and class-label mappings should be checked before comparing results.
- A fully automated end-to-end pipeline, pinned dependency environment, and independently verified benchmark are not provided.

## Acknowledgments and references

- **CICIDS2017:** Canadian Institute for Cybersecurity, University of New Brunswick. [Dataset information](https://www.unb.ca/cic/datasets/ids-2017.html).
- **IMDiffusion:** Chen et al., *ImDiffusion: Imputed Diffusion Models for Multivariate Time Series Anomaly Detection* (2023). [arXiv:2307.00754](https://arxiv.org/abs/2307.00754). The diffusion-based component builds on this work; credit for the original method belongs to its authors.

## License

No license is specified in the supplied project. Add a license only after checking the terms applicable to the original IMDiffusion code and any other reused materials.
