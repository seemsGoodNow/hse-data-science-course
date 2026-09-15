# HW1 — One warehouse, and what you can prove about it

**Out:** Sep 16 (Session 3) | **Due:** Sep 30, 18:00 (Session 5) | **Weight:** 40% of the final grade | Individual.

You are assigned **one of four warehouses** in a direct message when the assignment goes out:
`podolsk`, `kazan`, `ekaterinburg` or `novosibirsk`. Your data is the matching folder inside
`data/` — take it exactly as it is, there is nothing to cut or filter first.

## The setup

You are an analyst hired by a warehouse operations manager. You get two weeks of that site's
operations logs — lifelike data closely modelled on the processes of a real warehouse.

**The four sites are genuinely different businesses.** One moves nearly a hundred thousand picks a
day in big bulk tasks; another does a quarter of that in tiny ones. One handles heavy goods, another
runs mostly at night with a stream of new hires. So the answer that is right for your site will be
wrong for your neighbour's — comparing methods is useful, comparing conclusions is not. AI
assistants are allowed under the course policy; you must be able to explain every line you submit.

**Deliverable:** one notebook (`.ipynb`, plus an exported `.html`), written as a **researcher's
story**, sent as a direct message to the instructor in the course chat. Start from
`hw1_template.ipynb` in this folder: it holds the setup cell and the part titles. **Name your
warehouse in the very first cell**: every number in your work is read against that site.

How to export the HTML. Colab: File → Download → Download .html. VS Code / Jupyter: File → Save and
Export Notebook As → HTML. Send both files.

## Your data

Everything sits in `data/<your-site>/`, about 12–25 MB per site, a few seconds to clone:

| file | what |
|---|---|
| `picking_events.parquet` | the event log — every scan, pick, drop, task start and finish |
| `topology.parquet` | where each cell physically is: rack, level, sector, zone |
| `ovh.parquet` | weight (kg) and volume (litres) per item |
| `categories.parquet` | commercial category per item |
| `item_tags.parquet` | handling tags per item (a list — an item can have several, or none) |
| `item_prices.parquet` | price per item |
| `warehouse_info.pickle` | site metadata **and the pay rules** — read it, it matters |

Every column is documented in the [data dictionary](../../data/README.md). Ids are local to your
site: join tables **within** your folder, never across sites.

```python
import pandas as pd, pickle
from pathlib import Path

DATA = Path("data/kazan")          # <- your site

# --- Google Colab? Uncomment these two lines to fetch the course repo: ---
# !git clone https://github.com/seemsGoodNow/hse-data-science-course.git course
# DATA = Path("course/homeworks/hw1/data/kazan")

events = pd.read_parquet(DATA / "picking_events.parquet")
info = pickle.loads((DATA / "warehouse_info.pickle").read_bytes())
```

## The researcher's story format

This is a report, not a scratchpad. Hard rules:

1. **Hypothesis before code.** Every investigation starts with a markdown cell: what you expect and
   why — plus one sentence on **what evidence would confirm it, and what would change your mind**.
   The conclusion comes after the evidence — even if (especially if) the hypothesis failed.
2. **Every step says why.** A cell of code without a sentence of intent is noise.
3. **It ends with words, not code.** The last section is for a human who will never open pandas.

## Structure

### Part 0 — Meet your data
Establish the basics: period, size, how many workers, how many events of each type, how the site is
laid out. Run a data-quality pass: missing values, duplicates, timestamps that can't be real. Decide
what to do with the problems you find — and say why. Some columns are empty **by design**; work out
which, and what that tells you about the process.

### Part 1 — Picking performance
Define a productivity metric for picking (e.g. successful picks per active hour, or picks per
hour inside a task, as in Session 3 — your definition, justified). Show its distribution across
workers. Deal with outliers consciously: drop, cap, or keep — argue your choice. "Active" is yours
to define: hours with at least one event, or the shift span from Part 3. Say which you chose.

### Part 2 — Three hypotheses
Test three hypotheses about what drives performance on **your** site. Two are fixed, the third is
yours:

1. **Task size:** do bigger picking tasks go faster or slower *per item*?
2. **Item properties:** do heavy or bulky items slow picking down? Weight and volume are in
   `ovh.parquet`. *(requires a join)*
3. **Your own** — anything defensible from the remaining tables: category, shelf level, zone, cell
   crowding, time of day, day of week, worker tenure…

For each: hypothesis → check → verdict, with at least one chart per hypothesis that a non-analyst
could read. Two honest tips. If a straightforward split shows nothing, consider what other shapes
the relationship could take (linear? threshold-like?) before writing the verdict — "no effect" and
"no effect *the way I first looked*" are different findings. And **where you cut matters**: a split
at the median of something asks a different question than a split at its top quartile.

**Statistical tests are optional.** A clear split, a chart and a defended verdict is a complete
answer. The tools for "is this difference real or luck" arrive in Session 4 (23.09); adding one
after that strengthens the argument, but no part of the grade depends on it. Charts are graded
against the five rules from Session 3, including "if you cut something out of a chart, say how
much".

### Part 3 — The fork (no right answer)
The manager asks: *"How much does each worker earn per shift?"* The pay rules ship with your data in
`warehouse_info.pickle`: a rate per successful pick, a night-hours multiplier, and a minimum
guarantee per shift. But nobody tells you what a "shift" is — the logs contain only events. Define a
shift yourself, defend the definition, compute per-worker earnings per shift (applying all three pay
rules), and name the top and bottom performers. Then answer honestly: is your comparison fair? What
could make it misleading?

There is more than one reasonable way to cut an event log into shifts, and Session 3 showed
one pattern that can help. Pick a definition, say why, and note what a different choice would
have changed. Whatever you compute, check it on one worker by eye before trusting it for all.

*This part is graded on the quality of your reasoning, not on matching some expected answer — there
isn't one.*

### Part 4 — Tell the manager
≤ 10 sentences, no code: what you found, what you'd recommend, what you'd check next if given
another week. Your recommendation should follow from **your** site — the right advice for a
cramped, error-prone warehouse is not the right advice for a fast bulk one.

## Rubric

| Criterion | Weight | What it means |
|---|---|---|
| Correctness of analysis | 40% | Joins don't silently lose or duplicate rows; metrics computed right; claims match the data |
| Research logic | 25% | Hypothesis → check → verdict discipline; honest handling of surprises; the fork is argued, not just answered |
| Business conclusions | 20% | Part 4 is specific, follows from the evidence, and would be useful to a real manager |
| Artifact quality | 15% | The notebook reads top-to-bottom; charts are titled and readable; no dead cells |

A complete, correct submission on all four criteria scores **up to 8 of 10**. The last two points
are for **one extra move**, done well:

- a **second own hypothesis** in Part 2, on a different table or dimension than the first three,
  with the same hypothesis → check → verdict shape (a well-argued "no effect" qualifies);
- an **AI-generated HTML one-pager** presenting your findings for the manager;
- a **statistical test from Session 4** applied to one of your hypotheses, with the result read
  honestly (a p-value alone is not an argument).

Exactly one extra counts, whichever is strongest; three half-done extras earn nothing. The extras
are not required for 8.

## Rules

- **AI is allowed**; you must be able to explain every line you submit. For 2–3 students per
  assignment I ask a follow-up question in a direct message ("why this join?") — announced practice,
  not suspicion.
- **Late work** loses 10% of the earned score per started day, up to three days; after that it is not
  accepted. Tell me *before* the deadline if something is wrong.

## Questions

Ask in the course chat — if something in the task feels underspecified, that may well be the point:
make an assumption and write it down.
