# Lab 1 — The planet that emits six times too much

**ECBS5294 — Working with Data · Session 1, Block 1**

## Start here

| | |
|---|---|
| **The question** | How much CO₂ did the world emit in 2024, what was China's share, and how many countries reported? A country reported if its `co2` for 2024 is not `NULL`. |
| **The file** | `data/raw/owid_co2.csv` — Our World in Data. What each column means: `data/raw/owid_codebook.csv`, a table you read the same way (`SELECT * FROM 'data/raw/owid_codebook.csv'`). |
| **What is wrong** | The report runs without an error, and its numbers disagree with a row in the same file. |
| **What you hand in** | `DIAGNOSIS.md` on Moodle, before you leave. |
| **First thing to do** | Run your colleague's three report cells and write the three numbers down. |

The syntax you need is on one page: `REFERENCE-sql.md`. The full reference, with a task index in plain English, is
on the course site: https://earino.github.io/ecbs5294-2026/site/reference.html

## Get the project

If you cloned it during the stretch, you already have it. Otherwise, in your terminal (Git Bash on Windows,
Terminal on macOS), in the folder where you keep course work:

```bash
git clone https://github.com/earino/ecbs5294-lab01-grain.git
cd ecbs5294-lab01-grain
uv sync
```

Open **this folder** in VS Code (*File → Open Folder…*; trust the authors if asked), open `notebooks/report.ipynb`,
pick the `.venv` kernel, and run all cells. The first code cell prints the folder it is working in: it must be this
project's folder.

`data/raw/` has three files, and `notebooks/` has two notebooks. `online_retail.parquet` and `notebooks/lecture.ipynb` are the lecture's (the notebook holds every query the lecture ran, numbered as the slides called them, for review). This lab is `notebooks/report.ipynb`, on `owid_co2.csv`; `owid_codebook.csv` says what its columns mean.

## What is broken

Your colleague's report says the world emitted about **245,700 million tonnes** of CO₂ in 2024, that China's share was
**5%**, and that **254 countries** reported.

The file has its own figure for the whole world: a row whose `country` is `World`; it is the top row of inspection
query 4. It does not agree with the report.
Nothing errors. Find out why, before you change anything.

## What you must produce

Work in the notebook. Run the three cells under **Your colleague's report, as handed over** first and write the three
numbers down. Then, under **Your work starts here**, top to bottom:

1. **Inspect** (section A). The four inspection queries from the lecture, on the CO₂ file. Fill in the file name;
   write the key test yourself. Paste the two numbers from the key test and the first ten rows of the census into
   `DIAGNOSIS.md`, part 3, **before you change anything**.
2. **The filter** (section B). Two views, `countries` and `excluded`, opposites of each other. Both cells run
   as they are, with `WHERE TRUE`; replace `TRUE` in both. Then read what you excluded, for 2024, every row. A supplied
   check proves the two views split the file with nothing lost and nothing counted twice. When section D sends you
   back here, both views move.
3. **The ten largest emitting countries in 2024** (section C). Write this query yourself, from the empty cell.
4. **The check** (section D). A supplied cell puts the file's `World` row beside the sum over your view and accounts
   for the difference. What is left over must be smaller than 0.01 Mt: rounding, nothing else. If it is not, a row is
   missing from or extra in `countries`: fix both views in section B, not this cell. Write down what the first run
   printed; it is evidence for your note. A zero row passes this comparison unseen; reading what you excluded, in
   section B, is the proof you kept exactly the countries.
5. **The corrected report** (section E): your colleague's three headings again, each number from your view.
   "Reported" means `co2 IS NOT NULL`, not the rows in your view: the reported, plus the `NULL`s, must equal the
   view's rows for 2024, and you print that arithmetic.
6. `DIAGNOSIS.md`, all five parts, short. Then the last ten minutes, below, and the Moodle checkpoint.

Section F is supplied. Run it last: it writes the country table Block 2 starts from.

## Rules

- **Never edit `data/raw/`.** The raw data is the evidence. Fix the queries.
- Fix the *cause*: a rule that says which rows are one place. `WHERE country != 'World'` deletes the row you noticed and is
  not a filter.
- ***Country* is the file's word, not a political one.** The column is named `country`, and the codebook defines it
  as "Geographic location". In this lab a country is a row for one place, as opposed to a group of places (a
  continent, an income group, the world), a category (aviation, shipping) or an entry that is only historical.
  Antarctica and Christmas Island are countries in this sense. Nothing in this course asks what any place is
  politically.
- You may use AI to explain an error or a function. You must be able to explain every line you hand in: your
  neighbour will ask, at minute 33, without notes.

## Hints, if stuck

Staff will say these over the room at minutes 5, 10, and 15. Read them earlier if you want.

1. Run the census (inspection query 4) and read the first ten rows it returns. Is each of them one place?
2. Compare the `iso_code` of the rows that are countries with the `iso_code` of the rows that are not. What pandas
   prints as `NaN` or `None` is SQL's `NULL`: neither word is in the file, and the only test for it is `IS NULL`.
3. Rows that are groups of countries have no ISO code. With that as your only rule, the check in section D leaves an
   amount over: one more row has no ISO code, and no column in the file separates it from the groups. Read your
   `excluded` view for 2024, smallest first: which row has that amount? The `World` total counts it and no row
   in your view holds it, so name that row in the filter, and write in part 4 of your note what the check showed.

If you know which rows you want and not how to write it, your colleague's report is the shape of everything below it:

- **Section B:** report cell 2 has a `WHERE` with two conditions joined by `AND`. Yours needs a `WHERE` that keeps a
  row when *either* of two things is true, and the word for that is `OR`. The lecture's cell 10 has one.
- **Section D:** the four numbers are four queries of the same shape as report cell 2, each kept with
  `.fetchone()[0]`, then the arithmetic in Python. That cell is supplied; read it.
- **Section E:** report cell 2 already computes a share and prints it as a percentage. Yours is that line, with the
  numbers from your view.

## Diagnosis note

In `DIAGNOSIS.md`: the template is there. Part 3 is the evidence, not a transcript: the two numbers from the key test
and the first ten rows of the census. Part 4 is your `CREATE OR REPLACE VIEW`, and the sentence that justifies the
one row your filter names: what the check in section D showed. Part 5 is the check from section D: paste its output.

## Stretch task

What exactly is in `World` that is in no country? `International aviation` and `International shipping` are rows of
their own. How much of the residual do they explain, and how much do they not? Then: what share of 2024's emissions
belonged to no country at all?

## Git thread

Commit after your filter works, and again after the check closes. The message says what the cause was, the way
the lecture's example did — "Exclude postage and bank charges from product revenue: they are not products" — never
"fix" or "update".

## The last ten minutes

At minute 33, finished or not, turn to the person next to you (three if the row is odd). One of you explains, about a
minute: what was wrong, why, the query that proved it, what you changed, how you know it is right. Point at the
screen; do not read the note. The other asks:

1. **Show me the query that proves it.**
2. **Why was it wrong, not just where?**
3. **The what-if question on the slide.**

Then swap. If either of you is unsure, or you disagree, put a hand up: staff come to you first. Then the answer to
the what-if, for everyone. An unfinished repair is explained the same way: what you found so far.

Before you leave: the lab's **checkpoint on Moodle**. Upload `DIAGNOSIS.md` with its first line filled in. That is
what "complete" means; nobody signs you off.

## If you got lost: how to reset

Both of these **destroy work**. Read before running.

**Discard uncommitted changes (destructive)** — throw away edits and new files; keep your commits:

```bash
git restore --staged --worktree .    # every tracked file back to the last commit, staged or not
git clean -fd                        # and remove new, untracked files
```

> ⚠️ Permanently deletes uncommitted changes, staged or not, and any new untracked files.

**Full reset to the starter state (destructive)** — back to exactly what you cloned; throws away your commits too:

```bash
git reset --hard origin/main
git clean -fdx
```

> ⚠️ Discards your local commits and uncommitted changes. The `-x` also removes every ignored file — `data/silver/`, the
> `.venv/` environment, and anything else `.gitignore` lists, such as `.vscode/` and `.env` — so the folder matches a fresh clone. `uv sync` rebuilds the environment in a minute.
