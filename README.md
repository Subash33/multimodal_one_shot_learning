Multimodal One-Shot Learning of Speech and Images
=================================================

Overview
--------
This repository contains the full code recipe for building models that can acquire novel concepts from only *one* paired audio-visual example per class, without receiving any hard labels. These models can then be used to match new continuous speech input to the correct visual instance (e.g. the spoken word "lego" is matched to the visual signal of lego, without receiving any textual labels, and after seeing only a single paired speech-image example of a different *lego* instance). This is *multimodal one-shot learning*, a new task which we formalise in the following paper:

- R. Eloff, H. A. Engelbrecht, H. Kamper, "Multimodal One-Shot Learning of Speech and Images," *arXiv preprint arXiv:1811.03875*, 2018. [[arXiv](https://arxiv.org/abs/1811.03875)]

Please cite this paper if you use the code.

Datasets
--------

The following datasets are required for these experiments:

- [TIDigits](https://catalog.ldc.upenn.edu/LDC93S10)
- [Flickr audio](https://groups.csail.mit.edu/sls/downloads/flickraudio/)
- [Flickr8k text](http://nlp.cs.illinois.edu/HockenmaierGroup/Framing_Image_Description/Flickr8k_text.zip)

Note that the Flickr8k text corpus is used purely for obtaining train/validation/test splits.
The instructions that follow assume that you have obtained these datasets and placed them somewhere sensible (e.g. ../data/tidigits).

Pre-requisites
--------------
The following steps need to be completed before running the experiment scripts:

1. Install [Docker](https://docs.docker.com/install/) (I also recommend following the [linux post-install step](https://docs.docker.com/install/linux/linux-postinstall/) to manage Docker as a non-root user)

2. Install [nvidia-docker](https://github.com/NVIDIA/nvidia-docker) (version 2.0) for NVIDIA GPU access in docker containers

3. Pull required images from [Docker Hub](https://hub.docker.com):

| Docker image | Docker pull command |
| ------------- | -------------------|
| [Kaldi](https://hub.docker.com/r/reloff/kaldi) for extracting speech features | `docker pull reloff/kaldi:5.4` |
| [TensorFlow](https://hub.docker.com/r/reloff/tensorflow-base) used as base for research environment | `docker pull reloff/tensorflow-base:1.11.0-py36-cuda90` |
| [Multimodal one-shot research environment](https://hub.docker.com/r/reloff/multimodal-one-shot) | `docker pull reloff/multimodal-one-shot` |

Alternatively you can build these images locally from their DockerFiles:
- [Kaldi Dockerfile](https://github.com/rpeloff/research-images/blob/master/kaldi/Dockerfile)
- [TensorFlow Dockerfile](https://github.com/rpeloff/research-images/blob/master/tensorflow_base/python36_cuda90/Dockerfile)
- [Multimodal one-shot research environment Dockerfile](https://github.com/rpeloff/multimodal-one-shot-learning/blob/master/docker/Dockerfile)

Kaldi feature extraction
------------------------

Extract speech features by simply running `run_feature_extraction [OPTIONS]` (use `--help` flag for more information):

```bash
./run_feature_extraction.sh \
    --tidigits=<path to TIDigits> \
    --flickr-audio=<path to Flickr audio> \
    --flickr-text=<path to Flickr8k text> \
    --n-cpu-cores=<number of CPU cores>
```
    
Replace each path with the full path to the corresponding dataset. The `--n-cpu-cores` flag specifies the number of CPU cores used for feature extraction (defaults to 8; set higher or lower depending on available CPU cores), where more cores may speed up the process. For example:

```bash
./run_feature_extraction.sh --tidigits=/home/rpeloff/datasets/datasets/speech/tidigits --flickr-audio=/home/rpeloff/datasets/speech/flickr_audio --flickr-text=/home/rpeloff/datasets/text/Flickr8k_text --n-cpu-cores=8
```

Train and test multimodal models
--------------------------------

The multimodal one-shot models are demonstrated in two separate Jupyter notebooks:

1. `experiments/nb1_unimodal_train_test.ipynb`  trains and tests unimodal models for one-shot speech or image classification

2. `experiments/nb2_multimodal_test.ipynb`  extends unimodal models to the multimodal one-shot case, testing on one-shot cross-modal speech-image digit matching

To run these notebooks and reproduce the results in the paper, execute the `run_notebooks.sh [OPTIONS]` script (use `--help` flag for more information),

```bash
./run_notebooks.sh --port=8888
```

and navigate to http://127.0.0.1:8888/. Follow the experiment notebooks and execute the code cells to train, test, and summarise the unimodal and multimodal one-shot models.

Note
----
All code used for the paper is present in this repo, and the experiment notebooks should reproduce all results.
If you find any mistakes in the code or notebooks, please let us know by raising an issue!
Also feel free to raise issues if you have general comments! :smile:
