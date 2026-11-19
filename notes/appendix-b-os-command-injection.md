[← README](../README.md) · [← Appendix A. SQL injection](appendix-a-sql-injection.md) · [Appendix C. Week 4 and Week 7 labs →](appendix-c-week-4-and-week-7-labs.md)

# Appendix B. OS command injection

> **Beyond the core scope.** Outside the listed scope of the first assessment, so revise it after the core topics. It still matters for the final assessment and the project.

## How it happens ★★★

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

## The fix ★★★

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

[← README](../README.md) · [← Appendix A. SQL injection](appendix-a-sql-injection.md) · [Appendix C. Week 4 and Week 7 labs →](appendix-c-week-4-and-week-7-labs.md)
