# Week 5: Weak Password Policy Exposes Billing Portal
**Course:** CPSC 4584 | Special Topics in Information Security
**Date:** September 28, 2026
**Analyst:** [Isaac Diaz]
**Incident ID:** INC-2026-0928-001

---

## Incident Summary

[A credential stuffing attack compromised the billing_admin_03 using stolent credentials and exploiting the fact that there was no Multi-Factor Authentication and rate limiting. This unauthorized access compromised personal and medical records of 3,247 patients.]

---

## TLS Assessment

**TLS Version:** TLS 1.3
**Status:** Compliant
**What TLS Protected:** [Data in transit between client and server]
**What TLS Did Not Protect:** [Authentication -- the attacker had valid credentials]

---

## Authentication Controls Gap Analysis
| Control | Required | Status | Finding |
|---------|----------|--------|---------|
| MFA | Maplewood sensitive-account standard | Not Implemented | [Sensitive admin accounts lacked mandatory MFA, allowing attackers to gain full access with only a set of stolen credentials.] |
| Failed Attempt Protection | Account-based throttling and alerting | Not Implemented | [The system failed to enforce account lockout or threshold alerting, which allowed for continous login attempts without triggering security alarms.] |
| Password Policy | NIST SP 800-63B-4 aligned | Needs Improvement | [The existing policy relied on weak and predicatable password requirements.] |
| Compromised Credential Response | Detect and invalidate confirmed compromised authenticators | Not Implemented | [No automated system was put in place to check against known data breaches to invalidate exposed credentials upon detection.] |
| Automated Attack Controls | Throttling, bot detection, or adaptive controls as appropriate | Not Implemented | [Authentication endppint lacks any bot detection tools which leaves the portal vulnerable to credential stuffing scripts.] |

---

## OpenSSL Commands Practiced

| Command | Purpose |
|---------|---------|
| openssl genrsa -out private_key.pem 2048 | [This command created an unencrypted 2048-bit RSA private key file named "private_key.pem" that was encoded in Base64, which contains both private and public key parameters.] |
| openssl rsa -in private_key.pem -pubout | [This command extracted and derived the public key from the private_key.pem file. Output was in Base64 encoding which contained the modulus and public exponent.] |
| openssl rsa -in private_key.pem -text -noout | [The outout was raw cryptographic parameters that had the key size, modulus, public exponent, private exponent, prime factors, and exponent helper values.] |
| cat public_key.pem | [This PEM file contains the public key material containing the RSA modulus and public exponent.] |

---

## Escalation Summary

[Confirmed Findings - "billing_admin_03" account was compromised by a credential stuffing attack using a compromised crednetial. This unauthorized access exposed personal and medical records of 3,247 patients from the billing portal. While TLS was able to protect the data in transit, the authentication layer lacked defensive controls to detect or block continious login attempts.

Authentication Gaps:
- Lack of Multi-Factor Authentication
- Lack of rate limiting and bot controls
- Weak Password and Breached Credential Polices

Required Leadership Review:
- Mandatory Control Enforcement
- Patient Notification and HIPPA Reporting
- Investment in Security Infrastructure]

---
*CPSC 4584 | Governors State University | Fall 2026*
    
