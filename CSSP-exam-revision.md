# CSSP Exam Revision: The 80/20 Version

*Cybersecurity and Secure Programming. Sources: "CSSP Lecture Notes to date" (all 10 chapters, checked 17 November 2026), "CSSP Complete Lecture Slides" (182 pages), the Week 7 SQL injection lecture, the Week 2, 4, 5, 6 and 7 labs, the command injection lab and the two CTF briefs on Moodle.*

**Key dates (from the Assessments section on Moodle):**

| Date | What | Details |
|---|---|---|
| **Thu 19 Nov 2026, 2 PM** | Assessment | 1 hour, 25% of the module |
| Thu 19 Nov 2026 | Project specification released | |
| **Tue 15 Dec 2026, 2 PM** | Final assessment | 2 hours, 25%. Arrive ten minutes early |
| **Wed 13 Jan 2027** | Project submission | 50%. Vivas on Thu 14 and Fri 15 January |

**Scope for Thursday (from Mike):** sections 1 to 10 of this file match the listed topics, and the lecture notes are the main source.
- Points marked **(slides)** come only from the slides, so treat them as extras.
- Section 11 keeps only the labs that support listed topics.
- SQL injection, command injection and the Week 4 and 7 labs aren't on the list, so they're in the **Appendix** at the end, with their own self-test.

**How to use this (about 60 minutes):**
1. Read **The 7 big ideas** and **Exam traps** first. They carry the most marks per minute.
2. Learn the [Must know] items by heart: CIA, risk terms, Four Questions, STRIDE, OWASP Top 10, the Week 5 leak table, malware types, Argon2id, cookie settings and SameSite.
3. Practise the **calculations** until you can do them without looking.
4. Cover the Answer column and do the **self-test** at the end. Re-read only what you get wrong.

> **Note:** the priorities and memory tricks here are my own, not your lecturer's. I don't know your exam format. For the five new lab ZIPs (Week 6, Week 7, command injection and the two CTFs), only the instructions were read and none of their code was run. The Week 4 and 5 ZIPs weren't opened, because their workbooks were already covered. The Galway Notes practical wasn't on the Moodle page. Check anything your lecturer stressed in class.

---

## The 7 big ideas

If you remember nothing else, remember these. The whole module repeats them.

1. **A clue is not proof.** Records show what happened on the system, not who did it or why. This is the most repeated idea in the notes.
2. **Security is a relationship, not a product:** what must be protected + what threatens it + what happens if protection fails.
3. **Any single control can fail**, so use layers: Prevent → Detect and respond → Recover.
4. **Be specific.** Name the actor, action, target and impact. "Broken access control" alone is not a threat statement.
5. **Keep similar things separate.** Many questions test the difference between two terms, such as authentication and authorisation. See **Pairs people mix up**.
6. **The server decides.** The user controls the browser, so only a check on the server is enforced. Tell outsiders as little as possible, and keep the detail where only your team can read it.
7. **Removing or hiding is not fixing.** A deleted key still works, Git history still holds it, a removed malware file can leave the attacker's access behind, and a hidden button doesn't stop a direct request.

---

## 1. Cybersecurity foundations

### CIA triad [Must know]

| Objective | Ask | Example controls |
|---|---|---|
| **Confidentiality** | Who can see it? | Access control, encryption |
| **Integrity** | Can it be trusted? | Hashes, signatures, audit trails |
| **Availability** | Is it there when needed? | Backups, redundancy, capacity planning |

The objectives can conflict. Emergency records must be available fast, but therapy notes need tighter access. Base the choice on the likely harm.

A control can backfire (slides): a strict password policy can improve confidentiality in theory but weaken it in practice if users write passwords down or reuse them.

### Threat, vulnerability, impact, risk [Must know]

Example: an exposed admin API with no login.

| Term | Question | In the example |
|---|---|---|
| **Threat** | What could cause harm? | Automated internet scanning |
| **Vulnerability** | What weakness makes it possible? | No authentication on the API |
| **Impact** | What is the harm if it happens? | Private records read or changed |
| **Risk** | How much attention does it deserve? | Likelihood and impact together |

**Risk ≈ likelihood × impact.** Use this to compare and prioritise. It is not an exact calculation, because the inputs are usually estimates.

### HSE ransomware incident, 2021 [Important]

| Date | Event |
|---|---|
| **18 March 2021** | Phishing email: a malicious attachment gives the first foothold |
| **31 March** | Antivirus detects activity, but it is in **monitoring mode**, so nothing is blocked. Third-party monitoring is not continuous overnight |
| **14 May** | Major service disruption from ransomware |

- **Scale:** data from about 500 patients published online. More than 100,000 people needed to be contacted by November 2022. These figures measure different things at different stages.
- **All of CIA was hit:** data disclosed (C), trusted systems and accounts compromised (I), services down (A).
- **Main lesson:** detection without response does not stop an attack. An incident is a *sequence* of stages, not one event.

### Attack surface and attack vector [Important]

- **Surface = where** contact is possible: interfaces, data entry points, identities and credentials, dependencies.
- **Vector = how** an attacker uses that contact.
- Example: the login form is the surface. Credential stuffing is the vector.

### Threat actors [Useful]

| Actor | Typical goal or behaviour |
|---|---|
| **Cybercriminals** | Money: theft, fraud, extortion |
| **State-linked groups** | Intelligence, influence, disruption; patient and persistent |
| **Insiders** | Already have trusted access; malicious, coerced or a **mistake** |
| **Opportunists** | Scan broadly and exploit what they find |

Motive is a **hypothesis**, not proof of who did it. Motive changes the **target**, the **tempo** and **what detection looks like**: fast money means broad scanning and visible impact; long-term spying means narrow targets, persistence and avoiding detection.

### Layered defence and zero trust [Important]

**Defence in depth** assumes a control can fail:

| Stage | Purpose | Examples |
|---|---|---|
| **Prevent** | Reduce exposure | Patching, strong authentication, least privilege |
| **Detect and respond** | Find and contain | Monitoring, alert review, incident response |
| **Recover** | Restore and learn | Tested backups, recovery plans, post-incident changes |

**Zero trust** is an architecture and set of principles, **not a product**. Being inside the network does not earn trust.
- Authenticate and authorise explicitly, for each session.
- Decide using identity, device and context.
- Give only the access needed for the task.
- Keep re-checking, because trust can change.

### Secure programming habits [Useful]

Validate input · Enforce authorisation on the server · Encode output for its destination · Handle errors safely · Use safe defaults · Manage dependencies · Log security events

---

## 2. Threat modelling

Threat modelling is a structured way to understand a system, find plausible ways it could fail or be misused, and decide what to do. It is most useful **during design**. Revisit it when the system changes.

- **Three traits (slides):** multidisciplinary, decision-focused, maintained.
- **Security across the lifecycle:** Requirements (set objectives) → Design (analyse threats, choose controls) → Implementation (apply and test) → Operation (monitor and reassess).
- **Why early matters:** a widely cited estimate puts fixing a defect after delivery at up to **100 times** the cost of fixing it in requirements and design (Boehm and Basili, 2001). The multiplier is not a universal rule.

### Shostack's Four Questions [Must know]

1. **What are we working on?** Scope, users, data, boundaries.
2. **What can go wrong?** Trace data flows, mark trust boundaries, use STRIDE.
3. **What are we going to do about it?** Choose a response and explain why.
4. **Did we do a good enough job?** Review coverage, assumptions and whether controls work.

**Memory trick: "Work, Wrong, Do, Done."** It is a cycle, because a design change can create new threats.

### Make a simple system model [Important]

1. **List the parts:** actors, processes, data stores and external systems. Include external services and dependencies, not only your own code.
2. **Describe each important data flow:** its source, destination, the data it carries, the identity involved and any **change in privilege**.
3. **Mark a trust boundary** wherever information crosses between components with different levels of trust or control.
4. **Record assumptions and boundaries**, so someone else knows what the model does and doesn't cover.

Example: a browser sending a note to a web application crosses a trust boundary; the application's query to its database is another flow to examine. A model is a map for asking questions, not a guarantee that every threat has been found.

The output can be a diagram, a threat table or design notes. What matters is that each finding leads to a documented decision.

### STRIDE [Must know]

| Letter | Threat | Property at stake | Example |
|---|---|---|---|
| **S** | Spoofing | **Authentication** | Reusing a stolen session token |
| **T** | Tampering | **Integrity** | Changing an account ID to edit someone's record |
| **R** | Repudiation | **Accountability** | Admin deletes a record with no trustworthy audit event |
| **I** | Information disclosure | **Confidentiality** | Changing a URL reveals another user's record |
| **D** | Denial of service | **Availability** | Large uploads fill storage and block users |
| **E** | Elevation of privilege | **Authorisation** | Submitting an admin role value to reach admin functions |

STRIDE finds threat **types**. It does **not** give likelihood, impact or priority. Assess those separately.

**For denial of service, ask "what is finite?"** Time, memory, storage, connections, money and human attention.

### Writing a good threat [Important]

Formula: **who + action + target + consequence**

> "An unauthenticated attacker changes a record identifier in a request and reads another user's private record."

### Four responses [Important]

| Response | Meaning |
|---|---|
| **Eliminate** | Remove the feature, path or assumption that creates the threat |
| **Mitigate** | Reduce likelihood or impact with controls |
| **Transfer** | Assign agreed parts of the risk to a provider or insurer; oversight still matters |
| **Accept** | Keep the risk knowingly, with a reason and an owner |

**Memory trick: "Every Mistake Teaches Answers"** (E, M, T, A).

Record the **decision, rationale, owner and residual risk**.

---

## 3. Data hiding

### Steganography vs cryptography [Must know]

| | Question it answers | What it protects |
|---|---|---|
| **Cryptography** | "What does this message say?" | The **contents** |
| **Steganography** | "Is there a message here?" | The **presence** of the message |

Steganography alone does not make content confidential. They can be combined, but neither guarantees avoiding detection.

**Parts of the basic model:** message + carrier + embedding method → stego object.

### Technique families [Important]

- **Spatial (LSB):** changes low-order pixel bits, such as the last bit of a colour value.
  - 44 (`00101100`) → 45 (`00101101`) when the message bit is 1.
  - 101 (`01100101`) → 100 (`01100100`) when the message bit is 0.
  - Re-saving, resizing or converting the image may destroy the payload.
- **Transform-domain:** changes values in a transformed version of the carrier, such as DCT or wavelet coefficients.
- **Spread-spectrum:** distributes the signal across the carrier.

Steganography has legitimate uses (watermarking, protecting people at risk) as well as abusive ones (covert data transfer, command-and-control).

**Steganalysis uses layered checks (slides):** file properties (size, format, metadata), statistical patterns, visual or audio artefacts, and context and behaviour. Simple checks can miss careful embedding, and comparing with a known original helps but an original is often unavailable.

### Shannon entropy [Important]

**H(X) = −Σ p(x) log₂ p(x)** measures uncertainty.

| Case | Entropy |
|---|---|
| Value always known | 0 bits |
| Fair binary choice | 1 bit |
| Byte with 256 equally likely values | 8 bits (maximum) |

- High entropy may mean compressed, encrypted, packed or random data. It is a **clue, not a verdict**.
- For keys and tokens, use the platform's **CSPRNG**. A high entropy score does not prove a generator is cryptographically unpredictable.

### NTFS alternate data streams (ADS) [Important]

- An NTFS file has a default stream and may have named streams, such as `report.csv:review`.
- Basic listings and programs that read only the default stream may miss them.
- A normal hash covers the **default stream only**.
- A stream does not run itself. A process must read or act on it.
- `Zone.Identifier` is a legitimate example: Windows may use it to record where a download came from.
- Presence alone is **not suspicious**.
- ADS came from NTFS support for Macintosh **resource forks**. Microsoft **Sysinternals Streams** also lists them.
- Abuse cases: hide data beside an innocent file, store tools or payloads, or form one step of a persistence chain.

```powershell
Get-Item -LiteralPath .\sample.txt -Stream *            # list streams
Get-Content -LiteralPath .\sample.txt -Stream review    # read one stream
```

**Steganography vs ADS:** steganography hides data *inside the carrier's content*. ADS attaches a *separate named stream* to a file.

---

## 4. OWASP Top 10:2025 [Must know]

**Memory trick for the order: "All My Students Can Improve During Autumn If Learning's Easy."**

| # | Category | In plain words | Named example | Key defence |
|---|---|---|---|---|
| **A01** | **A**ccess Control (Broken) | Reach resources without permission | First American: change a document number | Server checks user, resource and action on every request |
| **A02** | **M**isconfiguration | Settings expose too much | Toyota cloud configuration; default admin password | Compare against a secure baseline after deployment |
| **A03** | **S**upply Chain Failures | Risk from suppliers, builds, updates | SolarWinds | Inventory of components and versions; verify sources |
| **A04** | **C**ryptographic Failures | Data poorly protected | LinkedIn: unsalted SHA-1 | Password-specific hash; enforce HTTPS |
| **A05** | **I**njection | Input changes the meaning of a query, command or page | MOVEit SQL injection; XSS in comments | Parameterised SQL; context-aware output encoding |
| **A06** | Insecure **D**esign | A security rule is missing from the design | Ticket bots; weak account recovery | Model misuse cases; test business rules |
| **A07** | **A**uthentication Failures | Identity not reliably established or kept | 23andMe credential stuffing | MFA, throttling, session expiry |
| **A08** | **I**ntegrity Failures | Software or data accepted without checking | Codecov: hash mismatch | Verify origin and integrity before use |
| **A09** | **L**ogging and Alerting Failures | Events not recorded, noticed or acted on | Equifax: expired certificate blocked traffic inspection | Actionable alert with an owner and response path |
| **A10** | **E**xceptional Conditions | Errors cause unsafe behaviour | MFA fail-open; payment debited but not credited | Deny unsafe access, roll back, log and alert |

The lecture uses the named incidents as illustrations. OWASP has not necessarily classified them under these categories. Categories can also overlap.

**Don't confuse these:**
- **A01 vs A07:** A07 asks "who are you?" A01 asks "what may you access?" A valid login doesn't justify access to everything.
- **A03 vs A08:** A03 is the broad supplier and build chain. A08 asks whether *this* artifact or data object is authentic and unchanged.
- **A06 vs a coding bug:** A06 means the rule was never designed. A coding defect means a correctly designed rule was built wrongly.
- **Hashing vs encryption:** hash passwords with a password-specific scheme. Encrypt data you need to recover later.
- **Parameters vs output encoding:** they protect different interpreters. One does not replace the other.

---

## 5. Cybersecurity signals

### Observe → Interpret → Test [Must know]

1. **Observe:** cite the exact file, event, account, path, source and UTC time.
2. **Interpret:** state a hypothesis and why the combination is unusual.
3. **Test:** check an independent source, such as process, file, endpoint or network evidence.

**Lecture example (fictional):** three failed network logons (10:14–10:16) → success for the same account and source (10:18) → new startup entry (10:21) → new USB device (10:24).
This warrants investigation. It does **not** prove malware execution or data loss. Consider benign explanations.

**Three questions for any file (slides):** Identity (what is it?), Integrity (has it changed?), Activity (what happened when?).

### File checks: each answers a different question [Important]

| Check | What it tells you | Limit |
|---|---|---|
| **Extension** | The type the file claims to be | Can be changed |
| **Signature** | Whether the first bytes match | A DOCX is a ZIP, so a ZIP signature doesn't prove a valid document |
| **Parser** | Whether the structure is valid for the claimed format | Answers only the structure question; apply policy checks too |
| **SHA-256** | Whether the bytes match a trusted baseline | Doesn't say who, when or why; the baseline must be trustworthy |

**File-intake decision:**
- Signature matches → continue with parsing and policy checks.
- Signature conflicts → hold, reject or quarantine under policy, and record the reason.
- Signature unknown → keep it as unknown and inspect further. It is **not** proof of malware.

### Windows evidence [Important]

| Item | Meaning |
|---|---|
| **Event 4624** | Successful logon |
| **Event 4625** | Failed logon |
| **Logon type 2** | Interactive, at the computer |
| **Logon type 3** | Network access |
| **Event 4688** | Process creation, only if the audit policy is enabled; command-line capture is a separate setting |
| **Run keys** | Can start programs at sign-in; used by legitimate software and malware. Persistence technique **MITRE ATT&CK T1547.001** |
| **USBSTOR** | Storage device was seen; does **not** prove a file was copied |
| **HKCU / HKLM** | Current user / the computer |

- **Where to look:** the **Security log** for authentication and audit events, the **System log** for operating-system and service events. For each entry, record the log name, provider, event ID, timestamp and fields such as account, source and logon type. Check the XML details when the summary is unclear.
- **Registry values need a baseline:** a current value doesn't show when it first appeared, so compare with a trusted earlier snapshot before calling it "new". Recent-document entries and typed paths can be leads about user activity.
- **USB evidence:** phones may appear under different device classes. `SetupAPI.dev.log` adds device-installation context, but the installation time isn't the time of every later connection.
- Don't change audit settings on shared lab machines just to look for past evidence.

### Building a timeline and application logs [Important]

- **Keep both times:** the source time exactly as recorded with its UTC offset, *and* the converted UTC. Example: **08:40 +01:00 = 07:40 UTC**.
- **Normalise before sorting** records from different systems.
- **Not the event time:** a ZIP extraction time, a file's display time or a local clock setting is not automatically when the event happened.
- **For each observation,** note the source system and any clock uncertainty.
- **Good application logs include:** an unambiguous time, the action, outcome and resource, and a correlation or request ID.
- **Never log:** passwords, session tokens or unnecessary personal data. A rejected upload can record the decision and reason without storing the file's secret or the user's token.

---

## 6. NTFS security analysis

### Key terms [Important]

| Term | Meaning |
|---|---|
| **MFT record** | Identifies a file and holds its attributes; commonly 1,024 bytes |
| **File reference, e.g. `4128:7`** | Record number 4128, sequence number 7. Sequence separates a new file from an old one when a record is reused |
| **Resident data** | Stored inside the MFT record |
| **Nonresident data** | Stored in clusters elsewhere; data runs map to them |
| **VCN / LCN** | Position in the file / position on the volume |
| **Slack** | Allocated space after the file's logical end; not part of the current content |

**Storage hierarchy:** disk image → partition → volume → directory entry → MFT record → attributes and streams

A **rename keeps the same reference** but changes the name or parent directory.

**Attribute type codes (slides):** `0x10` $STANDARD_INFORMATION, `0x30` $FILE_NAME, `0x80` $DATA.

**USN rename records:** the journal writes a `RENAME_OLD_NAME` record and a `RENAME_NEW_NAME` record with the same file reference. Reason flags can be combined.

### Timestamps: SI vs FN [Important]

- NTFS records creation, last write, MFT-record change and access times. Last-access may be delayed or disabled.
- **$STANDARD_INFORMATION (SI):** common timestamps; some can be set through ordinary Windows APIs.
- **$FILE_NAME (FN):** a second set, stored with the filename and parent reference; may update differently.
- **If SI and FN disagree:** investigate. It could be copy or restore, application behaviour or deliberate alteration. A mismatch alone proves **none** of these.
- **Lecture example (synthetic record 4128:7):** SI creation 2026-09-01 08:00:00 UTC, FN creation 2026-09-28 09:14:00 UTC, SI last-write 09:14:02 UTC. This is an inconsistency to investigate, not proof of tampering. Compare change records and application logs, and test copy, rename or restore behaviour before naming a cause.

### Evidence sources [Important]

| Source | Question it answers | Limit |
|---|---|---|
| **$MFT** | What records, names and allocation state exist? | Unused records may keep attributes without content |
| **$UsnJrnl:$J** | What changes were recorded? | Limited retention; doesn't identify the process or person |
| **$LogFile** | What supports file-system consistency and recovery? | Not a permanent user-action history |
| **Application / process logs** | Which operation may explain a change? | Coverage, identity and clock reliability must be assessed |

Use USNs to order records **within one journal**.

### Deletion [Useful]

Deletion guarantees **neither recovery nor erasure**. You might get a complete file, metadata only, fragments or nothing. Reuse, TRIM and encryption all reduce recovery. Report partial recovery as partial.

### The missing-file case [Important]

The application writes `C:\Reports\report.csv`; twenty minutes later it is missing. Was it renamed, deleted, replaced or removed by expected cleanup?

| Record | What it supports |
|---|---|
| USN 984320: `RENAME_OLD_NAME`, `report.csv`, reference 4128:7 | The old side of a rename |
| USN 984416: `RENAME_NEW_NAME`, `cache.tmp`, same reference 4128:7 | The new side: the same file object was renamed |
| A later unused MFT record for 4128:7 | The file was deleted |
| App log: `export.complete`, request r-204, account `report-service` | Context for the export, but a service account isn't the human requester |

None of these records shows **who** or **which process** did it, or **why**. Check job details, cleanup settings, process telemetry, clock settings and where each record came from.

**Workflow:** Inspect (browse the volume, for example in Autopsy) → Compare (metadata, content and application logs) → Explain (what's supported, what's uncertain).

### Automated review and application design [Useful]

- **A metadata script should:**
  - validate fields and formats, and record the parser version and settings
  - keep original timestamps and make UTC conversions explicit
  - correlate by **volume + full file reference**, and sort within a journal by USN
  - flag rename pairs, deleted-file candidates and timestamp differences, while keeping the source rows and showing parsing errors
- **Lessons for application design:**
  - set retention and cleanup rules for exports and temporary files
  - check what copy, export and scanning tools do with named streams and retained deleted data
  - log job IDs, operations, paths, outcomes and clear times, without secrets or unnecessary file content

### File systems compared [Useful]

| Feature | FAT32 | exFAT | NTFS |
|---|---|---|---|
| Allocation | Cluster chains in the FAT | Bitmap; chains may also be used | Attributes and data runs in MFT records |
| Windows ACLs | No | No | **Yes** |
| Named streams (ADS) | No | No | **Yes** |
| Journal / change log | No | No (ordinary exFAT) | **Supported** (when present and retained) |

**FAT32 chain example (slides):** the directory entry gives the first cluster (5); the FAT links 5 → 9 → 12 → END. The file is read in chain order even when the clusters are far apart.

On FAT32 deletion, the directory entry is marked **0xE5** and the cluster chain can be released. Without the chain, you may not know that cluster 9 followed cluster 5.

**Tools:** Autopsy (browse a volume), MFTECmd (parse `$MFT` and `$J` to CSV).

---

## 7. Information disclosure and safe failure

Information leaks through five routes: **responses, error messages, login behaviour, source files and logs**.

**Memory trick: "Real Engineers Log Secrets Loosely"** (Responses, Errors, Login, Source, Logs).

The Week 5 lab practises most of this chapter.

### Terms [Important]

| Term | Meaning |
|---|---|
| **Information disclosure (CWE-200)** | Sensitive information reaches someone who shouldn't receive it. A broad class |
| **Excessive data exposure** | An API returns more properties than the client needs. Earlier OWASP API Top 10 name |
| **BOPLA (OWASP API3:2023)** | Broken Object Property Level Authorization: reading *or changing* individual fields without permission |
| **Error-message exposure (CWE-209)** | An error response reveals details about users, data or the environment |

### Response design [Must know]

- **Input validation** asks "is what came in acceptable?" **Response design** asks "what should go back, to whom, and in what form?" A valid request from a correctly signed-in user can still leak data.
- **The response path:** Data store → Application → Representation (JSON, HTML, file) → Delivery (headers, caches, client).
- **The page shows only part; the client receives everything.** Check what was actually sent: Developer Tools → Network → Fetch/XHR → Response.
- **Example:** a profile page needs `displayName` and `course`, but an overbroad response also returns `email`, `studentNumber`, `accountStatus` and `recoveryToken`. A recovery token is credential material and never belongs in a profile response.
- **Object-level vs property-level:** "may you see this record?" vs "which fields of it may you read or change?" Ownership can be checked correctly while a serializer still leaks an internal field.
- **Three questions per field: Need, Audience, Permission.** Select the fields to send (a response contract). Sending everything and removing known secrets is harder to maintain, because a new internal field becomes public by accident. Hiding a field in the front end enforces nothing.
- **Other channels leak too:** headers, downloads, exports, generated documents and caches. Use `Cache-Control: no-store` for sensitive responses.
- **Disclosure vs output encoding:** disclosure is *what* is sent; encoding is *how* untrusted text is safely displayed. Fixing one doesn't fix the other.

### Safe errors [Important]

| Public response | Restricted diagnostic record |
|---|---|
| The operation failed, plus a safe next step | Event, time and technical context |
| An opaque support reference | The same reference, to link the records |
| No stack trace, query text or secrets | No passwords, tokens or unnecessary personal data |

Example: "We could not complete that request. Reference: R7Q2." Also check whether the failure left an unintended change behind.

### Username enumeration [Must know]

- "Password is incorrect" vs "No account with that address" reveals which accounts exist, **even with the same HTTP status**.
- **Timing can leak too.** With identical messages, an unknown username may be rejected at once while a real one runs the costly password check. A *repeatable* delay is a clue; a single slow response proves nothing.
- **Fix:** use an established login flow that does comparable work for both failures, limit repeated attempts and monitor abuse. Changing only the message isn't enough.
- An account list helps phishing and password attacks. It doesn't prove any account was compromised.

### Secrets and Git [Must know]

- An **API key** is a credential a program presents to another service.
- Keep keys out of shared source and browser files. Supply them through protected server configuration or a secret store, give each key only the permissions it needs, and support replacing it. If a required key is missing, **stop safely**, with no fallback. Moving a key out of source doesn't secure its new location.
- **Git keeps history.** A key committed on Monday and deleted on Tuesday is still in Monday's commit. `.gitignore` only affects untracked files; it doesn't erase history.
- **An exposed `.git` folder** on a web server can let anyone rebuild the source *and its history*, including removed secrets. Deploy only built public files and deny Git paths. Turning off directory listings doesn't help, because the filenames are known. No public GitHub repository is needed for this to happen.

**A leaked secret: four steps**

1. **Contain** access to the exposed files.
2. **Revoke or rotate** the credential and invalidate the old value.
3. **Investigate** its use and preserve evidence.
4. **Clean** history and other copies under a coordinated plan.

**Memory trick: "Can Robbers Inspect Code?"** Revoking disables a value; rotating replaces it and retires the old one. Cleaning history can't recall copies already downloaded.

### Logs and failing closed [Important]

- **Log selected fields, not whole forms:** `audit({ event: "sign_in_failed", requestId, outcome: "denied" })`.
- Record failed sign-ins, refused access and permission changes. Leave out passwords, tokens and complete submitted forms, and restrict who can read logs.
- If secrets were already logged, deal with the stored copies and the exposed credentials too.
- **If permission can't be checked, stop.** An unavailable permission service hasn't granted permission, so refuse the export. Decide *before* releasing data, because an error shown afterwards can't take it back.

---

## 8. Malware and ransomware

### Basics [Useful]

- A computer can be malware's **target** (files encrypted, data stolen), its **tool** (sending spam, relaying attacks) or **both** (a hijacked account fooling its contacts). The owner may notice nothing.
- **Crimeware roles:** developer, access seller, operator or affiliate, infrastructure provider. **Ransomware-as-a-service** supplies tools to affiliates. Identifying the malware family doesn't identify the person or group.

### Malware types [Must know]

| Type | What defines it | Key point |
|---|---|---|
| **Virus** | Copies its code into a host program or file; runs when the host runs | Replication and payload (the damage) are separate |
| **Worm** | Spreads between systems without a host | Doesn't need a zero-day. Widespread damage doesn't prove it's a worm |
| **Trojan** | Looks useful or legitimate but hides malicious behaviour | Often staged: deleting the installer doesn't remove later parts |
| **Backdoor** | Provides unauthorised access | Not the same as a rootkit |
| **Rootkit** | Alters or conceals the system to hide activity | The system may report false information; compare with trusted sources or examine offline |
| **Ransomware** | Restricts access to data or systems, then demands payment | Defined by its purpose |

The labels describe different properties: virus and worm describe **replication**, Trojan describes **presentation**, ransomware describes **purpose**. They overlap: a Trojan can deliver ransomware.

**Memory trick: "A virus needs a host, a worm walks alone, a Trojan wears a disguise."**

**Polymorphic vs metamorphic:** polymorphic malware changes its *wrapper* (varying encryption and decoder); metamorphic malware *rewrites its own code* while keeping the behaviour. Either way, an exact hash catches only one version.

### Delivery [Important]

| Document mechanism | Control |
|---|---|
| Active content such as macros | Restrict execution and trusted sources |
| Exploit against the document reader | Patch and isolate the application |
| Link to another resource | Check the destination and requested action |

- **Social engineering:** impersonation, a believable pretext or a fake login page. Verify through a route you already know, such as the normal web address or a known phone number, never one in the message. Good spelling and branding prove nothing.
- **Scareware** fakes a security scan to get payment or installation; the "results" are just claims. Ransomware really does block access.

### Detection [Must know]

- **Detection signature:** a pattern linked to malware. An exact hash identifies one known sample; broader signatures match related samples.
- **Digital signature:** shows who signed software and that it hasn't changed. **It does not prove the software is harmless.**
- **Heuristics** apply rules about suspicious traits; **behavioural detection** watches activity. Examples: a program launching a scripting tool, many documents rewritten quickly, a process reading credentials, new persistence settings. Context matters, because backup software reads many files legitimately.

| Analysis | How | Limit |
|---|---|---|
| **Static** | Inspect without running: structure, strings, imports, signatures | Packing or obfuscation can hide content |
| **Dynamic** | Run it and watch processes, file changes and connections | Only sees behaviour triggered in that environment and time; malware may detect analysis |

A quiet test does not prove a file is harmless.

### Ransomware [Must know]

- A **ransom demand doesn't prove** how much was encrypted or stolen, or that recovery would work.
- **Paying** doesn't rebuild services, remove the attacker's access or prove stolen data was deleted.
- **Renaming the extension back** doesn't decrypt anything, because the contents changed.
- Files on **writable shared drives** can be encrypted without every device being infected.

| Activity | CIA impact |
|---|---|
| Encrypting live data | **Availability** |
| Copying data | **Confidentiality** |
| Altering records | **Integrity** |

**Double extortion** combines encryption with a threat to publish stolen data. Theft can also happen without encryption. **A backup restores availability, not confidentiality.**

### Recovery [Important]

- **Deleting the malicious file** removes only that file. The attacker may still have credentials, sessions or a second component. Investigate the entry route, affected accounts and later activity.
- **A useful backup** is accessible, recent enough and protected from changes. A finished backup *job* is not proof; a successful *restore test* is. A **synchronised copy** can copy the damage, so keep version history.

| Control | Purpose |
|---|---|
| Patching and reduced exposure | Remove known routes in |
| Restricted execution and least privilege | Limit what code can run and change |
| Segmentation and separate administration | Limit reach across systems |
| Monitoring with response cover | Spot and act on suspicious activity |
| Protected, tested backups | Recover data and services |

**Forensics:** link the suspect file to its parent and child processes, accounts, network destinations and persistence changes. A file, a connection and a ransom note are separate observations; none tells the whole story.

---

## 9. Authentication and sessions

### Three checks [Must know]

| Point in the visit | Question | Check |
|---|---|---|
| Login | Is this valid evidence for the claimed account? | **Authentication** |
| Next request | Does it carry a valid session? | **Session** |
| Results page | May this account read these results? | **Authorisation** |

An **IP address is not identity**: people share addresses, and one person's address can change. A password match lets authentication continue; it doesn't authorise access to every record.

### Storing passwords [Must know]

If someone copies the account table:

| Stored value | Result |
|---|---|
| Original password | Readable immediately |
| Encrypted password | Recoverable with the key |
| Password-hash record | Guesses can be tested, but the original isn't stored |

| Operation | What happens | Example use |
|---|---|---|
| **Encryption** | A key recovers the original | A stored document |
| **Hashing** | Produces a digest; no decryption step | Checking a file hasn't changed |
| **Password hashing** | Salted, deliberately costly calculation | Checking a submitted password |

HTTPS protects the password in transit; password hashing protects the stored copy.

### Adobe, 2013 [Important]

- Troy Hunt's analysis: **152,989,508 records**, of which **109,305,888** had a password hint (about 109.3 million).
- **Equal passwords gave equal encrypted values** (no salt): **3,011 accounts** shared one value with the hint "color". Cracking it once would expose all 3,011.
- **Hints were clues:** of the **top 100 hint strings**, 24 were family names. That is 24 of 100 strings, **not 24% of accounts**.
- Cross-breach pairs (Adobe hint vs Sony password): "fruit" → grapefruit, "name birthyear" → mike1953.
- **Lessons:** use salted, costly password hashing, and use a reset flow instead of storing hints.

### Save and verify [Important]

- **Save:** the password arrives over HTTPS → the library makes a fresh random salt → computes the hash with its cost settings → stores hash, salt and settings. Never the password, not even in logs.
- **Verify:** load the saved record → recompute with the *saved* salt and settings → compare. Nothing is decrypted. In the Python library the function is `verify()`; Argon2id is the algorithm.

### Argon2id [Must know]

| SHA-256 | Argon2id |
|---|---|
| Built to be fast | Built for password hashing |
| Good for file-integrity checks | Adjustable time and memory cost |
| Each password guess is cheap | Each guess is deliberately expensive |

- **A salt stops equal-hash and precomputed shortcuts; it does not add cost.** Don't build your own loop around a fast hash.
- **The "id":** Argon2i uses data-independent memory access (resists side channels); Argon2d uses data-dependent access (resists memory-saving attacks); **Argon2id** is a hybrid, using i-style for the first half of the first pass and d-style after.

| Setting | Meaning | Lecture example |
|---|---|---|
| **m** | Total working memory per calculation | 65,536 KiB = 64 MiB |
| **t** | Passes over that memory | 3 |
| **p** | Parallel lanes in one calculation | 4 (16 MiB each) |

The example is the RFC 9106 low-memory profile. **The server pays the cost too**, so benchmark concurrent logins and limit work in flight.

**Stored record:** `$argon2id$v=19$m=65536,t=3,p=4$<salt>$<hash>`
- `v=19` is the version, decimal 19 = `0x13`, **not the year 2019**.
- Salt and hash are Base64-encoded; Base64 is encoding, not encryption.
- Store the whole record, because verification needs the algorithm, salt and settings.

---

## 10. Cookies, sessions and access control

### From password to session [Must know]

1. The user signs in over HTTPS.
2. The server verifies the password and creates a session record.
3. `Set-Cookie` gives the browser an opaque, unpredictable session ID.
4. Later requests carry the ID in the `Cookie` header, not the password.
5. The server validates the session, then checks permission for the record, action and fields.

```
Set-Cookie: __Host-session=<id>; Path=/; Secure; HttpOnly; SameSite=Lax; Max-Age=1800
Cookie: __Host-session=<id>
```

The later `Cookie` header sends only name and value; the attributes tell the browser how to handle it. **A session ID is a credential:** keep it out of URLs, screenshots and logs, and renew it when login or privilege changes.

### Access control [Must know]

Cedar Home shop (fictional): Lina owns order 7201, Omar owns 7202.

| Requester | Read 7201 | Read 7202 | Issue refund |
|---|---|---|---|
| Lina | Allow | Deny | Deny |
| Omar | Deny | Allow | Deny |
| Support staff | Allow | Allow | Allow |
| No valid session | Deny | Deny | Deny |

- **An ID says which object; it doesn't say who may access it.** If Lina changes `/api/orders/7201` to `7202`, the server must refuse. Input validation can confirm 7202 is well formed, not that Lina may read it.
- **Ownership check:** `account.id === purchase.ownerId || account.role === "support"`, with both records from trusted server state.
  - Comparing with `purchase.id` is wrong: that's the order number, not the owner.
  - Using `&&` instead of `||` would require being owner *and* support.
  - `Boolean(account && purchase)` only proves both records exist.
- **Refund check:** `account.role === "support"`, enforced before anything changes. A denied request must leave the order and financial records unchanged. **Hiding the refund button doesn't stop a direct API request.**

| Failure | Name | Direction |
|---|---|---|
| Reading another customer's order | **IDOR / BOLA** (OWASP API1) | **Horizontal** |
| Performing a support-only action | **Broken function-level authorisation** (OWASP API5) | **Vertical** |

**First American, 2019:** changing a number in an EaglePro URL exposed unrelated documents. The SEC reported **over 800 million images** exposed (findings June 2021). That counts images, not people, and doesn't prove all were downloaded. Random IDs reduce guessing but don't replace a permission check.

### Cookie attributes [Must know]

| Attribute | What it does |
|---|---|
| **Secure** | Sent only over HTTPS |
| **HttpOnly** | Page JavaScript can't read it, but the browser still sends it. **Doesn't stop script injection** |
| **SameSite** | Controls sending in cross-site requests |
| **Max-Age=1800** | Browser keeps it for 1,800 seconds = 30 minutes |
| **Path=/** | Valid for all paths on the host; grants no access to records |
| **`__Host-` prefix** | Requires Secure, Path=/ and no Domain, so it stays on the host that set it |

Cookie settings never replace server-side access checks.

### SameSite [Must know]

| Request | Strict | Lax | None; Secure |
|---|---|---|---|
| Same-site request | Send | Send | Send |
| Another site → normal link (top-level GET) | Omit | **Send** | Send |
| Another site → form POST | Omit | Omit | Eligible* |
| Shop inside another site's iframe | Omit | Omit | Eligible* |

\*Browser third-party-cookie restrictions may still block it. `None` requires `Secure`.

**Site vs origin:**
- **Origin** = scheme + host + port.
- **Site** = scheme + registrable domain.
- `https://shop.cedar.example` and `https://help.cedar.example` are **same-site but different origins**. Changing only the port changes the origin; switching HTTPS to HTTP changes the site.

**Choosing:**
- **Lax** for an ordinary shop, because incoming links keep the session.
- **Strict** for tighter isolation, after testing external links and sign-in returns.
- **None; Secure** only for genuine cross-site use.

### CSRF [Important]

**Cross-site request forgery:** another page makes the browser send a request that is authenticated by an existing cookie, though the user didn't intend it. For example, a hidden form makes a signed-in support user issue a refund; the role check passes anyway.

**Defences:**
- Use the framework's CSRF protection (validated tokens) and origin checks.
- Keep state changes off GET.
- Treat SameSite as an extra layer only.

CORS is not a universal CSRF defence, and cookie settings can't fix a missing ownership check.

---

## 11. Labs that support the listed topics

Week 5 supports information disclosure, Week 2 supports data hiding, Week 6 supports authentication and sessions, and the CTFs support information disclosure and access control.

### Week 5: "What goes out" [Must know]

**The rule behind every fix:** say as little as possible to the outside, and keep the detail where only your team can read it.

| Leak | CWE | Fix | Remember |
|---|---|---|---|
| Error page shows stack trace, file paths, database path, Node.js version | **CWE-209** | Generic message + reference ID; detail goes to the log | The bug still exists; the fix limits what it reveals |
| Different login messages and status codes (404 vs 401) reveal which emails have accounts: **user enumeration** | **CWE-204** | Same status and same message for both failures | The right password must still log in |
| Payment key and session secret written in source code | **CWE-798** | Read from the environment (`process.env`); if missing, stop safely | A default value in the code is still a secret in the code |
| Passwords stored as typed | **CWE-256** | **scrypt** with a **random salt per password**; compare in constant time | Damaged stored values return false; don't encrypt or use Base64 |
| Passwords and card numbers in the log | **CWE-532** | Choose the safe fields to keep, rather than the ones to remove | Hiding everything is no fix: the log must still show what happened |
| No audit trail for security events | **CWE-778** | Fixed fields: **event, user, result, time**; no secrets | Record the events that matter, not every page view |
| Check says yes when it hits an error: **fail open** | **CWE-636** | If the check can't decide, refuse and log a warning | Staff must still get in |

**Password trio:**
- **Hash:** one-way. It can check a password but can't give it back.
- **Salt:** random per password, so equal passwords look different.
- **Slow function:** makes guessing expensive.

**Retesting a fix:** run that exercise's tests, repeat the real attack, *and* check that a normal request still works.

### Week 2: Hidden data [Important]

The lab's message: a quick visual check can miss information.

| Method | What you saw | Control for a secure program |
|---|---|---|
| **Alternate data streams** | `dir` shows nothing unusual; `dir /r` shows `welcome.txt:secret.txt:$DATA`. A ZIP can't preserve ADS | Enumerate named streams |
| **PDF containers** | Pages look the same but file sizes differ. Data can sit after the final `%%EOF`, or be a genuine attachment | Inspect the full container, including attachments |
| **Image steganography** | You can't tell which image carries a file by looking. The result is saved as PNG so the hidden bits stay intact. Compare SHA-256 to prove full recovery | Validate stored lengths and paths |
| **Shannon entropy** | Measured from 0 to 8 bits per byte. The extension alone doesn't predict the score, and a DOCX is a ZIP-based container | Interpret with context |

**Entropy blocks:** a block is a consecutive chunk of file bytes, not a disk cluster. Block scores can locate a change that a whole-file score hides.

### Week 6: Riverton Live (passwords, sessions, access control) [Important]

The lab version of chapters 9 and 10.

| Exercise | What you saw | The lesson |
|---|---|---|
| **1. Argon2id** | The same password hashed twice gives different salts and hashes; the same password, salt and costs give the same hash | Verification reuses the stored salt and costs; nothing is decrypted |
| **2. Network** | `GET /api/catalogue → 200` returns `doorsOpen: "19:30"`, which the page never shows | An extra field is an observation; the access policy decides whether it's private |
| **3. Login** | Wrong password → `401`, right → `200`. `Set-Cookie` carries a random session ID with `HttpOnly` and `SameSite=Lax` | The cookie holds a session ID, never the password. The lab omits `Secure` only because it runs on local HTTP |
| **4. Ownership (IDOR)** | Ada could read Ben's booking 1042 | Fix: user and order exist, and (`user.id === order.userId` or role is `staff`). After: 1041 → 200, 1042 → **403**; staff → 200; no session → **401** |
| **5. Refunds (function-level)** | Ada could `POST /api/orders/1041/refund` with no refund button visible | Fix: only role `staff`. After: Ada → **403** and the booking stays paid; staff → 200, then **409** for a duplicate |
| **6. Logout and expiry** | After sign-out, a saved session ID still got `200` | Fix: **revoke on the server** (`session.revoked = true`) → `401` after logout. An expired session gets `401` even if the cookie is still there |

- **Lab Argon2id profile:** the OWASP minimum, `m=19456` (19 MiB), `t=2`, `p=1`, a 16-byte salt and a 32-byte hash. The lecture example (64 MiB, t=3, p=4) is the RFC 9106 low-memory profile. Both are documented choices; measure on the real server.
- **Retest tip:** reset data before retesting a denied refund. Otherwise a duplicate-refund `409` could hide whether the permission check works.
- **Status codes:** `200` OK, `401` no valid session, `403` permission refused, `409` conflict (e.g. second refund).

### CTFs: Nightfall Games and Harbour Dispatch [Useful]

Both are short "inspect → explain → verify" challenges as an **ordinary user**, with no password guessing or brute force.

- **Nightfall Games (Week 6, 15 minutes):** find an unreleased beta code in HTTP responses the page doesn't display, then redeem it. The lessons: data hidden on screen can still be in the response, and a valid player session is not permission for a restricted resource.
- **Harbour Dispatch (20 minutes):** follow a chain of references across responses to an internal depot record. The detail route and the **export/download route** behave differently, so every endpoint needs its own ownership check. Topics: information disclosure and object-level authorisation.

---

## Calculations [Must know]

**File: 9,400 bytes. Cluster size: 4,096 bytes. Runs: (start 120, length 2) and (start 900, length 1).**

| Question | Working | Answer |
|---|---|---|
| Clusters needed? | 9,400 ÷ 4,096 = 2.3, round **up** | **3 clusters** |
| Space allocated? | 3 × 4,096 | **12,288 bytes** |
| Slack? | 12,288 − 9,400 | **2,888 bytes** |
| Read order of clusters? | Follow the runs | **120, 121, 900** |
| Where is byte 8,500? | 8,500 − (2 × 4,096) = 308, in the third cluster | **LCN 900, offset 308** |
| Volume byte offset? | (900 × 4,096) + 308 | **3,686,708** |

If working from a whole-disk image, also **add the volume's start offset**.

**Optional (notes): decoding the run list `11 02 78 21 01 0C 03 00`**

| Bytes | Meaning |
|---|---|
| `11` | Header: 1 byte for the run length, 1 byte for the offset |
| `02 78` | Run of 2 clusters starting at LCN 0x78 = **120** |
| `21` | Header: 1 byte for the length, 2 bytes for the offset |
| `01 0C 03` | Run of 1 cluster; little-endian offset delta 0x030C = 780, so 120 + 780 = LCN **900** |
| `00` | End of the run list |

Real run lists can use signed deltas and more bytes; this example excludes sparse and compressed runs.

**Time zones:** subtract a positive offset to get UTC. 08:40 +01:00 → **07:40 UTC**.

**LSB capacity (Week 2 lab): a 3840 × 2560 image at 2 bits per colour channel.**

| Step | Working | Answer |
|---|---|---|
| Pixels | 3,840 × 2,560 | 9,830,400 |
| Bits | Pixels × 3 channels × 2 bits | 58,982,400 |
| Bytes | Bits ÷ 8 | **7,372,800** (the lab gives 7,372,771 after overhead) |

**Argon2id memory (m=65536, p=4):**

| Question | Working | Answer |
|---|---|---|
| Memory per verification? | 65,536 KiB ÷ 1,024 | **64 MiB** |
| Memory per lane? | 64 MiB ÷ 4 lanes | **16 MiB** |
| 20 simultaneous verifications? | 20 × 64 MiB = 1,280 MiB; ÷ 1,024 | **1.25 GiB** |

This is working memory during the check, not the size of the stored record.

**Cookie lifetime:** Max-Age=1800 → 1,800 ÷ 60 = **30 minutes**.

**Argon2 version:** `v=19` → decimal 19 = hex **0x13** (not a year).

---

## Exam traps: "X does not prove Y" [Must know]

| This evidence… | …does NOT prove |
|---|---|
| High entropy | Hidden data or malware |
| An alternate data stream exists | Malicious activity |
| Unknown file signature | Malware |
| Matching signature | The file is valid or safe |
| SHA-256 mismatch | Who changed it, when or why |
| SI and FN timestamps disagree | Tampering |
| USBSTOR record | A file was copied or stolen |
| Missing event log entry | The action never happened (could be audit settings, retention or permissions) |
| New Run key value | Malware (and a current value doesn't show when it first appeared) |
| USN journal record | Which person or process did it |
| Service account in a log | Which human requested it |
| Looks normal to the eye | The image is safe or unchanged |
| Deleted file | Either erasure or recoverability |
| Likely motive | Who the attacker was |
| Two PDFs with identical pages | The files contain the same data |
| A browser check blocks a value | The server would refuse it |
| Same message for both failed logins | No enumeration (timing can still leak) |
| Secret deleted from code, or file added to `.gitignore` | The secret is gone or no longer works |
| Random, hard-to-guess IDs | Access control is in place |
| Refund button hidden from customers | Customers can't issue refunds |
| `HttpOnly` cookie | Protection from script injection |
| Password matches | The user may access every record |
| Same IP address | Same person |
| Widespread disruption | The malware is a worm |
| Software is digitally signed | It is harmless |
| Quiet during a dynamic test | The file is harmless |
| Exact hash doesn't match known malware | The file isn't a variant |
| Malware family identified | Who carried out the attack |
| Malicious file deleted | The attacker has lost access |
| Ransom demand | How much was encrypted or stolen |
| Paying the ransom | Services restored or stolen data deleted |
| Backup restored | Stolen data recovered |
| Backup job completed | The service can be restored |
| Adobe "Family name: 24" | 24% of accounts (it's 24 of the top 100 hint strings) |
| 800 million exposed images (First American) | 800 million people, or that all were downloaded |
| Browser shows "signed out" | The server revoked the session |
| Session cookie still present | The session is still valid (the server checks expiry) |
| The detail page checks ownership | The export or download route does too |

**When a question asks "what can you conclude?", state what the record shows, then what it cannot show, then what you would check next.**

---

## Pairs people mix up

| Pair | The difference |
|---|---|
| **Authentication vs authorisation** | Who are you? vs what may you do? |
| **Threat vs vulnerability** | What could cause harm vs the weakness it exploits |
| **Attack surface vs vector** | Where vs how |
| **Steganography vs cryptography** | Hides that it exists vs hides what it says |
| **Steganography vs ADS** | Inside the carrier's content vs a named stream attached to the file |
| **Hashing vs encryption** | One-way check vs recoverable with a key |
| **Signature vs parser** | First bytes vs whole structure |
| **Resident vs nonresident** | Inside the MFT record vs in clusters elsewhere |
| **VCN vs LCN** | Position in the file vs position on the volume |
| **$UsnJrnl vs $LogFile** | Change records vs file-system recovery |
| **Logging vs alerting** | Recorded vs noticed and acted on |
| **Client-side vs server-side check** | Convenience the user controls vs actually enforced |
| **Allowlist vs blocklist** | Define what is valid vs list what is bad |
| **Fail closed vs fail open** | Refuse when unsure vs allow when unsure |
| **Application log vs audit log** | What happened in the app vs fixed security fields: event, user, result, time |
| **HTTP 201 / 400 / 401 / 403 / 404 / 500** | Created / bad request / unauthorised / forbidden / not found / server error |
| **Input validation vs response design** | Is what came in acceptable? vs what should go out, and to whom? |
| **Object-level vs property-level authorisation** | May you see this record? vs which fields of it? |
| **BOLA/IDOR (API1) vs BOPLA (API3) vs function-level (API5)** | Wrong record vs wrong field vs wrong action |
| **Horizontal vs vertical access failure** | Another user's data vs a higher role's action |
| **Information disclosure vs output encoding** | What is sent vs how it is safely displayed |
| **Revoke vs rotate** | Disable a value vs replace it and retire the old one |
| **Virus vs worm vs Trojan** | Needs a host vs spreads alone vs wears a disguise |
| **Backdoor vs rootkit** | Gives access vs hides activity |
| **Polymorphic vs metamorphic** | Wrapper changes vs code rewritten |
| **Detection signature vs digital signature** | Malware pattern vs publisher identity and integrity |
| **Heuristic vs behavioural detection** | Rules about suspicious traits vs watching activity |
| **Static vs dynamic analysis** | Inspect without running vs run and observe |
| **Backup job vs restore test** | A copy was made vs proof the service comes back |
| **Sync copy vs backup history** | Mirrors the damage vs keeps older versions |
| **Hash vs password hash** | Fast digest vs salted, deliberately costly verifier |
| **Salt vs cost** | Stops equal-hash shortcuts vs makes each guess expensive |
| **Argon2i vs Argon2d vs Argon2id** | Data-independent (side channels) vs data-dependent (memory trade-offs) vs hybrid |
| **Set-Cookie vs Cookie** | Server sets value and attributes vs browser sends name and value only |
| **Site vs origin** | Scheme + registrable domain vs scheme + host + port |
| **SameSite Strict vs Lax vs None** | Never cross-site vs top-level links only vs cross-site allowed (needs Secure) |
| **Browser logout vs server-side revocation** | Clears your cookie vs makes the session ID stop working |
| **401 vs 403 vs 409** | No valid session vs permission refused vs state conflict |

---

## Self-test

Cover the Answer column and answer out loud, then check.

| # | Question | Answer |
|---|---|---|
| 1 | Name the CIA objectives and one control for each. | Confidentiality (access control, encryption); Integrity (hashes, signatures, audit trails); Availability (backups, redundancy). |
| 2 | Define threat, vulnerability, impact and risk. | Threat: what could cause harm. Vulnerability: the weakness. Impact: the harm. Risk: likelihood and impact together. |
| 3 | Why did the HSE antivirus detection on 31 March not stop the attack? | It was in monitoring mode, so it did not block the activity, and third-party monitoring was not continuous overnight. |
| 4 | What are the Four Questions? | What are we working on? What can go wrong? What are we going to do about it? Did we do a good enough job? |
| 5 | Expand STRIDE and give the property for each letter. | Spoofing–authentication; Tampering–integrity; Repudiation–accountability; Information disclosure–confidentiality; Denial of service–availability; Elevation of privilege–authorisation. |
| 6 | What does STRIDE *not* tell you? | Likelihood, impact or priority. |
| 7 | Name the four threat responses. | Eliminate, mitigate, transfer, accept. |
| 8 | Write a specific threat for an API that lets users change an account ID. | For example: "An authenticated user changes the account ID in an update request and modifies another customer's record, because the server doesn't check ownership." |
| 9 | List OWASP A01–A10 in order. | Broken Access Control, Security Misconfiguration, Software Supply Chain Failures, Cryptographic Failures, Injection, Insecure Design, Authentication Failures, Software or Data Integrity Failures, Security Logging and Alerting Failures, Mishandling of Exceptional Conditions. |
| 10 | A user changes their profile URL and sees another user's profile. Which category, and what is the fix? | A01 Broken Access Control. Check permissions on the server for each object and action. |
| 11 | A session token still works after logout. Which category? | A07 Authentication Failures. |
| 12 | What is the difference between steganography and cryptography? | Steganography hides that a message exists. Cryptography makes its contents unreadable without the key. |
| 13 | Does a normal hash of a file cover its named streams? | No, only the default stream's bytes. |
| 14 | What is the maximum entropy of a byte? | 8 bits. |
| 15 | What are events 4624 and 4625, and logon types 2 and 3? | 4624 is a successful logon and 4625 a failed logon. Type 2 is interactive at the computer and type 3 is network access. |
| 16 | A file has 10,000 bytes on 4,096-byte clusters. How many clusters, and how much slack? | 3 clusters (12,288 bytes), so 12,288 − 10,000 = **2,288 bytes** of slack. |
| 17 | Convert 14:30 +02:00 to UTC. | **12:30 UTC.** |
| 18 | What does `4128:7` mean? | MFT record number 4128, sequence number 7. |
| 19 | Three failed logons, then a success, then a new USB device. What can you conclude? | It warrants investigation, but it does not prove compromise or data theft. Record the exact entries, consider benign explanations and check independent evidence such as process, file and network records. |
| 20 | In one sentence, why is client-side validation not a security control? | The user controls the browser and can change or skip its checks, so only a check on the server is enforced. |
| 21 | What should a customer see when the server fails, and where does the detail go? | A generic message and a reference ID. The stack trace and other detail go to the log, under the same reference. |
| 22 | What is user enumeration, and how do you stop it? | Different login responses let a stranger work out which emails have accounts. Use the same status and the same message for an unknown email and a wrong password. |
| 23 | What are a hash and a salt for? | A hash lets you check a password without being able to get it back. A random salt makes equal passwords look different. |
| 24 | Which fields should an audit entry have, and what must it never hold? | Event, user, result and time. Never a password, card number or other secret. |
| 25 | A role lookup throws an error. What should the access check do? | Fail closed: refuse access and log a warning so the fault gets fixed. |
| 26 | Why must a steganography output stay PNG in the lab? | So the hidden bits remain intact. The lab states this, and lossy formats and re-saving can damage a payload. |
| 27 | What are the four layers of the response path? | Data store, application, representation, delivery. |
| 28 | A profile page shows only name and course. Is the response safe? | Not necessarily. The client receives the whole response, so check it in Developer Tools (Network → Fetch/XHR → Response) and exclude extra fields, like a recovery token, on the server. |
| 29 | Which three questions decide whether a field goes in a response? | Need, audience, permission. |
| 30 | Both failed logins show the same message. How can accounts still be enumerated? | Timing: a real account runs the costly password check, an unknown one is rejected at once. Do comparable work for both and limit attempts. |
| 31 | A developer deletes a committed API key and adds the file to `.gitignore`. Is the key safe? | No. Git history still holds it and the key still works. Contain, revoke or rotate, investigate, clean history. |
| 32 | The permission service is down during an export. What should happen? | Refuse the export (fail closed) and record the problem. |
| 33 | What separates a virus, a worm and a Trojan? | A virus attaches to a host file; a worm spreads between systems on its own; a Trojan disguises itself as something useful. |
| 34 | Why can't one exact hash catch polymorphic malware? | Each variant's bytes differ, so a hash identifies only one version. |
| 35 | Does a digital signature prove software is harmless? | No. It shows who signed it and that it hasn't changed since. |
| 36 | A sample did nothing in a dynamic test. Is it harmless? | No. Dynamic analysis only sees behaviour triggered then, and malware may detect analysis or wait for a condition. |
| 37 | Map ransomware activities to CIA. | Encrypting data: availability. Copying data: confidentiality. Altering records: integrity. |
| 38 | Why doesn't restoring a backup end a double-extortion incident? | It restores availability but can't recover copied data, and the attacker's remaining access must still be removed. |
| 39 | What three checks happen when a student opens their results? | Authentication at login, a valid session on the next request, and authorisation to read these results. |
| 40 | Why is plain SHA-256 a poor password hash? | It's fast, so each guess is cheap. Argon2id adds memory and time cost; a salt alone adds no cost. |
| 41 | What do m, t and p mean in Argon2id? | Memory per calculation (64 MiB), passes over it (3), parallel lanes (4). |
| 42 | How much memory do 20 simultaneous verifications need at 64 MiB each? | 1,280 MiB = 1.25 GiB. |
| 43 | Lina changes `/api/orders/7201` to `7202`. What should the server do, and what is the failure called? | Refuse, by checking `account.id === purchase.ownerId`. The failure is IDOR/BOLA (OWASP API1), a horizontal access failure. |
| 44 | What do Secure, HttpOnly, SameSite and Max-Age do? | HTTPS only; page JavaScript can't read it; controls cross-site sending; retention time in seconds. |
| 45 | With SameSite=Lax, is the cookie sent on a cross-site form POST? On a normal link? | POST: no. Top-level link (GET): yes. |
| 46 | Are `https://shop.cedar.example` and `https://help.cedar.example` same-site? Same-origin? | Same-site, but different origins. |
| 47 | What is CSRF, and how do you defend against it? | Another page makes the browser send an authenticated request the user didn't intend. Use CSRF tokens and origin checks, keep changes off GET, and treat SameSite as an extra layer. |
| 48 | After sign-out, a saved session ID still works. What's missing? | Server-side revocation. Logout must mark the session revoked on the server, so the ID gets `401`. |
| 49 | Why reset data before retesting that a customer can't issue a refund? | A duplicate-refund `409` could hide whether the permission check works. Test against a paid, refundable order. |
| 50 | A detail page refuses another customer's record, but its export route returns it. What's the lesson? | Every endpoint, including downloads and exports, needs its own ownership check on the server. |
| 51 | How does the lab's Argon2id profile differ from the lecture example? | Lab: OWASP minimum, 19 MiB, t=2, p=1. Lecture: RFC 9106 low-memory profile, 64 MiB, t=3, p=4. |

---

# Appendix: beyond the listed scope

Mike's list doesn't include these, so revise them only after everything above. OWASP A05 (Injection) is in scope, so its table row in section 4 still matters. They may still matter for the final assessment or the project.

---

## A1. SQL injection (Week 7 lecture)

### How it happens

- **SQL injection (CWE-89):** external input changes the *structure* of a SQL query. The database runs whatever it receives, with the application's permissions.
- **The mistake:** inserting request input straight into the SQL text, e.g. `` `... AND surname LIKE '%${term}%'` ``. Calling `prepare()` *after* that still prepares the altered query.
- **Apostrophes:** searching `O'Neill` closes the string early and causes a syntax error for a valid name. A quote error is a clue, not proof. A generic error page doesn't prove the query is safe, and banning apostrophes breaks real names without fixing anything.

**Worked example: search for `' OR 1=1 --`**

```sql
SELECT id, surname FROM employees
WHERE department_id = 10
AND surname LIKE '%' OR 1=1 -- %'
```

1. `'` **ends the string.**
2. `OR 1=1` **adds an always-true condition.**
3. `--` **comments out the rest** (the leftover `%'`).

**AND is evaluated before OR**, so it reads as (department matches AND name matches) OR TRUE. All 5 staff rows come back, including 2 from another department. The department filter is bypassed without being deleted. Adding parentheses alone doesn't fix it.

**Memory trick: "Quote, OR, Comment."**

### Forms of SQL injection

| Form | How it works | Note |
|---|---|---|
| **Visible results** | Changed filters return extra rows; `UNION` can add another query's results | `UNION` needs matching column counts and compatible types |
| **Blind** | Differences in page content or response time reveal whether a condition is true | One slow response isn't proof. Hiding errors doesn't remove the flaw |
| **Second-order** | A stored value is later inserted unsafely into another query | Data read from your own database isn't automatically safe |

Possible effects: unauthorised access, changed records and authentication bypass. The impact depends on the query, the database and its permissions.

### Real incidents

| Incident | Chain | Detail |
|---|---|---|
| **Heartland** (2009 disclosure) | SQL injection → malware → card theft | The "130 million cards" figure covers the wider alleged campaign, not Heartland alone |
| **TalkTalk** (2015) | SQL injection → database access → customer data | **156,959 customers**. Legacy webpages, outdated database software and missed earlier attacks. ICO fine **£400,000** (2016) |
| **MOVEit Transfer** (2023) | SQL injection → web shell → data theft | CL0P exploited **CVE-2023-34362** and installed the **LEMURLOOT** web shell. Third-party software needs patching too |

### The fix: parameter binding

```javascript
const statement = db.prepare(`
  SELECT id, surname FROM employees
  WHERE department_id = ?
  AND surname LIKE ?
`);
const rows = statement.all(departmentId, '%' + term + '%');
```

- **Fixed SQL template + values bound separately.** The `?` placeholders are not inside quotes, and the driver treats quotes and `OR` as data.
- `'%' + term + '%'` is fine, because it builds a *data value*, not SQL text.
- **Binding is not authorisation.** Take the department from the authenticated user's permitted scope. If a user sends department 20 and the server binds it without checking, injection is prevented but access control is still broken.
- **LIKE wildcards still work in bound values:** `%` matches any run of characters, `_` matches one. Searching `%` returning all 3 *permitted* rows may be intended. For literal matching, escape them with SQL `ESCAPE`.
- **Placeholders are only for values.** For column names (`ORDER BY`), use an allowlist that maps each option to a fixed name, such as `name` → `surname`, and reject anything else. Treat ASC and DESC the same way.
- ORM raw queries and stored procedures that build dynamic SQL can still be vulnerable.

### Supporting controls

| Control | Purpose |
|---|---|
| Query construction | Bind values; fixed options for query structure |
| Permissions | Least privilege for the database account. SQLite: file permissions and a read-only connection |
| Operations | Patch, don't expose SQL errors, monitor unusual access |
| Input rules | Validate type, length and allowed choices, while still accepting valid names and punctuation |

**None of these replaces parameter binding.** A web application firewall adds filtering but doesn't repair query construction, and browser-side checks can be bypassed.

### Testing the fix

| Search (as a department 10 user) | Expected |
|---|---|
| `Patel` | The matching authorised employee |
| `O'Neill` | Works, no SQL error |
| `NobodyHere` | Empty result, normal response |
| `' OR 1=1 --` | Treated as search text; no extra access |
| `%` or empty | All 3 permitted rows (under this search policy) |
| Request department 20 | Denied or restricted by the server |

Check the returned record IDs and departments, not just the status code: HTTP 200 says nothing about who owns the data.

---

## A2. OS command injection (lab)

### How it happens

- **OS command injection (CWE-78):** customer input changes **shell syntax**, so extra operating-system commands run. It's not an AI prompt attack: the commands really execute, as the server's account.
- **The lab:** a WHOIS lookup (Northline Domains) and a ping tool (Relay Network). The Python app builds one string, such as `ping -n -c 2 -W 1 -- {value}`, and runs it with `shell=True`.
- **Proof:** add `; id` after a normal value. The output now includes the account running the command (a low-privilege `labuser`).

| Linux shell symbol | Effect |
|---|---|
| `;` | Runs the next command whatever happens |
| `&&` | Runs the next command only if the first succeeds |
| `&` | Runs the first command in the background |

Windows uses different separators.

**Impact shown in the lab:**
- **Confidentiality (WHOIS):** reading a staff-only archive that the public lookup should never reveal.
- **Integrity (ping):** overwriting the public homepage headline with `printf` and `>`. The change survives a reload because it's on the server. Editing the page in Developer Tools only changes *your* browser's copy, so it proves nothing.

**Trace the path:** HTTP request → Python app → **shell parser** → operating-system program. The shell parser is where data becomes instructions. Hiding the error or filtering in the browser doesn't fix it.

### The fix

- **Don't use a shell.** Pass an argument list, with a fixed program and fixed options, and the user's value as **one** argument. The shape, as the lab asks for: `["ping", "-n", "-c", "2", "-W", "1", "--", value]` with `shell=False`.
- **Keep `--`**, so the value can't be read as an option starting with `-`. That is **option injection**.
- **Validate** the value for the tool, such as a hostname or IPv4 address. Validation helps, but avoiding the shell is the structural fix. Quoting is shell-specific and unnecessary once no shell is involved.
- **Run as non-root** (the lab also uses a read-only filesystem, dropped capabilities, resource limits and no internet). This **reduces impact but doesn't fix injection**.
- Command execution through a web page is not the same as an interactive terminal on your computer.

**Same idea as SQL injection:** keep data separate from instructions. In SQL, bind parameters; at the shell, pass an argument list without a shell.

**A good finding report:** target and scope, the exact request, relevant output, the source-level cause, the impact, the repair diff and the retest results. Collect only the personal data you need.

---

## A3. Labs outside the listed topics

### Week 4: "The server decides" (what comes in)

**One-sentence answer:** client-side validation is not a security control, because the user can change or skip anything in the browser.

**The lab's order form has two separate browser checks:** the HTML `max="2"` attribute and a JavaScript check. Removing one does not remove the other.

**Ways to get past them:**

| Method | What it does |
|---|---|
| Developer tools | Delete `max`, then disable JavaScript |
| Console, `form.submit()` | Sends the form without running the page's checks |
| `curl` | Sends the request directly, with no browser at all |
| `fetch` in the console | Sends the same request from any page of the app |
| Intercepting proxy (Burp, ZAP) | Catches the request and changes it before forwarding |

**The three fixes the lab builds up to:**

1. **Add the limit on the server.** A `quantity > 2` check stops 20 tickets, but 0, negative and fractional values still pass. Always state what a fix does *not* cover.
2. **Use an allowlist.** Define exactly what is valid (digits only, 1 to 4, sent once) and refuse everything else. Blocking bad values one by one is less dependable.
3. **Let the server work out the money.** The customer chooses only the ticket and quantity. Price, discount and total come from the server's own records, because hidden fields can be edited too. In the lab, a total edited to €0.02 was charged as €0.02.

**Odd inputs the original server accepted:**

| Sent | Saved or treated as | Why it matters |
|---|---|---|
| `-3` | −3 places, total −€36, places left go up | Negative value changes stock and money |
| `0` | 0 places | Outside the rules |
| `1.5` | 1 | Input silently changed |
| `2abc` | 2 | `parseInt` reads digits from the start and stops at the first letter |
| `1e3` | 1 | The record no longer shows what was sent |
| `quantity=3&quantity=99` | 3 | A field can arrive twice, though a browser never sends that |

**Legal point:** testing a system you don't own, or have no written permission to test, is an offence under the **Criminal Justice (Offences Relating to Information Systems) Act 2017**.

### Week 7: SQL injection lab

| Site | Attack | What happened | Fix |
|---|---|---|---|
| **Aster Observatory** search | `' OR 1=1 -- ` | 8 results, including 2 **unreleased** ones: the `published = 1` filter was bypassed. `' AND 1=2 -- ` gave none, proving real SQL logic | `WHERE published = 1 AND title LIKE ?`, binding `%term%`. Don't quote `?`; keep the published filter |
| **Bracket Supply** login | Username `admin' -- ` | Logged in as the administrator with **any password**: the comment removed the `password_matches(...)` condition. The hash was never cracked | Bind **both** username and password. Usernames with real apostrophes then work too |
| **Field notes** (extension) | `' OR 1=1 -- ` | Showed other members' private notes | `WHERE member_id = ? AND title LIKE ?`, binding both values; don't hard-code the member |

Password hashing and access checks don't make string-built SQL safe. Binding doesn't replace the published, member or role filters either.

---

## A4. Extra calculation

**Group discount (Week 4 lab):** 4 × €45 = €180, less 10% = **€162**.

## A5. Extra exam traps

| This evidence… | …does NOT prove |
|---|---|
| SQL error after typing a quote | An exploitable injection |
| Generic error page | The query is safe |
| `prepare()` was called | The query is safe (not if input was inserted first) |
| All SQL values are bound | Access control is correct |
| HTTP 200 from a search | The right records were returned |
| A web application firewall is in place | Query construction is fixed |
| Running the command as a non-root account | No command injection |
| Editing the page in Developer Tools | The server's data changed |

## A6. Extra pairs

| Pair | The difference |
|---|---|
| **Parameter binding vs authorisation** | How a value reaches SQL vs whether the user may see those rows |
| **Bind it vs allowlist it** | Data values vs parts of the query structure (column names, ASC/DESC) |
| **Visible vs blind vs second-order SQL injection** | Extra rows shown vs inferred from differences vs stored value used later |
| **Shell command string vs argument list** | Parsed by a shell (injectable) vs passed straight to the program |
| **Command injection vs option injection** | Adds a new command vs turns the value into a `-` option |

## A7. Extra self-test

| # | Question | Answer |
|---|---|---|
| A1 | A user deletes `max` in developer tools but leaves JavaScript on, then submits 20. What happens? | The JavaScript check still runs and blocks it, because it is separate from `max`. |
| A2 | Why does `parseInt("2abc", 10)` give 2? | `parseInt` reads digits from the start and stops at the first letter. |
| A3 | Which values should the server work out itself at checkout? | Price, discount and total. The customer chooses only the ticket and quantity. |
| A4 | What is SQL injection? | External input changes the structure of a SQL query (CWE-89). |
| A5 | Why does searching `O'Neill` break the unsafe query? | The apostrophe closes the SQL string early, causing a syntax error. |
| A6 | Explain how `' OR 1=1 --` returns every row. | The quote ends the string, `OR 1=1` adds an always-true condition, and `--` comments out the rest. AND is evaluated before OR, so the WHERE clause becomes (A AND B) OR TRUE. |
| A7 | Input is inserted into a SQL template string and then `prepare()` is called. Is it safe? | No. Preparation doesn't undo the changed structure. Use placeholders and bind the values. |
| A8 | All values are bound, but the department filter is missing. Can other departments' rows come back? | Yes. Binding stops injection, not access-control mistakes. The server must restrict results to the user's permitted scope. |
| A9 | A parameterised search for `%` returns every row in your department. Why? | `%` is a LIKE wildcard even when bound. That may be intended; use `ESCAPE` if literal matching is required. |
| A10 | How do you safely let users choose a sort column? | Map each allowed option to a fixed column name and reject anything else. Placeholders can't stand in for column names. |
| A11 | What is second-order SQL injection? | A stored value is later inserted unsafely into another query. |
| A12 | Give the attack chain for Heartland, TalkTalk and MOVEit. | Heartland: SQL injection → malware → card theft. TalkTalk: SQL injection → database access → data on 156,959 customers. MOVEit: SQL injection (CVE-2023-34362) → LEMURLOOT web shell → data theft. |
| A13 | How does blind SQL injection get information? | From differences in page content or response time that show whether a condition is true. |
| A14 | What is OS command injection, and what is its CWE? | Input changes shell syntax so extra operating-system commands run. CWE-78. |
| A15 | What does adding `; id` to a ping value prove? | The shell ran a second command, and `id` shows which account it ran as. |
| A16 | What's the difference between `;`, `&&` and `&` in a Linux shell? | `;` runs the next command regardless; `&&` runs it only if the first succeeds; `&` backgrounds the first. |
| A17 | How do you repair command injection? | Don't use a shell. Pass a fixed program and options plus the value as one argument (`shell=False`), keep `--`, and validate the value. |
| A18 | Why keep `--` in the argument list? | So a value starting with `-` can't become a command-line option (option injection). |
| A19 | Does running the tool as a non-root account fix command injection? | No. It reduces the impact, but the injection is still there. |
| A20 | Why doesn't editing the headline in Developer Tools prove the same issue? | It only changes your browser's copy. The injection changed the file on the server, so the change survives a reload for everyone. |
| A21 | How does the username `admin' -- ` log in without a password? | The quote ends the username string and `--` comments out the password check, so the query finds the admin without checking a password. |
