[← README](../README.md) · [← 13. Final review sheet](13-final-review-sheet.md) · [Appendix A. SQL injection →](appendix-a-sql-injection.md)

# 14. Practice MCQs

Questions marked **(select all that apply)** can have one or more correct answers. All the others have exactly one. Answer before you open each answer.

## Foundations and threat modelling

**Q1.** An admin API is exposed to the internet with no login. Which statement describes the *vulnerability*?

- **A.** Automated internet scanning
- **B.** No authentication on the API
- **C.** Private records being read or changed
- **D.** Likelihood combined with impact

<details><summary>Answer</summary>

**B.** A is the threat, C is the impact and D is the risk.

</details>

**Q2.** Which statements about the 2021 HSE incident are correct? **(select all that apply)**

- **A.** A phishing email with a malicious attachment gave the first foothold
- **B.** Antivirus detected activity on 31 March but was in monitoring mode
- **C.** Detection on 31 March contained the attack
- **D.** Confidentiality, integrity and availability were all affected

<details><summary>Answer</summary>

**A, B, D.** Detection without response stopped nothing. The ransomware hit on 14 May.

</details>

**Q3.** Which best describes zero trust?

- **A.** A product that replaces the firewall
- **B.** Trusting every device that's on the corporate VPN
- **C.** An architecture where network location earns no trust, and each access is authorised explicitly
- **D.** Authenticating once per day at first login

<details><summary>Answer</summary>

**C.** Zero trust is a set of principles, not a product.

</details>

**Q4.** In STRIDE, *repudiation* threatens which property?

- **A.** Integrity
- **B.** Accountability
- **C.** Authentication
- **D.** Authorisation

<details><summary>Answer</summary>

**B.** Tampering → integrity, spoofing → authentication, elevation of privilege → authorisation.

</details>

**Q5.** Which statements about STRIDE are true? **(select all that apply)**

- **A.** It helps identify types of threat
- **B.** It ranks threats by likelihood and impact
- **C.** Elevation of privilege relates to authorisation
- **D.** Spoofing relates to authentication

<details><summary>Answer</summary>

**A, C, D.** STRIDE gives no likelihood, impact or priority.

</details>

**Q6.** A company buys cyber insurance to cover the cost of a data breach. Which threat response is this?

- **A.** Eliminate
- **B.** Mitigate
- **C.** Transfer
- **D.** Accept

<details><summary>Answer</summary>

**C.** Transfer. The company still keeps oversight and responsibility.

</details>

**Q7.** Which threat statement is best written?

- **A.** "Broken access control."
- **B.** "Hackers might attack the API."
- **C.** "An authenticated user changes the account ID in an update request and modifies another customer's record."
- **D.** "The system is insecure."

<details><summary>Answer</summary>

**C.** It names who, the action, the target and the consequence.

</details>

**Q8.** In Shostack's Four Questions, STRIDE is mainly used to answer:

- **A.** What are we working on?
- **B.** What can go wrong?
- **C.** What are we going to do about it?
- **D.** Did we do a good enough job?

<details><summary>Answer</summary>

**B.**

</details>

## Data hiding

**Q9.** Steganography mainly protects:

- **A.** The contents of a message
- **B.** The existence of a message
- **C.** The integrity of a message
- **D.** The sender's identity

<details><summary>Answer</summary>

**B.** Cryptography protects the contents. Steganography alone gives no confidentiality.

</details>

**Q10.** A pixel value of 44 (`00101100`) carries one message bit with value 1 using LSB embedding. What is the new value?

- **A.** 43
- **B.** 44
- **C.** 45
- **D.** 172

<details><summary>Answer</summary>

**C.** The last bit becomes 1: `00101101` = 45.

</details>

**Q11.** Which statements about NTFS alternate data streams are true? **(select all that apply)**

- **A.** A normal file hash covers only the default stream
- **B.** The presence of an ADS is evidence of malware
- **C.** `Zone.Identifier` is a legitimate stream that can record a download's origin
- **D.** A named stream runs automatically when the file is opened

<details><summary>Answer</summary>

**A, C.** Presence alone isn't suspicious, and a stream never runs itself.

</details>

**Q12.** A file scores 7.9 bits per byte of entropy. What is the best conclusion?

- **A.** It's encrypted malware
- **B.** It contains steganography
- **C.** It may be compressed, encrypted, packed or random, so interpret it in context
- **D.** It's corrupted

<details><summary>Answer</summary>

**C.** High entropy is a clue, not a verdict.

</details>

## OWASP Top 10 (2025)

**Q13.** A user changes a document number in a URL and sees another customer's documents. Which category is this?

- **A.** A01 Broken Access Control
- **B.** A02 Security Misconfiguration
- **C.** A05 Injection
- **D.** A07 Authentication Failures

<details><summary>Answer</summary>

**A.** This is the First American case. The fix is a server-side permission check on every object.

</details>

**Q14.** A session token still works after the user logs out. Which category is this?

- **A.** A01 Broken Access Control
- **B.** A04 Cryptographic Failures
- **C.** A07 Authentication Failures
- **D.** A09 Logging and Alerting Failures

<details><summary>Answer</summary>

**C.** Session management belongs to authentication.

</details>

**Q15.** An MFA service is unreachable, so the login code lets the user in anyway. Which category is this?

- **A.** A06 Insecure Design
- **B.** A07 Authentication Failures
- **C.** A08 Integrity Failures
- **D.** A10 Mishandling of Exceptional Conditions

<details><summary>Answer</summary>

**D.** The code fails open on an error. It should fail closed.

</details>

**Q16.** Which incident-to-category pairings match the lecture's examples? **(select all that apply)**

- **A.** LinkedIn unsalted SHA-1 → A04 Cryptographic Failures
- **B.** SolarWinds → A03 Software Supply Chain Failures
- **C.** 23andMe credential stuffing → A01 Broken Access Control
- **D.** Equifax expired certificate stopping traffic inspection → A09 Logging and Alerting Failures

<details><summary>Answer</summary>

**A, B, D.** 23andMe is A07 Authentication Failures.

</details>

**Q17.** A build pipeline runs a downloaded script whose hash differs from the published one (the Codecov case). Which category fits *best*?

- **A.** A03 Software Supply Chain Failures
- **B.** A08 Software or Data Integrity Failures
- **C.** A02 Security Misconfiguration
- **D.** A05 Injection

<details><summary>Answer</summary>

**B.** A08 asks whether *this particular* artifact is authentic and unchanged. A03 is the broader supplier chain.

</details>

**Q18.** Which statement is correct?

- **A.** Parameterised queries prevent XSS
- **B.** Output encoding prevents SQL injection
- **C.** Parameterised queries and output encoding protect different interpreters
- **D.** Either one makes the other unnecessary

<details><summary>Answer</summary>

**C.** Parameters protect the database, and encoding protects the browser.

</details>

## Signals, evidence and NTFS

**Q19.** What does Windows event ID 4625 record?

- **A.** A successful logon
- **B.** A failed logon
- **C.** Process creation
- **D.** A USB device connection

<details><summary>Answer</summary>

**B.** 4624 is a successful logon and 4688 is process creation.

</details>

**Q20.** Logon type 3 means:

- **A.** Interactive, at the keyboard
- **B.** Network
- **C.** Service
- **D.** Remote desktop

<details><summary>Answer</summary>

**B.** Type 2 is interactive.

</details>

**Q21.** A USBSTOR registry entry shows that:

- **A.** Files were copied to the device
- **B.** A USB storage device was seen by the system
- **C.** The user stole data
- **D.** Malware spread by USB

<details><summary>Answer</summary>

**B.** It doesn't prove any file was copied.

</details>

**Q22.** A log shows 08:40 +01:00. What is that in UTC?

- **A.** 07:40
- **B.** 08:40
- **C.** 09:40
- **D.** 08:41

<details><summary>Answer</summary>

**A.** Subtract a positive offset.

</details>

**Q23.** An upload named `report.pdf` starts with a ZIP signature. Which statements are true? **(select all that apply)**

- **A.** The extension only states the type the file claims to be
- **B.** The ZIP signature proves the file is malware
- **C.** The upload should be held, rejected or quarantined under policy, with the reason recorded
- **D.** Office files such as DOCX also begin with a ZIP signature

<details><summary>Answer</summary>

**A, C, D.** A conflicting signature calls for a policy decision. It isn't proof of malware.

</details>

**Q24.** Three failed logons are followed by a success, a new Run key and a new USB device. What is the best conclusion?

- **A.** The account was compromised and data was stolen
- **B.** Malware was installed
- **C.** The sequence warrants investigation, so check independent sources and rule out benign explanations
- **D.** Nothing can be learned from logs

<details><summary>Answer</summary>

**C.** A clue is not proof.

</details>

**Q25.** What does the NTFS file reference `4128:7` mean?

- **A.** Cluster 4128, offset 7
- **B.** MFT record 4128, sequence number 7
- **C.** USN 4128, reason flag 7
- **D.** Inode 4128, 7 hard links

<details><summary>Answer</summary>

**B.**

</details>

**Q26.** A file's $STANDARD_INFORMATION and $FILE_NAME creation times disagree. What is the best response?

- **A.** Conclude the timestamps were tampered with
- **B.** Ignore it, because it's normal
- **C.** Investigate: a copy, restore, application behaviour or tampering could all explain it
- **D.** Trust FN and discard SI

<details><summary>Answer</summary>

**C.** A mismatch alone proves none of these causes.

</details>

**Q27.** Which NTFS source records changes such as `RENAME_OLD_NAME` and `RENAME_NEW_NAME`?

- **A.** $MFT
- **B.** $LogFile
- **C.** $UsnJrnl:$J
- **D.** $Bitmap

<details><summary>Answer</summary>

**C.** It records *what* changed, not who did it or which process.

</details>

**Q28.** Which features does NTFS support that FAT32 doesn't? **(select all that apply)**

- **A.** Windows ACLs
- **B.** Alternate data streams
- **C.** A change journal
- **D.** Cluster-based allocation

<details><summary>Answer</summary>

**A, B, C.** Every one of these file systems allocates in clusters.

</details>

## Calculations

**Q29.** A 9,400-byte file is stored on 4,096-byte clusters. How much slack is there?

- **A.** 1,304 bytes
- **B.** 2,888 bytes
- **C.** 3,104 bytes
- **D.** 0 bytes

<details><summary>Answer</summary>

**B.** 3 clusters × 4,096 = 12,288, and 12,288 − 9,400 = 2,888.

</details>

**Q30.** A 10,000-byte file is stored on 4,096-byte clusters. How much slack is there?

- **A.** 2,288 bytes
- **B.** 2,888 bytes
- **C.** 1,808 bytes
- **D.** 4,096 bytes

<details><summary>Answer</summary>

**A.** 12,288 − 10,000 = 2,288.

</details>

**Q31.** A file's runs are (start 120, length 2) and (start 900, length 1), with 4,096-byte clusters. Where is file byte 8,500?

- **A.** LCN 121, offset 308
- **B.** LCN 900, offset 308
- **C.** LCN 122, offset 212
- **D.** LCN 900, offset 8,500

<details><summary>Answer</summary>

**B.** 8,500 − 8,192 = 308, which falls in the third cluster, and the third cluster is LCN 900.

</details>

**Q32.** With Argon2id at m = 65,536 KiB and p = 4, how much memory does each lane use?

- **A.** 4 MiB
- **B.** 16 MiB
- **C.** 64 MiB
- **D.** 256 MiB

<details><summary>Answer</summary>

**B.** 64 MiB ÷ 4 lanes = 16 MiB.

</details>

**Q33.** How much memory do 20 simultaneous Argon2id verifications need at 64 MiB each?

- **A.** 640 MiB
- **B.** 1 GiB
- **C.** 1.25 GiB
- **D.** 64 MiB

<details><summary>Answer</summary>

**C.** 20 × 64 = 1,280 MiB, and 1,280 ÷ 1,024 = 1.25 GiB.

</details>

## Information disclosure

**Q34.** A login page says "No account with that address" for unknown emails and "Password is incorrect" for known ones. What is this weakness?

- **A.** CWE-209 error-message exposure
- **B.** CWE-204 user enumeration (response discrepancy)
- **C.** CWE-798 hard-coded credentials
- **D.** CWE-636 failing open

<details><summary>Answer</summary>

**B.** The fix is the same status and message for both failures, comparable work behind each, and attempt limits.

</details>

**Q35.** Both failed-login messages are now identical. How could accounts still be enumerated?

- **A.** They can't be
- **B.** Through response timing, because real accounts run the slow password check
- **C.** Through the HTTPS certificate
- **D.** Through the `HttpOnly` flag

<details><summary>Answer</summary>

**B.** A repeatable timing difference leaks which accounts exist.

</details>

**Q36.** A developer commits an API key, deletes it in the next commit and adds the file to `.gitignore`. Which actions are needed? **(select all that apply)**

- **A.** Revoke or rotate the key
- **B.** Investigate how the key was used
- **C.** Rely on `.gitignore`, which removes the key from history
- **D.** Contain access to the exposed files or repository

<details><summary>Answer</summary>

**A, B, D.** `.gitignore` only affects untracked files. The order is Contain → Revoke → Investigate → Clean.

</details>

**Q37.** During an export, the permission service is down. What should the application do?

- **A.** Allow the export, so users aren't blocked
- **B.** Refuse the export and log the problem
- **C.** Export only half the data
- **D.** Retry until it works, then export without checking

<details><summary>Answer</summary>

**B.** Fail closed. An unavailable check hasn't granted permission.

</details>

**Q38.** A profile page shows only the name and course, but the JSON response also contains `recoveryToken`. What is the best fix?

- **A.** Hide the field with CSS
- **B.** Return only the fields the client needs, decided on the server (a response contract)
- **C.** Base64-encode the token
- **D.** Add `HttpOnly` to the response

<details><summary>Answer</summary>

**B.** Choose the fields to send. Hiding them on the page enforces nothing.

</details>

**Q39.** What should a public error response include? **(select all that apply)**

- **A.** A generic message with a safe next step
- **B.** An opaque support reference ID
- **C.** The stack trace, to help the user
- **D.** The failed SQL query text

<details><summary>Answer</summary>

**A, B.** The detail goes to a restricted log under the same reference.

</details>

## Malware and ransomware

**Q40.** Which type of malware spreads between systems on its own, with no host file?

- **A.** Virus
- **B.** Worm
- **C.** Trojan
- **D.** Rootkit

<details><summary>Answer</summary>

**B.**

</details>

**Q41.** A program has a valid digital signature. What does this show?

- **A.** It's harmless
- **B.** It has been scanned for malware
- **C.** Who signed it, and that it hasn't changed since it was signed
- **D.** It can't contain a backdoor

<details><summary>Answer</summary>

**C.**

</details>

**Q42.** Ransomware *encrypting* live data mainly harms which objective?

- **A.** Confidentiality
- **B.** Integrity
- **C.** Availability
- **D.** Accountability

<details><summary>Answer</summary>

**C.** Copying data harms confidentiality, and altering records harms integrity.

</details>

**Q43.** Which statements about ransomware recovery are true? **(select all that apply)**

- **A.** Restoring backups recovers the confidentiality of stolen data
- **B.** A synchronised copy can replicate the encryption damage
- **C.** A successful restore test is better evidence than a completed backup job
- **D.** Renaming the file extensions back decrypts the files

<details><summary>Answer</summary>

**B, C.** A backup restores availability only, and renaming doesn't change the encrypted contents.

</details>

**Q44.** Malware that rewrites its own code while keeping the same behaviour is:

- **A.** Polymorphic
- **B.** Metamorphic
- **C.** A rootkit
- **D.** Scareware

<details><summary>Answer</summary>

**B.** Polymorphic malware changes only its wrapper (encryption and decoder).

</details>

**Q45.** A sample does nothing during dynamic analysis. What can you conclude?

- **A.** It's harmless
- **B.** It's a worm
- **C.** Nothing definite: it may detect analysis or need a trigger
- **D.** Its hash is safe to allowlist

<details><summary>Answer</summary>

**C.**

</details>

## Authentication and passwords

**Q46.** What does a salt add to password hashing?

- **A.** Extra computational cost per guess
- **B.** Different hashes for equal passwords, which defeats precomputed tables
- **C.** Encryption of the password
- **D.** The ability to recover the password

<details><summary>Answer</summary>

**B.** Cost comes from the algorithm's settings, not from the salt.

</details>

**Q47.** Why is plain SHA-256 a poor choice for storing passwords?

- **A.** It's reversible
- **B.** It's fast, so each guess is cheap
- **C.** It produces outputs that are too short
- **D.** It can't be salted

<details><summary>Answer</summary>

**B.** Use Argon2id, which has tunable time and memory cost.

</details>

**Q48.** In `$argon2id$v=19$m=65536,t=3,p=4$...`, what does `v=19` mean?

- **A.** The year 2019
- **B.** 19 iterations
- **C.** Version 0x13 of the algorithm
- **D.** A 19-byte salt

<details><summary>Answer</summary>

**C.**

</details>

**Q49.** In the Adobe breach, "24 of the top 100 hints were family names" means:

- **A.** 24% of accounts used family names
- **B.** 24 of the 100 most common hint strings were family names
- **C.** 24 million accounts were cracked
- **D.** 24 passwords were recovered

<details><summary>Answer</summary>

**B.**

</details>

**Q50.** Which statements about password verification with Argon2id are true? **(select all that apply)**

- **A.** The stored hash is decrypted and compared with the input
- **B.** The submitted password is rehashed using the stored salt and settings
- **C.** The whole record (algorithm, settings, salt, hash) must be stored
- **D.** The salt and hash in the record are Base64-encoded

<details><summary>Answer</summary>

**B, C, D.** Nothing is ever decrypted.

</details>

## Sessions, cookies and access control

**Q51.** What does the `HttpOnly` cookie attribute do?

- **A.** Sends the cookie only over HTTPS
- **B.** Stops page JavaScript reading the cookie, while the browser still sends it
- **C.** Prevents XSS
- **D.** Blocks cross-site requests

<details><summary>Answer</summary>

**B.** A is `Secure` and D is roughly `SameSite`. HttpOnly doesn't stop script injection.

</details>

**Q52.** A cookie has `SameSite=Lax`. In which case is it sent?

- **A.** A form POST from another site
- **B.** Your site loaded inside another site's iframe
- **C.** The user clicks a normal link on another site that leads to your site
- **D.** Never on any cross-site request

<details><summary>Answer</summary>

**C.** Lax allows top-level GET navigations only.

</details>

**Q53.** `https://shop.cedar.example` and `https://help.cedar.example` are:

- **A.** Same-site and same-origin
- **B.** Same-site but different origins
- **C.** Cross-site but same-origin
- **D.** Cross-site and different origins

<details><summary>Answer</summary>

**B.** They share the scheme and registrable domain, but the hosts differ.

</details>

**Q54.** What does the `__Host-` cookie prefix require? **(select all that apply)**

- **A.** The `Secure` attribute
- **B.** `Path=/`
- **C.** No `Domain` attribute
- **D.** The `HttpOnly` attribute

<details><summary>Answer</summary>

**A, B, C.** HttpOnly is good practice but isn't required by the prefix.

</details>

**Q55.** Lina changes `/api/orders/7201` to `/api/orders/7202` and sees Omar's order. What is this failure?

- **A.** BOPLA (API3), property level
- **B.** IDOR/BOLA (API1), horizontal
- **C.** Broken function-level authorisation (API5), vertical
- **D.** CSRF

<details><summary>Answer</summary>

**B.** It's another user's record at the same role.

</details>

**Q56.** A customer calls the refund endpoint directly, even though the refund button is hidden from customers. What is this failure?

- **A.** Horizontal (IDOR)
- **B.** Vertical (broken function-level authorisation, API5)
- **C.** Information disclosure
- **D.** Not a vulnerability, because the button is hidden

<details><summary>Answer</summary>

**B.** The server must check the role before anything changes.

</details>

**Q57.** Which ownership check is correct?

- **A.** `account.id === purchase.id || account.role === "support"`
- **B.** `account.id === purchase.ownerId && account.role === "support"`
- **C.** `account.id === purchase.ownerId || account.role === "support"`
- **D.** `Boolean(account && purchase)`

<details><summary>Answer</summary>

**C.** A compares with the order number, B demands both conditions, and D only proves the records exist.

</details>

**Q58.** After sign-out, a saved session ID still returns `200`. What is missing?

- **A.** The `Secure` flag
- **B.** Server-side session revocation
- **C.** A shorter cookie name
- **D.** Client-side cookie deletion

<details><summary>Answer</summary>

**B.** Once the session is revoked, the old ID gets `401`.

</details>

**Q59.** Which are valid CSRF defences? **(select all that apply)**

- **A.** Validated CSRF tokens
- **B.** Origin checks
- **C.** No state changes on GET requests
- **D.** Setting `HttpOnly` on the session cookie

<details><summary>Answer</summary>

**A, B, C.** SameSite helps as an extra layer. HttpOnly has nothing to do with CSRF.

</details>

**Q60.** Ada is signed in and requests Ben's booking after the ownership fix. Which status should she get?

- **A.** 200
- **B.** 401
- **C.** 403
- **D.** 409

<details><summary>Answer</summary>

**C.** She is authenticated but not permitted. 401 is for no valid session.

</details>

---

[← README](../README.md) · [← 13. Final review sheet](13-final-review-sheet.md) · [Appendix A. SQL injection →](appendix-a-sql-injection.md)
