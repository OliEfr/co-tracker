FROM nvcr.io/nvidia/cuda:11.8.0-cudnn8-devel-ubuntu22.04

ENV DEBIAN_FRONTEND=noninteractive

# Install Python 3.10 and other necessary dependencies
RUN apt-get update && apt-get install -y \
    wget \
    curl \
    python3.10 \
    python3.10-dev \
    python3.10-distutils \
    python3-pip \
    python3.10-venv \
    libgl1-mesa-glx \
    libglib2.0-0 \
    git \
    unzip \
    tar \
    htop \
    qt5-qmake \
    qtbase5-dev \
    xvfb \
    python3-tk \ 
    && rm -rf /var/lib/apt/lists/* && apt-get clean

# Set Python 3.10 as the default python version
RUN update-alternatives --install /usr/bin/python3 python3 /usr/bin/python3.10 1 && update-alternatives --install /usr/bin/python python /usr/bin/python3.10 1 && update-alternatives --install /usr/bin/pip pip /usr/bin/pip3 1

# Create and activate a virtual environment
RUN python -m venv /opt/venv
ENV PATH="/opt/venv/bin:$PATH"
RUN echo 'source /opt/venv/bin/activate' >> ~/.bashrc

# Nvidia env vars
ENV NVIDIA_VISIBLE_DEVICES ${NVIDIA_VISIBLE_DEVICES:-all}
ENV NVIDIA_DRIVER_CAPABILITIES ${NVIDIA_DRIVER_CAPABILITIES:+$NVIDIA_DRIVER_CAPABILITIES,}graphics

RUN pip3 install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu126

# Create the checkpoints directory
RUN mkdir -p /checkpoints

# Download the online and offline models
RUN cd /checkpoints && \
    wget https://huggingface.co/facebook/cotracker3/resolve/main/scaled_online.pth && \
    wget https://huggingface.co/facebook/cotracker3/resolve/main/scaled_offline.pth

# Install necessary Python packages
RUN pip install matplotlib flow_vis tqdm tensorboard imageio[ffmpeg] ipykernel ipympl

