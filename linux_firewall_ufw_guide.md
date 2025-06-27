
# 🔥 Linux Firewall (UFW) Configuration and Testing Guide

This guide documents the steps I followed to inspect, modify, and test basic firewall rules using `ufw` (Uncomplicated Firewall) on a Linux system (tested on Kali Linux).

---

## 1. Open the Firewall Tool

Make sure UFW is installed and enabled:
```bash
sudo ufw status verbose
```

If UFW is inactive:
```bash
sudo ufw enable
```

---

## 2. List Current Firewall Rules

Check existing rules and their status:
```bash
sudo ufw status numbered
```

This shows a list of all current rules with rule numbers for easy reference/deletion.

---

## 3. Add a Rule to Block Inbound Traffic on Port 23 (Telnet)

To block Telnet:
```bash
sudo ufw deny 23/tcp
```

This blocks any incoming traffic trying to access your machine on TCP port 23.

---

## 4. Test the Block Rule

Try connecting to port 23 locally:
```bash
telnet localhost 23
```

Expected: connection should be refused or timeout.  
Alternative using netcat:
```bash
nc -zv localhost 23
```

---

## 5. Allow SSH (Port 22) Just in Case

If working over SSH or need remote access, allow it before applying stricter rules:
```bash
sudo ufw allow 22/tcp
```

---

## 6. Remove the Test Rule

To delete the Telnet block rule:
```bash
sudo ufw delete deny 23/tcp
```

Or delete by rule number if known from step 2:
```bash
sudo ufw delete <number>
```

---

## 7. Commands Summary

| Action                 | Command                          |
|------------------------|----------------------------------|
| Enable UFW             | `sudo ufw enable`               |
| Check status           | `sudo ufw status verbose`       |
| List rules             | `sudo ufw status numbered`      |
| Block port 23 (Telnet) | `sudo ufw deny 23/tcp`          |
| Allow port 22 (SSH)    | `sudo ufw allow 22/tcp`         |
| Delete block rule      | `sudo ufw delete deny 23/tcp`   |

---

## 8. How UFW Filters Traffic (Summary)

UFW is a frontend to iptables that helps control what traffic is allowed or denied. It filters:

- 🔐 **Inbound traffic** (e.g., someone trying to connect to your machine)
- 📤 **Outbound traffic** (you accessing outside services)

Each rule specifies a **direction**, a **protocol** (TCP/UDP), and a **port**. Based on these rules, UFW allows or blocks packets as they come in or go out of your system.

---

> ✅ Tested on Kali Linux 2024 with UFW. Handy for quickly locking down insecure ports and ensuring safe SSH access.
