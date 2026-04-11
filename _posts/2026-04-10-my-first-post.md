---
title: Building a Security Sentinel - My First Bash IPS
date: 2026-04-10 18:30:00 -0700
categories: [Cybersecurity, Projects]
tags: [bash, linux, networking, security]
description: A deep dive into creating an automated network reconnaissance and IP banning tool.
---

## Introduction
Alright, it's live. I'll just start here. At the end of IT135 we were asked to think of some script to build that would help us on our career.  So, I thought I'd develop a little tool for network maintenance. First, I wanted it to detect what OS you're using, and what shell. One of the challenges of writing security tools is that log locations change depending on the OS. For example, macOS uses the log show command, while Linux systems typically look at /var/log/auth.log. By using the uname command at the start, the Security Sentinel can pivot its logic based on the host environment.

``` bash
# OS Detection Logic
# Detects if the script is running on macOS or Linux to handle log paths correctly.

OS_TYPE=$(uname)

if [[ "$OS_TYPE" == "Darwin" ]]; then
    PLATFORM="mac"
    echo "System detected: macOS"
elif [[ "$OS_TYPE" == "Linux" ]]; then
    PLATFORM="linux"
    echo "System detected: Linux"
else
    PLATFORM="unknown"
    echo "Unknown OS. Proceeding with caution."
fi
```

Next it checked all ports against allowed ports. 
> "In a secure network, every open port is a conversation you didn't start, and every unknown IP is a guest you didn't invite. The Sentinel’s job is to check the guest list at the door."

``` bash
# IP Banning Logic
# Blocks a target IP using iptables after it crosses the threat threshold

echo "ALERT: Threat threshold exceeded for $target_ip"
echo "Banning IP address... "

# The -A appends the rule to the INPUT chain
# The -j DROP tells the firewall to ignore all traffic from this source
sudo iptables -A INPUT -s "$target_ip" -j DROP

echo "IP $target_ip has been added to the blacklist."
```
> "And then to clean it up so the IPs are free for use in the future"

```bash
# IP Release Logic
# Removes the block for a specific IP address

echo "Initiating release for $target_ip..."

# The -D deletes the specific rule we created earlier
sudo iptables -D INPUT -s "$target_ip" -j DROP

echo "Access restored for $target_ip. Firewall updated."
```
All in all, it was a fun little project and if you're interested, I shared it below.

## Deployment & Usage

To use the Sentinel in a production environment, follow these steps to ensure the script has the correct permissions:

1. **Clone the repository:**
   `git clone https://github.com/akempley/security-sentinel.git`

2. **Set Executable Permissions:**
   The script requires execution rights. Use `chmod` to set them:
   ```bash
   chmod +x security_sentinel.sh
   ```

### Key Features
* **Cross-Platform Compatibility:** Automatic detection for macOS (Darwin) and Linux.
* **Intelligent Logging:** Switches between `log show` and `/var/log/auth.log` based on environment.
* **Automated Mitigation:** Instant IP banning via `iptables` once a threat threshold is met.
* **Administrative Recovery:** Built-in IP release function to prevent permanent accidental lockouts.