# SOC Investigation — Brute Force Attack Detected

**Case ID:** SOC-BF-001  
**Event ID:** N/A  
**Severity:** High  
**Category:** Authentication Attack  
**Event Time:** March 2026  
**Analyst:** Michael Eziuzor  
**Verdict:** True Positive  

---

## 1. Alert Summary

A SIEM alert was triggered for a high volume of failed authentication 
attempts against a Windows server within a short timeframe. The pattern 
was consistent with an automated brute force attack targeting the 
built-in Administrator account via RDP.

---

## 2. Initial Triage

- **Target Host:** WINSRV-01
- **Targeted Account:** Administrator
- **Attack Type:** Brute Force via RDP
- **Source IP:** 185.234.219.40
- **Traffic Direction:** External to Internal
- **Logon Type:** 3 (Network Logon)
- **Planned/Authorized Test:** No

---

## 3. Threat Intelligence

**VirusTotal — 185.234.219.40**
- Flagged as malicious by 22 vendors
- Associated with brute force and scanning activity

**AbuseIPDB — 185.234.219.40**
- 38,291 abuse reports
- Confidence of Abuse: 98%
- ISP: Frantech Solutions
- Usage Type: Data Center / Bulletproof Hosting
- Country: Luxembourg

---

## 4. Log Analysis

Security event log analysis on WINSRV-01 revealed the following:

- 87 instances of Event ID 4625 (Failed Logon) from 185.234.219.40 
  within a 6-minute window
- All attempts targeted the built-in Administrator account
- Logon Type 3 confirmed network-based authentication attempts
- Failure reason: Unknown username or bad password across all attempts
- Event ID 4624 (Successful Logon) recorded 8 minutes after the 
  failed attempts began from the same source IP
- Successful logon used Logon Type 3 confirming remote access achieved
- Event ID 4672 (Special Privileges Assigned) immediately followed 
  the successful logon confirming elevated access obtained

**Attack Timeline:**

| Time | Event ID | Description |
|------|----------|-------------|
| 02:11 AM | 4625 | First failed logon attempt |
| 02:17 AM | 4625 | 87th failed logon attempt |
| 02:19 AM | 4624 | Successful logon — Administrator |
| 02:19 AM | 4672 | Special privileges assigned |

---

## 5. Attack Classification

- **Type:** Brute Force
- **Protocol:** RDP (Remote Desktop Protocol)
- **Target:** Built-in Administrator account
- **Execution:** Successful — Administrator access achieved
- **Impact:** Critical — Full administrative access to WINSRV-01 obtained

---

## 6. Conclusion

This is a confirmed True Positive. An external threat actor originating 
from 185.234.219.40, hosted on bulletproof infrastructure in Luxembourg, 
conducted an automated brute force attack against WINSRV-01 via RDP. 
After 87 failed attempts over 6 minutes the attacker successfully 
authenticated as the built-in Administrator account and was assigned 
special privileges. Full administrative access to the server was achieved. 
Immediate isolation and Tier 2 escalation are required.

---

## 7. Recommendations

- Isolate WINSRV-01 from the network immediately
- Escalate to Tier 2 Incident Response team
- Reset Administrator account credentials immediately
- Disable the built-in Administrator account and use named admin accounts
- Block 185.234.219.40 at perimeter firewall
- Restrict RDP access — allow only from trusted IP ranges via VPN
- Implement account lockout policy after 5 failed attempts
- Enable MFA for all remote access
- Audit all activity performed during the compromised session
- Review other servers for similar brute force activity from same IP

---

## 8. SIEM Detection Rule

index=windows EventCode=4625
| stats count by src_ip, user, host
| where count > 10
| sort -count

---

**Status:** Closed — True Positive | Successful Compromise  
**Escalation Required:** Yes — Immediate Tier 2 IR

