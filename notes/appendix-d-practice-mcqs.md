[← README](../README.md) · [← Appendix C. Week 4 and Week 7 labs](appendix-c-week-4-and-week-7-labs.md)

# Appendix D. Practice MCQs

Questions on appendices [A](appendix-a-sql-injection.md), [B](appendix-b-os-command-injection.md) and [C](appendix-c-week-4-and-week-7-labs.md). Questions marked **(select all that apply)** can have one or more correct answers.

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

[← README](../README.md) · [← Appendix C. Week 4 and Week 7 labs](appendix-c-week-4-and-week-7-labs.md)
