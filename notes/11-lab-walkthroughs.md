[← README](../README.md) · [12. Calculations →](12-calculations.md)

# 11. Lab walkthroughs

The labs below support the core topics. Week 5 → [information disclosure](../README.md#7-information-disclosure-and-safe-failure). Week 2 → [data hiding](../README.md#3-data-hiding). Week 6 → [authentication](../README.md#9-authentication-and-password-storage) and [sessions](../README.md#10-sessions-cookies-and-access-control). The CTFs → disclosure and access control.

## Week 5: "What goes out" ★★★

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

## Week 2: Hidden data ★★

The lab's message is that a quick visual check can miss hidden information.

| Method | What you observed | Control for a secure program |
|---|---|---|
| **Alternate data streams** | `dir` shows nothing unusual, but `dir /r` shows `welcome.txt:secret.txt:$DATA`. A ZIP archive can't preserve ADS | Enumerate named streams |
| **PDF containers** | The pages look identical but the file sizes differ. Data can sit after the final `%%EOF` marker, or be a genuine attachment | Inspect the whole container, attachments included |
| **Image steganography** | You can't tell by eye which image carries a file. The output is saved as **PNG** so the hidden bits survive. Comparing SHA-256 hashes proves full recovery | Validate stored lengths and paths |
| **Shannon entropy** | Scored from 0 to 8 bits per byte. The extension alone doesn't predict the score, and a DOCX is a ZIP-based container | Interpret it in context |

**Entropy blocks:** a block is a consecutive chunk of the file's bytes, not a disk cluster. Block-by-block scores can pinpoint a change that a whole-file score hides.

## Week 6: Riverton Live (passwords, sessions, access control) ★★

This is the hands-on version of sections [9](../README.md#9-authentication-and-password-storage) and [10](../README.md#10-sessions-cookies-and-access-control).

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

## CTFs: Nightfall Games and Harbour Dispatch ★

Both are short **inspect → explain → verify** challenges done as an **ordinary user**, with no password guessing or brute force.

- **Nightfall Games (15 minutes):** find an unreleased beta code in HTTP responses that the page doesn't display, then redeem it. **Lessons:** data hidden on screen can still be in the response, and a valid player session isn't permission to use a restricted resource.
- **Harbour Dispatch (20 minutes):** follow a chain of references across responses to an internal depot record. The detail route and the **export/download route** behave differently. **Lesson:** every endpoint needs its own ownership check.

---

[← README](../README.md) · [12. Calculations →](12-calculations.md)
