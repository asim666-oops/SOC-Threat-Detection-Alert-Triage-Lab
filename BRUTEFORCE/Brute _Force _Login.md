
 
## Detection of Brute Force Login Attack Using Windows Security Logs & Splunk SIEM

This project demonstrates how a Security Operations Center (SOC) detects brute-force login attacks using Windows Security Event Logs ingested into Splunk SIEM. The lab simulates repeated failed authentication attempts followed by a successful login, representing a common brute-force attack pattern. The project includes detection logic, alert criteria, MITRE ATT&CK mapping, and SOC investigation workflow to reflect real-world SOC operations.

---

## 🎯 Objectives

- Simulate a brute-force login attack on a Windows system  
- Generate real Windows Security Event Logs  
- Detect repeated failed login attempts  
- Identify successful authentication after failures  
- Apply SOC L1 investigation and escalation logic  
- Map activity to MITRE ATT&CK framework  

---

## 🧪 Lab Environment

This lab was built using a virtualized environment:

- **Virtualization Platform:** VirtualBox  
- **Victim Machine:** Windows 10  
- **Attacker System:** Kali Linux (not used due to protocol restrictions)  
- **SIEM Platform:** Splunk Enterprise  
- **Log Source:** Windows Security Event Logs  

---

## 🔎 Why Manual Brute Force Was Used

Modern Windows 10 systems restrict remote brute-force attempts via SMB or RDP due to security controls such as Network Level Authentication (NLA), firewall rules, and account protection mechanisms. Because of these restrictions, manual interactive login attempts were used to simulate a brute-force attack.

This approach is realistic because SOC teams analyze authentication behavior patterns rather than attack tools. Manual login failures still generate legitimate Windows Security events and are commonly observed in insider threat scenarios, unauthorized physical access attempts, and administrator account misuse cases.

---

## 🔧 Windows Logging Configuration

To ensure proper logging, advanced audit policies were enabled on the Windows victim machine:

**Path:**  
Local Security Policy → Advanced Audit Policy Configuration → Audit Policies

The following audit policies were enabled for both Success and Failure:

- Audit Logon  
- Audit Special Logon  
- Audit Credential Validation  

These configurations ensured all failed and successful login attempts were recorded in the Windows Security log.

---

## 🔴 Attack Simulation

A manual brute-force login attempt was simulated by entering incorrect passwords multiple times for a privileged account.

### Steps Performed

1. System booted to Windows login screen  
2. Target account selected: **Administrator**  
3. 9 incorrect passwords entered within 2–3 minutes  
4. On the 10th attempt, the correct password was entered  
5. System successfully logged in  

This sequence closely mimics a real brute-force attack where an attacker eventually guesses the correct password.

---

## 📑 Windows Security Events Generated

The following event IDs were generated during the simulation:

- **4625** – Failed logon attempt  
- **4624** – Successful logon  
- **4672** – Special privileges assigned (Administrator login)  

**Logon Type Observed:**  
- Logon Type 2 – Interactive logon (keyboard-based login)

This logon type is valid and expected for manual login attempts.

---

## 📥 Splunk Log Ingestion & Verification

Windows Security logs were forwarded to Splunk and verified using the following search query:
index=main sourcetype=WinEventLog:Security EventCode=4625


Logs confirmed:
- Account name  
- Timestamp  
- Failure reason  
- Logon type  

---

## 🛡 Detection Logic (SOC L1)

The following SPL query was created to detect brute-force behavior:
index=main sourcetype=WinEventLog:Security EventCode=4625
| bin _time span=5m
| stats count by _time, Account_Name, Logon_Type
| where count >= 5


### Detection Criteria

- 5 or more failed login attempts  
- Same account targeted  
- Within a 5-minute window  

---

## 🚨 Alert Criteria

An alert is triggered when:

- Multiple failed logons are detected in a short time period  
- A privileged account (Administrator) is targeted  
- A successful login (Event ID 4624) follows repeated failures  
- Special privileges are assigned (Event ID 4672)  

**Severity Level:** High  

---

## 🔎 SOC L1 Investigation Workflow

When the alert is triggered, the SOC L1 analyst performs:

1. Identify the affected account  
2. Verify number of failed attempts  
3. Confirm logon type  
4. Check for successful login (Event ID 4624)  
5. Confirm administrator privileges (Event ID 4672)  
6. Determine whether activity is expected or suspicious  

---

## ⬆ SOC Escalation (L1 → L2)

The incident is escalated to SOC L2 when:

- A privileged account is involved  
- A successful login follows repeated failed attempts  
- High-frequency login failures occur within a short timeframe  

---

## 🧠 MITRE ATT&CK Mapping

- **Brute Force** – T1110  
- **Valid Accounts** – T1078  

---

## 📌 Conclusion

This project successfully demonstrates detection of a brute-force login attack using real Windows Security logs and Splunk SIEM. Although the attack was manually simulated, it produced genuine security events identical to those generated by automated brute-force tools. The detection logic, alert criteria, and SOC escalation workflow accurately reflect real-world SOC operations.

---

## 🧠 Key Learnings

- Brute-force attacks can be detected through authentication log patterns  
- Manual login failures are valid attack simulations  
- SOC analysis focuses on behavioral indicators rather than attack tools  
- Windows Security logs provide critical visibility into authentication activity  

---

## ⚠ Disclaimer

This project was conducted in a controlled lab environment for educational and defensive cybersecurity purposes only.
