### Report:
  Author: Harish Babu G
  Project title: "Linux Server Hardening – Network Traffic Capture & Analysis"

### Overview: 
  The project focuses on hardening a Linux server by securing SSH, configuring a
  firewall, installing Fail2ban to block brute-force attacks, and capturing network
  traffic to verify the effectiveness of the security measures.

### Steps performed:
  ssh_key_generation_and_copy:
    description: >
      Generated RSA key pair on Kali and copied the public key to Ubuntu container
      to enable key-based login.
    commands:
      - "ssh-keygen -t rsa -b 4096 -f ~/.ssh/harish_key"
      - "ssh-copy-id -i ~/.ssh/harish_key.pub harish@<container_ip>"

  ssh_hardening:
    description: "Disabled root login and password authentication in SSH config."
    commands:
      - "sudo nano /etc/ssh/sshd_config"
      - "PermitRootLogin no"
      - "PasswordAuthentication no"
      - "sudo systemctl restart sshd"

  ufw_firewall_configuration:
    description: "Configured UFW firewall to allow only SSH traffic."
    commands:
      - "sudo ufw allow 22/tcp"
      - "sudo ufw default deny incoming"
      - "sudo ufw default allow outgoing"
      - "sudo ufw enable"
      - "sudo ufw status verbose"

  fail2ban_installation_and_configuration:
    description: "Installed and configured Fail2ban to protect SSH."
    commands:
      - "sudo apt update"
      - "sudo apt install fail2ban -y"
      - "sudo nano /etc/fail2ban/jail.local"
      - |
        [sshd]
        enabled = true
        port = 22
        maxretry = 3
        bantime = 3600
      - "sudo systemctl restart fail2ban"
      - "sudo fail2ban-client status sshd"

  traffic_capture_and_analysis:
    description: >
      Captured SSH traffic using tcpdump, transferred pcap to Kali, and analyzed
      in Wireshark. Verified no passwords were transmitted due to key-based login.
    commands:
      - "sudo tcpdump -i any port 22 -w ssh_traffic.pcap"
      - "docker cp ubuntu_hardening:/root/ssh_traffic.pcap ~/Desktop/ssh_traffic.pcap"
      - "wireshark ~/Desktop/ssh_traffic.pcap"

### before_after_summary:
  - feature: "Root SSH login"
    before: "Allowed"
    after: "Disabled"
  - feature: "Password authentication"
    before: "Enabled"
    after: "Disabled (key-based only)"
  - feature: "UFW firewall"
    before: "Not active"
    after: "Active, only port 22 allowed"
  - feature: "Fail2ban"
    before: "Not installed"
    after: "Active with SSH protection"
  - feature: "Traffic security"
    before: "Passwords visible if attempted"
    after: "All SSH traffic encrypted"

### recommendations:
  - "Maintain key-based SSH only; permanently disable password login."
  - "Keep Fail2ban active to monitor suspicious login attempts."
  - "Regularly update UFW rules to block unnecessary ports."
  - "Periodically monitor network traffic to ensure no sensitive data leaks."

### screenshots:
  description: "All relevant screenshots of SSH config, UFW, Fail2ban, and Wireshark analysis can be   found in the 'screenshots' folder."

