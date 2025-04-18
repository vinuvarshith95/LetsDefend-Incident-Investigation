# 🛡️ Incident Response: SOC338 – Lumma Stealer via DLL Side-Loading (Phishing Attack)

Welcome! This case study documents a real-world incident investigation I performed on the **LetsDefend** platform. The alert was associated with **SOC338** and involved the delivery of the **Lumma Stealer malware** through a **Click-Fix phishing campaign** using **DLL side-loading** techniques.

🔗 **Type:** Phishing → Post-Exploitation → Credential Stealing  
📆 **Status:** Resolved | Host Contained | Email Removed  
📌 **Platform:** LetsDefend SOC

## 🎯 Objective

To perform a complete investigation and incident response on a **critical phishing alert** that resulted in malware execution. Key goals included:

- Identifying the origin and nature of the phishing attack  
- Tracing malware behavior on the endpoint  
- Correlating logs and actions across email, endpoint, and HTTP  
- Taking immediate containment actions to limit further damage  

## 🛠 Tools & Features Used

| Tool / Feature          | Purpose                                           |
|-------------------------|---------------------------------------------------|
| LetsDefend SOC Platform | Alert triage, case management, and containment    |
| Email Security Panel    | Analyzing suspicious email metadata and content   |
| VirusTotal              | URL and payload reputation scanning               |
| Endpoint Security Logs  | Tracing PowerShell activity and URL access        |
| Log Management          | Reviewing HTTP logs and execution context         |
| EDR Dashboard           | Host containment and live user tracking           |

## 🧠 Skills Demonstrated

- SOC alert triage and structured playbook execution  
- Email header and phishing content analysis  
- Threat intelligence lookups for URLs and payloads  
- PowerShell behavior investigation post-email interaction  
- Endpoint and log-based correlation  
- Host isolation and incident resolution  

## 🪜 Investigation Workflow

### 1. 📨 **Alert Detection & Email Analysis**
- Alert SOC338 appeared in **Monitoring dashboard** with a suspicious IP.
- Created a case and started the playbook.
- Investigated the phishing email:
  - Sender appeared abnormal
  - Body contained a suspicious “Update Now” URL
- **VirusTotal check** confirmed it was **malicious** (flagged by 10+ vendors)

### 2. 💻 **Endpoint Behavior Review**
- Tracked user activity tied to the suspicious email
- Found URL accessed through the browser (`chrome.exe`)
- Discovered **PowerShell commands** executed soon after
- Extracted URL from PowerShell → **also malicious** (VirusTotal confirmed)
- Concluded: **Malware execution via DLL side-loading** in progress

### 3. 🧾 **Log Management Correlation**
- HTTP logs showed:
  - 200 OK response from malicious server
  - Referrer: internal email system
  - Execution by process: `chrome.exe`
- Confirmed user clicked the phishing link directly from their mailbox

### 4. 🔒 **EDR Containment**
- Identified the endpoint:  
  - Host: Windows 10  
  - User: Dylan  
- Verified active session during attack
- Used **EDR containment** to isolate the machine and block communication

## ✅ Outcome

- **Confirmed** the phishing attempt and execution of **Lumma Stealer**  
- Successfully **contained** the infected host  
- **Removed** the phishing email from the user's inbox  
- Preserved logs and artifacts for further analysis  
- Built a full chain-of-evidence across email → URL → PowerShell → execution  

## 📊 Summary

| Field              | Detail                          |
|-------------------|----------------------------------|
| **Attack Vector** | Phishing (Click-Fix style email) |
| **Malware**       | Lumma Stealer (Credential Theft) |
| **Technique**     | DLL Side-Loading via phishing link |
| **Initial Access**| IP `132.232.40.201`              |
| **Victim Host**   | `172.16.17.216` (User: Dylan)    |
| **Impact**        | Post-exploitation activity + data risk |
| **Status**        | Host isolated, threat neutralized |

## 🧩 Key Takeaways

- **Email security** is your first line of defense — always verify sender and links  
- **Threat intel tools** like VirusTotal are vital in triage  
- **PowerShell logs** often reveal post-compromise behavior  
- **Log correlation** provides context and confidence  
- **Fast containment** prevents lateral movement and further compromise  

## 🙌 Thanks for Reading

This case reinforced how even “routine” phishing can escalate quickly — and how important it is to correlate data across systems to respond swiftly.  
Stay tuned for more incident response case studies!

