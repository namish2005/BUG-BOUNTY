# BUG-BOUNTY
# Bounty Hunt 101 Report

**Author:** Namish K.S.  
**Project Type:** Cyber Security – Bug Hunt 101 (Recon to Report)

---

## 📖 Abstract
This project documents a comprehensive security assessment of the **OWASP Juice Shop** application. The goal was to simulate a real-world bug bounty cycle, including asset discovery, vulnerability exploitation, and professional reporting.

---

## 🛠 Tools Used
- **Burp Suite (Community/Professional):** Intercepting and manipulating HTTP traffic  
- **Node.js:** Runtime environment for the target application  
- **Google Chrome (Burp Browser):** Testing interface  
- **Nmap:** Network service discovery and enumeration  

---

## 🔎 Phase 1 – Reconnaissance
- **Command:** `nmap -sV -p 3000 localhost`  
- **Finding:** Port 3000 open, running Node.js Express server → narrowed attack surface to web vulnerabilities.

---

## 💉 Phase 2 – SQL Injection (SQLi)
- **Vulnerability:** Login form fails to sanitize email input.  
- **Payload (PoC):** `admin@juice-sh.op' OR 1=1 --`  
- **Impact:** Bypasses authentication, logs attacker into Administrator account without valid credentials.

---

## ⚡ Phase 3 – Reflected XSS
- **Vulnerability:** Search functionality reflects user input without encoding.  
- **Payloads:**  
  - `<iframe src="javascript:alert('XSS')">`  
  - `<h1>Hacked</h1>`  
- **Impact:** Arbitrary JavaScript execution → session hijacking, defacement.

---

## 🔑 Phase 4 – IDOR (Insecure Direct Object Reference)
- **Vulnerability:** Basket feature allows direct object manipulation.  
- **PoC Steps:**  
  1. Intercept `/rest/basket/1` request  
  2. Modify ID → `/rest/basket/2`  
  3. Access another user’s shopping cart data.  
- **Impact:** Unauthorized access to private user data.

---

## 🔐 Phase 5 – Sensitive Data Exposure
- **Test:** Payment gateway with Visa test card `4111 1111 1111 1111`.  
- **Finding:** Application stores and displays full card numbers in “My Payment Options.”  
- **Impact:** High-risk exposure of financial data.

---

## 📊 Key Findings
- **Authentication & Authorization:**  
  - SQLi → Admin account bypass  
  - IDOR → Unauthorized data access  
- **Client-Side Security:**  
  - Reflected XSS → Arbitrary script execution  
- **Data Security:**  
  - Sensitive card details exposed  

---

## ✅ Recommendations
- **Parameterized Queries:** Use prepared statements to prevent SQLi.  
- **Robust Access Controls:** Validate object requests against user sessions.  
- **Context-Aware Output Encoding:** Escape user input before rendering in browser.  
- **Secure Payment Handling:** Mask or tokenize card details, never store full PAN.

---

## 📌 Conclusion
The assessment revealed **critical vulnerabilities** across authentication, authorization, client-side security, and data handling. Exploitation in production could lead to **database theft, session hijacking, financial compromise, and total platform breach.**  
Immediate remediation is required to protect users and maintain trust.

---
