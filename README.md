# 🛡️ PowerShell Suspicious Web Request Detection & Incident Response

---
## 🛡️ Objective
- Threat actors who gain access to a system often leverage built-in tools such as PowerShell to download additional payloads from the internet. This technique allows attackers to blend into legitimate administrative activity while expanding control, establishing persistence, or communicating with command-and-control (C2) infrastructure.
- The objective of this project was to:
- Detect suspicious PowerShell web requests using Microsoft Sentinel
- Trigger and investigate an incident generated from this behavior
- Respond to and close the incident following the NIST 800-61 Incident Response Lifecycle

  <img width="650" height="300" alt="image" src="https://github.com/user-attachments/assets/86b36fba-7284-4f11-b573-d0daba6a3186" />




---
## 🎯 Environment & Tools
- Azure Virtual Machine (Windows)
- Microsoft Defender for Endpoint (MDE)
- Microsoft Sentinel (SIEM)
- Log Analytics Workspace
- PowerShell
- MITRE ATT&CK Framework

---

## 🎯 Threat Scenario Overview

- During post-exploitation activity, an attacker may use PowerShell commands such as Invoke-WebRequest to download scripts or binaries directly from external servers. These scripts may be    executed immediately or staged for later use.

- This behavior is commonly associated with:
- Malware deployment
- Credential harvesting
- Data exfiltration
- C2 communication
- Detecting PowerShell-driven downloads is critical for identifying active compromise.

---

## 🎯 Part 1: Detection – Sentinel Analytic Rule
- Detection Logic
- A Scheduled Query Rule was created in Microsoft Sentinel to detect PowerShell processes executing web download commands.


** Sample Query **

<img width="800" height="350" alt="image" src="https://github.com/user-attachments/assets/3f7a8b70-18e7-4a11-96a7-141ab7dab448" />

---

## 🎯 Part 2: Alert & Incident Creation

Once the detection rule is enabled, suspicious PowerShell activity was generated on the VM.
This resulted in a Sentinel incident titled: “PowerShell Suspicious Web Request”


<img width="700" height="300" alt="image" src="https://github.com/user-attachments/assets/5b0be281-284a-4daa-8c27-dcb5288e04be" />

---

## 🎯 Part 3: Detection & Analysis


#### ** Identify and validate the incident **
- I Observe the incident and assign it to myself, set the status to Active.
- Investigate the Incident by Actions → Investigate (sometimes takes time for entities to appear)


<img width="900" height="294" alt="image" src="https://github.com/user-attachments/assets/fa391f13-37ba-4a6c-8d8a-f6802d75be4f" />

#### ** Gather relevant evidence and assess impact **
  - In this case the actual script files would be evidence, but are not necessarily the threat. The threat would be how they got there in the first place, or why the user (or system            account) is downloading them and executing them.
  - In real life, this could have happened from accidentally downloading malware or installing a game or free software or any number of ways.
  
#### ** Notes: For the lab, let’s pretend like I contacted the user and they said they recently installed a free software at the same time the events took place. (But the reality is in this        case, the attack simulator downloaded the scripts and executed them.

#### ** Investigation evidence: ** 

- #### Entities (3)
 - Device: soclab1
 - User: soclab1
 - "powershell.exe" -ExecutionPolicy Bypass -Command Invoke-WebRequest -Uri https://raw.githubusercontent.com/joshmadakor1/lognpacific-public/refs/heads/main/cyber-range/entropy-gorilla/     eicar.ps1 -OutFile C:\programdata\eicar.ps1


<img width="850" height="350" alt="image" src="https://github.com/user-attachments/assets/6e68b4ec-a2f0-4cf4-8b81-b497c58165e0" />


#### ** Findings **

The incident was triggered on one device  "Soclab" by one user account "Soclab1"
PowerShell was used to download malicious script "eicar.ps1" from external URLs: https://raw.githubusercontent[.]com/joshmadakor1/lognpacific-public/refs/heads/main/cyber-range/entropy-gorilla/eicar[.]ps1

---

## 🎯 Containment, Eradication, and Recovery

- Affected machine was isolated in Microsoft Defender for Endpoint
- Network communication restricted

### Eradication

- Anti-malware scan initiated from MDE
- Downloaded scripts reviewed to determine execution status



Execution Review

It was observed that after download:

Script ___ was executed by the ___ account

Script ___ was not executed

Script Analysis

Script ___ was observed to _______

Script ___ was observed to _______

In this lab, scripts were executed by the attack simulator. In a real-world scenario, further root cause analysis would be required.

---

## 🎯 Post-Incident Activities

- Incident notes documented in Sentinel
- Lessons learned recorded


#### 🎯 Recommendations identified:
- Restrict PowerShell usage via policy
- Enhance endpoint attack surface reduction rules

---
## 🎯 Sentinel Rule Configuration

A Microsoft Sentinel analytics rule was created to alert on brute force authentication patterns.


<img width="500" height="250" alt="image" src="https://github.com/user-attachments/assets/643700b0-c3b9-466f-ba59-466049398bd1" />


