# DNS Log Analysis & Threat Hunting Using Splunk SIEM

This project demonstrates SOC-style DNS monitoring and threat hunting using Splunk SIEM and Wireshark. The objective is to analyze DNS log data to identify anomalies, suspicious domains, and potential DNS-based Command-and-Control (C2) or data exfiltration techniques, while mapping detections to the MITRE ATT&CK framework.

The workflow follows a real SOC investigation process:

Log Ingestion → Field Extraction → Statistical Analysis → Anomaly Detection → Packet Validation → MITRE Mapping → SOC Response Planning

---

##  Project Objectives

- Ingest and analyze DNS logs in Splunk  
- Perform statistical and behavioral DNS analysis  
- Detect anomalies related to DNS tunneling and C2  
- Validate findings using Wireshark  
- Map detections to MITRE ATT&CK  
- Document findings in SOC-style reporting format  

---

## Tools & Technologies Used

- Splunk Enterprise (SIEM)
- Wireshark (Packet Analysis)
- Sample DNS Log Dataset
- MITRE ATT&CK Framework

---

## Project Architecture

DNS Log Files  
↓  
Splunk SIEM (Indexing & Analysis)  
↓  
SOC Investigation & MITRE Mapping  
↓  
Wireshark (Packet-Level Validation)

---

## Dataset Information

**Source:** DNS Log Files  

**Extracted Fields:**
- `src_ip` – Source IP Address  
- `dst_ip` – Destination IP Address  
- `domain_name` – Queried Domain  
- `query` – DNS Query Type  
- `response_code` – DNS Response Code  
- `_time` – Timestamp  

---

## Step-by-Step Investigation Process

### 🔹 Step 1: DNS Log Preparation

- Downloaded DNS logs from a public repository  
- Extracted and stored logs locally  
- Verified logs contained valid DNS events  

---

### 🔹 Step 2: Log Ingestion into Splunk

- Navigated to **Settings → Add Data → Upload**
- Selected DNS log file
- Assigned `sourcetype=dns`
- Completed indexing and verified ingestion

**Verification Query:**

index=_* OR index=* sourcetype=dns


---

### 🔹 Step 3: Initial DNS Event Exploration

Retrieved all DNS events to understand log structure:

index=_* OR index=* sourcetype=dns


---

### 🔹 Step 4: Filter DNS-Related Events

index=_* OR index=* sourcetype=dns
| regex _raw="(?i)\b(dns|domain|query|response|port 53)\b"

---

### 🔹 Step 5: DNS Frequency & Statistical Analysis

Identified frequently queried domains:

index=_* OR index=* sourcetype=dns
| stats count by domain_name
| sort -count

---

### 🔹 Step 6: Identify Top DNS Sources

Detected hosts generating the most DNS traffic:

index=_* OR index=* sourcetype=dns
| top domain_name, src_ip

---

### 🔹 Step 7: Detect DNS Tunneling Indicators

Checked for unusually long DNS queries (possible encoded payloads):
index=_* OR index=* sourcetype=dns
| eval domain_len=len(domain_name)
| where domain_len > 50

---

### 🔹 Step 8: Detect High-Volume DNS Activity (Beaconing)

index=_* OR index=* sourcetype=dns
| stats count by src_ip
| where count > 1000

Used to identify:
- Excessive DNS traffic  
- Potential C2 beaconing behavior  

---

### 🔹 Step 9: Suspicious Domain Investigation

Manually verified suspicious domains using threat intelligence sources (e.g., VirusTotal):

index=_* OR index=* sourcetype=dns domain_name="maliciousdomain.com"


---

### 🔹 Step 10: Wireshark Packet-Level Validation

Used Wireshark to validate DNS behavior.

**Filters Used:**

dns
dns && strlen(dns.qry.name) > 50
!mdns


Analyzed:
- DNS query length  
- Subdomain patterns  
- Request frequency  
- Known tunneling tool patterns (dnscat, dns2tcp)  

---

## MITRE ATT&CK Mapping

| Observed Behavior | Tactic | Technique | ID |
|-------------------|--------|----------|----|
| Long DNS Queries | Command & Control | DNS C2 | T1071.004 |
| High DNS Frequency | Command & Control | Beaconing | T1071.004 |
| Encoded Subdomains | Exfiltration | Alternative Protocol | T1048 |
| DNS Tunneling | Exfiltration | Alternative Protocol | T1048 |

---

## SOC Response & Investigation Plan

Even though no malicious activity was detected, documented response steps demonstrate SOC readiness.

---

###  1. Long DNS Queries Detected  
**MITRE:** T1071.004 – DNS-based Command & Control  

SOC Actions:
1. Identify affected `src_ip`  
2. Validate query length in Splunk + Wireshark  
3. Check domain reputation (VirusTotal)  
4. Correlate with endpoint logs (Sysmon / Windows Logs)  
5. Isolate endpoint if suspicious  
6. Block domain at DNS firewall  
7. Escalate to Tier-2 SOC  

---

###  2. High DNS Frequency (Beaconing)  
**MITRE:** T1071.004 – Command & Control  

SOC Actions:
1. Identify high-volume source IP  
2. Analyze periodic patterns  
3. Validate timestamps in Wireshark  
4. Inspect endpoint processes  
5. Apply containment if needed  
6. Block C2 domain/IP  
7. Document incident  

---

###  3. Encoded Subdomains  
**MITRE:** T1048 – Exfiltration Over Alternative Protocol  

SOC Actions:
1. Extract suspicious subdomains  
2. Perform entropy analysis  
3. Inspect DNS packet payloads  
4. Search for exfiltration indicators  
5. Isolate system  
6. Preserve forensic evidence  
7. Notify Incident Response Team  

---

###  4. DNS Tunneling  
**MITRE:** T1048 – Exfiltration Over Alternative Protocol  

SOC Actions:
1. Confirm tunneling behavior (Splunk + Wireshark)  
2. Identify tunneling tool signatures  
3. Block DNS communication immediately  
4. Capture memory/disk artifacts  
5. Reset credentials if required  
6. Perform root cause analysis  
7. Update SOC detection rules  

---

## Findings

- No malicious domains detected  
- DNS query lengths within normal limits  
- No evidence of DNS tunneling or C2 activity  
- DNS traffic behavior observed was normal  

---

##  Conclusion

This project demonstrates SOC-level DNS monitoring and threat hunting using Splunk SIEM and Wireshark. Although no malicious activity was identified, the investigation validated structured threat-hunting methodology and effective anomaly detection aligned with the MITRE ATT&CK framework.

---

##  SOC Incident Summary

- **Incident Status:** False Positive  
- **Risk Level:** Low  
- **Confidence Level:** High  

---

##  Disclaimer

This project was conducted in a controlled lab environment for educational and defensive cybersecurity purposes only.



