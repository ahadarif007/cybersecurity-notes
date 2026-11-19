# CSSP Revision Notes: Cybersecurity and Secure Programming

*Exam-focused notes on the module's core topics, built for multiple-choice exams: memory tricks, exam traps and practice questions.*

> **About these notes.** A summary of the module's lecture notes, [slides](CSSP%20Complete%20Lecture%20Slides.pdf) and labs. The priorities and memory tricks are my own, not the lecturer's. Real organisations (HSE, Adobe, First American, TalkTalk and others) are incidents discussed in lectures. Cedar Home, Riverton Live, Lina, Omar, Ada and Ben are fictional lab scenarios.

**Priority:** ★★★ **must know**, learn it by heart · ★★ **important**, spot it among the options · ★ **useful context**, read once

---

## Contents

**Start here:** [How to use these notes](#how-to-use-these-notes) · [MCQ strategy](#mcq-strategy) · [The 7 big ideas](#the-7-big-ideas)

**⚡ Exam in 5 minutes?** Read the [last-minute summary](notes/last-minute-summary.md): everything below on one page.

**Core topics** (on this page)
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

**Apply and review** (separate pages)

11. [Lab walkthroughs](notes/11-lab-walkthroughs.md)
12. [Calculations](notes/12-calculations.md)
13. [Final review sheet](notes/13-final-review-sheet.md)
14. [Practice MCQs](notes/14-practice-mcqs.md)

**Appendix: beyond the core scope** (separate pages)
- [A. SQL injection](notes/appendix-a-sql-injection.md)
- [B. OS command injection](notes/appendix-b-os-command-injection.md)
- [C. Week 4 and Week 7 labs](notes/appendix-c-week-4-and-week-7-labs.md)
- [D. Practice MCQs](notes/appendix-d-practice-mcqs.md)

---

## How to use these notes

**Scope.** Sections 1–10 are the core assessed topics, and section 11 covers the labs behind them. The appendix (SQL injection, OS command injection, Week 4 and Week 7 labs) was outside the listed scope of the first assessment but matters for the final assessment and the project. OWASP A05 (Injection) is core either way.

**A 90-minute plan**

| Step | Time | What to do |
|---|---|---|
| 1 | 10 min | Read the [MCQ strategy](#mcq-strategy) and [the 7 big ideas](#the-7-big-ideas). They decide most "tricky" options |
| 2 | 40 min | Work through sections 1–10. Learn every ★★★ item, and read each **⚠️ Exam traps** box twice |
| 3 | 10 min | Do the [calculations](notes/12-calculations.md) without looking at the answers |
| 4 | 25 min | Do the [practice MCQs](notes/14-practice-mcqs.md). Reread the section behind every wrong answer |
| 5 | 5 min | On exam morning, read only the [last-minute summary](notes/last-minute-summary.md) and the [final review sheet](notes/13-final-review-sheet.md) |

---

## MCQ strategy

This module tests **precision**. Most wrong options are *almost* right: they overclaim, swap two similar terms, or offer a fix that only hides the problem.

| When an option… | Think… |
|---|---|
| says **proves, guarantees, always, never, completely, eliminates** | Usually false. Evidence is a *clue* and most controls *reduce* risk. Look for the exception |
| hides something (a button, an error, a field on screen, a deleted key) | Hiding is not fixing. The data or capability is still there |
| reduces impact (non-root account, random IDs, WAF, SameSite, firewall) | A useful *extra layer*, not a fix for the root cause |
| puts the check in the browser | Not enforced. **The server decides** |
| swaps a paired term (authentication/authorisation, Strict/Lax, 401/403) | Check the exact word. See [Pairs people mix up](notes/13-final-review-sheet.md#pairs-people-mix-up) |
| is a *first step* in incident handling | **Contain** and **revoke** before you clean up or rewrite history |

**"Select all that apply":** judge each option as its own true/false statement; a *partly* wrong option is wrong; any number can be correct, from one to all; beware the right keyword with the wrong claim ("HttpOnly *prevents XSS*").

**"What can you conclude?":** pick what the record **shows**, not what it might **mean**. The best option often says "warrants investigation" or "check an independent source".

---

## The 7 big ideas

Most exam traps are one of these in disguise.

1. **A clue is not proof.** A record shows what happened on a system, not who did it or why.
2. **Security is a relationship, not a product:** what needs protecting, what threatens it, and what happens if protection fails.
3. **Any single control can fail,** so use layers: Prevent → Detect and respond → Recover.
4. **Be specific.** A good threat names actor, action, target and impact. "Broken access control" alone is not a threat statement.
5. **Keep similar things separate.** Many questions test two close terms, such as authentication and authorisation.
6. **The server decides.** The user controls the browser, so only server-side checks are enforced. Tell outsiders as little as possible; keep the detail where only your team can read it.
7. **Removing or hiding is not fixing.** A deleted key still works and stays in Git history. A deleted malware file can leave the attacker's access behind. A hidden button doesn't stop a direct request.

---

# Core topics

## 1. Cybersecurity foundations

### CIA triad ★★★

💡 *The three goals of information security: keep data secret from the wrong people, accurate, and available when needed.*

| Objective | Question | Example controls |
|---|---|---|
| **Confidentiality** | Who can see it? | Access control, encryption |
| **Integrity** | Can it be trusted? | Hashes, digital signatures, audit trails |
| **Availability** | Is it there when needed? | Backups, redundancy, capacity planning |

- **Objectives can conflict.** Emergency records must be available fast; therapy notes need tighter access. Choose by the likely harm.
- **A control can backfire.** A strict password policy helps confidentiality on paper but hurts it if people write passwords down or reuse them.

### Threat, vulnerability, impact, risk ★★★

Example: an admin API exposed to the internet with no login.

| Term | Question | In the example |
|---|---|---|
| **Threat** | What could cause harm? | Automated internet scanning |
| **Vulnerability** | What weakness makes it possible? | No authentication on the API |
| **Impact** | What harm results? | Private records read or changed |
| **Risk** | How much attention does it deserve? | Likelihood and impact together |

```text
Threat ──exploits──> Vulnerability ──leads to──> Impact
(scanning)           (no authentication)         (records read or changed)
```

**Risk ≈ likelihood × impact.** Use it to compare and prioritise. It isn't an exact calculation: both inputs are estimates.

### Attack surface vs attack vector ★★

**Surface = where** contact is possible: interfaces, data entry points, identities and credentials, dependencies. **Vector = how** an attacker uses that contact. The login form is **surface**; credential stuffing (trying username/password pairs leaked from other sites) is the **vector**.

### HSE ransomware incident, 2021 ★★

💡 *The HSE is Ireland's public health service. A 2021 ransomware attack forced it to shut down its IT systems.*

```text
18 Mar 2021              31 Mar                             14 May
─────●──────────────────────●──────────────────────────────────●──────>
  Phishing email +       Antivirus DETECTS activity, but    Ransomware:
  malicious attachment   is in MONITORING MODE, so it       major service
  -> foothold            blocks nothing. Third-party        disruption
                         monitoring not continuous overnight
```

- **Scale:** data on about 500 patients published online; more than 100,000 people needed contacting by November 2022. The figures measure different things at different stages.
- **All of CIA was hit:** data disclosed (C), systems and accounts compromised (I), services down (A).
- **Lesson:** detection without response doesn't stop an attack, and an incident is a *sequence* of stages, not one event.

### Threat actors ★

💡 *The people or groups behind attacks, and what drives them.*

| Actor | Typical goal or behaviour |
|---|---|
| **Cybercriminals** | Money: theft, fraud, extortion |
| **State-linked groups** | Intelligence, influence, disruption. Patient and persistent |
| **Insiders** | Already trusted. Malicious, coerced or simply **mistaken** |
| **Opportunists** | Scan widely and exploit whatever they find |

Motive is a **hypothesis**, not attribution. It shapes the **target**, **tempo** and **what detection looks like**: quick money → broad scanning and visible impact; long-term spying → narrow targets, persistence, staying hidden.

### Defence in depth and zero trust ★★

**Defence in depth** assumes any single control can fail.

```text
attack ──> [ Prevent ] ──if it fails──> [ Detect & respond ] ──if it fails──> [ Recover ]
```

| Layer | Purpose | Examples |
|---|---|---|
| **Prevent** | Reduce exposure | Patching, strong authentication, least privilege |
| **Detect and respond** | Find and contain | Monitoring, alert review, incident response |
| **Recover** | Restore and learn | Tested backups, recovery plans, post-incident changes |

**Zero trust** is an architecture and set of principles, **not a product**. Being inside the network earns no trust. Authenticate and authorise explicitly, per session · decide on identity, device and context · grant only what the task needs (least privilege) · keep re-checking, because trust changes.

### Secure programming habits ★

Validate input · Enforce authorisation on the server · Encode output for its destination · Handle errors safely · Use safe defaults · Manage dependencies · Log security events

> **⚠️ Exam traps**
> - Risk is **not** an exact number. Likelihood × impact is only for comparing.
> - Detection is **not** protection. HSE detected the activity and still suffered the attack.
> - A likely motive does **not** identify the attacker.
> - Zero trust is **not** something you buy, and the internal network grants nothing.

---

## 2. Threat modelling

A structured way to understand a system, find plausible ways it could fail or be misused, and decide what to do about them. It pays off most **during design** and is revisited whenever the system changes.

- **Three traits:** multidisciplinary, decision-focused, maintained.
- **Across the lifecycle:** Requirements (set objectives) → Design (analyse threats, choose controls) → Implementation (apply and test) → Operation (monitor and reassess).
- **Why start early:** fixing a defect after delivery can cost up to **100 times** fixing it in requirements and design (Boehm and Basili, 2001). A rough guide, not a universal rule.

### Shostack's Four Questions ★★★

💡 *Adam Shostack's four-question framework for running any threat model.*

| # | Question | What you do |
|---|---|---|
| 1 | **What are we working on?** | Define scope, users, data and boundaries |
| 2 | **What can go wrong?** | Trace data flows, mark trust boundaries, apply STRIDE |
| 3 | **What are we going to do about it?** | Choose a response and justify it |
| 4 | **Did we do a good enough job?** | Review coverage, assumptions and whether controls work |

🧠 **"Work, Wrong, Do, Done."** It's a cycle, because design changes can introduce new threats:

```text
Work ──> Wrong ──> Do ──> Done ──┐
 ^                               │
 └──── system or design changes ─┘
```

### Building a system model ★★

1. **List the parts:** actors, processes, data stores, external systems. Include third-party services and dependencies, not just your own code.
2. **Describe each important data flow:** source, destination, data carried, identity involved and any **change in privilege**.
3. **Mark a trust boundary** wherever data crosses between components with different trust or control (e.g. a browser sending a note to a web app).
4. **Record assumptions and limits,** so others know what the model covers and leaves out.

```text
   untrusted     ┊           server side (trusted)
                 ┊
[Browser] ──note─┊──> (Web app) ──> [Database]
  actor          ┊     process       data store
                 ┊        └──> {Payment API}   external system / dependency
          trust boundary: data crosses into a zone with different trust
```

The output can be a diagram, threat table or design notes; what matters is that every finding leads to a **documented decision**. A model is a map for asking questions, not a guarantee that every threat has been found.

### STRIDE ★★★

💡 *A checklist of six threat types, one per letter, each breaking one security property.*

| Letter | Threat | Property violated | Example |
|---|---|---|---|
| **S** | Spoofing | **Authentication** | Reusing a stolen session token |
| **T** | Tampering | **Integrity** | Changing an account ID to edit someone else's record |
| **R** | Repudiation | **Accountability** (non-repudiation) | An admin deletes a record and no trustworthy audit event exists |
| **I** | Information disclosure | **Confidentiality** | Changing a URL reveals another user's record |
| **D** | Denial of service | **Availability** | Huge uploads fill storage and lock users out |
| **E** | Elevation of privilege | **Authorisation** | Submitting `role=admin` to reach admin functions |

- STRIDE finds threat **types**. It gives **no** likelihood, impact or priority; assess those separately.
- **For denial of service, ask "what is finite?"** Time, memory, storage, connections, money, human attention.

### Writing a good threat ★★

**Who + action + target + consequence:** *"An unauthenticated attacker changes a record identifier in a request and reads another user's private record."*

### Four responses to a threat ★★★

| Response | Meaning |
|---|---|
| **Eliminate** | Remove the feature, path or assumption that creates the threat |
| **Mitigate** | Reduce likelihood or impact with controls |
| **Transfer** | Hand agreed parts of the risk to a provider or insurer. You still need oversight |
| **Accept** | Keep the risk knowingly, with a reason and a named owner |

🧠 **"Every Mistake Teaches Answers"** (Eliminate, Mitigate, Transfer, Accept). For each threat, record the **decision, rationale, owner and residual risk**.

> **⚠️ Exam traps**
> - STRIDE does **not** rank or prioritise threats.
> - Repudiation → **accountability**, not integrity. Elevation of privilege → **authorisation**, not authentication.
> - Transfer does **not** remove responsibility. Accept does **not** mean ignore: it needs a reason and an owner.
> - Threat modelling is **not** one-off. Revisit it whenever the system changes.

---

## 3. Data hiding

### Steganography vs cryptography ★★★

💡 **Steganography:** hiding a secret message inside an innocent-looking file (the *carrier*), such as an image. **Cryptography:** scrambling a message so only someone with the key can read it.

| | The question it answers | What it protects |
|---|---|---|
| **Cryptography** | "What does this message say?" | The **contents** |
| **Steganography** | "Is there a message here?" | The **existence** of the message |

- Steganography alone does **not** make content confidential. The two can be combined, but neither guarantees the message goes undetected.
- **Model:** message + carrier + embedding method → **stego object**.
- **Legitimate uses:** watermarking, protecting people at risk. **Abuse:** covert data transfer, command-and-control.

### Embedding techniques ★★

💡 *The methods used to write a hidden message into a carrier without visibly changing it.*

| Family | How it works |
|---|---|
| **Spatial (LSB)** | Changes low-order bits of pixel values |
| **Transform-domain** | Changes coefficients of a transformed carrier, such as DCT (the transform JPEG uses) or wavelet |
| **Spread-spectrum** | Spreads the signal thinly across the carrier |

**LSB (least significant bit):** the rightmost bit, which changes the value by at most 1, too little to see. The last bit becomes the message bit.

```text
 44 = 0010110[0]  -- embed 1 -->  0010110[1] =  45
101 = 0110010[1]  -- embed 0 -->  0110010[0] = 100
```

Re-saving, resizing or converting (e.g. to lossy JPEG) can destroy an LSB payload.

**Steganalysis** (detecting hidden messages) **layers its checks:** file properties (size, format, metadata), statistical patterns, visual or audio artefacts, context and behaviour. Simple checks can miss careful embedding. A known original helps, but often there isn't one.

### Shannon entropy ★★

💡 *A score for how unpredictable data is. Repetitive data scores low; encrypted or compressed data scores high.*

**H(X) = −Σ p(x) log₂ p(x)** measures uncertainty: value always the same = **0 bits** · fair coin = **1 bit** · all 256 byte values equally likely = **8 bits** (the maximum per byte).

- **High entropy** can mean compressed, encrypted, packed or random data. A **clue, not a verdict**.
- A high score does **not** prove a generator is cryptographically secure. For keys and tokens, use the platform's **CSPRNG** (cryptographically secure random number generator).

### NTFS alternate data streams (ADS) ★★

💡 *A hidden extra "compartment" of data attached to a file on NTFS, the standard Windows file system.*

```text
report.csv                  <- what a basic listing shows
 ├── (default, unnamed)     <- the only stream a normal hash covers
 ├── :review                <- named stream: report.csv:review
 └── :Zone.Identifier       <- legitimate: where a download came from
```

- An NTFS file has a default (unnamed) data stream and can have **named streams**. Basic listings, and programs that read only the default stream, may not show them.
- A normal file hash covers the **default stream only**. A stream doesn't run itself: a process has to read it or act on it.
- **`Zone.Identifier`** is a legitimate stream Windows can use to record where a download came from. **An ADS is not suspicious in itself.**
- ADS exist because NTFS supported Macintosh **resource forks**. **Sysinternals Streams** lists them.
- **Abuse:** hiding data beside an innocent file, storing tools or payloads, one step in a persistence chain.

```powershell
Get-Item -LiteralPath .\sample.txt -Stream *            # list all streams
Get-Content -LiteralPath .\sample.txt -Stream review    # read one named stream
```

**Steganography vs ADS:** steganography hides data *inside the carrier's content*; an ADS attaches a *separate named stream* to the file.

> **⚠️ Exam traps**
> - Steganography hides **existence**, not meaning. No confidentiality on its own.
> - High entropy does **not** prove hidden data, encryption or malware.
> - An ADS does **not** prove malicious activity, and a normal hash does **not** cover it.
> - An image that looks normal is **not** proven unchanged. Compare hashes.

---

## 4. OWASP Top 10 (2025)

💡 *OWASP (Open Worldwide Application Security Project) publishes this ranked list of the ten most critical web application security risks.*

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
| **A04: hashing vs encryption** | Hash passwords with a password-specific scheme. Encrypt data you need to read back |
| **A05: parameters vs output encoding** | They protect different interpreters (database vs browser). Neither replaces the other |

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
> - Parameterised queries do **not** stop XSS; output encoding does **not** stop SQL injection.
> - A09 is about **alerting** too, not just logging. A log nobody acts on fails.

---

## 5. Security signals and evidence

💡 *Reading the traces a system leaves (logs, file metadata, registry values) to work out what happened, without overclaiming.*

### Observe → Interpret → Test ★★★

| Step | What you do |
|---|---|
| 1. **Observe** | Cite the exact file, event, account, path, source and **UTC** time |
| 2. **Interpret** | State a hypothesis and why the combination is unusual |
| 3. **Test** | Check an **independent** source: process, file, endpoint or network evidence |

**Lecture example (fictional):**

```text
10:14–10:16  3 failed network logons        (4625, logon type 3)
10:18        success, same account+source  (4624)
10:21        new startup entry             (Run key)
10:24        new USB storage device        (USBSTOR)
             => WARRANTS INVESTIGATION
```

It does **not** prove malware ran or data was stolen, and benign explanations need ruling out.

**Three questions for any file:** **Identity** (what is it?), **Integrity** (has it changed?), **Activity** (what happened to it, and when?).

### File checks: each answers a different question ★★

| Check | What it tells you | Limit |
|---|---|---|
| **Extension** | The type the file *claims* to be | Anyone can change it |
| **Signature** (magic bytes) | Whether the first bytes match a format | A DOCX is a ZIP, so a ZIP signature doesn't prove a valid document |
| **Parser** | Whether the whole structure is valid for the format | Structure only, so apply policy checks too |
| **SHA-256** (a hash: a fingerprint of the bytes) | Whether the bytes match a trusted baseline | Can't say who changed it, when or why. The baseline must be trustworthy |

**File intake:** signature **matches** → continue with parsing and policy checks · **conflicts** with the extension → hold, reject or quarantine under policy, and record why · **unknown** → record it and inspect further (unknown is **not** proof of malware).

### Windows evidence ★★★

💡 *Windows records activity in **event logs** (numbered events) and the **registry** (its central settings database).*

| Item | Meaning |
|---|---|
| **Event 4624** | Successful logon |
| **Event 4625** | Failed logon |
| **Logon type 2** | Interactive (at the keyboard) |
| **Logon type 3** | Network |
| **Event 4688** | Process creation. Logged only if that audit policy is enabled; command-line capture is a separate setting |
| **Run keys** | Start programs at sign-in. Used by legitimate software and malware alike. Persistence (surviving a reboot) technique **MITRE ATT&CK T1547.001** (ATT&CK: a public catalogue of attacker techniques) |
| **USBSTOR** | A USB storage device was seen. It does **not** prove a file was copied |
| **HKCU / HKLM** | Settings for the current user / the whole machine |

- **Where to look:** the **Security log** has authentication and audit events; the **System log** has OS and service events. Record log name, provider, event ID, timestamp and key fields (account, source, logon type). Read the XML view when the summary is unclear.
- **Registry values need a baseline.** A current value doesn't show when it first appeared, so compare with a trusted earlier snapshot before calling it "new". Recent-document lists and typed paths are leads about user activity.
- **USB detail:** phones can appear under other device classes. `SetupAPI.dev.log` adds installation context, but the install time isn't the time of every later connection.
- Don't change audit settings on shared lab machines to hunt for past evidence.

### Timelines and application logs ★★

💡 *A timeline puts events from different sources into one time order, so you can see what happened in sequence.*

- **Keep both times:** the original with its UTC offset *and* the converted UTC, e.g. **08:40 +01:00 = 07:40 UTC** (subtract a positive offset). **Normalise to UTC before sorting** records from different systems.
- **Not the event time:** a ZIP extraction time, a file's display time, a local clock setting. Note each record's source system and any clock uncertainty.
- **A good application log entry has** an unambiguous time, action, outcome, resource, and a correlation or request ID.
- **Never log** passwords, session tokens or unnecessary personal data. A rejected upload can record the decision and reason without storing the file's secret or the user's token.

> **⚠️ Exam traps**
> - A suspicious sequence warrants investigation. It is **not** proof of compromise.
> - An unknown signature is **not** malware; a matching signature is **not** proof the file is valid or safe.
> - A SHA-256 mismatch shows *that* the file changed, **not** who, when or why.
> - A missing log entry does **not** prove nothing happened. Audit settings, retention or permissions may explain it.
> - A new Run key value is **not** proof of malware. A USBSTOR record does **not** prove a file was copied.

---

## 6. NTFS security analysis

💡 *NTFS is the default Windows file system. Its metadata records which files exist and how they changed, so it can serve as evidence.*

### Key terms ★★

| Term | Meaning |
|---|---|
| **Cluster** | The smallest unit of disk space the file system allocates, e.g. 4,096 bytes |
| **MFT record** (Master File Table) | Identifies a file and holds its attributes. Usually **1,024 bytes** |
| **File reference, e.g. `4128:7`** | MFT record **4128**, sequence number **7**. The sequence number goes up when a record is reused, so a new file isn't mistaken for an old one |
| **Resident data** | Small enough to live inside the MFT record |
| **Nonresident data** | Stored in clusters elsewhere, located through **data runs** (lists of start cluster + length) |
| **VCN / LCN** | Virtual cluster number (position **in the file**) / logical cluster number (position **on the volume**) |
| **Slack** | Allocated space after the file's logical end. Not part of the current content |

```text
Data runs (start 120, len 2) and (start 900, len 1):

File position (VCN):      0       1       2
                          │       │       └──────────────┐
Volume position (LCN):  [120]   [121]   ...  ...  ...  [900]
                        └─ run 1: 2 clusters ─┘          └ run 2: 1 cluster

Last cluster:  |#########...........|   # = file data   . = slack (allocated, not content)
```

**Storage hierarchy:** disk image → partition → volume → directory entry → MFT record → attributes and streams

**Attribute type codes:** `0x10` = $STANDARD_INFORMATION · `0x30` = $FILE_NAME · `0x80` = $DATA

- A **rename** keeps the same file reference and changes only the name or parent directory.
- **USN rename records:** a `RENAME_OLD_NAME` and a `RENAME_NEW_NAME` record with the **same file reference**. Reason flags can be combined.

### Timestamps: SI vs FN ★★★

💡 *NTFS keeps two sets of timestamps per file, in two different attributes. Comparing them can reveal copying, restoring or tampering.*

- NTFS records **creation, last write, MFT-record change and last access** times. Last-access updates may be delayed or disabled.
- **$STANDARD_INFORMATION (SI)** holds the common timestamps; some can be set through ordinary Windows APIs.
- **$FILE_NAME (FN)** holds a second set, stored with the filename and parent reference, and may update differently.
- **If SI and FN disagree,** investigate: a copy or restore, application behaviour or deliberate tampering. **A mismatch alone proves none of them.**

**Lecture example (synthetic record 4128:7):** SI creation 2026-09-01 08:00:00 UTC, FN creation 2026-09-28 09:14:00 UTC, SI last write 09:14:02 UTC. An inconsistency to investigate, not proof of tampering. Compare change records and application logs, and test copy, rename and restore behaviour before naming a cause.

### Evidence sources ★★★

💡 *The NTFS system files an investigator reads, and what each can and can't tell you.*

| Source | Question it answers | Limit |
|---|---|---|
| **$MFT** | Which records, names and allocation states exist? | Unused records may keep attributes but not content |
| **$UsnJrnl:$J** (USN = update sequence number change journal) | Which changes were recorded? | Limited retention. **Doesn't identify the process or person** |
| **$LogFile** | What supports file-system consistency and recovery? | Not a permanent history of user actions |
| **Application / process logs** | Which operation might explain a change? | Coverage, identity and clock reliability need checking |

USN numbers order records **within one journal** only.

### Deletion ★

Deletion guarantees **neither recovery nor erasure**: you might recover the whole file, metadata only, fragments or nothing. Cluster reuse, TRIM and encryption reduce what's recoverable. Report partial recovery as partial.

### Case study: the missing file ★★

An application writes `C:\Reports\report.csv`; twenty minutes later it's gone. Renamed, deleted, replaced or normal cleanup?

| Record | What it supports |
|---|---|
| USN 984320: `RENAME_OLD_NAME`, `report.csv`, ref 4128:7 | The old side of a rename |
| USN 984416: `RENAME_NEW_NAME`, `cache.tmp`, same ref 4128:7 | The new side: the **same file object** was renamed |
| A later unused MFT record for 4128:7 | The file was then deleted |
| App log: `export.complete`, request r-204, account `report-service` | Context for the export. A service account is **not** the human requester |

None of these shows **who**, **which process** or **why**. Check job details, cleanup settings, process telemetry, clock settings and where each record came from.

**Workflow:** **Inspect** (browse the volume, e.g. in Autopsy) → **Compare** (metadata, content, application logs) → **Explain** (what's supported and what's uncertain).

### Automated review and design lessons ★

- **A metadata-parsing script should:** validate fields and formats; record parser version and settings; keep original timestamps and make UTC conversions explicit; correlate by **volume + full file reference**; sort within a journal by USN; flag rename pairs, deleted-file candidates and timestamp differences; keep source rows and show parsing errors.
- **For application design:** set retention and cleanup rules for exports and temp files; check what copy, export and scanning tools do with named streams and deleted data; log job IDs, operations, paths, outcomes and clear times, without secrets.

### File systems compared ★★

💡 *FAT32 and exFAT are simpler file systems, common on USB drives and memory cards.*

| Feature | FAT32 | exFAT | NTFS |
|---|---|---|---|
| Allocation | Cluster chains in the FAT | Allocation bitmap (chains may also be used) | Attributes and data runs in MFT records |
| Windows ACLs | No | No | **Yes** |
| Named streams (ADS) | No | No | **Yes** |
| Journal / change log | No | No (standard exFAT) | **Yes** (if present and retained) |

- **FAT32 chain:** the directory entry gives the first cluster (5); the FAT links 5 → 9 → 12 → END. The file is read in chain order, even when clusters are far apart.
- **FAT32 deletion:** the directory entry's first byte becomes **0xE5** and the chain can be released. Without the chain, you may not know cluster 9 followed cluster 5.
- **Tools:** **Autopsy** browses a volume. **MFTECmd** parses `$MFT` and `$J` into CSV.

> **⚠️ Exam traps**
> - SI/FN disagreement is **not** proof of tampering.
> - A USN record does **not** say which person or process made the change.
> - A service account in a log is **not** the human who asked for the action.
> - A deleted file is **not** guaranteed to be recoverable, or erased.
> - VCN = position **in the file**, LCN = position **on the volume**. Don't swap them.

---

## 7. Information disclosure and safe failure

**CWE** (Common Weakness Enumeration) is a numbered catalogue of software weakness types. Information leaks through five routes: **responses, error messages, login behaviour, source files and logs**. 🧠 **"Real Engineers Log Secrets Loosely."**

### Key terms ★★

| Term | Meaning |
|---|---|
| **Information disclosure (CWE-200)** | Sensitive information reaches someone who shouldn't have it. A broad class |
| **Excessive data exposure** | An API returns more properties than the client needs (an older OWASP API Top 10 name) |
| **BOPLA (OWASP API3:2023)** | Broken Object Property Level Authorization: reading **or changing** individual fields without permission |
| **Error-message exposure (CWE-209)** | An error reveals details about users, data or the environment |

### Response design ★★★

- **Input validation** asks "is what came in acceptable?" **Response design** asks "what should go back, to whom, and in what form?" A valid request from a correctly signed-in user can still leak data.
- **The response path, and the example profile page:**

```text
Data store ──> Application ──> Representation ──> Delivery
               (picks fields)  (JSON, HTML, file)  (headers, caches, client)

JSON sent:  displayName  course │ email  studentNumber  accountStatus  recoveryToken
Page shows: displayName  course │ (not shown, but the client still received them)
            └──── needed ─────┘
```

- **The page shows part of the response, but the client receives all of it.** Check what was sent in Developer Tools → Network → Fetch/XHR → Response. A recovery token is credential material and never belongs in a profile response.
- **Object level vs property level:** "may you see this record?" vs "which fields may you read or change?" Ownership can be checked correctly while a serializer still leaks an internal field.
- 🧠 **Three questions per field: Need, Audience, Permission.** Choose the fields to send (a **response contract, i.e. an allowlist**). Sending everything and stripping known secrets (a blocklist) breaks as soon as someone adds a new internal field. Hiding a field in the front end enforces nothing.
- **Other channels leak too:** headers, downloads, exports, generated documents, caches. Use `Cache-Control: no-store` for sensitive responses.
- **Disclosure vs output encoding:** *what* is sent vs *how* untrusted text is displayed safely. Fixing one doesn't fix the other.

### Safe errors ★★

```text
           ┌──> PUBLIC (user):  "We could not complete that request. Reference: R7Q2."
           │                    failure + safe next step + opaque reference
Failure ───┤                    NEVER: stack trace, query text, paths, versions
           │
           └──> RESTRICTED LOG (team):  R7Q2 + event, time, technical context
                                        NEVER: passwords, tokens, unneeded personal data
```

The shared reference links the two records. Also check the failure didn't leave a half-finished change behind.

### Username enumeration ★★★

💡 *Working out which accounts exist from how the login page responds.*

- "Password is incorrect" vs "No account with that address" reveals which accounts exist, **even when the HTTP status is the same**.
- **Timing leaks too,** even with identical messages. A **repeatable** delay is a clue; one slow response proves nothing.

```text
unknown user ──> rejected at once ............ fast
real user    ──> slow password check ......... slow   <- repeatable gap reveals real accounts
```

- **Fix:** an established login flow that does comparable work for both failures, plus attempt limits and abuse monitoring. Changing the message alone isn't enough.
- A list of valid accounts helps phishing and password attacks, but doesn't prove any account was compromised.

### Secrets and Git ★★★

- An **API key** is a credential one program presents to another service.
- Keep keys out of shared source and browser files. Supply them through protected server configuration or a secret store, with only the permissions needed, and make them replaceable. **If a required key is missing, stop safely, with no fallback value.** Moving a key out of source doesn't automatically secure its new location.
- **Git keeps history.** A key committed Monday and deleted Tuesday is still in Monday's commit. `.gitignore` only affects *untracked* files and doesn't touch history.
- **An exposed `.git` folder** on a web server lets anyone rebuild the source *and its history*, removed secrets included. Deploy only built public files and block Git paths. Disabling directory listing doesn't help (the file names are well known). No public GitHub repository is needed for this to happen.

```text
Mon: commit A adds key ──> Tue: commit B deletes key ──> key still in commit A, and still WORKS
```

**Leaked secret: four steps, in order.** 🧠 **"Can Robbers Inspect Code?"**

1. **Contain:** cut off access to the exposed files.
2. **Revoke or rotate:** kill the credential and invalidate the old value.
3. **Investigate:** check how it was used and preserve evidence.
4. **Clean:** remove it from history and other copies, under a coordinated plan.

**Revoke** disables a value; **rotate** replaces it and retires the old one. Cleaning history can't recall copies already downloaded, which is why revoking comes first.

### Logging safely and failing closed ★★

- **Log selected fields, not whole forms:** `audit({ event: "sign_in_failed", requestId, outcome: "denied" })`.
- Record failed sign-ins, refused access and permission changes. Leave out passwords, tokens and complete form submissions, and restrict who can read logs.
- If secrets were already logged, deal with the stored copies **and** the exposed credentials.
- **If permission can't be checked, refuse (fail closed).** A permission service that's down hasn't granted anything, so refuse the export. Decide **before** releasing data: an error shown afterwards can't take the data back.

> **⚠️ Exam traps**
> - Hiding a field on the page does **not** stop it being sent.
> - Identical failed-login messages do **not** fully stop enumeration: timing can still leak.
> - Deleting a secret from code or adding the file to `.gitignore` does **not** remove it from history or stop it working.
> - A generic error page does **not** fix the bug. It limits what the bug reveals.
> - Fail **closed** (refuse when unsure), never **open**.

---

## 8. Malware and ransomware

💡 *Malware is software made to harm or misuse a computer, its data or its users. Ransomware is malware that blocks access and demands payment.*

### Basics ★

- A computer can be malware's **target** (files encrypted, data stolen), its **tool** (sending spam, relaying attacks) or **both** (a hijacked account fooling its contacts). The owner may notice nothing.
- **Crimeware roles:** developer, access seller, operator or affiliate, infrastructure provider. **Ransomware-as-a-service (RaaS)** supplies tools to affiliates. Naming the malware family doesn't identify who's behind an attack.

### Malware types ★★★

| Type | Defining feature | Key point |
|---|---|---|
| **Virus** | Inserts its code into a **host** program or file; runs when the host runs | Replication and payload (the damage) are separate parts |
| **Worm** | Spreads between systems **on its own, with no host** | Doesn't need a zero-day (a flaw with no patch yet). Widespread damage doesn't prove it's a worm |
| **Trojan** | **Looks legitimate** but hides malicious behaviour | Often staged, so deleting the installer doesn't remove later components |
| **Backdoor** | Provides **unauthorised access** | Not the same as a rootkit |
| **Rootkit** | Alters the system to **hide** activity | The system may report false information. Compare with trusted sources or examine offline |
| **Ransomware** | Blocks access to data or systems, then **demands payment** | Defined by its purpose |

- The labels describe different properties, so they overlap (a Trojan can deliver ransomware):

```text
How it SPREADS     virus (needs a host)  ·  worm (spreads alone)
How it PRESENTS    Trojan (looks legitimate)
What it's FOR      ransomware (blocks access, demands payment)
What it DOES       backdoor (gives access)  ·  rootkit (hides activity)
```
- 🧠 **"A virus needs a host, a worm walks alone, a Trojan wears a disguise."**
- **Polymorphic** malware changes its *wrapper* (varying encryption and decoder). **Metamorphic** malware *rewrites its own code* while keeping the same behaviour. Either way, one exact hash catches only one version.

### Delivery ★★

| Document mechanism | Control |
|---|---|
| Active content such as macros (small programs inside documents) | Restrict execution to trusted sources |
| An exploit against the document reader | Patch and isolate the application |
| A link to another resource | Check the destination and what it asks you to do |

- **Social engineering** (manipulating people rather than systems) uses impersonation, a believable pretext or a fake login page. Verify through a channel you already know (the normal web address, a known phone number), never one given in the message. Good spelling and branding prove nothing.
- **Scareware** fakes a security scan to get payment or an install; its "results" are just claims. **Ransomware** really does block access.

### Detection ★★★

💡 *How security tools recognise malware: by known patterns, suspicious traits or suspicious behaviour.*

- **Detection signature:** a pattern linked to malware. An exact hash identifies one known sample; broader signatures match related samples.
- **Digital signature:** shows **who signed** the software and that it **hasn't changed** since. **It does not prove the software is harmless.**
- **Heuristic detection** applies rules about suspicious traits. **Behavioural detection** watches what a program does: launching a scripting tool, rewriting many documents quickly, reading credentials, adding persistence. Context matters: backup software legitimately reads many files.

| Analysis | How | Limit |
|---|---|---|
| **Static** | Inspect without running: structure, strings, imports, signatures | Packing or obfuscation (compressing or disguising code) can hide the content |
| **Dynamic** | Run it in a controlled environment; watch processes, file changes, connections | Sees only behaviour triggered then and there. Malware may detect analysis or wait |

A sample that stays quiet in a dynamic test is **not** proven harmless.

### Ransomware ★★★

```text
encrypt live data ──> AVAILABILITY      (a backup restores this)
copy / steal data ──> CONFIDENTIALITY   (a backup does NOT undo this)
alter records     ──> INTEGRITY
```

- **Double extortion:** encryption plus a threat to publish stolen data. Data can also be stolen with no encryption at all.
- **A backup restores availability, not confidentiality.**
- A ransom note **doesn't prove** how much was encrypted or stolen, or that paying leads to recovery.
- **Paying** doesn't rebuild services, remove the attacker's access or prove stolen data was deleted.
- **Renaming the extension back** doesn't decrypt anything: the contents have changed.
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

**Forensics:** link the suspect file to its parent and child processes, accounts, network destinations and persistence changes. A file, a connection and a ransom note are separate observations; none tells the whole story.

> **⚠️ Exam traps**
> - Widespread disruption does **not** prove a worm.
> - A digitally signed program is **not** proven harmless.
> - No match against a known hash does **not** mean the file isn't a variant.
> - Identifying the malware family does **not** identify the attacker.
> - Deleting the malicious file does **not** remove the attacker's access.
> - Restoring a backup does **not** undo data theft. A completed backup job does **not** prove you can restore.

---

## 9. Authentication and password storage

💡 **Authentication** proves who you are (e.g. with a password). **Authorisation** decides what you may do. Password storage is about making a stolen password table useless.

### Three separate checks ★★★

| Point in the visit | Question | Check |
|---|---|---|
| Login | Is this valid evidence for the claimed account? | **Authentication** |
| Next request | Does it carry a valid session? | **Session validation** |
| Results page | May this account read these results? | **Authorisation** |

- **An IP address is not an identity.** People share addresses, and one person's address can change.
- A correct password lets authentication succeed. It does **not** authorise access to every record.

### How passwords should be stored ★★★

💡 **Hash:** a one-way fingerprint of data. **Salt:** a random value added to each password before hashing, so equal passwords hash differently.

If an attacker copies the account table: **plaintext** → readable immediately · **encrypted** → recoverable with the key · **password-hash record** → guesses can be tested, but the password itself isn't stored.

| Operation | What it does | Typical use |
|---|---|---|
| **Encryption** | Two-way: a key recovers the original | A document you need to read later |
| **Hashing** | One-way digest, no decryption step | Checking a file hasn't changed |
| **Password hashing** | Salted and **deliberately slow and costly** | Checking a submitted password |

**HTTPS protects the password in transit. Password hashing protects the stored copy.** You need both.

### Case study: Adobe, 2013 ★★

- Troy Hunt's analysis: **152,989,508 records**, **109,305,888** with a password hint.
- Passwords were **encrypted without salt**, so equal passwords gave equal ciphertexts. **3,011 accounts** shared one value with the hint "color": crack it once, expose all 3,011.
- **Hints gave clues away.** Of the **top 100 hint strings**, 24 were family names: 24 of 100 *strings*, **not 24% of accounts**.
- Comparing breaches (Adobe hint vs Sony password): "fruit" → grapefruit, "name birthyear" → mike1953.
- **Lessons:** salted, costly password hashing, and a reset flow instead of stored hints.

### Saving and verifying a password ★★

```text
SAVE    password ──HTTPS──> fresh random salt ──> hash(password, salt, costs) ──> store hash + salt + settings
VERIFY  attempt ──> load stored record ──> hash(attempt, STORED salt, STORED costs) ──> equal? ──> yes / no
```

The password is never stored, not even in logs. **Nothing is decrypted.** (In the Python library the function is `verify()`; Argon2id is the algorithm.)

### Argon2id ★★★

💡 *A modern password-hashing algorithm, deliberately slow and memory-hungry so that every guess costs an attacker.*

| SHA-256 | Argon2id |
|---|---|
| Designed to be **fast** | Designed for **password hashing** |
| Good for file-integrity checks | Adjustable **time and memory** cost |
| Each guess is cheap | Each guess is deliberately expensive |

- **A salt stops identical-hash and precomputed (rainbow table: a ready-made hash → password lookup) shortcuts, but adds no cost.** Cost comes from the algorithm's settings. Don't build your own loop around a fast hash.
- **Variants:** **Argon2i** = data-*independent* memory access (resists side-channel attacks, which spy on timing or memory access) · **Argon2d** = data-*dependent* access (resists memory-saving trade-off attacks) · **Argon2id** = **hybrid**, i-style for the first half of the first pass, d-style after.

| Parameter | Meaning | Lecture example |
|---|---|---|
| **m** | Total memory per calculation (KiB) | 65,536 KiB = **64 MiB** |
| **t** | Passes over that memory | **3** |
| **p** | Parallel lanes in one calculation | **4** (16 MiB each) |

- The lecture example is the **RFC 9106 low-memory profile**. The Week 6 lab uses the **OWASP minimum**: m = 19,456 KiB (19 MiB), t = 2, p = 1.
- **The server pays the cost too,** so benchmark concurrent logins and limit how many verifications run at once.

**Stored record:**

```text
$argon2id $v=19 $m=65536,t=3,p=4 $<salt> $<hash>
 │         │     │                 └─ Base64 ─┘  salt and hash are Base64-encoded (encoding, not encryption)
 │         │     └ cost settings: 64 MiB, 3 passes, 4 lanes
 │         └ algorithm version: decimal 19 = 0x13, NOT the year 2019
 └ algorithm
```

Store the whole record: verification needs the algorithm, salt and settings.

> **⚠️ Exam traps**
> - A salt makes equal passwords hash differently. It does **not** slow down guessing.
> - SHA-256 is **not** a password hash: it's fast by design.
> - Password verification does **not** decrypt anything.
> - Adobe's "24 family names" = 24 of the top 100 hint strings, **not** 24% of users.
> - `v=19` is a version number (0x13), **not** a year.
> - A matching password does **not** mean the user may access every record.

---

## 10. Sessions, cookies and access control

💡 *After login, a **session** lets the server recognise the user on later requests without resending the password. **Access control** decides what that user may do.*

### From password to session ★★★

```text
Browser                                          Server
   │── sign in (password, over HTTPS) ──────────────>│ verify password, create session record
   │<── Set-Cookie: opaque, unpredictable ID ────────│
   │                                                 │
   │── later request, Cookie: ID (never password) ──>│ 1. session valid?                 no -> 401
   │                                                 │ 2. allowed: record/action/fields? no -> 403
   │<── 200 + only the permitted fields ─────────────│
```

```http
Set-Cookie: __Host-session=<id>; Path=/; Secure; HttpOnly; SameSite=Lax; Max-Age=1800
Cookie: __Host-session=<id>
```

- The browser's `Cookie` header sends **only name and value**. The attributes tell the browser how to handle the cookie.
- **A session ID is a credential.** Keep it out of URLs, screenshots and logs, and issue a new one at login or when privilege changes.

### Cookie attributes ★★★

💡 *A cookie is a small name=value item that a site asks the browser to store and send back on later requests. Attributes set the rules for that.*

| Attribute | What it does |
|---|---|
| **Secure** | Sent only over HTTPS |
| **HttpOnly** | Page JavaScript can't read the cookie, but the browser still sends it. **It doesn't stop script injection (XSS)** |
| **SameSite** | Controls whether the cookie is sent on cross-site requests |
| **Max-Age=1800** | Kept for 1,800 seconds = **30 minutes** |
| **Path=/** | Sent for every path on the host. Grants no access to any record |
| **`__Host-` prefix** | Requires **Secure**, **Path=/** and **no Domain** attribute, so the cookie stays on the exact host that set it |

Cookie settings never replace server-side access checks.

### SameSite ★★★

💡 *A cookie attribute that decides whether the browser sends the cookie when a request comes from another site.*

| Request type | Strict | Lax | None; Secure |
|---|---|---|---|
| Same-site request | Sent | Sent | Sent |
| Another site → normal link (top-level GET navigation) | **Not sent** | **Sent** | Sent |
| Another site → form POST | Not sent | **Not sent** | Eligible* |
| Your site inside another site's iframe | Not sent | Not sent | Eligible* |

\*Third-party cookie restrictions may still block it. `SameSite=None` **requires** `Secure`.

🧠 **Strict = never cross-site. Lax = only when the user clicks a link to you. None = always (must be Secure).**

**Choosing:** **Lax** suits an ordinary shop (users arriving from links stay signed in) · **Strict** isolates more tightly; test external links and sign-in returns first · **None; Secure** only for genuine cross-site use.

**Site vs origin:**

```text
https :// shop.cedar.example : 443
└─┬─┘     └───────┬────────┘   └┬┘
scheme          host           port     ORIGIN = scheme + host + port
scheme         cedar.example            SITE   = scheme + registrable domain
```

`https://shop.cedar.example` and `https://help.cedar.example` are **same-site but different origins**. Changing only the port changes the **origin**, not the site. Switching HTTPS to HTTP changes **both**.

### CSRF ★★

**Cross-site request forgery (CSRF):** another page makes the victim's browser send a request authenticated by an existing cookie, which the user didn't mean to send. Example: a hidden form makes a signed-in support agent issue a refund; the role check passes, because it really is the agent's browser.

**Defences:** the framework's CSRF protection (validated tokens) plus origin checks · no state changes on GET · SameSite as an **extra** layer only. CORS (browser rules for cross-origin reads) is not a universal CSRF defence, and no cookie setting fixes a missing ownership check.

### Access control ★★★

💡 *The server's decision about who may do what to which resource, checked on every request.*

**Cedar Home shop (fictional):** Lina owns order 7201, Omar owns order 7202.

| Requester | Read 7201 | Read 7202 | Issue refund |
|---|---|---|---|
| Lina | Allow | Deny | Deny |
| Omar | Deny | Allow | Deny |
| Support staff | Allow | Allow | Allow |
| No valid session | Deny | Deny | Deny |

- **An ID says *which* object, not *who* may access it.** If Lina changes `/api/orders/7201` to `/api/orders/7202`, the server must refuse. Input validation can confirm 7202 is well formed, not that Lina may read it.
- **Correct ownership check:** `account.id === purchase.ownerId || account.role === "support"`, with both records loaded from trusted server-side state.
  - Comparing with `purchase.id` is wrong: that's the order number, not the owner.
  - `&&` instead of `||` would require being the owner *and* support.
  - `Boolean(account && purchase)` only proves both records exist.
- **Refund check:** `account.role === "support"`, enforced **before** anything changes. A denied request must leave order and financial records untouched. **Hiding the refund button doesn't stop a direct API request.**

| Failure | Name | Direction |
|---|---|---|
| Reading another customer's order | **IDOR / BOLA** (OWASP API1) | **Horizontal** (same role, another user's data) |
| Performing a support-only action | **Broken function-level authorisation** (OWASP API5) | **Vertical** (a higher role's action) |
| Reading or changing a field you shouldn't | **BOPLA** (OWASP API3) | Property level |

```text
               ^  VERTICAL (API5): a higher role's action, e.g. a customer issues a refund
               │
Lina (7201) ───┼──────> HORIZONTAL (API1, IDOR/BOLA): same role, Omar's order 7202
               │
               ·  PROPERTY (API3, BOPLA): the right record, but a field she shouldn't read/change
```

IDOR = Insecure Direct Object Reference · BOLA = Broken Object Level Authorization · BOPLA = Broken Object Property Level Authorization.

🧠 **API1 = wrong record · API3 = wrong field · API5 = wrong action.**

**Case study: First American, 2019.** Changing a number in an EaglePro document URL exposed unrelated customers' documents. The SEC reported **over 800 million images** exposed (findings published June 2021): *images*, not people, and not proof they were all downloaded. Random IDs make guessing harder but **don't replace a permission check**.

### HTTP status codes in these labs ★★

💡 *Three-digit codes a server returns with each response to say how the request went.*

**200** OK · **201** Created · **400** Bad request (invalid input) · **401** No valid session: not authenticated (despite the official name "Unauthorized") · **403** Authenticated, but permission refused · **404** Not found · **409** Conflict, e.g. a second refund for the same order · **500** Server error

> **⚠️ Exam traps**
> - `HttpOnly` does **not** prevent XSS. It only stops JavaScript reading the cookie.
> - A hidden button does **not** stop the action. The server must check the role.
> - Random, unguessable IDs are **not** access control.
> - A browser showing "signed out" does **not** mean the server revoked the session.
> - A cookie still in the browser does **not** mean the session is valid: the server checks expiry.
> - A detail page that checks ownership does **not** mean the export route does too.
> - Same-site does **not** mean same-origin. The same IP address does **not** mean the same person.

---

**Next:** [11. Lab walkthroughs](notes/11-lab-walkthroughs.md) → [12. Calculations](notes/12-calculations.md) → [13. Final review sheet](notes/13-final-review-sheet.md) → [14. Practice MCQs](notes/14-practice-mcqs.md)

*Good luck in the exam. If you spot an error, please let me know so these notes stay accurate for everyone.*
