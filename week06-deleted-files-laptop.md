# Week 6: Deleted Files on a Terminated Employee's Laptop
**Course:** CPSC 4584 | Special Topics in Information Security
**Date:** October 5, 2026
**Analyst:** [Isaac Diaz]
**Case ID:** FOR-2026-1005-001

---

## Incident Summary

[Following the termination of employee Jordan Ellis, a corporate laptop experienced an unaccounted 43-hour physical custody gap prior to forensic imaging. Forensic analysis and file carving of unallocated disk space subsequently recovered five deleted files, pointing to active manual file editing and deletion during the unmonitored window.]

---

## Chain of Custody Status

**Gap Period:** Oct 3, 2:47 PM to Oct 5, 9:30 AM (approximately 43 hours)
**Gap Significance:** [The 43-hour custody gap severely compromises the chain of custody, creating a major evidentiary vulnerability that opposing counsel can exploit.]
**Documented By:** Analyst on receipt of device

---

## Recovered File Evidence Classification

| Filename | Type | Evidentiary Value | Finding |
|----------|------|-------------------|---------|
| insurance_export_final.xlsx | Spreadsheet | [High Value] | [Contains opeartion and financial records. Carving this data proves deletion of business files and supports claims of unauthorized access.] |
| mhs_billing_schema_db.sql | SQL script | [High Value] | [The billing schema can reveal internal architecture and the core structure of busniess applications.] |
| vendor_contact_list_external.docx | Document | [Medium - High Value] | [Vendor contact lists carry significant value but is less of a risk since if any deletion or modification was done, the list can be recreated.] |
| __tmp_8f2a.dat | Partial artifact | [Low Value] | [The limitation of this file is because of the data's incompleteness and also it's lack of contect.] |
| personal_vacation_2025.jpg | Image | [Low Value] | [This is a low risk file since it does not reveal and useful data about the business itself.] |

---

## Metadata Inspection Commands Used

| Command | Purpose |
|---------|---------|
| stat .bashrc | [We inspected the timestamps, the ownership, the permissions that are set, and inode and size.] |
| ls -lai | [The inode information tells us that the files that are shown in this file are nearly identical inode numbers.] |
| xxd .bashrc \| head -3 | [The first bytes revealed the file signature and the text header.] |
| find . -newer .bashrc -type f | [The time-based search returns a complete list of every file that was created or modified after the inital account setup.] |

---

## Escalation Summary

[Technical Findings

Baseline Activity & System Usage: Metadata analysis of .bashrc (stat .bashrc) confirmed account provisioning on September 2, 2026. A time-bounded search isolated post-setup user artifacts, including command histories (.bash_history, .python_history), RSA key pairs (private_key.pem, public_key.pem), and custom scripts (rsa_decrypt.py).

Sequential Staging: Inode mapping (ls -lai) showed sequential allocations for data.enc through data.enc.3 (1157698133–1157698143), proving these encrypted files were generated rapidly via automated batch execution rather than manual user interaction.

Integrity & Evidentiary Value: File header verification (xxd) and file carving confirmed pristine configuration files alongside high-value proprietary assets (mhs_billing_schema_db.sql, insurance_export_final.xlsx). Lower-risk artifacts included personal media (personal_vacation_2025.jpg) and temporary fragments (__tmp_8f2a.dat).

Custody Limitations

43-Hour Gap: A 43-hour unmonitored window occurred between user termination and drive imaging, creating an evidentiary vulnerability regarding physical access.

Attribution Risk: Because physical access was unmonitored during this gap, host-based timestamps alone cannot definitively refute claims of post-termination tampering.

Corroboration Requirement: Digital findings must be correlated with secondary sources (SIEM logs, network flows, door access records, domain authentication) to establish sole user attribution.

Governance & Review Questions

Role Authorization: Did the suspect's official job responsibilities authorize them to generate local database schema dumps (.sql) or perform local RSA encryption?

Policy Enforcement: Should personal data (personal_vacation_2025.jpg) and external vendor documentation (vendor_contact_list_external.docx) be handled as standard Acceptable Use Policy violations or integrated into the formal insider threat case?

Disclosure & Prosecution Strategy: Does executive leadership intend to report the incident to law enforcement and regulatory bodies (e.g., PII/HIPAA notification requirements), given the chain-of-custody limitation?]


---
*CPSC 4584 | Governors State University | Fall 2026*
    
