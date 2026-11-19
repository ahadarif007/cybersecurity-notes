[← README](../README.md) · [← 12. Calculations](12-calculations.md) · [14. Practice MCQs →](14-practice-mcqs.md)

# 13. Final review sheet

Read this on the morning of the exam.

## Memory tricks

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
| **"Quote, OR, Comment"** | How `' OR 1=1 --` works ([appendix A](appendix-a-sql-injection.md)) |

## Key numbers

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
| TalkTalk | **156,959 / £400,000** | Customers affected / ICO fine ([appendix A](appendix-a-sql-injection.md)) |
| MOVEit | **CVE-2023-34362** | SQL injection exploited by CL0P ([appendix A](appendix-a-sql-injection.md)) |

## Pairs people mix up

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

## The universal answer pattern

For any "what can you conclude?" question, say what the evidence **shows**, then what it **can't show**, then what you'd **check next**.

---

[← README](../README.md) · [← 12. Calculations](12-calculations.md) · [14. Practice MCQs →](14-practice-mcqs.md)
