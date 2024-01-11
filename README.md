# Civitai SDXL Safetensor Cog template

Install Cog v0.7.2

    sudo curl -o /usr/local/bin/cog -L https://github.com/replicate/cog/releases/download/v0.7.2/cog_`uname -s`_`uname -m`
    sudo chmod +x /usr/local/bin/cog

check cog version 

    cog --version

Docker installation instructions:
    [Install](https://azdigi.com/blog/linux-server/tools/huonng-dan-cai-dat-docker-tren-ubuntu-22-04/)

Check inside the file /etc/apt/sources.list.d/nvidia-container-runtime.list:

    deb https://nvidia.github.io/libnvidia-container/ubuntu22.04/jammy /

Check inside the file /etc/apt/sources.list.d/nvidia-container-toolkit.list:

    deb [signed-by=/usr/share/keyrings/nvidia-container-toolkit-keyring.gpg] https://nvidia.github.io/libnvidia-container/stable/deb/$(ARCH) /
    #deb [signed-by=/usr/share/keyrings/nvidia-container-toolkit-keyring.gpg] https://nvidia.github.io/libnvidia-container/experimental/deb/$(ARCH) /


Install Toolkit Nvidia

    curl -s -L https://nvidia.github.io/libnvidia-container/stable/deb/nvidia-container-toolkit.list | sed 's#deb https://#deb [signed-by=/usr/share/keyrings/nvidia-container-toolkit-keyring.gpg] https://#g' | sudo tee /etc/apt/sources.list.d/nvidia-container-toolkit.list
    sudo apt-get update
    sudo apt-get install -y nvidia-container-toolkit

Restart Docker:
    
    sudo systemctl restart docker 

This is a template to push a Civitai SDXL model as a Cog model. [Cog packages machine learning models as standard containers.](https://github.com/replicate/cog)

First, get a Civitai SDXL Model URL and download the safetensors file:

    wget -O model.safetensors https://civitai.com/api/download/models/154625?type=Model&format=SafeTensor&size=full&fp=fp16


Then run the script convert to Diffusers, and download weights needed for SDXL (VAE, safety checker):

    cog run script/download-weights

Then, you can run predictions:

    cog predict -i prompt="An astronaut riding a rainbow unicorn"

And finally push to Replicate:

    cog login
    cog push r8.im/<Replicate_Username>/<Replicate_Model_name>

Example:

![alt text](output.0.png)