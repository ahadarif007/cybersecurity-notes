# CSSP Revision Notes: Cybersecurity and Secure Programming

*An exam-focused guide to the module's core topics: security foundations, threat modelling, data hiding, the OWASP Top 10, digital evidence, NTFS analysis, information disclosure, malware, authentication, sessions and access control. It's built for multiple-choice exams, with memory tricks, exam traps and practice questions.*

> **About these notes.** They summarise the module's lecture notes, slides and labs. The priorities and memory tricks are my own, not the lecturer's. Real organisations in the examples (HSE, Adobe, First American, TalkTalk and others) are incidents discussed in lectures. Names such as Cedar Home, Riverton Live, Lina, Omar, Ada and Ben belong to fictional lab scenarios.

**Priority key**

| Tag | Meaning |
|---|---|
| ★★★ | **Must know.** Learn it by heart |
| ★★ | **Important.** Understand it well enough to spot it among the options |
| ★ | **Useful context.** Read it once |

---

## Contents

**Start here**
- [How to use these notes](#how-to-use-these-notes)
- [MCQ strategy](#mcq-strategy)
- [The 7 big ideas](#the-7-big-ideas)

**Core topics**
1. [Cybersecurity foundations](#1-cybersecurity-foundations)
2. [Threat modelling](#2-threat-modelling)
3. [Data hiding](#3-data-hiding)
4. [OWASP Top 10 (2025)](#4-owasp-top-10-2025)
5. [Security signals and evidence](#5-security-signals-and-evidence)
6. [NTFS security analysis](#6-ntfs-security-analysis)
7. [Information disclosure and safe failure](#7-information-disclosure-and-safe-failure)
8. [Malware and ransomware](#8-malware-and-ransomware)
9. [Authentication and password storage](#9-authentication-and-password-storage)
10. [Sessions, cookies and access control](#10-sessions-cookies-and-access-control)

**Apply and review**

11. [Lab walkthroughs](#11-lab-walkthroughs)
12. [Calculations](#12-calculations)
13. [Final review sheet](#13-final-review-sheet)
14. [Practice MCQs](#14-practice-mcqs)

**Appendix: beyond the core scope**
- [A. SQL injection](#appendix-a-sql-injection)
- [B. OS command injection](#appendix-b-os-command-injection)
- [C. Week 4 and Week 7 labs](#appendix-c-week-4-and-week-7-labs)
- [D. Practice MCQs](#appendix-d-practice-mcqs)

---

## How to use these notes

**Scope.** Sections 1–10 cover the core assessed topics, and section 11 covers the labs that support them. The appendix covers SQL injection, OS command injection and the Week 4 and Week 7 labs. Those were outside the listed scope of the first assessment, but they matter for the final assessment and the project. OWASP A05 (Injection) is part of the core, so learn that row of the OWASP table either way.

**A 90-minute plan**

| Step | Time | What to do |
|---|---|---|
| 1 | 10 min | Read the [MCQ strategy](#mcq-strategy) and [the 7 big ideas](#the-7-big-ideas). They decide most "tricky" options |
| 2 | 40 min | Work through sections 1–10. Learn every ★★★ item, and read each chapter's **⚠️ Exam traps** box twice |
| 3 | 10 min | Do the [calculations](#12-calculations) without looking at the answers |
| 4 | 25 min | Do the [practice MCQs](#14-practice-mcqs). Reread the section behind every wrong answer |
| 5 | 5 min | On the morning of the exam, read only the [final review sheet](#13-final-review-sheet) |

---

## MCQ strategy

This module tests **precision**. Most wrong options are *almost* right: they overclaim, swap two similar terms, or offer a fix that only hides the problem.

| When an option… | Think… |
|---|---|
| says **proves, guarantees, always, never, completely, eliminates** | Usually false. In this module evidence is a *clue* and most controls *reduce* risk. Look for the exception |
| hides something (a button, an error, a field on screen, a deleted key) | Hiding is not fixing. The data or the capability is still there |
| reduces impact (non-root account, random IDs, WAF, SameSite, firewall) | A useful *extra layer*, but it doesn't fix the root cause |
| puts the check in the browser | Not enforced. **The server decides** |
| swaps a paired term (authentication/authorisation, Strict/Lax, 401/403) | Check the exact word. See [Pairs people mix up](#pairs-people-mix-up) |
| is a *first step* in incident handling | **Contain** and **revoke** before you clean up or rewrite history |

**Multiple-answer questions ("select all that apply")**
1. Treat every option as its own true/false statement.
2. An option that is *partly* wrong is wrong.
3. Don't assume a fixed number of correct answers. It can be one, or all of them.
4. Be wary of options that use the right keyword with the wrong claim, such as "HttpOnly *prevents XSS*".

**For "what can you conclude?" questions,** pick the option that says what the record **shows**, not what it might **mean**. The best option often adds "warrants investigation" or "check an independent source".

---

## The 7 big ideas

These ideas come up again and again in the module. Most exam traps are one of them in disguise.

1. **A clue is not proof.** A record shows what happened on a system, not who did it or why.
2. **Security is a relationship, not a product:** what needs protecting, what threatens it, and what happens if protection fails.
3. **Any single control can fail,** so use layers: Prevent → Detect and respond → Recover.
4. **Be specific.** A good threat names the actor, action, target and impact. "Broken access control" on its own is not a threat statement.
5. **Keep similar things separate.** Many questions test the difference between two close terms, such as authentication and authorisation.
6. **The server decides.** The user controls the browser, so only server-side checks are enforced. Tell outsiders as little as possible, and keep the detail where only your team can read it.
7. **Removing or hiding is not fixing.** A deleted key still works and stays in Git history. A deleted malware file can leave the attacker's access behind. A hidden button doesn't stop a direct request.

---

# Core topics

## 1. Cybersecurity foundations

### CIA triad ★★★

| Objective | Question it asks | Example controls |
|---|---|---|
| **Confidentiality** | Who can see it? | Access control, encryption |
| **Integrity** | Can it be trusted? | Hashes, digital signatures, audit trails |
| **Availability** | Is it there when it's needed? | Backups, redundancy, capacity planning |

- **The objectives can conflict.** Emergency records must be available fast, while therapy notes need tighter access. Choose based on the likely harm.
- **A control can backfire.** A strict password policy improves confidentiality on paper but weakens it in practice if people write passwords down or reuse them.

### Threat, vulnerability, impact, risk ★★★

Example: an admin API exposed to the internet with no login.

| Term | Question | In the example |
|---|---|---|
| **Threat** | What could cause harm? | Automated internet scanning |
| **Vulnerability** | What weakness makes it possible? | No authentication on the API |
| **Impact** | What harm results if it happens? | Private records read or changed |
| **Risk** | How much attention does it deserve? | Likelihood and impact together |

**Risk ≈ likelihood × impact.** Use it to compare and prioritise. It isn't an exact calculation, because both inputs are estimates.

### Attack surface vs attack vector ★★

- **Surface = where** contact is possible: interfaces, data entry points, identities and credentials, dependencies.
- **Vector = how** an attacker uses that contact.
- Example: the login form is part of the **surface**, and credential stuffing is the **vector**.

### HSE ransomware incident, 2021 ★★

| Date | Event |
|---|---|
| **18 March 2021** | A phishing email with a malicious attachment gives the attacker a foothold |
| **31 March** | Antivirus detects the activity but is in **monitoring mode**, so it blocks nothing. Third-party monitoring isn't continuous overnight |
| **14 May** | Ransomware causes major service disruption |

- **Scale:** data on about 500 patients was published online, and more than 100,000 people needed to be contacted by November 2022. The two figures measure different things at different stages.
- **All three CIA objectives were hit:** data disclosed (C), systems and accounts compromised (I), services down (A).
- **Lesson:** detection without response doesn't stop an attack, and an incident is a *sequence* of stages, not a single event.

### Threat actors ★

| Actor | Typical goal or behaviour |
|---|---|
| **Cybercriminals** | Money, through theft, fraud or extortion |
| **State-linked groups** | Intelligence, influence or disruption. Patient and persistent |
| **Insiders** | Already have trusted access. Can be malicious, coerced or simply **mistaken** |
| **Opportunists** | Scan widely and exploit whatever they find |

Motive is a **hypothesis**, not attribution. It shapes the **target**, the **tempo** and **what detection looks like**. Quick money means broad scanning and visible impact. Long-term spying means narrow targets, persistence and staying hidden.

### Defence in depth and zero trust ★★

**Defence in depth** assumes any single control can fail.

| Layer | Purpose | Examples |
|---|---|---|
| **Prevent** | Reduce exposure | Patching, strong authentication, least privilege |
| **Detect and respond** | Find and contain | Monitoring, alert review, incident response |
| **Recover** | Restore and learn | Tested backups, recovery plans, post-incident changes |

**Zero trust** is an architecture and a set of principles, **not a product**. Being inside the network earns no trust.
- Authenticate and authorise explicitly, for each session.
- Decide using identity, device and context.
- Grant only the access the task needs (least privilege).
- Keep re-checking, because trust can change.

### Secure programming habits ★

Validate input · Enforce authorisation on the server · Encode output for its destination · Handle errors safely · Use safe defaults · Manage dependencies · Log security events

> **⚠️ Exam traps**
> - Risk is **not** an exact number. Likelihood × impact is only for comparing.
> - Detection is **not** protection. HSE detected the activity and still suffered the attack.
> - A likely motive does **not** identify the attacker.
> - Zero trust is **not** something you buy, and being on the internal network grants nothing.

---

## 2. Threat modelling

Threat modelling is a structured way to understand a system, find plausible ways it could fail or be misused, and decide what to do about them. It pays off most **during design**, and it should be revisited whenever the system changes.

- **Three traits:** multidisciplinary, decision-focused, maintained.
- **Security across the lifecycle:** Requirements (set objectives) → Design (analyse threats, choose controls) → Implementation (apply and test) → Operation (monitor and reassess).
- **Why start early:** a widely cited estimate puts fixing a defect after delivery at up to **100 times** the cost of fixing it during requirements and design (Boehm and Basili, 2001). It's a rough guide, not a universal rule.

### Shostack's Four Questions ★★★

| # | Question | What you do |
|---|---|---|
| 1 | **What are we working on?** | Define the scope, users, data and boundaries |
| 2 | **What can go wrong?** | Trace data flows, mark trust boundaries, apply STRIDE |
| 3 | **What are we going to do about it?** | Choose a response and justify it |
| 4 | **Did we do a good enough job?** | Review coverage, assumptions and whether the controls work |

🧠 **"Work, Wrong, Do, Done."** It's a cycle, because design changes can introduce new threats.

### Building a system model ★★

1. **List the parts:** actors, processes, data stores and external systems. Include third-party services and dependencies, not just your own code.
2. **Describe each important data flow:** source, destination, data carried, identity involved and any **change in privilege**.
3. **Mark a trust boundary** wherever data crosses between components with different levels of trust or control. A browser sending a note to a web app crosses one.
4. **Record assumptions and limits,** so others know what the model covers and what it leaves out.

The output can be a diagram, a threat table or design notes. What matters is that every finding leads to a **documented decision**. A model is a map for asking questions, not a guarantee that every threat has been found.

### STRIDE ★★★

| Letter | Threat | Property violated | Example |
|---|---|---|---|
| **S** | Spoofing | **Authentication** | Reusing a stolen session token |
| **T** | Tampering | **Integrity** | Changing an account ID to edit someone else's record |
| **R** | Repudiation | **Accountability** (non-repudiation) | An admin deletes a record and no trustworthy audit event exists |
| **I** | Information disclosure | **Confidentiality** | Changing a URL reveals another user's record |
| **D** | Denial of service | **Availability** | Huge uploads fill the storage and lock users out |
| **E** | Elevation of privilege | **Authorisation** | Submitting `role=admin` to reach admin functions |

- STRIDE finds threat **types**. It gives **no** likelihood, impact or priority, so you assess those separately.
- **For denial of service, ask "what is finite?"** Time, memory, storage, connections, money and human attention.

### Writing a good threat ★★

**Who + action + target + consequence**

> "An unauthenticated attacker changes a record identifier in a request and reads another user's private record."

### Four responses to a threat ★★★

| Response | Meaning |
|---|---|
| **Eliminate** | Remove the feature, path or assumption that creates the threat |
| **Mitigate** | Reduce the likelihood or impact with controls |
| **Transfer** | Hand agreed parts of the risk to a provider or insurer. You still need oversight |
| **Accept** | Keep the risk knowingly, with a reason and a named owner |

🧠 **"Every Mistake Teaches Answers"** (Eliminate, Mitigate, Transfer, Accept).

For each threat, record the **decision, rationale, owner and residual risk**.

> **⚠️ Exam traps**
> - STRIDE does **not** rank or prioritise threats.
> - Repudiation maps to **accountability**, not integrity. Elevation of privilege maps to **authorisation**, not authentication.
> - Transfer does **not** remove responsibility, and Accept does **not** mean ignore. Acceptance needs a reason and an owner.
> - Threat modelling is **not** a one-off task. It's revisited whenever the system changes.

---

## 3. Data hiding

### Steganography vs cryptography ★★★

| | The question it answers | What it protects |
|---|---|---|
| **Cryptography** | "What does this message say?" | The **contents** |
| **Steganography** | "Is there a message here?" | The **existence** of the message |

- Steganography alone does **not** make content confidential. The two can be combined, but neither guarantees the message goes undetected.
- **The basic model:** message + carrier + embedding method → **stego object**.
- **Legitimate uses** include watermarking and protecting people at risk. **Abusive uses** include covert data transfer and command-and-control.

### Embedding techniques ★★

| Family | How it works |
|---|---|
| **Spatial (LSB)** | Changes low-order bits of pixel values |
| **Transform-domain** | Changes coefficients in a transformed version of the carrier, such as DCT or wavelet |
| **Spread-spectrum** | Spreads the signal thinly across the carrier |

**LSB examples** (the last bit becomes the message bit):
- 44 = `00101100` → embed **1** → `00101101` = **45**
- 101 = `01100101` → embed **0** → `01100100` = **100**

Re-saving, resizing or converting the image (for example to lossy JPEG) can destroy an LSB payload.

**Steganalysis uses layered checks:** file properties (size, format, metadata), statistical patterns, visual or audio artefacts, and context and behaviour. Simple checks can miss careful embedding. Comparing against a known original helps, but often there's no original to compare with.

### Shannon entropy ★★

**H(X) = −Σ p(x) log₂ p(x)** measures uncertainty.

| Case | Entropy |
|---|---|
| Value always the same | **0 bits** |
| Fair coin (two equally likely outcomes) | **1 bit** |
| Byte with all 256 values equally likely | **8 bits** (the maximum per byte) |

- **High entropy** can mean compressed, encrypted, packed or random data. It's a **clue, not a verdict**.
- A high entropy score does **not** prove a generator is cryptographically secure. For keys and tokens, use the platform's **CSPRNG**.

### NTFS alternate data streams (ADS) ★★

- An NTFS file has a default (unnamed) data stream and can also have **named streams**, such as `report.csv:review`.
- Basic listings, and programs that read only the default stream, may not show them.
- A normal file hash covers the **default stream only**.
- A stream doesn't run itself. A process has to read it or act on it.
- **`Zone.Identifier`** is a legitimate stream that Windows can use to record where a download came from. **The presence of an ADS is not suspicious in itself.**
- ADS exist because NTFS supported Macintosh **resource forks**. **Sysinternals Streams** can list them.
- **Abuse:** hiding data beside an innocent file, storing tools or payloads, or serving as one step in a persistence chain.

```powershell
Get-Item -LiteralPath .\sample.txt -Stream *            # list all streams
Get-Content -LiteralPath .\sample.txt -Stream review    # read one named stream
```

**Steganography vs ADS:** steganography hides data *inside the carrier's content*. An ADS attaches a *separate named stream* to the file.

> **⚠️ Exam traps**
> - Steganography hides **existence**, not meaning. It gives no confidentiality on its own.
> - High entropy does **not** prove hidden data, encryption or malware.
> - An ADS does **not** prove malicious activity, and a normal hash does **not** cover it.
> - An image that looks normal is **not** proven unchanged. Compare hashes.

---

## 4. OWASP Top 10 (2025)

### The list ★★★

🧠 **Order: "All My Students Can Improve During Autumn If Learning's Easy."**

| # | Category | In plain words | Lecture example | Key defence |
|---|---|---|---|---|
| **A01** | Broken **A**ccess Control | Reaching resources without permission | First American: change a document number in the URL | Server checks user, resource and action on **every** request |
| **A02** | Security **M**isconfiguration | Settings expose too much | Toyota cloud configuration; default admin password | Compare deployments against a secure baseline |
| **A03** | Software **S**upply Chain Failures | Risk from suppliers, builds and updates | SolarWinds | Inventory components and versions; verify sources |
| **A04** | **C**ryptographic Failures | Data poorly protected | LinkedIn: unsalted SHA-1 | Password-specific hashing; enforce HTTPS |
| **A05** | **I**njection | Input changes the meaning of a query, command or page | MOVEit SQL injection; XSS in comments | Parameterised SQL; context-aware output encoding |
| **A06** | Insecure **D**esign | A security rule is missing from the design | Ticket bots; weak account recovery | Model misuse cases; test business rules |
| **A07** | **A**uthentication Failures | Identity not reliably established or maintained | 23andMe credential stuffing | MFA, throttling, session expiry |
| **A08** | Software or Data **I**ntegrity Failures | Code or data accepted without verification | Codecov: hash mismatch | Verify origin and integrity before use |
| **A09** | Security **L**ogging and Alerting Failures | Events not recorded, noticed or acted on | Equifax: an expired certificate disabled traffic inspection | Actionable alerts with an owner and a response path |
| **A10** | Mishandling of **E**xceptional Conditions | Errors lead to unsafe behaviour | MFA fails open; payment debited but not credited | Fail closed, roll back, log and alert |

The lecture uses these incidents as illustrations. OWASP hasn't necessarily classified them this way, and categories can overlap.

### Telling neighbours apart ★★★

| Pair | How to tell them apart |
|---|---|
| **A01 vs A07** | A07: "Who are you?" A01: "What may you access?" A valid login doesn't justify access to everything |
| **A03 vs A08** | A03 is the whole supplier and build chain. A08 asks whether *this particular* artifact or data is authentic and unchanged |
| **A06 vs a coding bug** | A06: the rule was never designed. A bug: a correctly designed rule was built wrong |
| **A04: hashing vs encryption** | Hash passwords with a password-specific scheme. Encrypt data you need to read back later |
| **A05: parameters vs output encoding** | They protect different interpreters (the database vs the browser). Neither replaces the other |

**Quick classifier:**
- Session still valid after logout → **A07**
- Changing an ID shows someone else's data → **A01**
- Default credentials or an exposed admin console → **A02**
- Errors fail open → **A10**
- An attack happened but nobody was alerted → **A09**
- Tampered update or download accepted → **A08**

> **⚠️ Exam traps**
> - Authentication (A07) is **not** access control (A01).
> - Hashing is **not** encryption. You can't "decrypt" a hash.
> - Parameterised queries do **not** stop XSS, and output encoding does **not** stop SQL injection.
> - The A09 category is about **alerting** too, not just logging. A log nobody acts on fails.

---

## 5. Security signals and evidence

### Observe → Interpret → Test ★★★

| Step | What you do |
|---|---|
| 1. **Observe** | Cite the exact file, event, account, path, source and **UTC** time |
| 2. **Interpret** | State a hypothesis and why the combination is unusual |
| 3. **Test** | Check an **independent** source: process, file, endpoint or network evidence |

**Lecture example (fictional):** three failed network logons (10:14–10:16) → a successful logon for the same account and source (10:18) → a new startup entry (10:21) → a new USB device (10:24).
This **warrants investigation**. It does **not** prove malware ran or data was stolen, and benign explanations need ruling out.

**Three questions for any file:** **Identity** (what is it?), **Integrity** (has it changed?), **Activity** (what happened to it, and when?).

### File checks: each answers a different question ★★

| Check | What it tells you | Limit |
|---|---|---|
| **Extension** | The type the file *claims* to be | Anyone can change it |
| **Signature** (magic bytes) | Whether the first bytes match a format | A DOCX is a ZIP, so a ZIP signature doesn't prove a valid document |
| **Parser** | Whether the whole structure is valid for the format | Answers the structure question only, so apply policy checks too |
| **SHA-256** | Whether the bytes match a trusted baseline | Can't say who changed it, when or why. The baseline must be trustworthy |

**File intake decisions:**
- Signature **matches** → continue with parsing and policy checks.
- Signature **conflicts** with the extension → hold, reject or quarantine under policy, and record why.
- Signature **unknown** → record it as unknown and inspect further. Unknown is **not** proof of malware.

### Windows evidence ★★★

| Item | Meaning |
|---|---|
| **Event 4624** | Successful logon |
| **Event 4625** | Failed logon |
| **Logon type 2** | Interactive (at the keyboard) |
| **Logon type 3** | Network |
| **Event 4688** | Process creation. Logged only if that audit policy is enabled, and command-line capture is a separate setting |
| **Run keys** | Start programs at sign-in. Used by legitimate software and malware alike. Persistence technique **MITRE ATT&CK T1547.001** |
| **USBSTOR** | A USB storage device was seen. It does **not** prove a file was copied |
| **HKCU / HKLM** | Settings for the current user / for the whole machine |

- **Where to look:** the **Security log** holds authentication and audit events, and the **System log** holds operating-system and service events. For each entry, record the log name, provider, event ID, timestamp and key fields such as account, source and logon type. Read the XML view when the summary is unclear.
- **Registry values need a baseline.** A current value doesn't show when it first appeared, so compare it with a trusted earlier snapshot before calling it "new". Recent-document lists and typed paths are leads about user activity.
- **USB detail:** phones can appear under other device classes. `SetupAPI.dev.log` adds installation context, but the install time isn't the time of every later connection.
- Don't change audit settings on shared lab machines just to hunt for past evidence.

### Timelines and application logs ★★

- **Keep both times:** the original time with its UTC offset, *and* the converted UTC. Example: **08:40 +01:00 = 07:40 UTC**. To convert, subtract a positive offset.
- **Normalise to UTC before sorting** records from different systems.
- **These are not the event time:** a ZIP extraction time, a file's display time, or a local clock setting.
- Note each record's source system and any clock uncertainty.
- **A good application log entry has** an unambiguous time, the action, the outcome, the resource, and a correlation or request ID.
- **Never log** passwords, session tokens or unnecessary personal data. A rejected upload can record the decision and the reason without storing the file's secret or the user's token.

> **⚠️ Exam traps**
> - A suspicious sequence of events warrants investigation. It is **not** proof of compromise.
> - An unknown signature is **not** malware, and a matching signature is **not** proof the file is valid or safe.
> - A SHA-256 mismatch shows *that* the file changed, **not** who changed it, when or why.
> - A missing log entry does **not** prove nothing happened. Audit settings, retention or permissions may explain it.
> - A new Run key value is **not** proof of malware.
> - A USBSTOR record does **not** prove a file was copied.

---

## 6. NTFS security analysis

### Key terms ★★

| Term | Meaning |
|---|---|
| **MFT record** | Identifies a file and holds its attributes. Usually **1,024 bytes** |
| **File reference, e.g. `4128:7`** | MFT record **4128**, sequence number **7**. The sequence number goes up when a record is reused, so a new file isn't mistaken for an old one |
| **Resident data** | Small enough to be stored inside the MFT record itself |
| **Nonresident data** | Stored in clusters elsewhere, and located through **data runs** |
| **VCN / LCN** | Virtual cluster number (position **in the file**) / logical cluster number (position **on the volume**) |
| **Slack** | Allocated space after the file's logical end. Not part of the current content |

**Storage hierarchy:** disk image → partition → volume → directory entry → MFT record → attributes and streams

**Attribute type codes:** `0x10` = $STANDARD_INFORMATION · `0x30` = $FILE_NAME · `0x80` = $DATA

- A **rename** keeps the same file reference and changes only the name or parent directory.
- **USN rename records:** the journal writes a `RENAME_OLD_NAME` record and a `RENAME_NEW_NAME` record with the **same file reference**. Reason flags can be combined.

### Timestamps: SI vs FN ★★★

- NTFS records **creation, last write, MFT-record change and last access** times. Last-access updates may be delayed or disabled.
- **$STANDARD_INFORMATION (SI)** holds the common timestamps. Some can be set through ordinary Windows APIs.
- **$FILE_NAME (FN)** holds a second set, stored with the filename and parent reference. It may update differently.
- **If SI and FN disagree,** investigate. The cause could be a copy or restore, application behaviour or deliberate tampering. **A mismatch alone proves none of them.**

**Lecture example (synthetic record 4128:7):** SI creation 2026-09-01 08:00:00 UTC, FN creation 2026-09-28 09:14:00 UTC, SI last write 09:14:02 UTC. That's an inconsistency to investigate, not proof of tampering. Compare change records and application logs, and test copy, rename and restore behaviour before naming a cause.

### Evidence sources ★★★

| Source | Question it answers | Limit |
|---|---|---|
| **$MFT** | Which records, names and allocation states exist? | Unused records may keep attributes but not content |
| **$UsnJrnl:$J** (USN journal) | Which changes were recorded? | Limited retention. **Doesn't identify the process or person** |
| **$LogFile** | What supports file-system consistency and recovery? | Not a permanent history of user actions |
| **Application / process logs** | Which operation might explain a change? | Coverage, identity and clock reliability need checking |

USN numbers order records **within one journal** only.

### Deletion ★

Deletion guarantees **neither recovery nor erasure**. You might recover the whole file, metadata only, fragments or nothing. Cluster reuse, TRIM and encryption all reduce what can be recovered. Report partial recovery as partial.

### Case study: the missing file ★★

An application writes `C:\Reports\report.csv`, and twenty minutes later the file is gone. Was it renamed, deleted, replaced or removed by normal cleanup?

| Record | What it supports |
|---|---|
| USN 984320: `RENAME_OLD_NAME`, `report.csv`, ref 4128:7 | The old side of a rename |
| USN 984416: `RENAME_NEW_NAME`, `cache.tmp`, same ref 4128:7 | The new side: the **same file object** was renamed |
| A later unused MFT record for 4128:7 | The file was then deleted |
| App log: `export.complete`, request r-204, account `report-service` | Context for the export. A service account is **not** the human requester |

None of these records shows **who** did it, **which process** did it, or **why**. Check job details, cleanup settings, process telemetry, clock settings and where each record came from.

**Workflow:** **Inspect** (browse the volume, for example in Autopsy) → **Compare** (metadata, content, application logs) → **Explain** (what's supported and what's uncertain).

### Automated review and design lessons ★

- **A metadata-parsing script should:** validate fields and formats; record the parser version and settings; keep the original timestamps and make UTC conversions explicit; correlate by **volume + full file reference**; sort within a journal by USN; flag rename pairs, deleted-file candidates and timestamp differences; keep the source rows and show parsing errors.
- **Lessons for application design:** set retention and cleanup rules for exports and temporary files; check what copy, export and scanning tools do with named streams and deleted data; log job IDs, operations, paths, outcomes and clear times, without secrets.

### File systems compared ★★

| Feature | FAT32 | exFAT | NTFS |
|---|---|---|---|
| Allocation | Cluster chains in the FAT | Allocation bitmap (chains may also be used) | Attributes and data runs in MFT records |
| Windows ACLs | No | No | **Yes** |
| Named streams (ADS) | No | No | **Yes** |
| Journal / change log | No | No (standard exFAT) | **Yes** (if present and retained) |

- **FAT32 chain example:** the directory entry gives the first cluster (5), and the FAT links 5 → 9 → 12 → END. The file is read in chain order, even when the clusters are far apart.
- **FAT32 deletion:** the first byte of the directory entry becomes **0xE5** and the cluster chain can be released. Without the chain, you may not know that cluster 9 followed cluster 5.
- **Tools:** **Autopsy** browses a volume. **MFTECmd** parses `$MFT` and `$J` into CSV.

> **⚠️ Exam traps**
> - SI/FN disagreement is **not** proof of tampering.
> - A USN record does **not** say which person or process made the change.
> - A service account in a log is **not** the human who asked for the action.
> - A deleted file is **not** guaranteed to be either recoverable or erased.
> - VCN is the position **in the file**, and LCN is the position **on the volume**. Don't swap them.

---

## 7. Information disclosure and safe failure

Information leaks through five routes: **responses, error messages, login behaviour, source files and logs**.

🧠 **"Real Engineers Log Secrets Loosely"** (Responses, Errors, Login, Source, Logs)

### Key terms ★★

| Term | Meaning |
|---|---|
| **Information disclosure (CWE-200)** | Sensitive information reaches someone who shouldn't have it. A broad class |
| **Excessive data exposure** | An API returns more properties than the client needs (an older OWASP API Top 10 name) |
| **BOPLA (OWASP API3:2023)** | Broken Object Property Level Authorization: reading **or changing** individual fields without permission |
| **Error-message exposure (CWE-209)** | An error reveals details about users, data or the environment |

### Response design ★★★

- **Input validation** asks "is what came in acceptable?" **Response design** asks "what should go back, to whom, and in what form?" A valid request from a correctly signed-in user can still leak data.
- **The response path:** Data store → Application → Representation (JSON, HTML, file) → Delivery (headers, caches, client).
- **The page shows part of the response, but the client receives all of it.** Check what was actually sent in Developer Tools → Network → Fetch/XHR → Response.
- **Example:** a profile page needs `displayName` and `course`, but the response also returns `email`, `studentNumber`, `accountStatus` and `recoveryToken`. A recovery token is credential material and never belongs in a profile response.
- **Object level vs property level:** "may you see this record?" vs "which fields of it may you read or change?" Ownership can be checked correctly while a serializer still leaks an internal field.
- 🧠 **Three questions per field: Need, Audience, Permission.** Choose the fields to send (a **response contract, i.e. an allowlist**). Sending everything and stripping known secrets (a blocklist) breaks as soon as someone adds a new internal field. Hiding a field in the front end enforces nothing.
- **Other channels leak too:** headers, downloads, exports, generated documents and caches. Use `Cache-Control: no-store` for sensitive responses.
- **Disclosure vs output encoding:** disclosure is about *what* is sent, and encoding is about *how* untrusted text is displayed safely. Fixing one doesn't fix the other.

### Safe errors ★★

| Public response (to the user) | Restricted diagnostic record (for the team) |
|---|---|
| The operation failed, plus a safe next step | The event, time and technical context |
| An opaque support reference | The same reference, so the two can be linked |
| No stack trace, query text, paths or versions | No passwords, tokens or unnecessary personal data |

Example: *"We could not complete that request. Reference: R7Q2."* Also check that the failure didn't leave a half-finished change behind.

### Username enumeration ★★★

- "Password is incorrect" vs "No account with that address" reveals which accounts exist, **even when the HTTP status is the same**.
- **Timing can leak too.** Even with identical messages, an unknown username may be rejected at once while a real one runs the slow password check. A **repeatable** delay is a clue. One slow response proves nothing.
- **Fix:** use an established login flow that does comparable work for both failures, limit repeated attempts and monitor for abuse. Changing the message alone isn't enough.
- A list of valid accounts helps phishing and password attacks, but it doesn't prove any account was compromised.

### Secrets and Git ★★★

- An **API key** is a credential that one program presents to another service.
- Keep keys out of shared source code and browser files. Supply them through protected server configuration or a secret store, give each key only the permissions it needs, and make sure it can be replaced. **If a required key is missing, stop safely, with no fallback value.** Moving a key out of the source code doesn't automatically secure its new location.
- **Git keeps history.** A key committed on Monday and deleted on Tuesday is still in Monday's commit. `.gitignore` only affects *untracked* files and doesn't touch history.
- **An exposed `.git` folder** on a web server lets anyone rebuild the source *and its history*, removed secrets included. Deploy only the built public files and block Git paths. Turning off directory listing doesn't help, because the file names are well known. No public GitHub repository is needed for this to happen.

**Responding to a leaked secret: four steps, in order**

1. **Contain:** cut off access to the exposed files.
2. **Revoke or rotate:** kill the credential and invalidate the old value.
3. **Investigate:** check how it was used and preserve the evidence.
4. **Clean:** remove it from history and other copies, under a coordinated plan.

🧠 **"Can Robbers Inspect Code?"** (Contain, Revoke, Investigate, Clean)

- **Revoke** disables a value. **Rotate** replaces it and retires the old one.
- Cleaning history can't recall copies that were already downloaded, which is why revoking comes first.

### Logging safely and failing closed ★★

- **Log selected fields, not whole forms:** `audit({ event: "sign_in_failed", requestId, outcome: "denied" })`.
- Record failed sign-ins, refused access and permission changes. Leave out passwords, tokens and complete form submissions, and restrict who can read the logs.
- If secrets were already logged, deal with the stored copies **and** the exposed credentials.
- **If permission can't be checked, refuse (fail closed).** A permission service that's down hasn't granted anything, so refuse the export. Decide **before** releasing data, because an error shown afterwards can't take the data back.

> **⚠️ Exam traps**
> - Hiding a field on the page does **not** stop it being sent.
> - Using the same message for both failed logins does **not** fully stop enumeration, because timing can still leak.
> - Deleting a secret from the code or adding the file to `.gitignore` does **not** remove it from history or stop it working.
> - A generic error page does **not** fix the underlying bug. It limits what the bug reveals.
> - Fail **closed** (refuse when unsure), never fail **open**.

---

## 8. Malware and ransomware

### Basics ★

- A computer can be malware's **target** (files encrypted, data stolen), its **tool** (sending spam, relaying attacks) or **both** (a hijacked account fooling its contacts). The owner may notice nothing.
- **Crimeware roles:** developer, access seller, operator or affiliate, infrastructure provider. **Ransomware-as-a-service (RaaS)** supplies tools to affiliates. Naming the malware family doesn't identify the person or group behind an attack.

### Malware types ★★★

| Type | Defining feature | Key point |
|---|---|---|
| **Virus** | Inserts its code into a **host** program or file and runs when the host runs | Replication and payload (the damage) are separate parts |
| **Worm** | Spreads between systems **on its own, with no host** | Doesn't need a zero-day. Widespread damage doesn't prove it's a worm |
| **Trojan** | **Looks legitimate** but hides malicious behaviour | Often staged, so deleting the installer doesn't remove later components |
| **Backdoor** | Provides **unauthorised access** | Not the same thing as a rootkit |
| **Rootkit** | Alters the system to **hide** activity | The system may report false information. Compare with trusted sources or examine it offline |
| **Ransomware** | Blocks access to data or systems, then **demands payment** | Defined by its purpose |

- The labels describe different properties. Virus and worm describe **how it spreads**, Trojan describes **how it presents itself**, and ransomware describes **what it's for**. They overlap: a Trojan can deliver ransomware.
- 🧠 **"A virus needs a host, a worm walks alone, a Trojan wears a disguise."**
- **Polymorphic** malware changes its *wrapper* (varying encryption and decoder). **Metamorphic** malware *rewrites its own code* while keeping the same behaviour. Either way, one exact hash catches only one version.

### Delivery ★★

| Document mechanism | Control |
|---|---|
| Active content such as macros | Restrict execution to trusted sources |
| An exploit against the document reader | Patch and isolate the application |
| A link to another resource | Check the destination and what it asks you to do |

- **Social engineering** uses impersonation, a believable pretext or a fake login page. Verify through a channel you already know, such as the normal web address or a known phone number, never one given in the message. Good spelling and branding prove nothing.
- **Scareware** fakes a security scan to get payment or an install, and its "results" are just claims. **Ransomware** really does block access.

### Detection ★★★

- **Detection signature:** a pattern linked to malware. An exact hash identifies one known sample, and broader signatures match related samples.
- **Digital signature:** shows **who signed** the software and that it **hasn't changed** since. **It does not prove the software is harmless.**
- **Heuristic detection** applies rules about suspicious traits. **Behavioural detection** watches what a program does: launching a scripting tool, rewriting many documents quickly, reading credentials, adding persistence. Context matters, because backup software legitimately reads many files.

| Analysis | How | Limit |
|---|---|---|
| **Static** | Inspect without running: structure, strings, imports, signatures | Packing or obfuscation can hide the content |
| **Dynamic** | Run it in a controlled environment and watch processes, file changes and connections | Sees only behaviour triggered at that time and place. Malware may detect analysis or wait |

A sample that stays quiet in a dynamic test is **not** proven harmless.

### Ransomware ★★★

| Ransomware activity | CIA objective hit |
|---|---|
| Encrypting live data | **Availability** |
| Copying (stealing) data | **Confidentiality** |
| Altering records | **Integrity** |

- **Double extortion** combines encryption with a threat to publish stolen data. Data can also be stolen with no encryption at all.
- **A backup restores availability, not confidentiality.**
- A ransom note **doesn't prove** how much was encrypted or stolen, or that paying would lead to recovery.
- **Paying** doesn't rebuild services, remove the attacker's access or prove the stolen data was deleted.
- **Renaming the file extension back** doesn't decrypt anything, because the contents have changed.
- Files on a **writable shared drive** can be encrypted without every device being infected.

### Recovery ★★

- **Deleting the malicious file** removes only that file. The attacker may still have credentials, sessions or a second component, so investigate the entry route, affected accounts and later activity.
- **A useful backup** is accessible, recent enough and protected from changes. A completed backup *job* proves nothing; a successful **restore test** does. A **synchronised copy** can copy the damage too, so keep version history.

| Control | Purpose |
|---|---|
| Patching and reduced exposure | Close known routes in |
| Restricted execution and least privilege | Limit what code can run and change |
| Segmentation and separate admin accounts | Limit how far an attacker can reach |
| Monitoring with response cover | Spot suspicious activity and act on it |
| Protected, tested backups | Recover data and services |

**Forensics:** link the suspect file to its parent and child processes, accounts, network destinations and persistence changes. A file, a connection and a ransom note are separate observations, and none tells the whole story.

> **⚠️ Exam traps**
> - Widespread disruption does **not** prove the malware is a worm.
> - A digitally signed program is **not** proven harmless.
> - No match against a known hash does **not** mean the file isn't a variant.
> - Identifying the malware family does **not** identify the attacker.
> - Deleting the malicious file does **not** remove the attacker's access.
> - Restoring a backup does **not** undo data theft. A completed backup job does **not** prove you can restore.

---

## 9. Authentication and password storage

### Three separate checks ★★★

| Point in the visit | Question | Check |
|---|---|---|
| Login | Is this valid evidence for the claimed account? | **Authentication** |
| Next request | Does it carry a valid session? | **Session validation** |
| Results page | May this account read these results? | **Authorisation** |

- **An IP address is not an identity.** People share addresses, and one person's address can change.
- A correct password lets authentication succeed. It does **not** authorise access to every record.

### How passwords should be stored ★★★

If an attacker copies the account table:

| What is stored | What the attacker gets |
|---|---|
| The password itself (plaintext) | Readable immediately |
| An encrypted password | Recoverable with the key |
| A password-hash record | Guesses can be tested, but the password itself isn't stored |

| Operation | What it does | Typical use |
|---|---|---|
| **Encryption** | Two-way: a key recovers the original | Storing a document you need to read later |
| **Hashing** | One-way digest with no decryption step | Checking a file hasn't changed |
| **Password hashing** | Salted and **deliberately slow and costly** | Checking a submitted password |

**HTTPS protects the password in transit. Password hashing protects the stored copy.** You need both.

### Case study: Adobe, 2013 ★★

- Troy Hunt's analysis found **152,989,508 records**, and **109,305,888** of them had a password hint.
- Passwords were **encrypted without salt**, so equal passwords produced equal ciphertexts. **3,011 accounts** shared one value with the hint "color", so cracking it once would expose all 3,011.
- **Hints gave clues away.** Of the **top 100 hint strings**, 24 were family names. That's 24 of 100 *strings*, **not 24% of accounts**.
- Comparing breaches (an Adobe hint against a Sony password): "fruit" → grapefruit, "name birthyear" → mike1953.
- **Lessons:** use salted, costly password hashing, and use a reset flow instead of storing hints.

### Saving and verifying a password ★★

- **Save:** the password arrives over HTTPS → the library generates a fresh random salt → computes the hash with its cost settings → stores the hash, salt and settings. The password itself is never stored, not even in logs.
- **Verify:** load the stored record → recompute using the *stored* salt and settings → compare the results. **Nothing is decrypted.** (In the Python library the function is `verify()`, and Argon2id is the algorithm.)

### Argon2id ★★★

| SHA-256 | Argon2id |
|---|---|
| Designed to be **fast** | Designed for **password hashing** |
| Good for file-integrity checks | Adjustable **time and memory** cost |
| Each password guess is cheap | Each guess is deliberately expensive |

- **A salt stops identical-hash and precomputed (rainbow table) shortcuts, but it adds no cost.** The cost comes from the algorithm's settings. Don't build your own loop around a fast hash.
- **The variants:**
  - **Argon2i** uses data-*independent* memory access, which resists side-channel attacks.
  - **Argon2d** uses data-*dependent* access, which resists memory-saving (trade-off) attacks.
  - **Argon2id** is a **hybrid**: i-style for the first half of the first pass, d-style after that.

| Parameter | Meaning | Lecture example |
|---|---|---|
| **m** | Total memory per calculation (in KiB) | 65,536 KiB = **64 MiB** |
| **t** | Number of passes over that memory | **3** |
| **p** | Parallel lanes in one calculation | **4** (16 MiB each) |

- The lecture example is the **RFC 9106 low-memory profile**. The Week 6 lab uses the **OWASP minimum**: m = 19,456 KiB (19 MiB), t = 2, p = 1.
- **The server pays the cost too,** so benchmark concurrent logins and limit how many verifications run at once.

**Stored record format:** `$argon2id$v=19$m=65536,t=3,p=4$<salt>$<hash>`
- `v=19` is the algorithm version: decimal 19 = **0x13**. **It is not the year 2019.**
- The salt and hash are **Base64-encoded**. Base64 is an encoding, not encryption.
- Store the whole record, because verification needs the algorithm, salt and settings.

> **⚠️ Exam traps**
> - A salt makes equal passwords hash differently. It does **not** slow down guessing.
> - SHA-256 is **not** a password hash, because it's fast by design.
> - Password verification does **not** decrypt anything.
> - Adobe's "24 family names" means 24 of the top 100 hint strings, **not** 24% of users.
> - `v=19` is a version number (0x13), **not** a year.
> - A matching password does **not** mean the user may access every record.

---

## 10. Sessions, cookies and access control

### From password to session ★★★

1. The user signs in over HTTPS.
2. The server verifies the password and creates a session record.
3. The server sends `Set-Cookie` with an **opaque, unpredictable session ID**.
4. Later requests carry that ID in the `Cookie` header. The password is never resent.
5. On each request, the server validates the session, then checks permission for the record, action and fields.

```http
Set-Cookie: __Host-session=<id>; Path=/; Secure; HttpOnly; SameSite=Lax; Max-Age=1800
Cookie: __Host-session=<id>
```

- The browser's `Cookie` header sends **only the name and value**. The attributes tell the browser how to handle the cookie.
- **A session ID is a credential.** Keep it out of URLs, screenshots and logs, and issue a new one when the user logs in or their privilege changes.

### Cookie attributes ★★★

| Attribute | What it does |
|---|---|
| **Secure** | Sent only over HTTPS |
| **HttpOnly** | Page JavaScript can't read the cookie, but the browser still sends it. **It doesn't stop script injection (XSS)** |
| **SameSite** | Controls whether the cookie is sent on cross-site requests |
| **Max-Age=1800** | The browser keeps it for 1,800 seconds = **30 minutes** |
| **Path=/** | Sent for every path on the host. Grants no access to any record |
| **`__Host-` prefix** | Requires **Secure**, **Path=/** and **no Domain** attribute, so the cookie stays on the exact host that set it |

Cookie settings never replace server-side access checks.

### SameSite ★★★

| Request type | Strict | Lax | None; Secure |
|---|---|---|---|
| Same-site request | Sent | Sent | Sent |
| Another site → normal link (top-level GET navigation) | **Not sent** | **Sent** | Sent |
| Another site → form POST | Not sent | **Not sent** | Eligible* |
| Your site inside another site's iframe | Not sent | Not sent | Eligible* |

\*The browser's third-party cookie restrictions may still block it. `SameSite=None` **requires** `Secure`.

🧠 **Strict = never cross-site. Lax = only when the user clicks a link to you. None = always (it must be Secure).**

**Choosing a setting:**
- **Lax** suits an ordinary shop, because users arriving from links stay signed in.
- **Strict** gives tighter isolation. Test external links and sign-in returns first.
- **None; Secure** is only for genuine cross-site use.

**Site vs origin:**
- **Origin** = scheme + host + port.
- **Site** = scheme + registrable domain.
- `https://shop.cedar.example` and `https://help.cedar.example` are **same-site but different origins**.
- Changing only the port changes the **origin** but not the site. Switching HTTPS to HTTP changes **both**.

### CSRF ★★

**Cross-site request forgery:** another page makes the victim's browser send a request that's authenticated by an existing cookie, even though the user didn't mean to send it. Example: a hidden form makes a signed-in support agent issue a refund. The role check passes, because it really is the agent's browser.

**Defences:**
- The framework's CSRF protection (validated tokens) plus origin checks.
- No state changes on GET requests.
- SameSite as an **extra** layer only.

CORS is not a universal CSRF defence, and no cookie setting can fix a missing ownership check.

### Access control ★★★

**Cedar Home shop (fictional):** Lina owns order 7201 and Omar owns order 7202.

| Requester | Read 7201 | Read 7202 | Issue refund |
|---|---|---|---|
| Lina | Allow | Deny | Deny |
| Omar | Deny | Allow | Deny |
| Support staff | Allow | Allow | Allow |
| No valid session | Deny | Deny | Deny |

- **An ID says *which* object, not *who* may access it.** If Lina changes `/api/orders/7201` to `/api/orders/7202`, the server must refuse. Input validation can confirm that 7202 is well formed, but not that Lina may read it.
- **Correct ownership check:** `account.id === purchase.ownerId || account.role === "support"`, with both records loaded from trusted server-side state.
  - Comparing with `purchase.id` is wrong, because that's the order number, not the owner.
  - Using `&&` instead of `||` would require the user to be the owner *and* support.
  - `Boolean(account && purchase)` only proves that both records exist.
- **Refund check:** `account.role === "support"`, enforced **before** anything changes. A denied request must leave the order and financial records untouched. **Hiding the refund button doesn't stop a direct API request.**

| Failure | Name | Direction |
|---|---|---|
| Reading another customer's order | **IDOR / BOLA** (OWASP API1) | **Horizontal** (same role, another user's data) |
| Performing a support-only action | **Broken function-level authorisation** (OWASP API5) | **Vertical** (a higher role's action) |
| Reading or changing a field you shouldn't | **BOPLA** (OWASP API3) | Property level |

🧠 **API1 = wrong record · API3 = wrong field · API5 = wrong action.**

**Case study: First American, 2019.** Changing a number in an EaglePro document URL exposed unrelated customers' documents. The SEC reported **over 800 million images** exposed (findings published June 2021). That counts *images*, not people, and doesn't prove they were all downloaded. Random IDs make guessing harder but **don't replace a permission check**.

### HTTP status codes in these labs ★★

| Code | Meaning here |
|---|---|
| **200** | OK |
| **201** | Created |
| **400** | Bad request (invalid input) |
| **401** | No valid session: not authenticated (despite the official name "Unauthorized") |
| **403** | Authenticated, but permission refused |
| **404** | Not found |
| **409** | Conflict, e.g. a second refund for the same order |
| **500** | Server error |

> **⚠️ Exam traps**
> - `HttpOnly` does **not** prevent XSS. It only stops JavaScript reading the cookie.
> - A hidden button does **not** stop the action. The server must check the role.
> - Random, unguessable IDs do **not** amount to access control.
> - A browser showing "signed out" does **not** mean the server revoked the session.
> - A cookie still sitting in the browser does **not** mean the session is valid, because the server checks expiry.
> - A detail page that checks ownership does **not** mean the export route does too.
> - Same-site does **not** mean same-origin.
> - The same IP address does **not** mean the same person.

---

# Apply and review

## 11. Lab walkthroughs

The labs below support the core topics. Week 5 → information disclosure. Week 2 → data hiding. Week 6 → authentication and sessions. The CTFs → disclosure and access control.

### Week 5: "What goes out" ★★★

**The rule behind every fix:** say as little as possible to the outside, and keep the detail where only your team can read it.

| Leak | CWE | Fix | Remember |
|---|---|---|---|
| Error page shows a stack trace, file paths, database path and Node.js version | **CWE-209** | Generic message + reference ID, with the detail sent to the log | The bug still exists. The fix limits what it reveals |
| Different login messages and status codes (404 vs 401) reveal which emails have accounts: **user enumeration** | **CWE-204** | Same status and same message for both failures | The correct password must still log in |
| Payment key and session secret hard-coded in the source | **CWE-798** | Read them from the environment (`process.env`), and stop safely if one is missing | A default value in the code is still a secret in the code |
| Passwords stored as typed | **CWE-256** | **scrypt** with a **random salt per password**, compared in constant time | A damaged stored value must return false. Don't encrypt or Base64 instead |
| Passwords and card numbers written to the log | **CWE-532** | Choose the safe fields to keep (allowlist), rather than the ones to remove | Logging nothing isn't a fix either. The log must still show what happened |
| No audit trail for security events | **CWE-778** | Fixed fields: **event, user, result, time**, with no secrets | Record the events that matter, not every page view |
| The check says yes when it hits an error: **fail open** | **CWE-636** | If the check can't decide, refuse and log a warning | Legitimate staff must still get in |

🧠 **CWE quick map:** 200 disclosure (general) · 204 response discrepancy (enumeration) · 209 error message · 256 plaintext password · 532 secrets in logs · 636 fail open · 778 missing audit log · 798 hard-coded credential

**The password trio:**
- **Hash:** one-way. It can check a password but can't give it back.
- **Salt:** random per password, so equal passwords look different.
- **Slow function:** makes every guess expensive.

**Retesting a fix:** run that exercise's tests, repeat the real attack, *and* confirm that a normal request still works.

### Week 2: Hidden data ★★

The lab's message is that a quick visual check can miss hidden information.

| Method | What you observed | Control for a secure program |
|---|---|---|
| **Alternate data streams** | `dir` shows nothing unusual, but `dir /r` shows `welcome.txt:secret.txt:$DATA`. A ZIP archive can't preserve ADS | Enumerate named streams |
| **PDF containers** | The pages look identical but the file sizes differ. Data can sit after the final `%%EOF` marker, or be a genuine attachment | Inspect the whole container, attachments included |
| **Image steganography** | You can't tell by eye which image carries a file. The output is saved as **PNG** so the hidden bits survive. Comparing SHA-256 hashes proves full recovery | Validate stored lengths and paths |
| **Shannon entropy** | Scored from 0 to 8 bits per byte. The extension alone doesn't predict the score, and a DOCX is a ZIP-based container | Interpret it in context |

**Entropy blocks:** a block is a consecutive chunk of the file's bytes, not a disk cluster. Block-by-block scores can pinpoint a change that a whole-file score hides.

### Week 6: Riverton Live (passwords, sessions, access control) ★★

This is the hands-on version of sections 9 and 10.

| Exercise | What you observed | Lesson or fix |
|---|---|---|
| **1. Argon2id** | Hashing the same password twice gives different salts and hashes. The same password, salt and costs give the same hash | Verification reuses the stored salt and costs. Nothing is decrypted |
| **2. Network** | `GET /api/catalogue → 200` returns `doorsOpen: "19:30"`, which the page never shows | An extra field is an observation. The access policy decides whether it's private |
| **3. Login** | A wrong password gets `401` and the right one gets `200`. `Set-Cookie` carries a random session ID with `HttpOnly` and `SameSite=Lax` | The cookie holds a session ID, never the password. `Secure` is left off only because the lab runs on local HTTP |
| **4. Ownership (IDOR)** | Ada could read Ben's booking 1042 | Fix: the user and order exist, **and** (`user.id === order.userId` **or** role is `staff`). After the fix, 1041 → 200 and 1042 → **403**; staff → 200; no session → **401** |
| **5. Refunds (function level)** | Ada could `POST /api/orders/1041/refund` with no refund button on screen | Fix: allow only role `staff`. After the fix, Ada gets **403** and the booking stays paid; staff get 200, then **409** for a duplicate |
| **6. Logout and expiry** | After sign-out, a saved session ID still got `200` | Fix: **revoke on the server** (`session.revoked = true`), so the ID gets `401` after logout. An expired session gets `401` even if the cookie is still present |

- **Retesting tip:** reset the data before retesting a denied refund. Otherwise a duplicate-refund `409` could hide whether the permission check works.
- **Lab Argon2id profile:** OWASP minimum, m = 19,456 KiB (19 MiB), t = 2, p = 1, a 16-byte salt and a 32-byte hash.

### CTFs: Nightfall Games and Harbour Dispatch ★

Both are short **inspect → explain → verify** challenges done as an **ordinary user**, with no password guessing or brute force.

- **Nightfall Games (15 minutes):** find an unreleased beta code in HTTP responses that the page doesn't display, then redeem it. **Lessons:** data hidden on screen can still be in the response, and a valid player session isn't permission to use a restricted resource.
- **Harbour Dispatch (20 minutes):** follow a chain of references across responses to an internal depot record. The detail route and the **export/download route** behave differently. **Lesson:** every endpoint needs its own ownership check.

---

## 12. Calculations

### Clusters, slack and offsets ★★★

**Given:** a 9,400-byte file, 4,096-byte clusters, and data runs (start 120, length 2) and (start 900, length 1).

| Question | Working | Answer |
|---|---|---|
| Clusters needed? | 9,400 ÷ 4,096 = 2.29 → round **up** | **3 clusters** |
| Space allocated? | 3 × 4,096 | **12,288 bytes** |
| Slack? | 12,288 − 9,400 | **2,888 bytes** |
| Cluster read order? | Follow the runs | **120, 121, 900** |
| Where is file byte 8,500? | 8,500 − (2 × 4,096) = 308, so it's in the third cluster | **LCN 900, offset 308** |
| Byte offset within the volume? | (900 × 4,096) + 308 | **3,686,708** |

🧠 **Formulas:** clusters = ⌈size ÷ cluster size⌉ · slack = clusters × cluster size − size · volume offset = LCN × cluster size + offset within the cluster

If you're working from a whole-disk image, also **add the volume's starting offset**.

### Decoding a data run list ★

`11 02 78 21 01 0C 03 00`

| Bytes | Meaning |
|---|---|
| `11` | Header. Low nibble = 1 byte of length, high nibble = 1 byte of offset |
| `02 78` | Run of 2 clusters starting at LCN 0x78 = **120** |
| `21` | Header: 1 byte of length, 2 bytes of offset |
| `01 0C 03` | Run of 1 cluster. Offset delta (little-endian) 0x030C = 780, relative to the previous run: 120 + 780 = LCN **900** |
| `00` | End of the run list |

Real run lists can use signed (negative) deltas and longer fields. This example has no sparse or compressed runs.

### Time zones ★★★

Subtract a positive offset to get UTC (and add a negative one).
- 08:40 +01:00 → **07:40 UTC**
- 14:30 +02:00 → **12:30 UTC**

### LSB capacity (Week 2 lab) ★★

**A 3,840 × 2,560 RGB image, using 2 bits per colour channel:**

| Step | Working | Answer |
|---|---|---|
| Pixels | 3,840 × 2,560 | 9,830,400 |
| Bits | Pixels × 3 channels × 2 bits | 58,982,400 |
| Bytes | Bits ÷ 8 | **7,372,800** (the lab reports 7,372,771 after header overhead) |

### Argon2id memory ★★★

**Given:** m = 65,536 KiB, p = 4.

| Question | Working | Answer |
|---|---|---|
| Memory per verification? | 65,536 KiB ÷ 1,024 | **64 MiB** |
| Memory per lane? | 64 MiB ÷ 4 lanes | **16 MiB** |
| 20 verifications at once? | 20 × 64 MiB = 1,280 MiB, then ÷ 1,024 | **1.25 GiB** |

This is working memory used during the check, not the size of the stored record.

### Quick conversions ★★

- **Cookie lifetime:** Max-Age=1800 → 1,800 ÷ 60 = **30 minutes**
- **Argon2 version:** `v=19` → decimal 19 = hex **0x13** (not a year)
- **Entropy:** 256 equally likely byte values → log₂ 256 = **8 bits**

---

## 13. Final review sheet

Read this on the morning of the exam.

### Memory tricks

| Trick | Stands for |
|---|---|
| **"All My Students Can Improve During Autumn If Learning's Easy"** | OWASP A01–A10: Access control, Misconfiguration, Supply chain, Cryptographic, Injection, Design, Authentication, Integrity, Logging, Exceptional conditions |
| **"Work, Wrong, Do, Done"** | Shostack's Four Questions |
| **"Every Mistake Teaches Answers"** | Threat responses: Eliminate, Mitigate, Transfer, Accept |
| **STRIDE → A I A C A A** | Authentication, Integrity, Accountability, Confidentiality, Availability, Authorisation |
| **"Real Engineers Log Secrets Loosely"** | Leak routes: Responses, Errors, Login, Source, Logs |
| **"Need, Audience, Permission"** | Should this field be in the response? |
| **"Can Robbers Inspect Code?"** | Leaked secret: Contain, Revoke/rotate, Investigate, Clean |
| **"A virus needs a host, a worm walks alone, a Trojan wears a disguise"** | Malware types |
| **"Observe, Interpret, Test"** | Handling evidence |
| **"Inspect, Compare, Explain"** | NTFS analysis workflow |
| **"Prevent, Detect and respond, Recover"** | Defence in depth |
| **"API1 record, API3 field, API5 action"** | BOLA, BOPLA, function-level authorisation |
| **"Quote, OR, Comment"** | How `' OR 1=1 --` works (appendix) |

### Key numbers

| Topic | Number | Meaning |
|---|---|---|
| Windows | **4624 / 4625** | Successful / failed logon |
| Windows | **Type 2 / Type 3** | Interactive / network logon |
| Windows | **4688** | Process creation (needs audit policy) |
| Windows | **T1547.001** | MITRE ATT&CK: Registry Run keys |
| NTFS | **1,024 bytes** | Typical MFT record size |
| NTFS | **0x10 / 0x30 / 0x80** | $STANDARD_INFORMATION / $FILE_NAME / $DATA |
| FAT32 | **0xE5** | Deleted directory entry marker |
| Entropy | **0 / 1 / 8 bits** | Constant / fair coin / maximum per byte |
| Argon2id (lecture) | **m=64 MiB, t=3, p=4** | RFC 9106 low-memory profile |
| Argon2id (lab) | **m=19 MiB, t=2, p=1** | OWASP minimum |
| Argon2 | **v=19 = 0x13** | Version, not a year |
| Cookie | **Max-Age=1800** | 30 minutes |
| Threat modelling | **Up to 100×** | Cost of fixing after delivery vs in design (Boehm and Basili, 2001) |
| HSE | **18 Mar → 31 Mar → 14 May 2021** | Phishing → detected in monitoring mode → ransomware |
| Adobe | **152,989,508** | Records |
| Adobe | **3,011** | Accounts sharing one encrypted value (hint "color") |
| Adobe | **24 of top 100** | Hint strings that were family names |
| First American | **800 million+** | Images exposed (not people) |
| TalkTalk | **156,959 / £400,000** | Customers affected / ICO fine (appendix) |
| MOVEit | **CVE-2023-34362** | SQL injection exploited by CL0P (appendix) |

### Pairs people mix up

| Pair | The difference |
|---|---|
| **Authentication vs authorisation** | Who are you? vs what may you do? |
| **Threat vs vulnerability** | What could cause harm vs the weakness it exploits |
| **Attack surface vs vector** | Where contact is possible vs how it's used |
| **Steganography vs cryptography** | Hides that a message exists vs hides what it says |
| **Steganography vs ADS** | Inside the carrier's content vs a separate named stream |
| **Hashing vs encryption** | One-way check vs reversible with a key |
| **Hash vs password hash** | Fast digest vs salted, deliberately costly verifier |
| **Salt vs cost** | Stops equal-hash shortcuts vs makes each guess expensive |
| **Argon2i vs Argon2d vs Argon2id** | Data-independent (side channels) vs data-dependent (trade-off attacks) vs hybrid |
| **Signature (magic bytes) vs parser** | First bytes vs the whole structure |
| **Detection signature vs digital signature** | Malware pattern vs who signed it and that it's unchanged |
| **Resident vs nonresident** | Inside the MFT record vs in clusters elsewhere |
| **VCN vs LCN** | Position in the file vs position on the volume |
| **$UsnJrnl vs $LogFile** | Change records vs file-system consistency and recovery |
| **SI vs FN timestamps** | Common set, easier to change vs second set stored with the filename |
| **HKCU vs HKLM** | Current user vs whole machine |
| **Logging vs alerting** | Recorded vs noticed and acted on |
| **Application log vs audit log** | What happened in the app vs fixed security fields (event, user, result, time) |
| **Client-side vs server-side check** | Convenience the user controls vs actually enforced |
| **Allowlist vs blocklist** | Define what's valid vs list what's bad |
| **Fail closed vs fail open** | Refuse when unsure vs allow when unsure |
| **Input validation vs response design** | Is what came in acceptable? vs what should go out, and to whom? |
| **Information disclosure vs output encoding** | What is sent vs how it's displayed safely |
| **Object-level vs property-level authorisation** | May you see this record? vs which fields of it? |
| **Horizontal vs vertical access failure** | Another user's data vs a higher role's action |
| **Revoke vs rotate** | Disable a value vs replace it and retire the old one |
| **Virus vs worm vs Trojan** | Needs a host vs spreads alone vs disguised |
| **Backdoor vs rootkit** | Gives access vs hides activity |
| **Polymorphic vs metamorphic** | Wrapper changes vs the code itself is rewritten |
| **Heuristic vs behavioural detection** | Rules about suspicious traits vs watching what it does |
| **Static vs dynamic analysis** | Inspect without running vs run and observe |
| **Backup job vs restore test** | A copy was made vs proof the service comes back |
| **Sync copy vs versioned backup** | Mirrors the damage vs keeps older versions |
| **Set-Cookie vs Cookie** | Server sends value and attributes vs browser sends name and value only |
| **Site vs origin** | Scheme + registrable domain vs scheme + host + port |
| **SameSite Strict vs Lax vs None** | Never cross-site vs top-level links only vs always (needs Secure) |
| **Browser logout vs server-side revocation** | Clears your cookie vs makes the session ID stop working |
| **401 vs 403 vs 409** | Not authenticated vs permission refused vs state conflict |

### The universal answer pattern

For any "what can you conclude?" question, say what the evidence **shows**, then what it **can't show**, then what you'd **check next**.

---

## 14. Practice MCQs

Questions marked **(select all that apply)** can have one or more correct answers. All the others have exactly one. Answer before you open each answer.

### Foundations and threat modelling

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

### Data hiding

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

### OWASP Top 10 (2025)

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

### Signals, evidence and NTFS

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

### Calculations

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

### Information disclosure

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

### Malware and ransomware

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

### Authentication and passwords

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

### Sessions, cookies and access control

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

# Appendix: beyond the core scope

These topics were outside the listed scope of the first assessment, so revise them after everything above. They still matter for the final assessment and the project.

## Appendix A. SQL injection

### How it happens ★★★

- **SQL injection (CWE-89):** external input changes the *structure* of a SQL query. The database runs whatever it receives, with the application's permissions.
- **The mistake:** building SQL text from request input, e.g. `` `... AND surname LIKE '%${term}%'` ``. Calling `prepare()` *afterwards* still prepares the already-altered query.
- **Apostrophes:** searching for `O'Neill` closes the string early and causes a syntax error for a perfectly valid name. A quote error is a clue, not proof. A generic error page doesn't prove the query is safe, and banning apostrophes breaks real names without fixing anything.

**Worked example: searching for `' OR 1=1 --`**

```sql
SELECT id, surname FROM employees
WHERE department_id = 10
AND surname LIKE '%' OR 1=1 -- %'
```

1. `'` **ends the string.**
2. `OR 1=1` **adds a condition that's always true.**
3. `--` **comments out the rest** (the leftover `%'`).

**AND is evaluated before OR,** so the WHERE clause becomes (department matches AND name matches) OR TRUE. All 5 staff rows come back, including 2 from another department. The department filter is bypassed without being deleted, and adding parentheses alone doesn't fix it.

🧠 **"Quote, OR, Comment."**

### Forms of SQL injection ★★

| Form | How it works | Note |
|---|---|---|
| **Visible results** | Altered filters return extra rows; `UNION` can append another query's results | `UNION` needs a matching column count and compatible types |
| **Blind** | Differences in page content or response time reveal whether a condition is true | One slow response isn't proof. Hiding errors doesn't remove the flaw |
| **Second-order** | A stored value is later inserted unsafely into another query | Data from your own database isn't automatically safe |

Possible effects: unauthorised access, changed records and authentication bypass. The impact depends on the query, the database and its permissions.

### Real incidents ★★

| Incident | Attack chain | Detail |
|---|---|---|
| **Heartland** (disclosed 2009) | SQL injection → malware → card theft | The "130 million cards" figure covers the wider alleged campaign, not Heartland alone |
| **TalkTalk** (2015) | SQL injection → database access → customer data | **156,959 customers.** Legacy web pages, outdated database software, earlier attacks missed. ICO fine **£400,000** (2016) |
| **MOVEit Transfer** (2023) | SQL injection → web shell → data theft | CL0P exploited **CVE-2023-34362** and installed the **LEMURLOOT** web shell. Third-party software needs patching too |

### The fix: parameter binding ★★★

```javascript
const statement = db.prepare(`
  SELECT id, surname FROM employees
  WHERE department_id = ?
  AND surname LIKE ?
`);
const rows = statement.all(departmentId, '%' + term + '%');
```

- **A fixed SQL template, with values bound separately.** The `?` placeholders are not inside quotes, and the driver treats quotes and `OR` in the value as plain data.
- `'%' + term + '%'` is fine, because it builds a *data value*, not SQL text.
- **Binding is not authorisation.** Take the department from the authenticated user's permitted scope. If a user sends department 20 and the server binds it without checking, injection is prevented but access control is still broken.
- **LIKE wildcards still work inside bound values:** `%` matches any run of characters and `_` matches one character. If you need literal matching, escape them with SQL `ESCAPE`.
- **Placeholders are only for values.** For column names (as in `ORDER BY`), use an **allowlist** that maps each option to a fixed name (`name` → `surname`) and reject everything else. Treat ASC and DESC the same way.
- ORM raw queries and stored procedures that build dynamic SQL can still be vulnerable.

### Supporting controls ★

| Control | Purpose |
|---|---|
| Query construction | Bind values, and use fixed options for query structure |
| Permissions | Least privilege for the database account. With SQLite: file permissions and a read-only connection |
| Operations | Patch, hide SQL errors from users, monitor unusual access |
| Input rules | Validate type, length and allowed choices, while still accepting real names and punctuation |

**None of these replaces parameter binding.** A web application firewall adds filtering but doesn't repair query construction, and browser-side checks can be bypassed.

### Testing the fix ★

| Search (as a department 10 user) | Expected result |
|---|---|
| `Patel` | The matching authorised employee |
| `O'Neill` | Works, with no SQL error |
| `NobodyHere` | Empty result, normal response |
| `' OR 1=1 --` | Treated as search text, with no extra access |
| `%` or empty | All 3 permitted rows (under this search policy) |
| Requesting department 20 | Denied or restricted by the server |

Check the returned record IDs and departments, not just the status code. HTTP 200 says nothing about who owns the data.

> **⚠️ Exam traps**
> - An SQL error after typing a quote does **not** prove an exploitable injection.
> - A generic error page does **not** make the query safe.
> - Calling `prepare()` does **not** make the query safe if input was inserted first.
> - Bound values do **not** guarantee correct access control.
> - A WAF does **not** fix query construction.
> - HTTP 200 does **not** mean the right records came back.

---

## Appendix B. OS command injection

### How it happens ★★★

- **OS command injection (CWE-78):** user input changes **shell syntax**, so extra operating-system commands run. It's not an AI prompt attack: the commands really execute, with the server account's privileges.
- **The lab:** a WHOIS lookup (Northline Domains) and a ping tool (Relay Network). The Python app builds one string, such as `ping -n -c 2 -W 1 -- {value}`, and runs it with `shell=True`.
- **Proof:** add `; id` after a normal value. The output then shows which account ran the command (a low-privilege `labuser`).

| Linux shell operator | Effect |
|---|---|
| `;` | Runs the next command regardless of the outcome |
| `&&` | Runs the next command only if the first succeeds |
| `&` | Runs the first command in the background |

Windows uses different separators.

**Impact shown in the lab:**
- **Confidentiality (WHOIS):** reading a staff-only archive that the public lookup should never reveal.
- **Integrity (ping):** overwriting the public homepage headline with `printf` and `>`. The change survives a reload because it's on the server. Editing the page in Developer Tools only changes *your* browser's copy, so it proves nothing.

**Follow the path:** HTTP request → Python app → **shell parser** → operating-system program. The shell parser is where data turns into instructions. Hiding the error or filtering in the browser doesn't fix it.

### The fix ★★★

- **Don't use a shell.** Pass an argument list, with a fixed program and fixed options, and the user's value as **one** argument: `["ping", "-n", "-c", "2", "-W", "1", "--", value]` with `shell=False`.
- **Keep `--`,** so a value starting with `-` can't be read as an option. That attack is **option injection**.
- **Validate** the value for the tool, for example as a hostname or IPv4 address. Validation helps, but avoiding the shell is the structural fix. Quoting rules are shell-specific and unnecessary once no shell is involved.
- **Run as non-root.** The lab also uses a read-only filesystem, dropped capabilities, resource limits and no internet access. These **reduce the impact but don't fix the injection**.

**Same idea as SQL injection:** keep data separate from instructions. In SQL, bind parameters. At the operating system, pass an argument list with no shell.

**A good finding report covers** the target and scope, the exact request, the relevant output, the source-level cause, the impact, the repair diff and the retest results. Collect only the personal data you need.

> **⚠️ Exam traps**
> - Running as non-root does **not** remove command injection.
> - Editing a page in Developer Tools does **not** prove the server's data changed.
> - Browser-side filtering does **not** fix it. The shell is on the server.

---

## Appendix C. Week 4 and Week 7 labs

### Week 4: "The server decides" (what comes in) ★★

**In one sentence:** client-side validation is not a security control, because the user can change or skip anything in the browser.

The order form had **two separate browser checks**: the HTML `max="2"` attribute and a JavaScript check. Removing one doesn't remove the other.

**Ways around the browser:**

| Method | What it does |
|---|---|
| Developer tools | Delete `max`, then disable JavaScript |
| Console: `form.submit()` | Submits the form without running the page's checks |
| `curl` | Sends the request directly, with no browser at all |
| `fetch` in the console | Sends the same request from any page of the app |
| Intercepting proxy (Burp, ZAP) | Catches the request and edits it before forwarding |

**The three fixes the lab builds up to:**

1. **Enforce the limit on the server.** A `quantity > 2` check stops 20 tickets, but 0, negative and fractional values still get through. Always state what a fix does *not* cover.
2. **Use an allowlist.** Define exactly what's valid (digits only, 1 to 4, sent once) and refuse everything else. Blocking bad values one by one is less reliable.
3. **Let the server calculate the money.** The customer chooses only the ticket and quantity. Price, discount and total come from the server's own records, because hidden fields can be edited too. In the lab, a total edited to €0.02 was charged as €0.02.

**Odd inputs the original server accepted:**

| Sent | Saved or treated as | Why it matters |
|---|---|---|
| `-3` | −3 places, total −€36, places left went up | A negative value changed stock and money |
| `0` | 0 places | Outside the rules |
| `1.5` | 1 | Input silently changed |
| `2abc` | 2 | `parseInt` reads digits from the start and stops at the first non-digit |
| `1e3` | 1 | The record no longer matches what was sent |
| `quantity=3&quantity=99` | 3 | A field can arrive twice, even though a browser never sends that |

**Group discount example:** 4 × €45 = €180, less 10% = **€162**.

**Legal point:** testing a system you don't own, or don't have written permission to test, is an offence under Ireland's **Criminal Justice (Offences Relating to Information Systems) Act 2017**.

### Week 7: SQL injection lab ★★

| Site | Attack | What happened | Fix |
|---|---|---|---|
| **Aster Observatory** search | `' OR 1=1 -- ` | 8 results, including 2 **unreleased** ones, because the `published = 1` filter was bypassed. `' AND 1=2 -- ` returned nothing, proving real SQL logic was running | `WHERE published = 1 AND title LIKE ?`, binding `%term%`. Don't quote the `?`, and keep the published filter |
| **Bracket Supply** login | Username `admin' -- ` | Logged in as the administrator with **any password**, because the comment removed the `password_matches(...)` condition. No hash was cracked | Bind **both** the username and the password. Real usernames with apostrophes then work too |
| **Field notes** (extension) | `' OR 1=1 -- ` | Showed other members' private notes | `WHERE member_id = ? AND title LIKE ?`, binding both. Don't hard-code the member |

Password hashing and access checks don't make string-built SQL safe, and binding doesn't replace the published, member or role filters either.

### Extra pairs

| Pair | The difference |
|---|---|
| **Parameter binding vs authorisation** | How a value reaches SQL vs whether the user may see those rows |
| **Bind it vs allowlist it** | Data values vs parts of the query structure (column names, ASC/DESC) |
| **Visible vs blind vs second-order SQLi** | Extra rows shown vs inferred from differences vs stored value used later |
| **Shell command string vs argument list** | Parsed by a shell (injectable) vs passed straight to the program |
| **Command injection vs option injection** | Adds a new command vs turns the value into a `-` option |

---

## Appendix D. Practice MCQs

**D1.** How does `' OR 1=1 --` return every row from `WHERE department_id = 10 AND surname LIKE '%…%'`?

- **A.** It deletes the department filter
- **B.** AND is evaluated before OR, so the clause becomes (A AND B) OR TRUE
- **C.** It drops the table
- **D.** It disables authentication

<details><summary>Answer</summary>

**B.** The filter is bypassed, not deleted.

</details>

**D2.** Input is inserted into a SQL template string, and then `prepare()` is called. Is the query safe?

- **A.** Yes, because `prepare()` sanitises it
- **B.** Yes, if errors are hidden
- **C.** No, because the structure was already altered before preparation
- **D.** Only for numeric input

<details><summary>Answer</summary>

**C.** Use placeholders and bind the values.

</details>

**D3.** Which is the *primary* fix for SQL injection?

- **A.** A web application firewall
- **B.** Banning apostrophes
- **C.** Parameter binding with a fixed SQL template
- **D.** Least-privilege database accounts

<details><summary>Answer</summary>

**C.** The others are supporting layers.

</details>

**D4.** How should users be allowed to choose a sort column safely?

- **A.** Bind the column name as a `?` parameter
- **B.** Map each allowed option to a fixed column name and reject anything else
- **C.** Escape the column name
- **D.** Validate its length

<details><summary>Answer</summary>

**B.** Placeholders can't stand in for column names.

</details>

**D5.** All values are bound, but the department filter is missing. Can rows from other departments come back?

- **A.** No, because binding prevents it
- **B.** Yes, because binding stops injection, not access-control mistakes
- **C.** Only with `UNION`
- **D.** Only if errors are shown

<details><summary>Answer</summary>

**B.**

</details>

**D6.** Which statements about forms of SQL injection are true? **(select all that apply)**

- **A.** Blind SQLi infers information from differences in content or timing
- **B.** Second-order SQLi uses a stored value later in another query
- **C.** `UNION`-based injection needs matching column counts
- **D.** Hiding error messages removes blind SQLi

<details><summary>Answer</summary>

**A, B, C.**

</details>

**D7.** How does the username `admin' -- ` log in without a password?

- **A.** It cracks the admin hash
- **B.** The quote ends the string and `--` comments out the password check
- **C.** It exploits a session cookie
- **D.** It brute-forces the password

<details><summary>Answer</summary>

**B.**

</details>

**D8.** What is the correct fix for OS command injection in the ping tool?

- **A.** Escape semicolons
- **B.** Run the command as non-root
- **C.** Pass a fixed argument list with the value as one argument and `shell=False`
- **D.** Filter input in JavaScript

<details><summary>Answer</summary>

**C.** Keep `--` and validate the value as well.

</details>

**D9.** Why keep `--` in the argument list?

- **A.** It comments out the rest of the command
- **B.** It stops a value starting with `-` being read as an option (option injection)
- **C.** It runs the command in the background
- **D.** It's required by Python

<details><summary>Answer</summary>

**B.**

</details>

**D10.** Which statements about Linux shell operators are true? **(select all that apply)**

- **A.** `;` runs the next command regardless of the outcome
- **B.** `&&` runs the next command only if the first succeeds
- **C.** `&` runs the first command in the background
- **D.** `&&` runs both commands in parallel

<details><summary>Answer</summary>

**A, B, C.**

</details>

**D11.** Why does `parseInt("2abc", 10)` return 2?

- **A.** It rounds the value
- **B.** It reads digits from the start and stops at the first non-digit
- **C.** It ignores letters anywhere in the string
- **D.** It's a JavaScript bug

<details><summary>Answer</summary>

**B.**

</details>

**D12.** At checkout, which values should the server calculate itself? **(select all that apply)**

- **A.** Price
- **B.** Discount
- **C.** Total
- **D.** The ticket the customer chose

<details><summary>Answer</summary>

**A, B, C.** The customer chooses only the ticket and quantity, and the server validates both.

</details>

---

*Good luck in the exam. If you spot an error, please let me know so these notes stay accurate for everyone.*
