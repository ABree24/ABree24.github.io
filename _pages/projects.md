---
permalink: /projects/
layout: single
author_profile: true
---
# 💻 **Projects**

---

# 🛡️ Case Study: Brute-Force Attack Detection Using Microsoft Sentinel

## 📘 1. Executive Summary
This case study details the engineering and deployment of a custom Microsoft Sentinel analytic rule to detect successful logins following multiple failed attempts—a pattern indicative of brute-force or credential-stuffing attacks.

The objective was to simulate brute-force activity on a Windows VM, ingest Security Event Logs into Sentinel, build a resilient Kusto Query Language (KQL) rule, and automatically generate incidents. The project successfully demonstrated a full detect, analyze, and respond lifecycle aligned with real-world SOC operations.

---

## 🎯 2. Project Goal and Scope

**Goal:**  
Detect and alert on accounts that experience multiple failed login attempts (Event ID 4625) followed by a successful login (Event ID 4624) within a short timeframe.

**Scope:**  
- Monitored a Windows 10 VM connected to Microsoft Sentinel via Log Analytics.  
- Collected Windows Security Event Logs to track authentication activity.  
- Focused detection logic on identifying brute-force patterns and credential compromise.

**Skills Demonstrated:**  
`Security Event Analysis` · `KQL Query Design` · `SIEM Configuration` · `Incident Response` · `PowerShell Automation` · `SOC Workflow Design`

---

## ⚙️ 3. Tools and Environment Used

| Category | Tool / Service | Purpose |
|-----------|----------------|----------|
| **SIEM** | Microsoft Sentinel | Log ingestion, detection, incident creation |
| **Query Language** | Kusto Query Language (KQL) | Analytics rule development |
| **Host Environment** | Windows 10 VM (Azure) | Event source for simulation |
| **Data Connector** | Azure Monitor Agent (AMA) | Forwarded Security Event logs |
| **Automation** | PowerShell | Simulated failed/successful logons |
| **Investigation Tool** | Sentinel Investigation Graph | Visualized attack chain |

---

## 🔧 4. Process and Technical Implementation

### **Phase 1 — Data Collection and Setup**
1. Deployed a Windows 10 VM in Azure.  
2. Connected it to a **Log Analytics Workspace**.  
3. Enabled **Windows Security Events via AMA** connector in Sentinel.  
4. Verified connectivity using:
   ```kql
   Heartbeat
   | summarize count() by Computer


---

### **Phase 2 - Attack Simulation**

1. Failed Logins: Generated multiple failed logins using a PowerShell loop to simulate a brute-force attacker testing passwords:
    ```
for ($i=0; $i -lt 8; $i++) {
  net use \\127.0.0.1\C$ /user:nonexistent wrongpass 2>null
  Start-Sleep -Seconds 1
}


3. Successful Login: Followed up the failed attempts with a successful login to trigger the detection pattern:

locked the account and signed back-in.


3. Confirmation: Confirmed the presence of Event IDs 4625 (failed) and 4624 (successful) in the SecurityEvent table.

    ![Attack Simulation](assets/images/screenshot 7.png "Attack Simulation")
---

### **Phase 3 Detection Logic (KQL)**

Developed a robust KQL query to find a successful login within 15 minutes after three or more failed attempts from the same account:

let lookback = 6h;
let threshold = 3;
let window = 15m;
// 1. Find all failed login attempts
let Failed =
SecurityEvent
| where TimeGenerated >= ago(lookback)
| where EventID == 4625
| extend User = coalesce(tostring(Account), tostring(TargetUserName))
| summarize FailedCount = count(), LastFailed = max(TimeGenerated) by User;
// 2. Find all successful login attempts
let Success =
SecurityEvent
| where TimeGenerated >= ago(lookback)
| where EventID == 4624
| extend User = coalesce(tostring(Account), tostring(TargetUserName)), SuccessTime = TimeGenerated;
// 3. Join on User and apply timing logic
Failed
| join kind=inner (Success) on User
| where SuccessTime between (LastFailed .. LastFailed + window)
| where FailedCount >= threshold
| project User, FailedCount, LastFailed, SuccessTime

---

### **Phase 4 Validation & Automation**

1. Ran query → 20 detections confirmed.

2. Created Scheduled Analytics Rule: runs every 5 min, 30-min lookup.

3. Enabled auto-incident creation in Sentinel.

4. Linked a Logic App Playbook to email alerts to administrators.

---

## 🧩 5. Defensive Recommendations

1. Account Lockout Policy: Lock after 5 failed attempts.

2. MFA Enforcement: Mitigates credential compromise.

3. Geo-Correlation: Flag impossible travel or abnormal IPs.

4. Rule Tuning: Adjust thresholds to minimize false positives.

---

## 🧠 6. Key Learnings

This project reinforced the critical need for robust log analysis in threat detection. Specifically, I:

1. Mastered KQL's join and time-window functions to correlate events effectively across a dataset.

2. Built end-to-end SIEM detection pipeline in Sentinel.

3. Learned to operationalize a Microsoft Sentinel detection from raw logs to an automated incident response alert.

4. Demonstrated the full Detect → Analyze → Respond cycle of SOC operations.

---

## 🏁 7. Outcome

✅ Successfully detected simulated brute-force activity.
✅ Automatically generated Sentinel incidents with context.
✅ Showcased full SOC detection lifecycle: Detect → Analyze → Respond.

---

🧰 Tools & Technologies

Microsoft Sentinel · Azure Log Analytics · KQL · PowerShell · Windows Event Viewer · Logic Apps · Azure Monitor Agent

---

🚀 Future Improvements

Add detection for Event 4672 (privilege escalation).

Create Sentinel Workbook for visual logon trends.

Integrate Sysmon data for deeper endpoint telemetry.

Automate response (disable account / isolate host).

---

