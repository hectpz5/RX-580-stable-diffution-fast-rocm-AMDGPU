# RX-580-stable-diffution-fast-rocm-AMDGPU
Run amd polaris compatibility with stable deffution
REQUIREMENTS PC

build pytorch 2.1.2 with ROCm support more stability stable-diffusion-webui-forge

Linux Mint 21.2 mate low use RAM
Kernel 5.19.0-50-generic > block version with synaptic
Radeon RX 480/580 8GB > driver AMDGPU upgrade 6.8.5 more stability
RoCm 5.5 >  necesary and not upgrade

Python 3.10.12
- pytorch 2.1.2
- torchvision 0.16.2
-----------------------------------------------------------------------------------------------------------------------
Special thanks to viebrix repo pytorch-gfx803
## Kernel version requirement
The installation procedure involves building the kernel modules from the `amdgpu-dkms` package. DKMS from ROCm 5.5 will **not** build on kernels above 5.19 such as 6.2.x and 6.5.x that are already available in Linux Mint repos. At the time of writing the latest kernel version where `amdgpu-dkms` only works is **5.19.0-50**. Make sure to downgrade in advance if you run a freshier kernel.
```bash
KERN="5.19.0-50-generic"
sudo apt install "linux-image-$KERN" "linux-headers-$KERN" "linux-modules-$KERN" "linux-modules-extra-$KERN"
```
If you are running v5.19 already but you have freshier kernels installed then DKMS *might* quietly fail to build, breaking `dpkg` installation routines.

------------------------------------------------------------------------------------------------------------------------

## Optionals Install dependencies

```In terminal
sudo apt autoremove rocm-core amdgpu-dkms
sudo apt install libopenmpi3 libstdc++-12-dev libdnnl-dev ninja-build libopenblas-dev libpng-dev libjpeg-dev


-----------------------------------------------------------------------------------------------------------------------
## Install ROCm
wget https://repo.radeon.com/amdgpu-install/5.5/ubuntu/jammy/amdgpu-install_5.5.50500-1_all.deb
sudo apt install ./amdgpu-install_5.5.50500-1_all.deb
sudo amdgpu-install -y --usecase=rocm,hiplibsdk,mlsdk

sudo usermod -aG video "here user name in folder HOME"
sudo usermod -aG render "here user name in folder HOME"

# verify
rocminfo
rocm-smi
clinfo
-------------------------------------------------------------------------------------------------------------------------
## Upgrade AMDGPU driver more stability
#install key
sudo mkdir --parents --mode=0755 /etc/apt/keyrings

# Download the key
wget https://repo.radeon.com/rocm/rocm.gpg.key -O - | \
    gpg --dearmor | sudo tee /etc/apt/keyrings/rocm.gpg > /dev/null
## Change repo AMDGPU
sudo rm /etc/apt/sources.list.d/amdgpu.list

sudo rm -rf /var/cache/apt/*
sudo apt clean all
sudo apt update
sudo reboot

