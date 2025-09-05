FROM ubuntu:22.04

ENV DEBIAN_FRONTEND=noninteractive

# Install required packages
RUN apt-get update && apt-get install -y \
    curl sudo sshpass iputils-ping \
    net-tools git python3 python3-pip \
    ansible

# Add Python networking packages
RUN pip3 install netmiko paramiko

# Install Containerlab
RUN curl -sL https://get.containerlab.dev | bash

# Manually install NetOpsChic SR Linux plugin modules
RUN mkdir -p /usr/share/ansible/collections/ansible_collections/nokia/srlinux && \
    git clone https://github.com/NetOpsChic/srlinux-ansible-collection.git /tmp/srlinux-ansible && \
    cp -r /tmp/srlinux-ansible/ansible_collections/nokia/srlinux/plugins /usr/share/ansible/collections/ansible_collections/nokia/srlinux/ && \
    rm -rf /tmp/srlinux-ansible

# Create non-root user (and home)
RUN useradd -ms /bin/bash devuser && echo "devuser ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers

# Add default ansible.cfg AFTER user home exists
RUN echo "[defaults]\n\
inventory = ansible/inventory.ini\n\
collections_paths = /usr/share/ansible/collections\n\
host_key_checking = False\n\
retry_files_enabled = False\n\
stdout_callback = yaml\n\
timeout = 10" > /home/devuser/ansible.cfg && \
    chown devuser:devuser /home/devuser/ansible.cfg

# Switch to devuser
USER devuser
WORKDIR /home/devuser
