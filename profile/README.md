# QNX Ports

This group contains repositories for open-source projects that have been ported to run on QNX. Most projects have minimal changes from their upstream origin; however, some projects may deviate more to make sure they work with some dependency or architectural difference in the QNX OS.

## Working with QNX ports

To use a ported project in your own development, you can clone the port repo and build it yourself using your QNX software development environment. (If you are not yet set up for QNX development, get started for free in minutes at [https://www.qnx.com/getqnx](https://www.qnx.com/getqnx).) All of the projects here should be kept in a working state, meaning they build for the target QNX version(s), have available tests, and are operational when used.

While every open-source module has its own build process, the high-level steps remain relatively consistent. For instructions specific to a port, please look in that port's directory in [the Build Files repo](https://github.com/qnx-ports/build-files) above for a `README.md` file.

## Docker-based build environment

When you're trying to build multiple QNX projects (demo apps, QNX ports, and your own programs), it can be challenging to maintain a perfect development environment with all of the right dependencies available. This open-source Docker-based build environment aims to simplify building projects by helping you to create a clean and consistent build environment on your host.

The Dockerfile creates an Ubuntu container and pre-installs many common dependencies needed for project building. It also mounts your home directory into the container so folders such as your QNX license and SDP installation (`~/.qnx` and `~/qnx800` by default), plus any projects you are building, will be available to the build environment in the container. (If your QNX installation location is customized, you may wish to edit `docker-create-container.sh` to mount a more appropriate directory.)

Feel free to modify the Dockerfile and scripts for your own usage, and please consider contributing suggested changes or issues to this repo for the benefit of others in the community!

### Docker build pre-requisites

Before starting:

1. Install Docker: https://docs.docker.com/engine/install/ubuntu/
2. If on Linux, make sure to complete the post-installation steps: https://docs.docker.com/engine/install/linux-postinstall/

# Build examples

## Build example for SDP 8.0 using Docker

These instructions demonstrate the process for building an open-source port, using a Docker-based build environment, to run on QNX 8.0. This example assumes a you wish to compile the `mosquitto` MQTT broker for use on QNX SDP 8.0:

1. Create a workspace and clone the Build Files repo:

```
mkdir -p ~/qnx_workspace && cd ~/qnx_workspace
git clone https://github.com/qnx-ports/build-files.git && cd build-files/docker
```

2. Use the included helper scripts to build and start a QNX build environment container with Docker:

```
./docker-build-qnx-image.sh
./docker-create-container.sh
```

Note: the Docker container sources `qnxsdp-env.sh` for you, so you do not need to run it again when building your projects.

3. Clone the `mosquitto` port repository into your workspace:

```
cd ~/qnx_workspace
git clone https://github.com/qnx-ports/mosquitto.git
```

4. Start the build:

```
BUILD_TESTING=ON QNX_PROJECT_ROOT="$(pwd)/mosquitto" make -C ./build-files/ports/mosquitto install -j$(nproc)
```

5. When you are finished, exit the Docker container:

```
exit
```

> ***Note:*** If you deactivate a Python virtual environment in the Docker container, the PATH variable may be reset causing the QNX paths to be not found. You can correct this by reactivating the QNX environment script: `source ~/qnx800/qnxsdp-env.sh`.

## Build example for SDP 8.0 natively

These instructions demonstrate the process for building an open-source port, directly on an Ubuntu development host. This example assumes a you wish to compile the `mosquitto` MQTT broker for use on QNX SDP 8.0:

1. Clone the `mosquitto` port repository into your workspace:

```
git clone https://github.com/qnx-ports/build-files.git
git clone https://github.com/qnx-ports/mosquitto.git
```

2. Activate the QNX toolchain:
```
source ~/qnx800/qnxsdp-env.sh
```

3. Start the build:
```
BUILD_TESTING=ON QNX_PROJECT_ROOT="$(pwd)/mosquitto" make -C ./build-files/ports/mosquitto install -j$(nproc)
```

> ***Note:*** If you deactivate a Python virtual environment in the Docker container, the PATH variable may be reset causing the QNX paths to be not found. You can correct this by reactivating the QNX environment script: `source ~/qnx800/qnxsdp-env.sh`.

# Get support

The community is ready to help with your questions and issues! For any questions, please feel free to:

- Create an Issue or search existing Issues in the Issues section of a repo
- Ask your question with QNX tag on [Stack Overflow](https://stackoverflow.com/questions/tagged/qnx)
- Post to the community on Reddit at [r/qnx](https://www.reddit.com/r/qnx)