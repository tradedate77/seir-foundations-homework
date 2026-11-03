# Lab 1.0 — Personal notes

Terelle Dorceus · tradedate77 · SEIR Foundations, Module 1 / Lesson 1

## Runs

### Run 1 — i-00c073f2bcd8affac (2026-11-02, America/New_York)

This was my second run of the evening and the first one where the pip upgrade and the
COBOL build both went through cleanly without me having to reread the runbook. What went
differently from earlier runs is that I ran the ID line *before* starting uvicorn rather
than after, so the date, instance ID, and boot time all sat in the same screenshot as the
`Application startup complete.` message on the first try. I did not hit an error on this
run, which was a change from the first few where I was still guessing at paths.
What I understood this time that I had not before is why the boot time is printed in UTC
while the date is in my own zone — `uptime -s` gives the instance's own clock, so it reads
the next calendar day in the evening Eastern time, and that is expected rather than a bug.

Exact output line from this run:

```
INFO:     Started server process [55510]
```

I also started and stopped the server twice on this instance, so the second startup shows a
different PID:

```
INFO:     Started server process [35985]
```

### Run 2 — i-00de78bcb4a1cb8a4 (2026-10-31, America/New_York)

This was my first run on a Saturday evening and the first time I built the instance from my
saved launch template instead of walking the console from scratch, which cut the setup time
noticeably. The error I hit was `bash: venv/bin/activate: No such file or directory` when I
ran the activate command from the wrong folder; the fix was `cd /opt/balerica-bank` first,
because the virtualenv lives inside the bank directory, not in the home folder. Before this
run I did not understand that `source venv/bin/activate` is a *relative* path — it only works
from the directory that holds `venv`, which is why the same command works after a `cd` and
fails before one.

Exact output line from this run:

```
INFO:     Started server process [55880]
```

### Run 3 — i-01fe9fbc7f4bdff66 (2026-10-31, America/New_York)

This run was on the same evening as run 2 and I used it mainly to practise the COBOL return
codes rather than to redo the whole launch. What went differently is that I typed
`cd /opt/balerica-bank/cobol/bin/account_lookup 100001` as a single command, and bash answered
`bash: cd: too many arguments` — I had treated the binary as if it were a directory. The fix
was to `cd` to the folder and then run the binary as `./cobol/bin/account_lookup 100001`.
What I understood this time is the difference between a program's *exit code*, which is what
`echo $?` prints, and the `FILE-STATUS=` / `STATUS=` value the program prints itself; in the
rc 12 test the program reported `FILE-STATUS=35` while `$?` returned `12`.

Exact output line from this run:

```
INFO:     Started server process [35461]
```

### Run 4 — i-097c837462df8cd41 (2026-11-01, America/New_York)

This was my first Sunday run and the one where I stopped needing the runbook open beside me
for the launch steps. Nothing failed on the server start itself; what went differently is that
I ran the ID line twice — once before `cd /opt/balerica-bank` and once after — because I wanted
to be sure the instance ID I was recording was the one I was actually on. The thing I
understood this time that I had not before is that `ec2-metadata -i` answers from the instance
itself, so it cannot be faked or confused by being in a different folder, which is why the
assignment asks for it on every screenshot.

Exact output line from this run:

```
INFO:     Started server process [55521]
```

### Run 5 — i-08330f624a2530d0d (2026-11-01, America/New_York)

Second run of Sunday. The pip section of the setup reported `Found existing installation: pip
21.3.1`, `Uninstalling pip-21.3.1`, then `Successfully installed pip-26.0.1`, which told me the
bootstrap was running in the right order. What went differently is that I had to run the ID
line twice again because the first attempt scrolled past the interesting part, and the second
one landed right above the uvicorn banner. No errors on this run. What I understood is how
much of the setup is idempotent — the installer behaves the same way on a fresh instance as it
does on one where the packages already exist, which is why the runbook can be followed
top-to-bottom without checking what is already there.

Exact output line from this run:

```
INFO:     Started server process [55331]
```

### Run 6 — i-0b1b56f0ac18fdbf5 (2026-11-01, America/New_York)

Third run of Sunday, and the one where I first caught the return codes on purpose instead of
by accident. I ran `./cobol/bin/payment_post TXN-TD-0001 100001 5000000000.00 USD TEST-ZERO`
to force an rc 8, and the program answered `STATUS=REJECTED` with
`REASON=AMOUNT MUST BE GREATER THAN ZERO`, then `echo $?` printed `8`. What went differently is
that I also tried the wrong-folder test from `~` and got `ERROR=CANNOT OPEN data/accounts.dat`
with `FILE-STATUS=35`, which is the rc 12 environment failure. What I understood this time is
that the return code is the *contract* with the shell and the `ERROR=`/`REASON=` text is for the
human reading the log — the two carry the same information in different forms.

Exact output line from this run:

```
INFO:     Started server process [77800]
```

### Run 7 — i-06aa8fd9e5c68cbce (2026-11-02, America/New_York)

My last run and the one I used for the git clone evidence. What went differently is that I ran
the clone *after* shutting the server down with Ctrl+C, and I did not put any credentials on
the machine — the clone worked anonymously because the homework repo is public. I started and
stopped the server three times here while checking output, so this run has the most PIDs of any
of them. The error I hit was `bash: TXN0001: command not found`, from typing a transaction ID
at the prompt as if it were a program; the fix was to pass it as an argument to `payment_post`.
What I understood this time is that the shell treats the first word on the line as a command,
so a stray argument that lands first is always read as a program name.

Exact output line from this run:

```
INFO:     Started server process [53340]
```

(PID [35411] on the same instance shows the first start; [5628] shows a later one after the
clone was attempted.)

## COBOL practice

Instance used: run 6, `i-0b1b56f0ac18fdbf5` (2026-11-01, boot 2026-11-01 16:37:56).
Work done from `/opt/balerica-bank`. All five screenshots in this section
(`cobol_rc0.jpg`, `cobol_rc4.jpg`, `cobol_rc8.jpg`, `cobol_rc12.jpg`,
`cobol_ledger_decode.jpg`) are from this instance and carry the ID line.

I repeated the rc 0, rc 4 and payment checks on several other runs (1, 2, 4, 5 and 7) as
practice. The full sequence below, including the rc 8 with initials and the rc 12 environment
failure, was captured on run 6.

### 7.1 — Customers in the ledger

```bash
cat data/accounts.dat
```

Output:

```
100001Alice Johnson                ACTIVE  00000500000USD
100002Robert Smith                 ACTIVE  00000275000USD
100003Maria Garcia                 ACTIVE  00000810000USD
```

Three accounts: `100001`, `100002`, `100003`. All three are `ACTIVE`, so the bonus test
(posting to a non-ACTIVE account) is not available on this ledger. I hit the other rc 8 route
instead — an amount larger than the balance — which returns `REASON=INSUFFICIENT FUNDS`.

### 7.2 — Every return code, on purpose

**rc 0 — success**

```bash
./cobol/bin/account_lookup 100001
echo $?
```

Output:

```
ACCOUNT=100001
NAME=Alice Johnson
STATUS=ACTIVE
BALANCE=5000.00
CURRENCY=USD
0
```

Return code 0 means the lookup succeeded and the account was found. `$?` printed `0`.

**rc 4 — account not found**

```bash
./cobol/bin/account_lookup 999999
echo $?
```

Output:

```
ERROR=ACCOUNT NOT FOUND
ACCOUNT=999999
4
```

Return code 4 means the request was understood but no such account exists. The program does
not crash — it reports and exits, which is the point of using return codes.

**rc 8 — business rejection**

```bash
./cobol/bin/payment_post TXN-TD-0001 100001 0.00 USD TEST-ZERO
echo $?
```

Output:

```
TXN=TXN-TD-0001
ACCOUNT=100001
AMOUNT=0.00
CURRENCY=USD
STATUS=REJECTED
REASON=AMOUNT MUST BE GREATER THAN ZERO
8
```

And the second rejection route, an amount larger than the balance:

```bash
./cobol/bin/payment_post TXN-TD-0001 100001 5000000000.00 USD TEST-ZERO
echo $?
```

Output:

```
TXN=TXN-TD-0001
ACCOUNT=100001
AMOUNT=5000000000.00
CURRENCY=USD
STATUS=REJECTED
REASON=INSUFFICIENT FUNDS
8
```

Return code 8 means the request was well-formed but the *business rules* refused it. A zero
amount is not a syntax error; it is a rule the bank enforces, and it comes back as a rejection
rather than a failure. Both routes give 8 with a different `REASON=`, which is how the program
tells a human why it refused.

**rc 12 — environment failure**

```bash
cd ~
/opt/balerica-bank/cobol/bin/account_lookup 100001
echo $?
```

Output:

```
ERROR=CANNOT OPEN data/accounts.dat
FILE-STATUS=35
12
```

Return code 12 means the program could not do its job because its environment was wrong — it
looks for `data/accounts.dat` relative to the working directory, so running it from `~` means
it cannot find the ledger. `FILE-STATUS=35` is the program's own file-handling status code;
`$?` is the shell-level return.

### 7.3 — Decoding the fixed-width ledger by hand

```bash
cut -c1-6   data/accounts.dat    # account id
cut -c7-36  data/accounts.dat    # customer name
cut -c37-44 data/accounts.dat    # status
cut -c45-56 data/accounts.dat    # balance (implied decimal)
cut -c57-59 data/accounts.dat    # currency
```

Output:

```
100001
100002
100003
Alice Johnson
Robert Smith
Maria Garcia
ACTIVE
ACTIVE
ACTIVE
00000500000
00000275000
00000810000
USD
USD
USD
```

Alice Johnson's row, decoded:

| Columns | Field | Raw value |
|---------|-------|-----------|
| 1-6 | Account ID | `100001` |
| 7-36 | Customer name | `Alice Johnson` |
| 37-44 | Status | `ACTIVE` |
| 45-56 | Balance | `00000500000` |
| 57-59 | Currency | `USD` |

The raw balance characters are `00000500000`. The last two digits are the cents, so the value
is 500000 + 00 -> **$5000.00**. There is no decimal point in the file because this is a
fixed-width format inherited from COBOL: every record is exactly 59 characters, so a `.` would
consume a byte that the layout has not budgeted for, and the meaning of the last two digits is
defined by the *layout* rather than by a character in the data. This is the "implied decimal"
convention, and it is why `cut -c45-56` is described as the balance field rather than as a
number. Working the same rule on the other two accounts: `00000275000` is $2750.00 and
`00000810000` is $8100.00.

### 7.4 — Payments journal

```bash
cat data/payments.dat
```

Journal lines for my own transaction IDs:

```
TXN0001        100001000000000125500USDPOSTED  INV-1001
```

After posting, looking account `100001` up again gave the same `BALANCE=5000.00` as before the
payment. My best guess why: `payment_post` writes a journal entry to `payments.dat` and reports
`STATUS=POSTED`, `REASON=APPROVED`, but the account balance in `accounts.dat` is not rewritten
by that program — posting and settling appear to be separate steps, and the ledger file I am
reading is the opening state rather than the running balance. The journal line is the record of
the transaction; the balance would be derived from the journal plus the opening balance.

Optional — SQLite:

(Not run: `which sqlite3` was not checked on this instance.)

## Mistakes I made

1. **I used a transaction ID without my initials on most runs.** The assignment requires every
   `TXN` to start with my initials, and I posted `TXN0001` instead of `TXN-TD-0001` on runs 1, 2,
   4, 5 and 7. I caught it on run 6 and re-ran with `TXN-TD-0001`, which is the version in the
   screenshots.
2. **I typed the transaction ID as a command.** `TXN0001` at the prompt gave
   `bash: TXN0001: command not found`, because I forgot it belongs as an argument to
   `payment_post`. The shell reads the first word as the program name.
3. **I treated a binary as a directory.** `cd /opt/balerica-bank/cobol/bin/account_lookup 100001`
   produced `bash: cd: too many arguments`. I should have `cd`'d to the folder and run the
   binary with `./`.
4. **I activated the virtualenv from the wrong folder.** `source venv/bin/activate` failed with
   `No such file or directory` until I `cd`'d into `/opt/balerica-bank`, because the path is
   relative.
5. **My first submission of screenshots would not have counted.** I took them without the ID
   line visible, so there was nothing tying the run to an instance or a date. I had to repeat
   the exercise to get evidence that actually proves the work.
6. **I left the clone command running at a credential prompt.** The first `git_clone`
   screenshot stopped at `Username for 'https://github.com':` — the repo was not readable yet,
   and typing credentials there would have violated the "no tokens on the server" rule. I
   Ctrl+C'd and re-ran it once the repo was public.

## Commands I now know by heart

```bash
whoami && TZ='America/New_York' date && ec2-metadata -i && uptime -s
cd /opt/balerica-bank
source venv/bin/activate
uvicorn app.main:app --host 0.0.0.0 --port 8000
./cobol/bin/account_lookup 100001
./cobol/bin/payment_post TXN-TD-0001 100001 125.50 USD INV-1001
echo $?
cat data/accounts.dat
cat data/payments.dat
cut -c45-56 data/accounts.dat
git clone https://github.com/tradedate77/seir-foundations-homework.git
```
