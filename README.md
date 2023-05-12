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
     sudo apt remove docker-desktop
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
