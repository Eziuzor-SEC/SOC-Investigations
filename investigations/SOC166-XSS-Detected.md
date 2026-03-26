# SOC166 - Javascript Code Detected in Requested URL

**Case ID:** SOC166  
**Event ID:** 116  
**Severity:** Medium  
**Category:** Web Attack  
**Event Time:** February 26, 2022, 06:56 PM  
**Analyst:** Michael Eziuzor  
**Verdict:** True Positive  

---

## 1. Alert Summary

A SIEM rule triggered on a suspected XSS payload detected within a requested 
URL on WebServer1002. The rule — SOC166 Javascript Code Detected in Requested 
URL — flagged inbound traffic containing javascript code in the search 
parameter of the request.

---

## 2. Initial Triage

- **Source IP:** 112.85.42.13
- **Target:** WebServer1002
- **Traffic Direction:** External to Internal
- **Attack Type:** Reflected Cross-Site Scripting (XSS)
- **Planned/Authorized Test:** No

---

## 3. Threat Intelligence

**VirusTotal — 112.85.42.13**
- Community score: -15
- Flagged as malicious by multiple vendors

**AbuseIPDB — 112.85.42.13**
- 45,324 abuse reports
- ISP: China Unicom Nanjing
- Classified as a known malicious scanner

---

## 4. Log Analysis

Log Management review of the source IP revealed multiple requests over a 
22-minute period, indicating automated scanning activity prior to the 
payload attempt.

The XSS payload was injected into the search parameter (q parameter) of 
the URL. Examination of the q parameter confirmed the presence of a 
reflected XSS payload.

All requests from the source IP returned HTTP 302 redirects with 0 bytes 
in the response body, confirming the payload was not executed by the server.

---

## 5. Attack Classification

- **Type:** Reflected XSS
- **Method:** Payload injected via URL search parameter
- **Execution:** Failed — HTTP 302 redirect prevented execution
- **Impact:** None — no compromise detected on WebServer1002

---

## 6. Conclusion

This is a confirmed True Positive. The source IP 112.85.42.13, attributed 
to China Unicom Nanjing, conducted an automated XSS scanning operation 
against WebServer1002 over a 22-minute window. Multiple XSS payload 
variations were attempted via the search parameter. All requests were 
redirected with HTTP 302 responses and 0-byte bodies, meaning the payloads 
were not executed and no compromise occurred.

Tier 2 escalation is not required as the attack was unsuccessful.

---

## 7. Recommendations

- Block IP 112.85.42.13 at the WAF and firewall level
- Monitor for further scanning activity from the China Unicom ASN
- Review WAF rules to ensure XSS payload patterns are actively filtered
- Consider implementing input validation on all search parameters

---

**Status:** Closed — True Positive | Attack Unsuccessful  
**Escalation Required:** No
