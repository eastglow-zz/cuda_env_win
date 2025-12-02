# .devcontainer/Dockerfile

FROM nvcr.io/nvidia/nvhpc:24.9-devel-cuda12.6-ubuntu22.04

# 1. 리눅스 기본 패키지 설치 (+ python3-pip 추가!)
RUN apt-get update && apt-get install -y \
    git \
    make \
    cmake \
    vim \
    libopenblas-dev \
    python3-pip \
    && rm -rf /var/lib/apt/lists/*

# 2. Python 라이브러리 설치 (matplotlib, numpy 등 미리 설치)
RUN pip3 install matplotlib numpy scipy jupyterlab