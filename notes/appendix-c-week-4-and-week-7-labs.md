[← README](../README.md) · [← Appendix B. OS command injection](appendix-b-os-command-injection.md) · [Appendix D. Practice MCQs →](appendix-d-practice-mcqs.md)

# Appendix C. Week 4 and Week 7 labs

> **Beyond the core scope.** Outside the listed scope of the first assessment, so revise it after the core topics. It still matters for the final assessment and the project.

## Week 4: "The server decides" (what comes in) ★★

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

## Week 7: SQL injection lab ★★

| Site | Attack | What happened | Fix |
|---|---|---|---|
| **Aster Observatory** search | `' OR 1=1 -- ` | 8 results, including 2 **unreleased** ones, because the `published = 1` filter was bypassed. `' AND 1=2 -- ` returned nothing, proving real SQL logic was running | `WHERE published = 1 AND title LIKE ?`, binding `%term%`. Don't quote the `?`, and keep the published filter |
| **Bracket Supply** login | Username `admin' -- ` | Logged in as the administrator with **any password**, because the comment removed the `password_matches(...)` condition. No hash was cracked | Bind **both** the username and the password. Real usernames with apostrophes then work too |
| **Field notes** (extension) | `' OR 1=1 -- ` | Showed other members' private notes | `WHERE member_id = ? AND title LIKE ?`, binding both. Don't hard-code the member |

Password hashing and access checks don't make string-built SQL safe, and binding doesn't replace the published, member or role filters either.

## Extra pairs

| Pair | The difference |
|---|---|
| **Parameter binding vs authorisation** | How a value reaches SQL vs whether the user may see those rows |
| **Bind it vs allowlist it** | Data values vs parts of the query structure (column names, ASC/DESC) |
| **Visible vs blind vs second-order SQLi** | Extra rows shown vs inferred from differences vs stored value used later |
| **Shell command string vs argument list** | Parsed by a shell (injectable) vs passed straight to the program |
| **Command injection vs option injection** | Adds a new command vs turns the value into a `-` option |

---

[← README](../README.md) · [← Appendix B. OS command injection](appendix-b-os-command-injection.md) · [Appendix D. Practice MCQs →](appendix-d-practice-mcqs.md)
