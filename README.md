# Man-in-the-Middle (MITM) Attack Demonstration

**Coursework Project — Information Security**
Instructor: Dr. Subhash Sagar

## Overview
This project is a hands-on demonstration of a Man-in-the-Middle (MITM) attack, carried out in an isolated virtual lab. The goal was to see, in practice, how unencrypted (HTTP) traffic can expose sensitive information such as login credentials to someone on the same network, and to understand why defenses like HTTPS and network segmentation matter.

**This was performed entirely in a private, isolated environment against a site that is deliberately built for security testing practice — not against any real or third-party system!**

## Demo Video
The full walkthrough (VM setup → traffic interception → captured credentials) is available here: **[https://drive.google.com/file/d/1pJ_ey3yAKniEhplLTQjMdMtMV7ni7zQ7/view?usp=sharing]**
*(Video hosted externally due to file size; not included directly in this repository.)*

## Lab Setup

| Component | Details |
|---|---|
| Host OS | Windows |
| Attacker VM | Kali Linux (Oracle VirtualBox) |
| Target application | testphp.vulnweb.com (Acunetix's intentionally vulnerable test site) |
| Tools used | Wireshark , Ettercap|

## What Was Done
The task was carried out in two parts, using two different tools to achieve the same goal — capturing unencrypted traffic on the network.

**Part 1: Wireshark (passive capture)**
1. Set up a Kali Linux virtual machine in VirtualBox as the attacker system, on the same network as the victim traffic.
2. Used **Wireshark** to passively capture and filter live traffic (filtered on `http`) between the victim machine and the target site.
3. Submitted a login form on the target site and located the matching `POST` request in the capture.
4. Opened the packet and showed the submitted username and password visible in **plaintext**, since the site used HTTP instead of HTTPS.

**Part 2: Ettercap (active interception)**
1. Used **Ettercap** on the same Kali Linux VM to actively intercept traffic between the victim machine and the network.
2. Repeated the login submission and confirmed that the credentials were exposed the same way, demonstrating that the traffic could be captured even without direct access to the victim's own machine — as long as the connection was on the same network.

## Key Finding
Both approaches led to the same result: because the login form was sent over plain HTTP, the credentials were readable in plaintext. Wireshark showed this could be seen by anyone already positioned to observe network traffic, while Ettercap showed that an attacker could actively place themselves on the network to intercept traffic that wasn't otherwise visible to them. This reinforces that the real vulnerability is the **lack of encryption**, not just one specific tool or technique.

## How This Is Prevented in Practice

| Risk | Mitigation |
|---|---|
| Credentials sent in plaintext over HTTP | Enforce HTTPS (TLS) on all login pages, plus HSTS to block downgrades to HTTP |
| Traffic interception on a local network | Dynamic ARP Inspection (DAI) on switches, static ARP entries for critical devices |
| Attacker on the same network segment | Network segmentation / VLANs to separate trusted and untrusted devices |
| Interception going unnoticed | Port security and network monitoring/IDS to flag unusual traffic patterns |


## Disclaimer
This project was carried out strictly for educational purposes, in a private lab, against a site published specifically for security testing practice. These techniques should never be used on any system or network without explicit authorization.
