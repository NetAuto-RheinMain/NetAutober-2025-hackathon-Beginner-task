FROM ubuntu:22.04

ENV DEBIAN_FRONTEND=noninteractive

# Install system dependencies
RUN apt-get update && apt-get install -y \
    curl sudo sshpass iputils-ping \
    net-tools git python3 python3-pip \
    ansible

# Install Python dependencies
RUN pip3 install netmiko paramiko

# Install Containerlab
RUN curl -sL https://get.containerlab.dev | bash

# Install NetOpsChic's SR Linux Ansible Collection
RUN ansible-galaxy collection install netopschic.srlinux

# Create non-root user for Codespace
RUN useradd -ms /bin/bash devuser && echo "devuser ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers
USER devuser
WORKDIR /home/devuser
