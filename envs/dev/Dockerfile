FROM nvidia/cuda:9.0-cudnn7-devel-ubuntu16.04

# Install CUDA tools and various dependencies
RUN apt-get update && \
    DEBIAN_FRONTEND=noninteractive apt-get install -y --no-install-recommends \
        build-essential \
        ca-certificates \
        locales \
        pkg-config \
        software-properties-common

# TensorFlow requires CUPTI for CUDA profiling
# https://github.com/tensorflow/tensorflow/tree/master/tensorflow/tools/dockerfiles
# ENV LD_LIBRARY_PATH /usr/local/cuda/extras/CUPTI/lib64:$LD_LIBRARY_PATH

# Set correct locale for locale-aware programs or libraries
ENV LANG C.UTF-8
ENV LC_ALL C.UTF-8

# Install basic programs
RUN apt-get update && \
    DEBIAN_FRONTEND=noninteractive apt-get install -y --no-install-recommends \
        bzip2 \
        curl \
        git \
        graphviz \
        libgraphviz-dev \
        nano \
        rsync \
        unzip \
        wget

# Cleanup apt-get
RUN apt-get clean && \
    apt-get autoremove -y && \
    rm -rf /var/lib/apt/lists/*

# Set up Python with Miniconda3
RUN echo 'export PATH=/opt/conda/bin:$PATH' > /etc/profile.d/conda.sh \
    && curl -o ~/miniconda.sh -O https://repo.continuum.io/miniconda/Miniconda3-latest-Linux-x86_64.sh \
    && /bin/bash ~/miniconda.sh -b -p /opt/conda  \
    && rm ~/miniconda.sh \
    && /opt/conda/bin/conda update --yes conda \
    && /opt/conda/bin/conda update --yes --all \
    && /opt/conda/bin/conda clean -tipsy

# Add Miniconda3 to PATH
ENV PATH /opt/conda/bin:$PATH

# Update pip
RUN python -m pip install --upgrade --no-cache-dir pip

# Install NumPy, SciPy, and Scikit-Learn with MKL optimizations
RUN conda install \
    numpy \
    scipy \
    scikit-learn

# Install basic packages
RUN pip install --upgrade --no-cache-dir \
    dask \
    jupyterlab \
    ipdb \
    Pillow \
    imageio \
    matplotlib \
    nbimporter \
    pandas \
    pyyaml \
    requests \
    seaborn \
    sklearn-pandas \
    tqdm

# Packaging and testing
RUN pip install --upgrade --no-cache-dir \
    pytest \
    tox \
    twine

# Install ML libraries
RUN pip install --upgrade --no-cache-dir \
    torch \
    torchvision \
    tensorflow-gpu \
    catboost \
    sklearn \
    statsmodels

# Create a user to avoid problems with permissions
RUN useradd --create-home --shell /bin/bash user

# Activate user
USER user

# Create directory to mount
RUN mkdir /home/user/workspace
WORKDIR /home/user/workspace
