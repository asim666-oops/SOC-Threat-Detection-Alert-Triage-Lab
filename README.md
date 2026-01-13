# SOC-Splunk-Threat-Detection-Alert-Triage-Lab
# SOC Threat Detection & Alert Triage Lab

## 📌 Overview
This project demonstrates a real-world Security Operations Center (SOC) home lab
focused on threat detection, alert triage, investigation, and incident escalation
using Splunk SIEM.

The lab simulates real attacker behavior and SOC workflows using:
- Kali Linux (Attacker)
- Windows 10 (Victim Endpoint)
- Ubuntu Linux (Splunk SIEM)
- Windows Event Logs (No Sysmon)

---

## 🧪 Lab Architecture

- Attacker Machine: Kali Linux
- Victim Machine: Windows 10
- SIEM: Splunk Enterprise on Ubuntu
- Log Source: Windows Event Logs (Security, System, Application)
- Forwarding: Splunk Universal Forwarder

---

## 🎯 Objectives

- Simulate real cyber attacks from Kali Linux
- Detect threats using native Windows logs
- Create Splunk SPL detections and alerts
- Perform alert triage and investigation
- Tune alerts to reduce false positives
- Map detections to MITRE ATT&CK
- Document SOC escalation workflows
- Analyze external logs (DNS, DHCP, HTTP, SMTP)

---

## 🔴 Attack Simulations

### Reconnaissance
- Nmap scanning
- Port enumeration

### Credential Access
- Brute force attacks
- Manual

---

## 🔍 Detection & Analysis

- Windows Event IDs:
  - 4624 / 4625 – Login Success/Failure
  - 4688 – Process Creation
  - 4776 – Credential Validation
  - 5156 – Firewall Events

- SPL-based detections
- Dashboard-based monitoring
- Alert tuning and validation

---

## 🚨 SOC Workflow

1. Alert Trigger
2. Alert Triage (L1)
3. Log Correlation
4. Threat Validation
5. MITRE Mapping
6. Incident Escalation (L2)
7. Incident Closure

---

## 🧠 MITRE ATT&CK Mapping

| Tactic | Technique |
|------|----------|
| Reconnaissance | T1046 |
| Credential Access | T1110 |
| Execution | T1059 |
| Lateral Movement | T1021 |

---

## 📊 Dashboards

- SOC Overview Dashboard
- Authentication Monitoring
- Reconnaissance Detection
- Process Execution Analysis

---

## 📂 External Log Analysis

- DNS log investigation
- DHCP anomaly detection
- HTTP traffic analysis
- SMTP email abuse detection

---

## 🧾 Incident Response

- Alert triage playbooks
- Incident escalation reports
- Root cause analysis

---

## 🧠 Skills Demonstrated

- SIEM (Splunk)
- SOC L1/L2 Operations
- Windows Event Log Analysis
- Threat Detection Engineering
- MITRE ATT&CK
- Incident Response
- Alert Tuning
- Log Analysis (DNS, DHCP, HTTP, SMTP)

---

## ⚠️ Disclaimer
This lab is for educational and defensive security purposes only.
