[← README](../README.md) · [← 14. Practice MCQs](14-practice-mcqs.md) · [Appendix B. OS command injection →](appendix-b-os-command-injection.md)

# Appendix A. SQL injection

> **Beyond the core scope.** Outside the listed scope of the first assessment, so revise it after the core topics. It still matters for the final assessment and the project.

## How it happens ★★★

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

## Forms of SQL injection ★★

| Form | How it works | Note |
|---|---|---|
| **Visible results** | Altered filters return extra rows; `UNION` can append another query's results | `UNION` needs a matching column count and compatible types |
| **Blind** | Differences in page content or response time reveal whether a condition is true | One slow response isn't proof. Hiding errors doesn't remove the flaw |
| **Second-order** | A stored value is later inserted unsafely into another query | Data from your own database isn't automatically safe |

Possible effects: unauthorised access, changed records and authentication bypass. The impact depends on the query, the database and its permissions.

## Real incidents ★★

| Incident | Attack chain | Detail |
|---|---|---|
| **Heartland** (disclosed 2009) | SQL injection → malware → card theft | The "130 million cards" figure covers the wider alleged campaign, not Heartland alone |
| **TalkTalk** (2015) | SQL injection → database access → customer data | **156,959 customers.** Legacy web pages, outdated database software, earlier attacks missed. ICO fine **£400,000** (2016) |
| **MOVEit Transfer** (2023) | SQL injection → web shell → data theft | CL0P exploited **CVE-2023-34362** and installed the **LEMURLOOT** web shell. Third-party software needs patching too |

## The fix: parameter binding ★★★

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

## Supporting controls ★

| Control | Purpose |
|---|---|
| Query construction | Bind values, and use fixed options for query structure |
| Permissions | Least privilege for the database account. With SQLite: file permissions and a read-only connection |
| Operations | Patch, hide SQL errors from users, monitor unusual access |
| Input rules | Validate type, length and allowed choices, while still accepting real names and punctuation |

**None of these replaces parameter binding.** A web application firewall adds filtering but doesn't repair query construction, and browser-side checks can be bypassed.

## Testing the fix ★

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

[← README](../README.md) · [← 14. Practice MCQs](14-practice-mcqs.md) · [Appendix B. OS command injection →](appendix-b-os-command-injection.md)
