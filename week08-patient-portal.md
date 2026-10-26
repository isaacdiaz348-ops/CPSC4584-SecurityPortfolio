# Week 8: Patient Portal Login Page Behaving Strangely
**Course:** CPSC 4584 | Special Topics in Information Security
**Date:** October 19, 2026
**Analyst:** [Isaac Diaz]
**Case ID:** WEB-2026-1019-001

---

## Incident Summary

[On October 17, 2026, the Maplewood IT helpdesk received three patient reports describing unexpected portal behavior, including credentials appearing directly in the browser address bar. The subsequent SOC investigation confirmed four pre-existing security vulnerabilities in the login flow, including GET-based credential submission and unvalidated external redirects, though no active exploitation has been confirmed.]
---

## HTTP Vulnerability Findings

| Finding | Location | Observed Value | Vulnerability | Severity |
|---------|----------|----------------|---------------|----------|
| 1 | Login request method | [GET /login?username=jsmith&password=Summer2026!] | [Credential Exposure in URL] | [High] |
| 2 | Set-Cookie header | [Set-Cookie: sessionid=a3f8c2d1; Path=/ (Missing HttpOnly and Secure)] | [Insecure Cookie Configuration] | [High] 
| 3 | Response headers | [Absence of Strict-Transport-Security, Content-Security-Policy, and X-Frame-Options] | [Security Headers Missing] | [Medium] |
| 4 | Redirect handling | [Arbitrary external URLs supplied via the redirection parameter are accepted without validation or sanitization and directly executed] | [Redirect Not Validated] | [High] |

---

## Analysis Summary

**Finding 1:** [Evidence shows credentials appended to the URL query string in a GET request. The impact is permanent credential storage across browser history, server logs, reverse proxy logs, and HTTP Referer headers. The proposed remediation direction is to refactor the authentication endpoint to accept submissions exclusively via the POST method over HTTPS, transmitting credentials securely within the request body.]

**Finding 2:** [Evidence shows the session cookie issued during login lacks HttpOnly and Secure attributes. The impact is a heightened risk of session hijacking through local script access or plaintext network interception. The proposed remediation direction is to update cookie generation configuration to enforce HttpOnly, Secure, and SameSite flags on all session identifiers.]

**Finding 3:** [Evidence shows an absence of defensive response headers including Strict-Transport-Security, Content-Security-Policy, and X-Frame-Options. The impact includes vulnerability to protocol downgrade attacks, clickjacking, and reduced resistance against script injections. The proposed remediation direction is to deploy comprehensive security headers across all application response paths.]

**Finding 4:** [Evidence shows arbitrary external URLs supplied in the post-login redirection parameter are accepted without validation and directly executed. The impact is high susceptibility to phishing and social engineering campaigns directed at authenticated users. The proposed remediation direction is to restrict post-login redirection to validated relative paths starting with a single forward slash or enforce a strict whitelist of approved external domains.]

---

## HTTP Header Inspection Commands Used

| Command | Purpose |
|---------|---------|
| curl -I https://example.com | [Observed standard response headers including status code 200, content type, Cloudflare server identification, and cache status flags.] |
| curl -I + grep security headers | [ Returned an empty output with zero matching lines, confirming the complete absence of defensive headers such as Strict-Transport-Security, Content-Security-Policy, and X-Frame-Options in the response.] |
| curl -v + grep request/response | [Inspected raw request lines and response headers to map client requests against server metadata and payload content.] |
| curl -IL + grep HTTP/Location | [Observed the final status code 200 with no intermediate redirect locations or cookie assignments returned by the filter.] |

---

## Escalation Summary

[ The SOC confirmed four main vulnerabilities in the login flow: passwords exposed in URLs, session cookies missing security flags, missing security response headers, and unvalidated web redirects. These issues expose user credentials in server logs, increase the risk of session hijacking, leave the portal open to clickjacking and script attacks, and allow attackers to redirect users to fake login pages. To fix this, the portal must be updated to process logins through POST requests, enforce secure cookie attributes, add standard defensive headers, and restrict redirects to safe local paths. Authorized developers will update the application code, the security team will configure header policies, and operations will deploy the patch and monitor logs for ongoing issues.] 
---
*CPSC 4584 | Governors State University | Fall 2026*
    
