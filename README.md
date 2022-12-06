# Isaac Sim Dockerfiles

## Introduction

This repository contains Dockerfiles for building an Isaac Sim container.

### Pre-Requisites

Before getting started, ensure that the system has the latest [NVIDIA Driver](https://www.nvidia.com/en-us/drivers/unix/) and the [NVIDIA Container Toolkit](https://github.com/NVIDIA/nvidia-docker) installed.
Use your [NGC Account](https://docs.nvidia.com/ngc/ngc-overview/index.html#registering-activating-ngc-account) to get access to the [Isaac Sim Container](https://catalog.ngc.nvidia.com/orgs/nvidia/containers/isaac-sim) and generate your [NGC API Key](https://docs.nvidia.com/ngc/ngc-overview/index.html#generating-api-key).

## Build

Clone this repository and then build the image:

```bash
docker login nvcr.io
docker build --pull -t \
        isaac-sim:2022.1.1-ubuntu20.04 \
        --build-arg ISAACSIM_VERSION=2022.1.1 \
        --build-arg BASE_DIST=ubuntu20.04 \
        --build-arg CUDA_VERSION=11.4.2 \
        --build-arg VULKAN_SDK_VERSION=1.3.224.1 \
        --file Dockerfile.2022.1.1-ubuntu20.04 .
```

## Usage

To run the container and start Isaac Sim as a headless app:

```bash
docker run --name isaac-sim --entrypoint bash -it --gpus all -e "ACCEPT_EULA=Y" --rm --network=host \
        -v /usr/share/vulkan/icd.d/nvidia_icd.json:/etc/vulkan/icd.d/nvidia_icd.json \
        -v /usr/share/vulkan/implicit_layer.d/nvidia_layers.json:/etc/vulkan/implicit_layer.d/nvidia_layers.json \
        -v /usr/share/glvnd/egl_vendor.d/10_nvidia.json:/usr/share/glvnd/egl_vendor.d/10_nvidia.json \
        -v ~/docker/isaac-sim/cache/ov:/root/.cache/ov:rw \
        -v ~/docker/isaac-sim/cache/pip:/root/.cache/pip:rw \
        -v ~/docker/isaac-sim/cache/glcache:/root/.cache/nvidia/GLCache:rw \
        -v ~/docker/isaac-sim/cache/computecache:/root/.nv/ComputeCache:rw \
        -v ~/docker/isaac-sim/logs:/root/.nvidia-omniverse/logs:rw \
        -v ~/docker/isaac-sim/config:/root/.nvidia-omniverse/config:rw \
        -v ~/docker/isaac-sim/data:/root/.local/share/ov/data:rw \
        -v ~/docker/isaac-sim/documents:/root/Documents:rw \
        isaac-sim:2022.1.1-ubuntu20.04 \
        ./runheadless.native.sh
```

> **_NOTE:_**  The above command is recommmended for NVIDIA driver 470.

If you are using NVIDIA drivers 510 or later:

```bash
docker run --name isaac-sim --entrypoint bash -it --gpus all -e "ACCEPT_EULA=Y" --rm --network=host \
        -v /etc/vulkan/icd.d/nvidia_icd.json:/etc/vulkan/icd.d/nvidia_icd.json \
        -v /etc/vulkan/implicit_layer.d/nvidia_layers.json:/etc/vulkan/implicit_layer.d/nvidia_layers.json \
        -v /usr/share/glvnd/egl_vendor.d/10_nvidia.json:/usr/share/glvnd/egl_vendor.d/10_nvidia.json \
        -v ~/docker/isaac-sim/cache/ov:/root/.cache/ov:rw \
        -v ~/docker/isaac-sim/cache/pip:/root/.cache/pip:rw \
        -v ~/docker/isaac-sim/cache/glcache:/root/.cache/nvidia/GLCache:rw \
        -v ~/docker/isaac-sim/cache/computecache:/root/.nv/ComputeCache:rw \
        -v ~/docker/isaac-sim/logs:/root/.nvidia-omniverse/logs:rw \
        -v ~/docker/isaac-sim/config:/root/.nvidia-omniverse/config:rw \
        -v ~/docker/isaac-sim/data:/root/.local/share/ov/data:rw \
        -v ~/docker/isaac-sim/documents:/root/Documents:rw \
        isaac-sim:2022.1.1-ubuntu20.04 \
        ./runheadless.native.sh
```

Connect to Isaac Sim using the [Omniverse Streaming Client](https://docs.omniverse.nvidia.com/app_streaming-client/app_streaming-client/user-manual.html).
See [Container Deployment](https://docs.omniverse.nvidia.com/app_isaacsim/app_isaacsim/install_advanced_container_deployment.html) for information on container deployment.

## Licensing

The source code in this repository is licensed under [Apache 2.0](https://www.apache.org/licenses/LICENSE-2.0).
The resulting container images are licensed under the [NGC Deep Learning Container License](https://developer.nvidia.com/ngc/nvidia-deep-learning-container-license).

## Support

* Please use [NVIDIA Developer Forums](https://forums.developer.nvidia.com/c/agx-autonomous-machines/isaac/simulation/69) for questions and comments.
* See [Isaac Sim Documentation](https://docs.omniverse.nvidia.com/isaacsim/index.html) for more information.
