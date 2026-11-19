[← README](../README.md) · [Final review sheet →](13-final-review-sheet.md)

# Last-minute summary

The core topics (1–10) on one page. Five minutes before the exam.

## The rules behind most answers

- **A clue is not proof.** Records show *what* happened, not who or why → "warrants investigation".
- **Watch for proves / always / never / eliminates.** These words usually make an option false.
- **Hiding is not fixing.** A hidden button, field or error, or a deleted key, still leaves the capability there.
- **The server decides.** Browser checks are not enforced.
- **Incidents:** **contain and revoke first**, clean up later.
- **Fail closed:** refuse when unsure.

## One line per topic

| # | Topic | Must remember |
|---|---|---|
| 1 | **Foundations** | CIA = who can see / can it be trusted / is it available. Threat ≠ vulnerability ≠ impact; risk ≈ likelihood × impact (for comparing only). Surface = *where*, vector = *how*. HSE 2021: detected 31 Mar in **monitoring mode**, so detection without response stopped nothing. Zero trust is **not a product** |
| 2 | **Threat modelling** | Shostack: **Work, Wrong, Do, Done**. STRIDE → **Authentication, Integrity, Accountability, Confidentiality, Availability, Authorisation** (no ranking). Responses: **Eliminate, Mitigate, Transfer, Accept** (Accept needs a reason and an owner) |
| 3 | **Data hiding** | Steganography hides **existence**, cryptography hides **contents**. LSB: 44 + bit 1 → 45. Entropy 0 / 1 / **8 bits** max per byte; high entropy is only a clue. An ADS is not suspicious in itself, and a normal hash covers the **default stream only** |
| 4 | **OWASP 2025** | **All My Students Can Improve During Autumn If Learning's Easy** (A01 Access … A10 Exceptional). A07 = who are you, A01 = what may you access. A03 = whole supply chain, A08 = this artifact. Parameters protect the DB, encoding protects the browser |
| 5 | **Evidence** | Observe → Interpret → Test (independent source, **UTC**). **4624** success · **4625** fail · **4688** process · type **2** interactive · type **3** network · Run keys **T1547.001**. USBSTOR ≠ file copied. Signature ≠ valid, unknown ≠ malware |
| 6 | **NTFS** | `4128:7` = MFT record 4128, sequence 7. **VCN = in the file**, **LCN = on the volume**. SI vs FN mismatch → investigate, not proof. The USN journal says *what* changed, not who. FAT32 deleted entry = **0xE5** |
| 7 | **Disclosure** | Routes: **Responses, Errors, Login, Source, Logs**. Response contract = **allowlist** of fields. Same message still leaks through **timing**. Leaked secret: **Contain → Revoke → Investigate → Clean**. `.gitignore` doesn't touch history |
| 8 | **Malware** | Virus needs a host · worm walks alone · Trojan disguised · rootkit **hides** · backdoor gives **access**. A digital signature ≠ harmless. Encrypting hits **A**, stealing hits **C**, altering hits **I**. A backup restores availability, not confidentiality. Only a **restore test** proves a backup |
| 9 | **Passwords** | Hash ≠ encryption; verification **never decrypts**. Salt = no shared hashes or rainbow tables, but **no extra cost**. SHA-256 is too fast. Argon2id: lecture **m=64 MiB, t=3, p=4** (RFC 9106), lab **19 MiB, t=2, p=1** (OWASP). `v=19` = **0x13**, not a year. Adobe: 24 of the top 100 hint *strings* |
| 10 | **Sessions & access** | HttpOnly ≠ XSS fix. `__Host-` = Secure + Path=/ + no Domain. **Lax** = sent on a top-level link click only. Same-site ≠ same-origin. Ownership check: `id === ownerId` **OR** `role === "support"`. **API1 record (horizontal) · API3 field · API5 action (vertical)**. **401** no session · **403** refused · **409** conflict |

## If stuck on an answer

Say what the evidence **shows**, then what it **can't show**, then what you'd **check next**.

More detail: [Final review sheet](13-final-review-sheet.md) (all memory tricks, key numbers and confusable pairs).
