# Agnostic jet anomaly detection with foundation models

<div align="center">

Soumya Shaw, Anna Hallin, Gregor Kasieczka, Michael Krämer, Louis Moureaux, Alexander Mück, Humberto Reyes-González


[![arXiv](https://img.shields.io/badge/arXiv-2403.05618-b31b1b.svg)](https://arxiv.org/abs/2403.05618)
[![pytorch](https://img.shields.io/badge/PyTorch_2.2-ee4c2c?logo=pytorch&logoColor=white)](https://pytorch.org/get-started/locally/)
[![lightning](https://img.shields.io/badge/-Lightning_2.2.1-792ee5?logo=pytorchlightning&logoColor=white)](https://pytorchlightning.ai/)
[![hydra](https://img.shields.io/badge/Config-Hydra_1.3-89b8cd)](https://hydra.cc/)

</div>

This repository contains the code for the results presented in the paper
'[Agnostic jet anomaly detection with foundation models](https://arxiv.org/abs/2403.05618)'.

The code was also used in:

[![arXiv](https://img.shields.io/badge/arXiv-2412.10504-b31b1b.svg)](https://arxiv.org/abs/2412.10504)
[![arXiv](https://img.shields.io/badge/arXiv-2503.19165-b31b1b.svg)](https://arxiv.org/abs/2503.19165)

**Abstract**:

> Foundation models are multi-dataset and multi-task machine learning methods that once pre-trained can be fine-tuned for a large variety of downstream applications. The successful development of such general-purpose models for physics data would be a major breakthrough as they could improve the achievable physics performance while at the same time drastically reduce the required amount of training time and data.
> We report significant progress on this challenge on several fronts. First, a comprehensive set of evaluation methods is introduced to judge the quality of an encoding from physics data into a representation suitable for the autoregressive generation of particle jets with transformer architectures (the common backbone of foundation models). These measures motivate the choice of a higher-fidelity tokenization compared to previous works. Finally, we demonstrate transfer learning between an unsupervised problem (jet generation) and a classic supervised task (jet tagging) with our new OmniJet-α model. This is the first successful transfer between two different and actively studied classes of tasks and constitutes a major step in the building of foundation models for particle physics.

<img src=assets/OmniJet_different_stages_overview.excalidraw.png width=900 style="border-radius:10px">

______________________________________________________________________

**Table of contents**:

- [Agnostic jet anomaly detection with foundation models](#omnijet-%CE%B1-the-first-cross-task-foundation-model-for-particle-physics)
  - [How to run the code](#how-to-run-the-code)
    - [Dataset](#dataset)
    - [Installation](#installation)
    - [Tokenization](#tokenization)
    - [Generative training](#generative-training)
    - [Transfer learning / Classifier training](#transfer-learning--classifier-training)
  - [Citation](#citation)

## How to run the code

### Dataset

Instructions on how to download the dataset can be found in the
repository [jet-universe/particle_transformer](https://github.com/jet-universe/particle_transformer).

### Installation

The recommended (and by us tested) way of running the code is to use the
provided docker image at
[`jobirk/omnijet` on DockerHub](https://hub.docker.com/repository/docker/jobirk/omnijet/general).
The requirements listed in `docker/requirements.txt` are installed in the `conda` environment
`base` of the base image (official pytorch image).
Thus, you have to make sure that the `conda` environment is activated when running the code,
which can be done with `source /opt/conda/bin/activate`.

An interactive session inside a container can be started by running the following command:

```shell
# on a machine with Singularity
singularity shell docker://jobirk/omnijet:latest  # start a shell in the container
source /opt/conda/bin/activate  # activate the conda environment in the container
#
# on a machine with Docker
docker run -it --rm jobirk/omnijet:latest bash  # start a shell in the container
source /opt/conda/bin/activate  # activate the conda environment in the container
```

Alternatively, you can install the requirements from the `docker/requirements.txt` file, but
you'll have to add `pytorch` to the list of requirements, since this is not
included in the `requirements.txt` file (we use the official pytorch image as
base image).

Furthermore, you'll have to add/create a `.env` file in the root of the project
with the following content:

```bash
JETCLASS_DIR="<path to the jetclass dataset i.e. where the train_100M, val_5M, .. folders are>"
JETCLASS_DIR_TOKENIZED="<path to where you want to save the tokenized jetclass dataset>"

# stuff for hydra
LOG_DIR="<path to log dir>"
COMET_API_TOKEN="<your comet api token>"
HYDRA_FULL_ERROR=1
```