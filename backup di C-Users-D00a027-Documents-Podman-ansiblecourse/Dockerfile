FROM ubuntu:latest

# Install necessary packages
RUN apt-get update && apt-get install -y \
    sudo vim openssh-server passwd && \
    rm -rf /var/lib/apt/lists/*

# Add user and set password
RUN useradd -m -s /bin/bash automation && echo "automation:automation" | chpasswd

# Add the user to the sudoers file
RUN usermod -aG sudo automation

# Enable SSH service (optional)
RUN mkdir /var/run/sshd

# Set the default working directory for the user
WORKDIR /home/automation
