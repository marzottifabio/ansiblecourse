# Ansible course

## Module 1

### Memo, how to start the environment for the course
```bash
a- open 'Terminale'
b- podman machine start
c- docker-compose -f "C:\Users\D00a027\Documents\Podman\ansiblecourse\compose.yaml" up -d
```

### Control Node Setup

1. Create and configure automation user (made in the the dockerfile):
```bash
useradd automation
passwd automation  # Set password to 'automation'
sudo mkdir /home/automation
sudo chown automation:automation /home/automation
```

2. Configure sudo access (made in the the dockerfile):
```bash
vim /etc/sudoers.d/automation
# Add the following line:
automation ALL=(ALL) NOPASSWD:ALL
```

3. Test the configuration:
```bash
podman exec -it ansible-controller /bin/bash
su automation
```

4. Generate SSH key:
```bash
ssh-keygen  # Use default path: /home/automation/.ssh/id_rsa
            # Don't set password or passphrase
            # this to create the ssh key in /home/automation/.ssh/id_rsa
```

5. Set proper permissions:
```bash
sudo chown -R automation:automation /home/automation
```

6. Configure hosts file:
```bash
vim /etc/hosts
# Add the following entries:
10.89.0.2       ansible-controller ansible-controller
10.89.0.3       servera servera
10.89.0.4       serverb serverb
```

7. Copy SSH keys to managed nodes:
```bash
su automation
ssh-copy-id servera
ssh-copy-id serverb
```

8. Test SSH connectivity:
```bash
su automation
ssh servera  # Should connect without password
ssh serverb  # Should connect without password
```

9. Install Ansible:
```bash
su automation
sudo apt-get update
sudo apt-get install ansible
sudo ansible --version
```

### Server A Setup

1. Create and configure automation user (made in the the dockerfile):
```bash
useradd automation
passwd automation  # Set password to 'automation'
sudo mkdir /home/automation
sudo chown automation:automation /home/automation
```

2. Configure sudo access (made in the the dockerfile):
```bash
vim /etc/sudoers.d/automation
# Add the following line:
automation ALL=(ALL) NOPASSWD:ALL
```

3. Test the configuration:
```bash
su automation
```

### Server B Setup

1. Create and configure automation user (made in the the dockerfile):
```bash
useradd automation
passwd automation  # Set password to 'automation'
sudo mkdir /home/automation
sudo chown automation:automation /home/automation
```

2. Configure sudo access (made in the the dockerfile):
```bash
vim /etc/sudoers.d/automation
# Add the following line:
automation ALL=(ALL) NOPASSWD:ALL
```

3. Test the configuration:
```bash
su automation
```

### Network Configuration

All nodes should have these entries in their hosts file:
```bash
10.89.0.2       ansible-controller ansible-controller
10.89.0.3       servera servera
10.89.0.4       serverb serverb
```

## Module 2

### Inventory files

1. Create inventory file in automation user's home directory (file name inventory):
```ini
servera
[group1]
serverb
[group2]
servera
[group12:children]
group1
group2
```

2. Test connectivity with Ansible:
```bash
ansible webservers -m ping -i inventory
```

Note: A green "SUCCESS" status indicates proper configuration.

> Ad hoc commands explanation: Ansible ad hoc commands are quick, simple commands that allow executing specific tasks on one or more managed nodes without writing a full playbook. They use the `/usr/bin/ansible` command-line tool and are ideal for rarely repeated tasks or one-time operations.
Inventory files can be written in ini o yaml format.

```bash
ansible all --list-hosts -i inventory
ansible ungrouped --list-hosts -i inventory
ansible group1 --list-hosts -i inventory
```

