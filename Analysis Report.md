### Report
Author: Harish Babu G

Project title: "Linux Server Hardening – Network Traffic Capture & Analysis"

### Overview
  This project focuses on strengthening the security posture of an Ubuntu server 
  running in a Docker container by applying industry-standard hardening measures. 
  The main objectives include securing SSH access, implementing firewall rules, 
  installing and configuring Fail2ban to prevent brute-force attacks, and analyzing 
  network traffic to confirm that credentials are never transmitted in plaintext. 
  The overall goal is to reduce the server's attack surface and ensure resilience 
  against unauthorized access and network-based attacks.

### Environment
host_system: "Kali Linux"
target_system: "Ubuntu Docker container with SSH enabled"
user: "harish"
tools_used:
  - "ssh, ssh-keygen, ssh-copy-id (for SSH key-based authentication)"
  - "ufw (Uncomplicated Firewall for network access control)"
  - "fail2ban (to detect and block brute-force login attempts)"
  - "tcpdump, Wireshark (for network traffic capture and analysis)"

### Steps Performed

#### SSH Key Generation and Copy
  Generated a secure RSA key pair on Kali Linux with 4096-bit encryption. The public 
  key was copied to the Ubuntu container to enforce key-based authentication. 
  This ensures that SSH login is possible only using cryptographic keys, 
  eliminating password-based vulnerabilities.
commands_summary: 
  Full commands provided in the 'applied_commands_list.txt' file.

#### SSH Hardening
  Configured SSH daemon to disable root login and password authentication by editing 
  /etc/ssh/sshd_config. Restarted SSH service to apply the changes. These measures 
  prevent attackers from directly targeting the root account or guessing passwords 
  via brute-force attacks.

#### UFW Firewall Configuration
  Enabled UFW firewall and configured it to allow only SSH traffic on port 22 while 
  denying all other incoming connections. Outgoing traffic was allowed by default. 
  Firewall status was verified to ensure only permitted traffic can reach the server.

#### Fail2ban Installation and Configuration
  Installed Fail2ban and configured it to monitor SSH login attempts. Any IP exceeding 
  3 failed login attempts within the defined interval is temporarily banned. This 
  prevents automated brute-force attacks and reduces the risk of unauthorized access.

#### Traffic Capture and Analysis
  Captured live SSH traffic using tcpdump and transferred the pcap file to Kali 
  Linux for analysis in Wireshark. Filters were applied to focus on port 22 traffic. 
  Observations confirmed that all SSH traffic is encrypted after the initial handshake, 
  including authentication attempts. No plaintext passwords or credentials were visible.

### Before and After Summary
  The table below summarizes the key security features before and after hardening:

| Feature                  | Before Configuration           | After Configuration                      |
|---------------------------|--------------------------------|-----------------------------------------|
| Root SSH Login            | Allowed                        | Disabled                                 |
| Password Authentication   | Enabled                        | Disabled (key-based only)                |
| UFW Firewall              | Not active                     | Active, only port 22 allowed            |
| Fail2ban                  | Not installed                  | Installed, monitoring SSH                |
| Traffic Security          | Passwords could be exposed     | All SSH traffic encrypted and secure    |

### Network Traffic Analysis Findings
  Detailed analysis of SSH traffic revealed the following:
  - Handshake packets confirm exchange of key algorithms and initiation of encryption.
  - "New Keys" packets indicate encryption is active and all subsequent traffic is secure.
  - TCP stream analysis shows authentication data is fully encrypted; plaintext passwords are not visible.
  - Metadata indicates the authentication method used, but credentials remain protected.
  - Even temporary enabling of password authentication did not expose passwords in transit.

### Conclusion
  The applied hardening measures significantly improved the security posture of the server:
  - Root login disabled, reducing high-privilege attack risk.
  - Password authentication disabled, enforcing key-based authentication.
  - Firewall restricted access to only authorized services.
  - Fail2ban actively protects against repeated login attempts.
  - Network traffic is encrypted, preventing credential interception.
  Overall, the server is resilient against brute-force attacks, unauthorized logins, and network eavesdropping.

### Recommendations
- Maintain key-based SSH login and permanently disable password authentication.
- Keep Fail2ban running to automatically block suspicious activity.
- Regularly review and update UFW rules to close unnecessary ports.
- Periodically capture and analyze network traffic to ensure no sensitive data leaks.
- Consider additional hardening measures such as SSH rate-limiting and monitoring system logs.

### Screenshots
  All relevant screenshots, including SSH configuration, UFW status, Fail2ban monitoring, 
  and Wireshark traffic analysis, are included in the 'screenshots' folder.
