# 🌐 DHCP Log Analysis Using Splunk SIEM

This project demonstrates how a Security Operations Center (SOC) analyst can analyze DHCP logs using Splunk SIEM to monitor IP address assignments, detect anomalous network behavior, and identify unauthorized or suspicious clients.

The project follows a structured SOC workflow:

Log Ingestion → Field Extraction → Traffic Analysis → Anomaly Detection → IP Monitoring → Alerting

---

## 🎯 Project Objectives

- Monitor DHCP IP address assignments in real-time  
- Identify unauthorized or rogue devices requesting IP addresses  
- Detect anomalies in lease durations, renewals, or repeated requests  
- Build SOC-ready detections for network security monitoring  

---

## 🧪 Log Ingestion & Verification

To confirm DHCP logs were successfully ingested into Splunk:
index=main sourcetype=dhcp


Verify:
- Correct timestamps  
- Client IP addresses  
- MAC addresses  
- Lease durations  
- DHCP server IP  

---

## 🔎 Field Extraction

Key DHCP fields analyzed:

- `client_identifier` (MAC address)  
- `leased_ip` (Assigned IP address)  
- `lease_duration`  
- `lease_renewal`  
- `_time` (Timestamp)  

Proper field extraction ensures accurate detection and traffic analysis.

---

## 📊 DHCP Traffic Analysis

### 🔝 Top Leased IPs

index=main sourcetype=dhcp
| top limit=10 leased_ip


### 📈 IP Distribution Count

index=main sourcetype=dhcp
| stats count by leased_ip



These searches help identify:
- Frequently assigned IP addresses  
- High-usage network segments  
- IP allocation trends  

---

## 🚨 Anomaly Detection

### ⏳ DHCP Requests Over Time

index=main sourcetype=dhcp
| timechart span=1h count


Used to detect:
- Traffic spikes  
- Sudden increase in IP requests  
- Potential scanning or rogue activity  

---

### 🔐 Unauthorized Client Requests

index=main sourcetype=dhcp
| search NOT client_identifier="authorized_identifier"


Detects:
- Unknown or unauthorized MAC addresses  
- Potential rogue devices  

---

### 🔄 Multiple Lease Renewals

index=main sourcetype=dhcp
| stats count by leased_ip, lease_renewal
| where count > 1 AND lease_renewal="true"


Helps identify:
- Repeated renewal behavior  
- Misconfigured clients  
- Suspicious persistence patterns  

---

## 📡 Long-Term IP Monitoring

To analyze DHCP behavior over extended periods:

index=main sourcetype=dhcp
| timechart span=1d count by leased_ip


This helps detect:
- Rogue devices  
- Network misconfigurations  
- Abnormal client behavior  
- Sudden changes in IP allocation patterns  

---

## 🚨 Alerting Strategy

Alerts can be triggered when:

- Unauthorized MAC addresses request IP assignments  
- IP addresses are leased more frequently than baseline  
- Lease durations deviate from normal policy  
- Sudden spikes in DHCP traffic occur  

Severity levels can be adjusted based on organizational policy and risk assessment.

---

## 📌 Conclusion

Analyzing DHCP logs using Splunk SIEM provides strong visibility into IP address management and network behavior. Through structured log ingestion, traffic analysis, anomaly detection, and alerting, SOC analysts can detect rogue devices, identify abnormal activity, and improve overall network security posture.

---

## ⚠ Disclaimer

This project was conducted in a controlled lab environment for educational and defensive cybersecurity purposes only.
