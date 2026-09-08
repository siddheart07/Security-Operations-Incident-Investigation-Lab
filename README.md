# 🔐 Security Operations & Incident Investigation Lab

> A hands-on cybersecurity assessment and incident investigation project using Kali Linux and Windows in an isolated VMware lab environment.

---

## 📌 Project Overview

This project simulates a small Security Operations and Incident Investigation workflow between a Kali Linux security-testing machine and a Windows endpoint.

The objective was not simply to run security tools, but to follow a complete security workflow:

**Reconnaissance → Enumeration → Investigation → Risk Assessment → Remediation → Validation**

The assessment focused on network exposure, SMB security, Windows security configuration, authentication events, and remediation validation.

---

## 🎯 Objectives

- Perform network reconnaissance against a Windows endpoint
- Identify exposed network services
- Analyze SMB protocols and security configuration
- Enumerate SMB shares
- Investigate unauthenticated SMB access
- Assess Windows Defender Firewall configuration
- Identify and analyze listening services and processes
- Investigate Windows Security Event ID 4625
- Correlate security observations and assess risk
- Apply security remediation
- Perform post-remediation validation
- Document findings and evidence

---

## 🧪 Lab Environment

| Component | Details |
|---|---|
| Security Testing OS | Kali Linux |
| Target OS | Windows |
| Virtualization | VMware |
| Network | Isolated lab network |
| Assessment Type | Controlled security assessment |
| Primary Tools | Nmap, smbclient, PowerShell, Event Viewer |

> ⚠️ All testing was performed in an isolated, controlled lab environment for educational purposes.

---

## 🔍 Methodology

### 1. Network Reconnaissance

Nmap was used to identify accessible services on the Windows endpoint.

The assessment identified:

- TCP 135 — MSRPC
- TCP 139 — NetBIOS Session Service
- TCP 445 — SMB

These services were investigated further to understand the endpoint's network attack surface.

---

### 2. SMB Protocol Analysis

SMB protocol support was analyzed using Nmap NSE scripts.

The endpoint supported SMB 2.x and SMB 3.x protocols.

SMB signing was also checked.

### Positive Security Control

**SMB signing was enabled and required.**

This was documented as a positive security control rather than treating every observation as a vulnerability.

---

### 3. SMB Share Enumeration

SMB shares were enumerated using `smbclient`.

The following shares were observed:

- ADMIN$
- C$
- D$
- IPC$
- Users

Further testing was performed against the `Users` share.

---

### 4. Unauthenticated SMB Access

An unauthenticated SMB session to the `Users` share was successfully established in the initial lab configuration.

Directory listing was also possible without authentication.

This was documented as a security finding.

> The testing demonstrated unauthenticated directory listing, but did not establish unrestricted file read/write access.

---

### 5. Windows Firewall Assessment

Windows Defender Firewall profiles were inspected using PowerShell.

Initially, the following profiles were disabled:

- Domain
- Private
- Public

This increased the network exposure of the Windows endpoint within the lab environment.

---

### 6. Listening Service & Process Investigation

PowerShell was used to identify listening TCP ports and map them to their owning processes.

This provided additional context about which Windows processes were responsible for exposed services.

---

### 7. Authentication Event Investigation

Windows Security logs were investigated using Event Viewer and PowerShell.

**Event ID 4625** was analyzed because it represents a failed account logon.

The investigation examined:

- Target username
- Logon type
- Failure status
- Sub-status
- Source address
- Authentication details

Multiple failed authentication events were observed, including several events occurring within a short period.

The activity was documented as a **repeated/suspicious authentication pattern** rather than definitively labeling it as brute force based solely on the available evidence.

The investigated event showed:

- Target account: `Admin`
- Logon Type: `2`
- Source address: `127.0.0.1`
- Sub-status: `0xc000006a`

The source address indicated that the investigated failed login event was local to the Windows endpoint rather than originating from the Kali machine.

---

## ⚠️ Security Findings

| ID | Finding | Risk |
|---|---|---|
| F-01 | Exposed Windows network services | High |
| F-02 | Windows Firewall profiles initially disabled | High |
| F-03 | Unauthenticated SMB access | High |
| F-04 | SMB share enumeration | High |
| F-05 | Unauthenticated directory listing | High |
| F-06 | Repeated authentication failures | Medium |
| F-07 | SMB signing enabled and required | Informational / Positive Control |

---

## 🛠️ Remediation

The Windows Defender Firewall was enabled across all profiles.

The configuration was then verified using PowerShell.

The same network and SMB tests were repeated after remediation.

---

## ✅ Post-Remediation Validation

The remediation was validated rather than simply assuming the issue was resolved.

### Before Remediation

- Windows Firewall: Disabled
- TCP 135: Accessible
- TCP 139: Accessible
- TCP 445: Accessible
- Unauthenticated SMB access: Successful

### After Remediation

- Windows Firewall: Enabled
- TCP 135: Filtered
- TCP 139: Filtered
- TCP 445: Filtered
- Previous unauthenticated SMB connection: Timed out

This demonstrated that the applied firewall remediation successfully reduced the network exposure observed during the initial assessment.

---

## 🧠 Key Learning

The main learning from this project was that cybersecurity is not limited to identifying vulnerabilities.

The project provided practical experience with the complete workflow of:

**Identify → Investigate → Assess → Remediate → Validate**

I learned how to:

- Perform network reconnaissance
- Analyze exposed services
- Investigate SMB security
- Work with Windows Security logs
- Interpret authentication events
- Map network ports to processes
- Assess security risks
- Apply Windows security controls
- Validate remediation
- Collect and document technical evidence

---

## 🧰 Technologies & Tools

- Kali Linux
- Windows
- VMware
- Nmap
- smbclient
- PowerShell
- Windows Event Viewer
- Windows Defender Firewall
- SMB
- TCP/IP

---

## 📂 Project Structure

```text
Security-Operations-Incident-Investigation-Lab/
│
├── Evidence/
│   ├── 01_Nmap_Service_Scan.png
│   ├── 02_SMB_Protocol_Analysis.png
│   ├── 03_SMB_Security_Mode.png
│   ├── ...
│   └── 18_Remediation_Validation_Summary.png
│
├── Report/
│   └── Security_Assessment_Report.pdf
│
└── README.md
