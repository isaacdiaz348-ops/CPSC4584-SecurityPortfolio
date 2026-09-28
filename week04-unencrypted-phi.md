## Cryptographic Controls Evaluated

**Base64 encoding:** [Binary to text encoding scheme because it represents binary data in ASCII characters. Which makes it safe for transmission across text-based protocoll like email. Base64 is not an encryption and does not have a key.]
**Caesar cipher:** [A Caesar Cipher is classified as a symmetric, mono alphabetical, substitution cipher. The cipher replaces each letter in the message with a different letter based on a fixed value. A caesar cipher is weak to frequency analysis and easy to crack by brute-force.
**Modern encryption at rest:** [A recognized encryption approach that Maplewood should evaluate is AES-256. We recommend this approach because the main vulnerability Maplewood had involved unencrypted patient records. AES-265 directly addresses this issue by rendering stored patient health records unreadable to unauthorzied personel.]

---

## Hashing Commands Practiced

| Command | Purpose | Output Length |
|---------|---------|---------------|
| echo -n "..." \| sha256sum | [This demonstrated string hashing and how SHA-256 generates a fixed digest from the specific text input.] | [Digest length is 64 hexadecimals.] 
| echo -n "..." \| md5sum | [This comarison matters because even though we use the same text file, passing it through md5sum produces a complelety unique format and string length] | [The digest length is 32 hexadecimal characters.] |
| sha256sum .bashrc | [A file has supports integrity verification by generating a unique cryptographic fingerprint from a file's contents using an algorithm.] | [Digest length is 64 hexadecimals.] |

---

## Escalation Summary

[Confirmed Findings and Unknowns: The investigation confirmed that PATIENT_DATA_ARCHIVE stored 8,247 unencrypted ePHI records across 847 files with broad read and write access granted to all network users across four clinics and an inpatient facility. Since no historical logs exist prior to the discovery, it remains unknown whether any internal or external actor viewed, copied, or withdrew information.

Required Escalations: Leadership must approve emergency response resources, privacy and legal counsel must conduct a formal four-factor HIPPA risk assesment to determine if mandatory breach notifications to affected individuals are required, and IT Security must oversee forensic imagining and implement strict role-based access controls alongside AES-256 encryption]

---
*CPSC 4584 | Governors State University | Fall 2026*
    
