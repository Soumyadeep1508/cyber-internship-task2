# cyber-internship-task2
📧 Task 2 — Phishing Email Analysis

# 📧 Task 2 — Phishing Email Analysis
**Elevate Labs — Cyber Security Internship**

---

## 📌 Objective
Analyze a suspected phishing email sample to identify technical and social-engineering indicators, document findings, and provide mitigation recommendations.

---

## 🛠 Tools & Files Used
- **Tools:** MXToolbox (Email Header Analyzer), a text editor, web browser  
- **Files included in this repo:**  
  - `sample-1.eml` — original email file (raw)  
  - `header_analysis.pdf` — MXToolbox output / header transcription  
  - `screenshots/` — MXToolbox screenshots and hovered-link screenshots  
  - `README.md` — this file

*(Analysis is based on the email headers and MXToolbox screenshots included in this repo). :contentReference[oaicite:1]{index=1}*

---

## 🔎 Email Summary
- **From (display):** `BANCO DO BRADESCO LIVELO <banco.bradesco@atendimento.com.br>`  
- **Subject:** `CLIENTE PRIME - BRADESCO LIVELO: Seu cartão tem 92.990 pontos LIVELO expirando hoje!`  
- **Received From (technical):** `ubuntu-s-1vcpu-1gb-35gb-intel-sfo3-06 (137.184.34.4)` — a cloud VPS host, not a bank mail server.  
- **Delivery:** Routed via Microsoft/Office365 protection front-ends before reaching recipient.

---

## ✅ Key Findings (Technical Indicators)
1. **DMARC: No record found / DMARC compliance missing**  
   - The sending domain does not publish a DMARC policy. Legitimate financial institutions publish DMARC to prevent spoofing.

2. **SPF: TempError / Failed (sender IP not authorized)**  
   - `spf=temperror` and SPF lookup timed out for the sending hostname. Sender IP `137.184.34.4` is a cloud VPS (DigitalOcean / similar), not a bank mail server.

3. **DKIM: Not present (dkim=none)**  
   - No DKIM signature was found. Financial organizations sign messages to validate integrity and origin.

4. **Return-Path / Message-ID mismatch**  
   - `Return-Path: root@ubuntu-s-1vcpu-1gb-35gb-intel-sfo3-06` while the `From:` header uses `atendimento.com.br`. This reveals the true sending host.

5. **Suspicious sending host / IP**  
   - `X-Sender-IP: 137.184.34.4` — cloud VPS, frequently used for throwaway phishing infrastructure.

*(All header evidence and MXToolbox screenshots are included in `header_analysis.txt` and the `screenshots/` folder). :contentReference[oaicite:2]{index=2}*

---

## ⚠️ Key Findings (Social-engineering / Content Indicators)
- **Urgency & Scarcity:** Subject says points are expiring *today* — pressure to act immediately.  
- **High reward lure:** Large number of reward points used to entice clicks.  
- **HTML-heavy content:** Styled call-to-action and button (designed to mimic bank UI).  
- **External, unrelated link:** Link targets a domain not owned by the bank (example shown in screenshots).  
- **Grammar/formatting:** Although branding is present, the email header and infrastructure betray legitimacy.

---

## 🎯 Final Assessment
This email is **phishing**. Technical authentication (SPF/DKIM/DMARC) fails or is absent, the message originates from a cloud VPS instead of an official bank mail server, and the email leverages classic social-engineering (urgency and rewards). The combination of header mismatches and malicious external links indicates a credential-harvesting attempt.

Evidence source: header analysis and MXToolbox results included here. :contentReference[oaicite:3]{index=3}

---

## 🛡 Recommended Actions / Remediation
For recipients:
- **Do not click** links or download attachments.  
- **Report** the message to the email provider (Report phishing) and to the affected organization if they provide a fraud reporting channel.  
- **Delete** the email after reporting.  
- **Block** the sender if possible.

For domain owners / administrators (if you control the domain):
- Publish and enforce **DMARC** (p=quarantine / p=reject after monitoring).  
- Ensure **SPF** publishes authorized mail relays and resolves cleanly.  
- Sign outbound mail with **DKIM**.  
- Monitor for abuse, and block suspicious sending IPs.

---

## 📁 Repository Contents (what to review)

├── README.md

├── sample-1.eml

├── header_analysis.pdf

└── screenshots/

├── mxtoolbox-delivery.png

├── relay-info1.png

├── relay-info2.png

├── mxtoolbox-spf-dkim.png

├── mxtoolbox-headers1.png

├── mxtoolbox-headers2.png

├── mxtoolbox-headers3.png

├── mxtoolbox-headers4.png



---

## 🧭 How I analyzed this (reproducible steps)
1. Open `sample-1.eml` in a text editor / email client and copy full headers.  
2. Paste headers into **MXToolbox → Email Header Analyzer** (or Google Header Analyzer).  
3. Inspect: SPF result, DKIM status, DMARC lookup, Received chain, Message-ID and Return-Path.  
4. Hover links safely (do not click) to verify visible text vs. actual URL. Capture screenshots.  
5. Save MXToolbox output as `header_analysis.pdf` and export screenshots into `screenshots/`.

---

## 🔚 Conclusion
The analyzed sample is a credential-phishing email that impersonates a trusted financial brand. The lack of DKIM/DMARC and a failed SPF lookup, combined with a sending host from a cloud VPS and urgency-based messaging, confirms the malicious intent. This report and included artifacts (raw `.eml`, header output, screenshots) provide verifiable evidence for Task 2 submission.

---

