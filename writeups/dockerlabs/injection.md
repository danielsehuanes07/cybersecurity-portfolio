# Injection (DockerLabs)

**Platform:** DockerLabs  
**Difficulty:** Easy  
**Skills:** Enumeration, Web, Privilege Escalation

---

## 🧭 Summary
In this machine I performed basic enumeration, exploited a web vulnerability to gain access, and then escalated privileges to root.

---

## 1) Recon
I started with an Nmap scan to identify open ports and services.

```bash
nmap -sC -sV <IP> -oN nmap-basic.txt
nmap -p- --min-rate 5000 <IP> -oN nmap-full.txt
