# SOC342 - CVE-2025-53770 SharePoint ToolShell Auth Bypass and RCE

**Case ID:** SOC342  
**Event ID:** 320  
**Severity:** Critical  
**Category:** Web Attack  
**Event Time:** July 22, 2025, 01:07 PM  
**Analyst:** Michael Eziuzor  
**Verdict:** True Positive  

---

## 1. Alert Summary

A critical severity alert was triggered for a zero-day vulnerability 
designated CVE-2025-53770, known as ToolShell, affecting on-premises 
SharePoint Server deployments. The alert indicated authentication bypass 
and remote code execution activity on SharePoint01.

---

## 2. Initial Triage

- **Source IP:** 107.191.58.76
- **Target:** SharePoint01
- **Traffic Direction:** External to Internal
- **Attack Type:** Authentication Bypass and Remote Code Execution (RCE)
- **Planned/Authorized Test:** No

---

## 3. Threat Intelligence

**VirusTotal — 107.191.58.76**
- 11/94 vendor detections
- Hosted on Vultr VPS infrastructure
- Associated with known threat actor activity

---

## 4. Log Analysis

Investigation of endpoint and process logs on SharePoint01 revealed the 
following attack chain:

- w3wp.exe (SharePoint worker process) spawned an unexpected PowerShell 
  child process, indicating server-side code execution
- A web shell was planted at spinstall0.aspx, providing persistent 
  backdoor access to the server
- The attacker leveraged GetApplicationConfig() to extract the SharePoint 
  Machine Key
- A reverse shell callback was established to the attacker IP 107.191.58.76 
  at 13:08:04, confirming full remote access
- Full server compromise was confirmed

---

## 5. Attack Classification

- **Type:** Zero-Day RCE via Authentication Bypass (ToolShell)
- **CVE:** CVE-2025-53770
- **Method:** Auth bypass leading to web shell deployment and reverse shell
- **Execution:** Successful — full server compromise achieved
- **Impact:** Critical — Machine Key stolen, persistent web shell installed, 
  reverse shell established

---

## 6. Conclusion

This is a confirmed True Positive representing a critical security incident. 
The attacker exploited CVE-2025-53770 to bypass SharePoint authentication, 
execute remote code via w3wp.exe spawning PowerShell, plant a persistent 
web shell at spinstall0.aspx, and establish a reverse shell connection back 
to their infrastructure at 107.191.58.76. The SharePoint Machine Key was 
extracted via GetApplicationConfig(), potentially enabling further 
credential attacks. Full server compromise was achieved.

Immediate isolation and Tier 2 IR escalation are required.

---

## 7. Recommendations

- Immediately isolate SharePoint01 from the network
- Escalate to Tier 2 / Incident Response team
- Remove web shell at spinstall0.aspx
- Rotate SharePoint Machine Key immediately
- Block 107.191.58.76 at firewall and WAF level
- Apply Microsoft patch for CVE-2025-53770 immediately
- Conduct forensic analysis of SharePoint01 for additional persistence 
  mechanisms
- Review all SharePoint access logs for lateral movement activity

---

**Status:** Closed — True Positive | Full Compromise Confirmed  
**Escalation Required:** Yes — Tier 2 IR
