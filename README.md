# Lab 1 — The planet that emits six times too much

**ECBS5294 — Working with Data · Session 1, Block 1**

## Start here

| | |
|---|---|
| **The question** | How much CO₂ did the world emit in 2024, what was China's share, and how many countries reported? |
| **The file** | `data/raw/owid_co2.csv` — Our World in Data. What each column means: `data/raw/owid_codebook.csv`. |
| **What is wrong** | The report runs without an error, and its numbers disagree with a row in the same file. |
| **What you hand in** | `DIAGNOSIS.md` on Moodle, before you leave. |
| **First thing to do** | Run the notebook top to bottom and read the three numbers the report prints. |

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

`data/raw/` has three files, and `notebooks/` has two notebooks. `online_retail.parquet` and `notebooks/lecture.ipynb` are the lecture's (the notebook holds every query the lecture ran, for review). This lab is `notebooks/report.ipynb`, on `owid_co2.csv`; `owid_codebook.csv` says what its columns mean.

## What is broken

Your colleague's report says the world emitted about **245,700 million tonnes** of CO₂ in 2024, that China's share was
**5%**, and that **254 countries** reported.

The file has its own figure for the whole world: a row whose `country` is `World`. It does not agree with the report.
Nothing errors. Find out why, before you change anything.

## What you must produce

Work in the notebook, under **Your work starts here**, top to bottom:

1. **Inspect** (section A). Run the four inspection queries from the lecture on the CO₂ file. Paste what they showed
   you into `DIAGNOSIS.md`, part 3, **before you change anything**.
2. **The filter** (section B). Write a view, `countries`, that keeps only the rows that are countries. Then run the
   census again on your view and read every row it kept.
3. **The ten largest emitting countries in 2024** (section C). Write this query yourself, from the empty cell.
4. **The check** (section D). Put the file's `World` row beside the sum over your view, and account for the difference
   as the notebook explains. What is left over must be smaller than 0.01 Mt: rounding, nothing else. If it is not,
   your filter is wrong: find out how. The sum is not proof that you kept exactly the countries; the census of your
   view, in section B, is.
5. **The corrected report** (section E): the three numbers again, from your view.
6. `DIAGNOSIS.md`, all five parts, short. Then the last ten minutes, below, and the Moodle checkpoint.

Section F is supplied. Run it last: it writes the country table Block 2 starts from.

## Rules

- **Never edit `data/raw/`.** The raw data is the evidence. Fix the queries.
- Fix the *cause*: which rows are countries. Removing the one row you happened to notice is not a filter.
- You may use AI to explain an error or a function. You must be able to explain every line you hand in: your
  neighbour will ask, at minute 33, without notes.

## Hints, if stuck

Staff will say these over the room at minutes 5, 10, and 15. Read them earlier if you want.

1. Run the census (inspection query 4) and read the first ten rows it returns. Are they all countries?
2. Compare the `iso_code` of the rows that are countries with the `iso_code` of the rows that are not.
3. Rows that are groups of countries have no ISO code. So does one row that *is* a country — the check in section D
   will tell you if your filter lost it. Which rows with no `iso_code` have a value for 2024?

## Diagnosis note

In `DIAGNOSIS.md`: the template is there. Part 5 is the check from section D: paste its output.

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

> ⚠️ Discards your local commits and uncommitted changes. The `-x` also removes ignored files — `data/silver/`, the
> `.venv/` environment — so the folder matches a fresh clone. `uv sync` rebuilds the environment in a minute.
