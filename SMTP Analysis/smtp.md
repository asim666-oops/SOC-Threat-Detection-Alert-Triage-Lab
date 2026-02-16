project:
  title: "SMTP Log Analysis Using Splunk SIEM"

  overview: >
    This project demonstrates how a Security Operations Center (SOC) analyst analyzes
    SMTP (Simple Mail Transfer Protocol) log files using Splunk SIEM to monitor email
    activity, detect suspicious behavior, and identify potential security threats such
    as spam, phishing, brute-force login attempts, and data exfiltration via email.
    
    The project follows a basic SOC workflow:
    Log Ingestion → Analysis → Detection → Alerting

  objectives:
    - Analyze SMTP email traffic using Splunk SIEM
    - Identify normal and abnormal email behavior
    - Detect suspicious email activity and login attempts
    - Create basic detections suitable for a SOC environment

  tools_environment:
    siem_tool: "Splunk"
    index: "main"
    sourcetype: "smtp"
    log_type: "SMTP email server logs"

  workflow:

    step_1:
      title: "Search for SMTP Events"
      description: >
        The first step is to confirm that SMTP logs are successfully ingested into Splunk.
      query: |
        index=main sourcetype=smtp
      verification:
        - Email activity is being logged
        - Timestamps and SMTP events are visible
        - Required fields are available for analysis

    step_2:
      title: "Field Identification & Extraction"
      key_fields:
        - sender_ip
        - receiver_ip
        - user
        - action
        - status
        - attachment_type
        - attachment_size
        - src_ip
      extraction_method: >
        Field extraction can be done using Splunk’s Field Extractor
        or rex commands when required.

    step_3:
      title: "Analyze Email Traffic Patterns"

      top_email_senders:
        query: |
          index=main sourcetype=smtp
          | top limit=10 sender_ip
        purpose: "Establish baseline of normal email communication."

      top_email_recipients:
        query: |
          index=main sourcetype=smtp
          | top limit=10 receiver_ip
        purpose: "Establish baseline of normal email communication."

    step_4:
      title: "Detect Anomalies in Email Traffic"

      email_volume_over_time:
        query: |
          index=main sourcetype=smtp
          | timechart span=1h count
        indicators:
          - Spam campaigns
          - Compromised email accounts

      suspicious_attachments:
        query: |
          index=main sourcetype=smtp
          | search attachment_type IN ("exe","js","vbs","iso","zip")
        detects:
          - Malware delivery
          - Phishing attempts

    step_5:
      title: "Monitor User Behavior"

      email_activity_by_user:
        query: |
          index=main sourcetype=smtp
          | stats count by user

      failed_email_login_attempts:
        query: |
          index=main sourcetype=smtp
          | search action="login" status="failed"
          | stats count by user
        indicators:
          - Brute-force attacks
          - Account compromise attempts

    step_6:
      title: "Detection & Alert Use Cases"
      use_cases:
        - name: "Spam Detection"
          description: "High number of emails from one sender"
        - name: "Phishing Detection"
          description: "Suspicious attachment types"
        - name: "Brute Force Detection"
          description: "Multiple failed login attempts"
        - name: "Data Exfiltration"
          description: "Large email attachments"

  mitre_attack_mapping:
    - technique_id: "T1071.003"
      description: "Email Protocol"
    - technique_id: "T1566.001"
      description: "Phishing Attachment"
    - technique_id: "T1110"
      description: "Brute Force"
    - technique_id: "T1048"
      description: "Exfiltration Over Email"

  conclusion: >
    This project demonstrates a basic but effective SMTP log analysis
    using Splunk SIEM. It reflects real-world SOC analyst activities such as
    monitoring email traffic, identifying anomalies, and detecting suspicious behavior.
