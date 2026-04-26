---
{"dg-publish":true,"permalink":"/wiki/tech-and-engineering/git-and-dev-infra/new-vps-setup/","title":"New VPS Setup","tags":["Linux","Guide","Server","Technology"],"dg-note-properties":{"type":"Page","collections":"Knowledge & Guides","title":"New VPS Setup","aliases":null,"description":"A comprehensive step-by-step guide for setting up a new Linux VPS, including user creation, group assignments, SSH key configuration, and securing SSH access.","icon":null,"createdAt":"2025-11-29T08:54:28.634Z","lastUpdated":"2026-01-31T13:56:46.941Z","tags":["Linux","Guide","Server","Technology"],"coverImage":null}}
---


# New VPS Setup

## Setting up new user
First create a new user
```bash
sudo adduser username
```

Then add user to sudo and docker groups
```bash
sudo usermod -aG sudo username && sudo usermod -aG docker username
```

You can then also switch and verify with
```bash
su - username 
# And then run
whoami
```

Make sure to replace "username" with your desired username. The adduser command will prompt you to set a password and provide optional information like full name and phone number.

## Configure SSH
Now we will disable `root` account login and only allow login via SSH-Key authentication for the newly created user

First, generate SSH keys on your local machine if you haven't already:
```bash
ssh-keygen -t ed25519 -C "your_email@example.com"
```

Copy your public key to the server:
```bash
ssh-copy-id username@your_server_ip
```

Or manually add it to authorized_keys:
```bash
mkdir -p ~/.ssh
chmod 700 ~/.ssh
echo "your_public_key_content" >> ~/.ssh/authorized_keys
chmod 600 ~/.ssh/authorized_keys
```

Edit the SSH configuration file:
```bash
sudo nano /etc/ssh/sshd_config
```

Make the following changes to disable root login and password authentication:
```bash
# Disable root login
PermitRootLogin no
# Disable password authentication
PasswordAuthentication no
ChallengeResponseAuthentication no
# Allow only key-based authentication
PubkeyAuthentication yes
# Optional: Change SSH port (for additional security)
# Port 2222
```

Alternatively to enable root login but only via ssh
```bash
# Enable root login but only via SSH key
PermitRootLogin prohibit-password
```

This setting allows root login but only with SSH key authentication, not with passwords. It's generally more secure to use a non-root user with sudo privileges, but this option is available if needed.

Restart the SSH service to apply changes:
```bash
sudo systemctl restart sshd
# Depending on OS it might also be
sudo systemctl restart ssh
```

Important: Before logging out, test the new configuration in a new terminal session to ensure you can still access the server. This prevents lockouts.

```bash
ssh username@your_server_ip
```

