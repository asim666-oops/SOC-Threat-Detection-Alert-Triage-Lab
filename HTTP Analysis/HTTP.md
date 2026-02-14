# HTTP Log Analysis & Web Traffic Monitoring Using Splunk SIEM

This project demonstrates how a Security Operations Center (SOC) analyzes HTTP web server logs using Splunk SIEM to monitor web traffic, detect anomalies, and identify potential security threats such as brute-force attempts, suspicious file access, abnormal traffic spikes, and malicious user behavior.

The project follows a structured SOC workflow:

Log Ingestion → Field Extraction → Traffic Analysis → Anomaly Detection → User Behavior Monitoring

This lab simulates real-world SOC alert investigation and threat detection techniques.

---

##  Project Objectives

- Search and analyze HTTP events in Splunk  
- Extract meaningful fields from raw HTTP logs  
- Establish baseline web traffic behavior  
- Detect suspicious or anomalous activity  
- Monitor user behavior for potential attacks  
- Simulate real-world SOC alert investigation  

---

##  Tools & Technologies Used

- Splunk Enterprise / Splunk Free  
- HTTP / Web Server Access Logs  
- Search Processing Language (SPL)  
- SOC Analysis Methodology  

---

##  Fields Analyzed

- `timestamp`
- `method` (GET, POST, PUT, DELETE)
- `uri` (Requested endpoint)
- `status` (HTTP response code)
- `src_ip` (Client IP address)
- `user_agent`
- `user`
- `session_id`

---

##  Step 1: HTTP Log Ingestion & Verification

To confirm HTTP logs were successfully ingested:

index=main sourcetype=http

### Outcome:
- Verified successful ingestion of HTTP events  
- Raw HTTP logs visible in Splunk  
- Baseline visibility established  

---

##  Step 2: Field Extraction

HTTP logs are often unstructured. Field extraction enables meaningful statistical analysis.



### Extracted Fields:
- method  
- uri  
- status  
- src_ip  
- user_agent  
- user  
- session_id  

### Outcome:
- Improved log readability  
- Enabled statistical analysis  
- Prepared data for detection use cases  

---

##  Step 3: Web Traffic Analysis

### 3.1 HTTP Request Method Distribution

index=main sourcetype=http
| stats count by method

**SOC Insight:**
- Excessive POST requests may indicate brute-force attempts or data exfiltration  

---

### 3.2 Top Accessed URLs

index=main sourcetype=http
| top limit=10 uri

**SOC Insight:**
- Repeated access to admin or hidden endpoints may indicate scanning or exploitation  

---

### 3.3 HTTP Response Code Analysis

index=main sourcetype=http
| stats count by status

**SOC Insight:**
- High 404 / 403 responses → Directory brute-forcing  
- Repeated 500 errors → Possible exploitation attempts  

---

##  Step 4: Anomaly Detection

### 4.1 Traffic Volume Over Time

index=main sourcetype=http
| timechart span=1h count

**SOC Insight:**
- Sudden traffic spikes may indicate DDoS activity or automated attacks  

---

### 4.2 High Error Response Detection

index=main sourcetype=http
| stats count by status
| where status >= 400

**SOC Insight:**
- Indicates scanning, brute-force attempts, or authentication abuse  

---

### 4.3 Suspicious IP Investigation

index=main sourcetype=http
| search src_ip="suspicious_ip"

**SOC Insight:**
- Useful for threat intelligence correlation  
- Supports IP blocking and containment actions  

---

##  Step 5: User Behavior Monitoring

### 5.1 Failed Login Attempts

index=main sourcetype=http
| search action="login" status="failed"
| stats count by user

**SOC Insight:**
- Multiple login failures suggest brute-force or credential stuffing attacks  

---

### 5.2 Session Duration Analysis
index=main sourcetype=http
| stats range(_time) as session_duration by session_id
| stats avg(session_duration) as avg_session_duration by user

**SOC Insight:**
- Extremely long or very short sessions may indicate bots or session hijacking  

---

##  Security Findings

- Identified abnormal HTTP error trends  
- Detected suspicious access to sensitive endpoints  
- Observed potential brute-force login behavior  
- Established baseline web traffic patterns  
- Improved visibility into user activity and sessions  

---

##  MITRE ATT&CK Mapping

| Technique ID | Description |
|-------------|------------|
| T1071.001 | Web-Based Command and Control |
| T1110 | Brute Force |
| T1046 | Network Service Discovery |
| T1190 | Exploit Public-Facing Application |

---

##  Conclusion

This project demonstrates structured SOC-level HTTP traffic monitoring using Splunk SIEM. By performing field extraction, traffic analysis, anomaly detection, and user behavior monitoring, the lab replicates real-world SOC alert investigation processes. The detections align with MITRE ATT&CK techniques and reflect practical web threat monitoring methodologies.

---

##  Disclaimer

This project was conducted in a controlled lab environment for educational and defensive cybersecurity purposes only.


