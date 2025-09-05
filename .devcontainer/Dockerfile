FROM ubuntu:22.04

ENV DEBIAN_FRONTEND=noninteractive

# Install base packages
RUN apt-get update && apt-get install -y \
    curl sudo sshpass iputils-ping \
    net-tools git python3 python3-pip \
    ansible

# Install Python dependencies
RUN pip3 install netmiko paramiko

# Install Containerlab
RUN curl -sL https://get.containerlab.dev | bash

# Manually install NetOpsChic SR Linux plugin
RUN mkdir -p /usr/share/ansible/collections/ansible_collections/nokia/srlinux && \
    git clone https://github.com/NetOpsChic/srlinux-ansible-collection.git /tmp/srlinux-ansible && \
    cp -r /tmp/srlinux-ansible/plugins /usr/share/ansible/collections/ansible_collections/nokia/srlinux/ && \
    rm -rf /tmp/srlinux-ansible

# Add default Ansible config file
RUN echo "[defaults]\n\
inventory = ansible/inventory.ini\n\
collections_paths = /usr/share/ansible/collections\n\
host_key_checking = False\n\
retry_files_enabled = False\n\
stdout_callback = yaml\n\
timeout = 10" > /home/devuser/ansible.cfg

# Create non-root user
RUN useradd -ms /bin/bash devuser && echo "devuser ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers
USER devuser
WORKDIR /home/devuser
