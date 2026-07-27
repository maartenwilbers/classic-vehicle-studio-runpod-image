# syntax=docker/dockerfile:1.7
FROM pytorch/pytorch@sha256:66b41f1755d9644f6341cf4053cf2beaf3948e2573acf24c3b4c49f55e82f578

ARG CVS_COMMIT=unknown
ARG RUNPOD_STACK_VERSION=2.0.5

LABEL org.opencontainers.image.title="Classic Vehicle Studio M2 RunPod"
LABEL org.opencontainers.image.version="${RUNPOD_STACK_VERSION}"
LABEL org.opencontainers.image.revision="${CVS_COMMIT}"
LABEL nl.amklassiek.cvs.base-image="pytorch/pytorch:2.1.2-cuda11.8-cudnn8-devel@sha256:66b41f1755d9644f6341cf4053cf2beaf3948e2573acf24c3b4c49f55e82f578"

ENV DEBIAN_FRONTEND=noninteractive \
    CC=gcc-10 \
    CXX=g++-10 \
    CMAKE_COMMAND=/usr/bin/cmake \
    CUDA_HOME=/usr/local/cuda \
    TORCH_CUDA_ARCH_LIST=8.9 \
    PIP_DISABLE_PIP_VERSION_CHECK=1 \
    PIP_NO_CACHE_DIR=1

RUN apt-get update \
    && apt-get install --yes --no-install-recommends \
        build-essential \
        ca-certificates \
        cmake \
        curl \
        ffmpeg \
        g++-10=10.5.0-1ubuntu1~20.04 \
        gcc-10=10.5.0-1ubuntu1~20.04 \
        git \
        libgl1 \
        libglib2.0-0 \
        ninja-build \
        patch \
        wget \
    && test "$(gcc-10 -dumpfullversion)" = "10.5.0" \
    && test "$(g++-10 -dumpfullversion)" = "10.5.0" \
    && test "$(/usr/bin/cmake --version | awk 'NR == 1 { print $3 }')" = "3.16.3" \
    && test "$(/usr/bin/ninja --version)" = "1.10.0" \
    && rm -rf /var/lib/apt/lists/*

COPY runpod/requirements.lock /opt/classic-vehicle-studio/requirements.lock

RUN python -m pip install --require-hashes \
        --requirement /opt/classic-vehicle-studio/requirements.lock \
    && python - <<'PY'
import platform
import subprocess
import sys
import torch
import torchvision

assert platform.python_version() == "3.10.13", platform.python_version()
assert torch.__version__.split("+", 1)[0] == "2.1.2", torch.__version__
assert torchvision.__version__.split("+", 1)[0] == "0.16.2", torchvision.__version__
assert torch.version.cuda == "11.8", torch.version.cuda
subprocess.run([sys.executable, "-m", "pip", "check"], check=True)
PY

WORKDIR /workspace

CMD ["sleep", "infinity"]
