# 🎧 DeepSonic

**DeepSonic** is a fully open-source deep learning music experiment designed to **synthesize**, **generate**, **remix**, and **modify** music using state-of-the-art AI models. Leveraging open-source technologies like **Magenta (by Google)** and **Jukebox (by OpenAI)**, DeepSonic acts as a multi-functional **AI Audio Engineer**.

> 🎛️ This project runs natively on your machine. Cloud support is optional, but having a dedicated NVIDIA GPU is highly recommended.

---

## 🚀 Features

- 🎼 AI-powered music generation and remixing
- 🎹 MIDI file synthesis and playback
- 🎧 Deep audio style transformation
- 🧠 Combines TensorFlow’s Magenta and OpenAI’s Jukebox
- 🖥️ Fully local execution with GPU acceleration

---

## 💻 Hardware Requirements

| Component | Minimum | Recommended |
|----------|---------|-------------|
| **GPU** | NVIDIA GeForce RTX 2000 (8GB VRAM) | Tesla / Quadro with 16GB+ VRAM |
| **CPU** | AMD Ryzen 3 3200 / Intel i3 8100 | Higher = Better |
| **OS** | Debian-based Linux | Ubuntu / WSL2 (experimental) |

> ⚠️ Root privileges are required to install some audio dependencies.

---

## 🧪 Experiments & Use Cases

- Generate jazz or classical pieces from scratch  
- Style transfer using music transformers  
- MIDI-driven AI remixing  
- High-quality audio synthesis using Jukebox  

---

## 🙏 Acknowledgements

- [Magenta by Google](https://github.com/magenta/magenta)  
- [Jukebox by OpenAI](https://github.com/openai/jukebox)  
- [DDSP](https://github.com/magenta/ddsp)  


## ⚙️ Quick Installation Guide

> This guide is adapted from the official Magenta and Jukebox documentation.

```bash
# Audio library dependencies
sudo apt-get update && sudo apt-get install build-essential libasound2-dev libjack-dev portaudio19-dev

# Install Magenta
curl https://raw.githubusercontent.com/tensorflow/magenta/main/magenta/tools/magenta-install.sh > /tmp/magenta-install.sh
bash /tmp/magenta-install.sh

# Setup Jukebox environment
conda create --name jukebox python=3.7.5
conda activate jukebox
conda install mpi4py=3.0.3  # fallback: pip install mpi4py==3.0.3
conda install pytorch=1.4 torchvision=0.5 cudatoolkit=10.0 -c pytorch
git clone https://github.com/openai/jukebox.git && cd jukebox
pip install -r requirements.txt
pip install -e .
conda install av=7.0.01 -c conda-forge 
pip install ./tensorboardX
pip install -qU ddsp==1.6.5

# Download required MIDI and soundfont data
curl -o ./cs1-1pre.mid http://www.jsbach.net/midi/cs1-1pre.mid
curl -o ./arp.mid http://storage.googleapis.com/magentadata/papers/gansynth/midi/arp.mid

# Google Cloud SDK setup (if using GCP)
sudo apt-get install apt-transport-https ca-certificates gnupg
curl https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key --keyring /usr/share/keyrings/cloud.google.gpg add -
echo "deb [signed-by=/usr/share/keyrings/cloud.google.gpg] https://packages.cloud.google.com/apt cloud-sdk main" | sudo tee /etc/apt/sources.list.d/google-cloud-sdk.list
sudo apt-get update && sudo apt-get install google-cloud-sdk

# Optional: Download pretrained models
gsutil -q -m cp -r gs://magentadata/models/music_transformer/primers/* ./DeepSonic/
gsutil -q -m cp gs://magentadata/soundfonts/Yamaha-C5-Salamander-JNv5.1.sf2 ./DeepSonic/
pip install -q 'tensorflow-datasets < 4.0.0'
gsutil -q -m cp -r gs://magentadata/models/music_transformer/checkpoints/* ./musictransformermodels/

# Optional: Install Apex for faster training with fused Adam optimizer:
conda install pytorch=1.1 torchvision=0.3 cudatoolkit=10.0 -c pytorch
pip install -v --no-cache-dir --global-option="--cpp_ext" --global-option="--cuda_ext" ./apex
