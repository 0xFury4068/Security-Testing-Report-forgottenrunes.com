# Security Testing Report for forgottenrunes.com

## Overview
**Date of Testing:** September 2024  
**Tools Used:**
- OWASP ZAP
- Burp Suite
- CURL
- Browser Developer Tools (Inspector, Console)

### Testing Objectives
The primary objective of this assessment was to identify vulnerabilities on the website and provide recommendations for remediation.

---

## Methodology
We utilized automated tools to scan for common web vulnerabilities such as XSS, SQL injection, security header misconfigurations, and conducted manual testing to validate findings.

---

## Vulnerabilities Identified

### 1. Content Security Policy (CSP) Header Not Set
- **Description:** The Content-Security-Policy (CSP) header is missing, leaving the website vulnerable to injection attacks, such as cross-site scripting (XSS).  
- **Risk Level:** High. Without a CSP, malicious scripts can be executed on the page, increasing the risk of XSS attacks.  
- **Recommendation:** Implement a strict CSP header, allowing only trusted domains for script execution and resource loading.

### 2. Missing Anti-clickjacking Header
- **Description:** The X-Frame-Options header is absent, which prevents protection against clickjacking attacks.  
- **Risk Level:** Medium. Users could be tricked into clicking on a hidden iframe, exposing sensitive information.  
- **Recommendation:** Add the X-Frame-Options: `SAMEORIGIN` header or include frame-ancestors `'none'` in the CSP header.

### 3. Cross-Site Scripting (XSS)
- **Description:** Potential XSS vulnerability was discovered in input fields where user-supplied data is not properly sanitized or escaped.  
- **Testing Result:** Initial attempts at XSS did not execute due to input filtering, but the lack of robust security mechanisms means this could still pose a threat.  
- **Recommendation:** Ensure proper input validation and output encoding for user-generated content, especially in forms. Apply a comprehensive CSP policy.

### 4. Server Information Disclosure via "X-Powered-By" Header
- **Description:** The server exposes the X-Powered-By header, revealing that it is running Next.js.  
- **Risk Level:** Medium. Exposing server information provides attackers with additional details that could help in targeting specific vulnerabilities in the stack.  
- **Recommendation:** Remove or suppress the X-Powered-By header to obscure server details.

### 5. Strict-Transport-Security Header Not Set
- **Description:** The Strict-Transport-Security header is missing, which could allow attackers to downgrade connections from HTTPS to HTTP, making the site vulnerable to man-in-the-middle (MITM) attacks.  
- **Risk Level:** Medium. Without enforcing HTTPS, users are more susceptible to MITM attacks.  
- **Recommendation:** Add the Strict-Transport-Security header with a max-age value of at least one year to ensure all connections use HTTPS.

### 6. Cookies Without Secure Flag
- **Description:** Some cookies lack the Secure attribute, meaning they can be transmitted over unencrypted HTTP connections.  
- **Risk Level:** Medium. This can lead to the interception of sensitive data, such as session tokens, over insecure connections.  
- **Recommendation:** Ensure all cookies are flagged with Secure and HttpOnly to prevent their transmission over unsecured channels.

### 7. Cross-Domain Misconfiguration
- **Description:** The Access-Control-Allow-Origin header is set to `*`, allowing any domain to perform cross-origin requests.  
- **Risk Level:** High. This could lead to data leakage or enable XSS via cross-origin resource sharing.  
- **Recommendation:** Specify trusted domains in the Access-Control-Allow-Origin header instead of using a wildcard (`*`).

---

## Testing Techniques

### 1. Automated Scanning
Automated vulnerability scans using OWASP ZAP and Burp Suite revealed potential security issues and gathered server configuration data.

### 2. Manual XSS Testing
We manually tested for XSS by injecting scripts into input fields. While initial attempts were blocked, the lack of Content Security Policy (CSP) and robust input validation means XSS remains a possible threat.

### 3. Security Header Inspection
Using the `curl -I` command, we reviewed HTTP response headers to assess the presence and proper configuration of security headers, identifying missing headers.

---

## Recommendations

- **Implement CSP:** Deploy a Content Security Policy to prevent unauthorized script execution and resource loading.
- **Fix Security Headers:** Ensure Strict-Transport-Security, X-Frame-Options, and X-Content-Type-Options headers are configured properly.
- **Input Sanitization and Output Encoding:** Validate and sanitize user input and encode output to prevent XSS attacks.
- **Secure Cookies:** Flag all cookies with Secure and HttpOnly attributes to enhance data protection.

---

## Conclusion
The website has several critical and high-risk vulnerabilities that should be addressed as a priority. Correcting these issues will help secure the website against common web attacks such as XSS and man-in-the-middle attacks.
