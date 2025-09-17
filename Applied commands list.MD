### Applied Commands List
Author: Harish Babu G

Project title: "Linux Server Hardening – Applied Commands List"

### SSH Key Generation and Copy
description: >
  Generate a secure RSA key pair on Kali Linux and copy the public key to the Ubuntu container 
  to enable key-based SSH login.

commands:
```
  - "ssh-keygen -t rsa -b 4096 -f ~/.ssh/harish_key  # Generate 4096-bit RSA key pair"
  - "ssh-copy-id -i ~/.ssh/harish_key.pub harish@localhost  # Copy public key to Ubuntu container"
```
### SSH Hardening
description: >
  Disable root login and password authentication on Ubuntu SSH server.

commands:
 ```
  - "sudo nano /etc/ssh/sshd_config  # Edit SSH daemon configuration"
  - "PermitRootLogin no               # Disable root login"
  - "PasswordAuthentication no        # Disable password-based login"
  - "sudo systemctl restart sshd      # Restart SSH service to apply changes"
  - "ssh -i ~/.ssh/harish_key harish@<container_ip>  # Test key-based login"
```
### UFW Firewall Configuration
description: >
  Configure Uncomplicated Firewall (UFW) to allow only SSH traffic on port 22.

commands:
```
  - "sudo ufw allow 22/tcp               # Allow SSH traffic"
  - "sudo ufw default deny incoming      # Deny all other incoming connections"
  - "sudo ufw default allow outgoing     # Allow all outgoing traffic"
  - "sudo ufw enable                     # Enable UFW firewall"
  - "sudo ufw status verbose             # Verify firewall status"
```
### Fail2ban Installation and Configuration
description: >
  Install and configure Fail2ban to protect SSH from brute-force attacks.

commands:
```
  - "sudo apt update                      # Update package lists"
  - "sudo apt install fail2ban -y         # Install Fail2ban"
  - "sudo nano /etc/fail2ban/jail.local   # Create and edit local jail configuration"
  - |
    [sshd]
    enabled = true
    port = 22
    maxretry = 3
    bantime = 3600
  - "sudo systemctl restart fail2ban      # Restart Fail2ban service"
  - "sudo fail2ban-client status sshd     # Verify SSH jail status"
```
### SSH Traffic Capture
description: >
  Capture SSH traffic to verify encryption and absence of plaintext passwords.

commands:
```
  - "sudo tcpdump -i any port 22 -w ssh_traffic.pcap  # Capture SSH packets"
  - "docker cp ubuntu:/root/ssh_traffic.pcap ~/Desktop/ssh_traffic.pcap  # Transfer pcap to Kali"
  - "wireshark ~/Desktop/ssh_traffic.pcap             # Analyze traffic in Wireshark"
```
### Traffic Analysis Verification
description: >
  Use Wireshark to confirm encryption and review SSH session metadata.

commands:
```
  - "tcp.port == 22  # Apply filter to display only SSH traffic"
  - "Follow TCP stream  # View the full session content, verify encrypted payloads"
```
