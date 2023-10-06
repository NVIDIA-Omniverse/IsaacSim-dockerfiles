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
  isaac-sim:2023.1.0-ubuntu22.04 \
  --build-arg ISAACSIM_VERSION=2023.1.0 \
  --file Dockerfile.2023.1.0-ubuntu22.04 .
```

## Usage

To run the container and start Isaac Sim as a headless app:

```bash
docker run --name isaac-sim --entrypoint bash -it --gpus all -e "ACCEPT_EULA=Y" --rm --network=host \
  -e "PRIVACY_CONSENT=Y" \
  -v ~/docker/isaac-sim/cache/kit:/isaac-sim/kit/cache:rw \
  -v ~/docker/isaac-sim/cache/ov:/root/.cache/ov:rw \
  -v ~/docker/isaac-sim/cache/pip:/root/.cache/pip:rw \
  -v ~/docker/isaac-sim/cache/glcache:/root/.cache/nvidia/GLCache:rw \
  -v ~/docker/isaac-sim/cache/computecache:/root/.nv/ComputeCache:rw \
  -v ~/docker/isaac-sim/logs:/root/.nvidia-omniverse/logs:rw \
  -v ~/docker/isaac-sim/data:/root/.local/share/ov/data:rw \
  -v ~/docker/isaac-sim/documents:/root/Documents:rw \
  isaac-sim:2023.1.0-ubuntu22.04 \
  ./runheadless.native.sh -v
```

To run the container and start Isaac Sim as a windowed app:

```bash
xhost +
docker run --name isaac-sim --entrypoint bash -it --gpus all -e "ACCEPT_EULA=Y" --rm --network=host \
  -e "PRIVACY_CONSENT=Y" \
  -v $HOME/.Xauthority:/root/.Xauthority \
  -e DISPLAY \
  -v ~/docker/isaac-sim/cache/kit:/isaac-sim/kit/cache:rw \
  -v ~/docker/isaac-sim/cache/ov:/root/.cache/ov:rw \
  -v ~/docker/isaac-sim/cache/pip:/root/.cache/pip:rw \
  -v ~/docker/isaac-sim/cache/glcache:/root/.cache/nvidia/GLCache:rw \
  -v ~/docker/isaac-sim/cache/computecache:/root/.nv/ComputeCache:rw \
  -v ~/docker/isaac-sim/logs:/root/.nvidia-omniverse/logs:rw \
  -v ~/docker/isaac-sim/data:/root/.local/share/ov/data:rw \
  -v ~/docker/isaac-sim/documents:/root/Documents:rw \
  isaac-sim:2023.1.0-ubuntu22.04 \
  ./runapp.sh
```

Connect to Isaac Sim using the [Omniverse Streaming Client](https://docs.omniverse.nvidia.com/streaming-client/latest/user-manual.html).

See [Container Deployment](https://docs.omniverse.nvidia.com/isaacsim/latest/install_container.html#container-deployment) for information on container deployment.

## Licensing

The source code in this repository is licensed under [Apache 2.0](https://www.apache.org/licenses/LICENSE-2.0).

The resulting container images are licensed under the [NVIDIA Omniverse License Agreement](https://developer.nvidia.com/omniverse/license).

## Support

* Please use [NVIDIA Developer Forums](https://forums.developer.nvidia.com/c/omniverse/simulation/69) for questions and comments.
* See [Isaac Sim Documentation](https://docs.omniverse.nvidia.com/isaacsim/latest/index.html) for more information.
