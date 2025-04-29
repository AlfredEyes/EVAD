ARG PYTORCH_TAG=2.1.0-cuda12.1-cudnn8-devel
FROM pytorch/pytorch:${PYTORCH_TAG}
ARG DEBIAN_FRONTEND=noninteractive
ENV TZ=Europe/Paris

# Setup base dependencies
RUN apt update && apt install -y curl
RUN apt-get update -y \
    && apt-get install -y sudo build-essential git wget curl libgl1 libglib2.0-0 libsm6 libxrender1 libxext6 \
    vim gosu git-lfs
RUN rm -rf /var/lib/apt/lists/* \
    && apt-get purge -y --auto-remove

RUN pip install pytest cython simplejson iopath psutil opencv-python pandas tensorboard moviepy timm==0.4.12

RUN git clone https://github.com/facebookresearch/detectron2.git \
    && python -m pip install -e detectron2

RUN conda update -n base conda -y && \
    conda config --add channels conda-forge && \
    conda config --set channel_priority strict && \
    conda install av -c conda-forge -y

# Setup mountpoints
RUN mkdir -p /datasets/external_data
RUN mkdir -p /datasets/processed_data
RUN mkdir -p /datasets/raw_data
RUN mkdir -p /alfred/workspace

# Setup workspace user
RUN useradd alfred -d /alfred -s /bin/bash && \
    chown -R alfred:alfred /alfred && \
    echo 'alfred ALL=(ALL) NOPASSWD: /usr/bin/apt' > /etc/sudoers.d/00_alfred
WORKDIR /alfred/workspace
