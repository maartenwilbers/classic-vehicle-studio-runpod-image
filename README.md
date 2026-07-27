# Classic Vehicle Studio RunPod image

Public, reproducible CUDA runtime image used for Classic Vehicle Studio
RunPod acceptance runs.

This repository contains only the image build inputs:

- the pinned runtime Dockerfile;
- the hash-locked Python dependencies;
- the GitHub Actions publishing workflow.

It does not contain application source code, vehicle photographs, model
checkpoints, credentials, or generated videos.

Published image:

`ghcr.io/maartenwilbers/classic-vehicle-studio-runpod:2.0.4`
