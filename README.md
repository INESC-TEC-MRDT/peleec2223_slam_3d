# peleec2223_slam_3d

## Setup

### Docker

1. Check KVM virtualization support
   - Load manually the module
     ```sh
     modprobe kvm
     modprobe kvm_intel   # Intel processors
     modprobe kvm_amd     # AMD processors
     ```
   - View KVM diagnostics
     ```sh
     kvm-ok
     ```
   - Check if KVM modules are enabled
     ```sh
     lsmod | grep kvm
     ```
2. Setup KVM device user permissions
   ```sh
   ls -al /dev/kvm
   sudo usermod -aG kvm $USER
   ```
3. Install Docker
   - Install `gnome-terminal` for non-Gnome environments
     ```sh
     sudo apt install gnome-terminal
     ```
   - Uninstall tech preview of Docker
     ```sh
     sudo apt remove docker-desktop
     ```
   - Remove older configuration and data files
     ```sh
     rm -r $HOME/.docker/desktop
     sudo rm /usr/local/bin/com.docker.cli
     sudo apt purge docker-desktop
     ```
   - Install Docker Desktop (download latest DEB package from
     https://docs.docker.com/desktop/install/ubuntu/#install-docker-desktop)
     ```sh
     # Configure Docker apt repository
     sudo apt update
     sudo apt install ca-certificates curl gnupg
     sudo install -m 0755 -d /etc/apt/keyrings
     curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
     sudo chmod a+r /etc/apt/keyrings/docker.gpg
     echo "deb [arch="$(dpkg --print-architecture)" signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu "$(. /etc/os-release && echo "$VERSION_CODENAME")" stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

     # Install
     sudo apt update -y
     sudo apt install ./docker-desktop-<version>-<arch>.deb
     ```
4. Launch Docker Desktop
   ```sh
   systemctl --user start docker-desktop
   docker compose version
   docker --version
   docker version

   # Enable Docker Desktop on login
   systemctl --user enable docker-desktop
   ```

### ROS Docker

1. Pull a prebuilt OSRF ROS images on Docker Hub
   ```sh
   docker pull osrf/ros:melodic-desktop-full
   ```
   - See available ROS dockers in https://hub.docker.com/r/osrf/ros/tags
   - Do not forget to initialize Docker before running the previous command
2. Run to create the ROS docker container
   ```sh
   docker run -it osrf/ros:melodic-desktop-full
   ```

However, only tunning the ROS container will not let you save the state of the
container.

1. Build the ROS docker container with additional commands to pre-install the
   software in the container
   ```sh
   docker build -t osrf/ros:melodic-desktop-full .
   ```
   - **Note:** in the current directory, a Dockerfile should be present!
   - Example of a Docker file available in the [docker]() folder
2. Run to create the ROS docker container
   ```sh
   docker run -it osrf/ros:melodic-desktop-full
   ```

## Usage

### Docker Containers

```sh
# See all available containers
docker ps -a

# Start a container
docker start amazing_lehmann

# Open the bash command line connected to the previous container
docker exec -it amazing_lehmann bash

# Stop the container
docker stop amazing_lehmann
```
