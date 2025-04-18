# 🛡️ Incident Response: SOC168 – Command Injection via HTTP POST (whoami Detected)

Welcome! This case study highlights a real-world incident investigation conducted on the **LetsDefend SOC platform**. The alert, linked to **SOC168**, flagged a suspicious HTTP POST request containing a system command — `whoami` — embedded in the request body. The investigation confirmed this was a successful **command injection attack** originating from an external IP, targeting an internal web server.

🔗 **Type:** Web Attack → Command Execution → Reconnaissance  
📆 **Status:** Resolved | Host Contained | Exploitation Confirmed  
📌 **Platform:** LetsDefend SOC

## 🎯 Objective

To validate the nature of a potentially malicious HTTP request and determine whether it was a real exploitation attempt. Goals included:

- Identifying the intent behind the payload  
- Confirming system-level command execution on the target server  
- Determining the attack direction and scope  
- Containing the compromised host to stop further risk  

## 🛠 Tools & Features Used

| Tool / Feature          | Purpose                                               |
|-------------------------|-------------------------------------------------------|
| LetsDefend SOC Platform | Alert triage, log review, and containment execution   |
| Raw HTTP Log Viewer     | Extracting and analyzing POST request details         |
| Endpoint Telemetry      | Verifying shell access and executed commands          |
| Network Traffic Analysis| Confirming source and direction of traffic            |
| Containment Module      | Host isolation and incident response enforcement      |

## 🧠 Skills Demonstrated

- HTTP payload and header analysis  
- Identification of command injection patterns  
- Correlation between web logs and endpoint activity  
- Real vs. test/simulation traffic evaluation  
- Response orchestration and host isolation  

## 🪜 Investigation Workflow

### 1. 🔔 **Alert Trigger & Initial Clues**
- Alert `SOC168` triggered by the detection of `whoami` in a POST request body
- Suspicious request sent to:  
  `https://172.16.17.16/video/`
- User-Agent:  
  `Mozilla/4.0 (compatible; MSIE 6.0; Windows NT 5.1; SV1)` — indicating potential evasion attempt

### 2. 📄 **HTTP Log Review**
- Parsed alert details:
  - Method: POST  
  - Payload: `?c=whoami`  
  - Response: `200 OK`  
  - Size: 912 bytes  
  - Source IP: `61.177.172.87` (external)  
  - Destination IP: `172.16.17.16` (internal)  
  - Hostname: `WebServer1004`

### 3. 🔍 **Threat Verification**
- Verified that the request did **not** originate from any internal red team, simulation, or pentest tool  
- No signs of tools like Verodin, AttackIQ, or Picus  
- Determined traffic direction: **Internet → Internal server**  
- Classified the payload as a **real attack**, not a simulation

### 4. 💻 **Endpoint Confirmation**
- Terminal history from `WebServer1004` revealed the attacker executed multiple system commands:
  - `ls`  
  - `whoami`  
  - `uname`  
  - `cat /etc/passwd`  
  - `cat /etc/shadow`
- Confirmed **command injection was successful**, with real data returned

### 5. 🔒 **Containment Action**
- Containment requested and approved according to organizational policy  
- Endpoint `WebServer1004` was successfully isolated using the containment feature  
- No further connections or movement detected from the compromised host

## ✅ Outcome

- **Confirmed** command injection through HTTP POST  
- Detected active **system-level reconnaissance commands** executed remotely  
- Attack originated from a foreign IP, **not part of any planned activity**  
- Host was **isolated promptly**, and all forensic evidence was preserved  
- **False positive ruled out** — this was a real exploitation attempt

## 📊 Summary

| Field              | Detail                              |
|-------------------|--------------------------------------|
| **Attack Vector** | HTTP POST (Command Injection)        |
| **Commands Used** | whoami, uname, ls, cat /etc/passwd   |
| **Initial Access**| IP `61.177.172.87`                   |
| **Victim Host**   | `172.16.17.16` (WebServer1004)       |
| **HTTP Response** | 200 OK, 912 bytes                    |
| **Technique**     | Remote Shell Execution via Web Input |
| **Status**        | Host contained, threat neutralized   |

## 🧩 Key Takeaways

- Even simple commands like `whoami` can be the sign of deeper exploitation  
- Legacy or spoofed user-agents can indicate evasion attempts  
- HTTP request analysis is critical in web-based attacks  
- Forensics logs like terminal history help **confirm exploitation**  
- Isolation through EDR stops threats before they spread further
