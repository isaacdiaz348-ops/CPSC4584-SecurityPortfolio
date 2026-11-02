# Week 9: SQL Injection Attempt on Appointment Scheduling System
**Course:** CPSC 4584 | Special Topics in Information Security
**Date:** October 26, 2026
**Analyst:** [Isaac Diaz]
**Case ID:** SQLI-2026-1024-001

---

## Incident Summary

[The incident log captures a rapid four-stage attack window starting with a baseline query to verify normal application functionality. The attacker then probes the system using a single quote and comment dashes before executing a tautology payload designed to bypass security controls and extract protected health information records.] 
---

## SQL Injection Log Analysis

| Entry | Time | Decoded Input | Type | Severity |
|-------|------|---------------|------|----------|
| 1 | 02:14:07 | Johnson | Benign Input | Info |
| 2 | 02:14:52 | ' | Quote Probe | Medium |
| 3 | 02:18:31 | Smith' -- | Comment Syntax Test | High |
| 4 | 02:23:14 | ' OR '1'='1 | Tautology Pattern | High |
| 5 | 02:48:39 | ' UNION SELECT NULL,NULL,NULL -- | UNION Injection Attempt | Critical |
| 6 | 03:01:22 | ' UNION SELECT patient_name,date_of_birth,ssn FROM patients -- | Targeted PHI Extraction Attempt | Critical |

---

## Attack Progression Summary

**Entry 1:** [This input establishes the application's functionality with a valid request to verify the application's expected behavior.]
**Entry 2:** [This input is designed to test how the application treats special characters and if exposes how imrpoper database error messages.]
**Entry 3:** [The role of the comment syntax is to ignore the original developer query. ]
**Entry 4:** [The purpose of the tautology pattern is to force the logical condition to test as "true" so it bypasses security checks.]
**Entry 5:** [The UNION pattern tests how columns are structued and also tests data tyoes. to be able to map backend database schema. ]
**Entry 6:** [The targeted colums suggest that the data was already targeted for exfiltration. What can be unconfirmed is if the data exfiltration was successful without any downstream response logs.]

---

## Remediation Summary

**Primary application control:** [Parameterized queries enforce strict separation between executable code and data structures. This means inputs that are supplied by the user are never interpreted as SQL syntax. ]
**Input handling:** [Input validation checks incoming data against strict format, type, and length expectations to reject malformed requests early, though it serves as a defense-in-depth measure rather than a replacement for parameterization.]
**Database permissions:** [Least privilege makes sure that the application connects to the database using an account that only has the neccessary options.]
**Detection and monitoring:** [Web application firewall rules and automated log monitoring detect anomalous SQL syntax patterns, unusual parameter encodings, and unauthorized query attempts to trigger rapid incident response alerts.] 

---

## Escalation Summary

[Confirmed Findings

Targeted SQL Injection Attack: The web application logs confirm a structured, escalating SQL injection progression originating from a single source over a nearly one-hour window (02:14:07 to 03:01:22).

Schema Reconnaissance: The attacker successfully progressed beyond basic error probes (') and logic bypass attempts (' OR '1'='1) to schema enumeration via a three-column UNION payload (' UNION SELECT NULL,NULL,NULL --).

Targeted PHI Query Execution: The final logged request explicitly targeted Sensitive/Protected Health Information by issuing a query against specific column names (patient_name, date_of_birth, ssn) within the patients table.

Unresolved Question of Data Return

Exfiltration Status Unconfirmed: While the web server log confirms receipt and transmission of the malicious payloads to the application tier, it does not confirm whether the database successfully executed the query or returned populated records to the attacker.

Payload Outcome: It remains unknown whether the application returned a 200 OK with sensitive records, threw a 500 Internal Server Error, or truncated the response.

Next Required Evidence Source

Database & Application Response Logs: Before privacy officer or legal counsel reviews can evaluate breach notification mandates or disclosure requirements, responders must analyze the database execution logs, application response payload logs, or downstream egress bandwidth metrics.

Objective: These data sources are required to verify the HTTP status code, returned payload size (byte count), and actual database query execution results to determine whether an actual exfiltration of records occurred.]

---
*CPSC 4584 | Governors State University | Fall 2026*
    
