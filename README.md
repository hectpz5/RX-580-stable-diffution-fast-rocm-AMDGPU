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
# Kernel version requirement
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
 install key

sudo mkdir --parents --mode=0755 /etc/apt/keyrings

  Download the key

wget https://repo.radeon.com/rocm/rocm.gpg.key -O - | \
    gpg --dearmor | sudo tee /etc/apt/keyrings/rocm.gpg > /dev/null

install olds repo amd and rocm

echo "deb [arch=amd64 signed-by=/etc/apt/keyrings/rocm.gpg] https://repo.radeon.com/amdgpu/5.5/ubuntu jammy main" | sudo tee /etc/apt/sources.list.d/amdgpu.list

echo "deb [arch=amd64 signed-by=/etc/apt/keyrings/rocm.gpg] https://repo.radeon.com/rocm/apt/5.5 jammy main" | sudo tee --append /etc/apt/sources.list.d/rocm.list

Install script AMDGPU-INSTALL

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

sudo rm /etc/apt/sources.list.d/amdgpu*

sudo rm -rf /var/cache/apt/*
sudo apt clean all
sudo apt update
sudo reboot

Install New script AMDGPU-INSTALL

https://repo.radeon.com/amdgpu-install/6.2/ubuntu/jammy/amdgpu-install_6.2.60203-1_all.deb

Only terminal and say chANnge repo ROCM new version  white N no change

Reinstall Key

wget https://repo.radeon.com/rocm/rocm.gpg.key -O - | \
    gpg --dearmor | sudo tee /etc/apt/keyrings/rocm.gpg > /dev/null
 Change repo AMDGPU

sudo apt-get update && sudo apt-get full-upgrade

OR....

 Change repo AMDGPU manual

sudo nano /etc/apt/sources.list.d/amdgpu.list

Only line  5.5 to 6.2.3

sudo apt-get update && sudo apt-get full-upgrade

-----------------------------------------------------------------------------------------------------------------------------
### install in Automatic SD WebUI

``` AMD RX 580


git clone https://github.com/lllyasviel/stable-diffusion-webui-forge.git

or download here

cd stable-diffusion-webui-forge
python3 -m venv venv
source venv/bin/activate

Here use app inside folder recet create " /venv/bin" of stable-diffusion-webui-forge by enviroment virtual of python

python -m pip install --upgrade pip wheel
/home/*you_folder*/Dowload/stable-diffusion-webui-forge/venv/bin/pip uninstall torch torchvision
/home/*you_folder*/Download/stable-diffusion-webui-forge/venv/bin/pip3 install /home/*******Download/torchvision-0.16.2-cp310-cp310-linux_x86_64.whl
/home/*you_folder*/Download/stable-diffusion-webui-forge/venv/bin/pip3 install /home/*******Download/torch-2.1.2-cp310-cp310-linux_x86_64.whl
/home/*you_folder*/Download/stable-diffusion-webui-forge/venv/bin/pip3 install  -r requirements_versions.txt
pip list | grep 'torch'
Run
/home/*you_folder*/Download/stable-diffusion-webui-forge/venv/bin/python3 launch.py  --precision full --no-half  --opt-split-attention-v1 --upcast-sampling

  ## extra info

GPUs rx 400 rx 500 series
More 8gb de vram
**** --precision full --no-half --opt-split-attention-v1 --upcast-sampling "
 
Medium 4gb to 6gb de vram 
*** --precision full --no-half --medvram"

Or 4gb o low 
*** --precision full --no-half --lowvram"
## Reference and special thanks
- https://github.com/lllyasviel/stable-diffusion-webui-forge
- https://rocm.docs.amd.com/projects/install-on-linux/en/latest/install/native-install/ubuntu.html
- https://github.com/LinuxMadeEZ/PyTorch-Ubuntu-GFX803
- https://github.com/xuhuisheng/rocm-gfx803/issues/27#issuecomment-1892611849%E7%9A%84%E5%BB%BA%E8%AD%B0%E5%98%97%E8%A9%A6%E8%87%AA%E7%B7%A8torch%E5%92%8Ctorch%20vision%E8%A9%A6%E8%A9%A6%E7%9C%8B%E6%9C%83%E4%B8%8D%E6%9C%83%E6%88%90%E5%8A%9F
- https://github.com/viebrix/pytorch-gfx803/tree/main
- https://github.com/RadeonOpenCompute/ROCm/issues/1659
- https://github.com/xuhuisheng/rocm-gfx803
- https://github.com/xuhuisheng/rocm-gfx803/issues/27#issuecomment-1534048619
- https://github.com/Tokoshie/pytorch-gfx803/releases/tag/v2.1.0a0
- https://github.com/xuhuisheng/rocm-gfx803/issues/27#issuecomment-1892611849
- https://github.com/AUTOMATIC1111/stable-diffusion-webui/wiki

