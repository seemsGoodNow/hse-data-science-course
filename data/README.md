# Course data

Datasets used in class demos and homeworks.

| folder | what | used in |
|---|---|---|
| `alpha/`, `bravo/` | warehouse operations logs (see below) | sessions 1–4 |
| `complaints/` | consumer complaints about financial products (see bottom) | session 7 |

The bankruptcy dataset for sessions 5–6 ships with HW2 (`homeworks/hw2/data/`).
**HW1 does not use this folder.** It comes with four warehouses of its own in
`homeworks/hw1/data/`: `podolsk`, `kazan`, `ekaterinburg` and `novosibirsk`. Each one is a
different site with 14 days of its own logs, and each is handed over whole, so there is
nothing to slice. They follow the same schema and the same column dictionary as the folders
described below.

## `alpha/` and `bravo/` — two warehouses, one week

Operations logs of two e-commerce **fulfillment warehouses**, closely modelled on the live processes of real sites: the workflows, the rhythms and the imperfections are all lifelike. ALPHA is the site we met in Session 1; BRAVO joins the story in Session 3. Both samples cover the same week, **Monday 2026-03-02 … Sunday 2026-03-08**, and share one schema, so everything below applies to both folders.

### The process that produces the data

Nobody creates this data for analysts. It is the exhaust of the picking process:

1. Customer orders are batched by the warehouse management system (WMS) into **picking tasks**, lists of "go there, take that".
2. A picker accepts a task on a handheld scanner (**StartTask**) and takes an empty container.
3. The scanner routes the picker through the aisles. Arriving at a storage cell, they scan its barcode (**ScanCell**).
4. Inside the cell they find the item and scan it into the container (**PickCorrectItem**). If the scanned item turns out to be the wrong one (easy in a cell that holds several different products), the terminal rejects it (**PickWrongItem**) and the search continues.
5. Sometimes a line can't be completed: the item isn't found (**SkipItem**) or is damaged (**MarkDefectItem**).
6. When the container is full, it is dropped at a drop point and a new one starts (**DropBoxing**).
7. All lines done (**FinishTask**).

Every scan is one row with a timestamp. A day of warehouse work is thousands of rows of pure human movement.

### Files

| file | rows (alpha) | rows (bravo) | what it is |
|---|---|---|---|
| `picking_events.parquet` | 311,543 | 200,145 | the event log — one row per scan |
| `topology.parquet` | 28,805 | 39,839 | where every storage cell physically is |
| `ovh.parquet` | 60,000 | 60,000 | item weight and volume |
| `categories.parquet` | 60,000 | 60,000 | item commercial category |
| `item_tags.parquet` | 60,000 | 60,000 | item handling tags |
| `item_prices.parquet` | 60,000 | 60,000 | item price |
| `warehouse_info.pickle` | — | — | a small Python dict of warehouse metadata (incl. how pickers are paid) |

Each warehouse ships its **own copy of every table**, including the item catalogue. All ids (`cell_id`, `item_id`, `user_id`, …) are local to a site: join tables **within** one warehouse, never across the two.

### How to load

```python
import pandas as pd
import pickle

events = pd.read_parquet("alpha/picking_events.parquet")   # swap alpha → bravo for the second site

with open("alpha/warehouse_info.pickle", "rb") as f:
    info = pickle.load(f)   # a pickle is just a saved Python object
```

### Column dictionary

**`picking_events.parquet`**

| column | meaning |
|---|---|
| `event_time` | when the scan happened (millisecond precision) |
| `event_type` | one of the eight event types described above |
| `user_id` | anonymous id of the picker |
| `task_id` | the picking task this event belongs to |
| `cell_id` | the storage cell involved → join `topology` |
| `boxing_id` | the container being filled |
| `item_id` | the item involved → join the item tables |
| `items_in_cell_quantity` | units of the requested item sitting in the cell at that moment |
| `reason` | a short code the scanner attaches to *some* events — which ones, and what the codes explain, is for you to discover |

Not every event type fills every column, so the ids that can be empty (`cell_id`, `boxing_id`, `item_id`) load as floats in pandas, a classic real-world artifact. Which events leave which columns empty is for you to discover.

**`topology.parquet`** — one row per (cell, level)

| column | meaning |
|---|---|
| `cell_id` | storage cell id |
| `cell_level_in_rack` | shelf level, 1 = floor level |
| `rack_id` | the rack (shelving unit) the cell belongs to |
| `x`, `y` | rack centre coordinates, millimeters |
| `sector_id` | sector — a group of racks of one size type |
| `zone_id` | warehouse zone — a large area containing sectors |
| `floor` | building floor |
| `rack_is_drop_point` | 1 = this is a drop point where full containers are left |
| `width`, `length` | rack footprint, millimeters |
| `direction` | which way the rack opens, 0–3; back-to-back racks differ by 2 |

**Item tables**, all keyed by `item_id`, covering the full 60k-item catalogue (more items than moved during this particular week):

| table | columns |
|---|---|
| `ovh.parquet` | `weight` (kg), `volumeliter` (liters) |
| `categories.parquet` | `category` — commercial category, e.g. *Pet Supplies* |
| `item_tags.parquet` | `tags` — list of handling tags, e.g. *Fragile*, *Oversized*, *SmallParts*; most items have none |
| `item_prices.parquet` | `price` (RUB) |

**`warehouse_info.pickle`** — dict with `warehouse_name`, `period`, `currency`, pay rules (`pay_per_pick`, `night_bonus_mult`, `night_hours`, `shift_guarantee`), `box_volume_l` and human-readable `notes`. Read the `notes`: pay rules matter for some of the questions we'll ask.

### Practical notes

- The sample contains **whole tasks**: every task that *started* inside the window is included in full, so a thin tail of events runs past Sunday midnight into the small hours of Monday (in `alpha`, 607 events on 2026-03-09, the last one at 00:46).
- **The first day is partial as well.** The extract opens around midday on Monday rather than at midnight, so 2026-03-02 carries only part of a normal day's traffic: 25,857 events in `alpha`, against 43,000 to 53,000 on each of the full days that follow. The HW1 folders are cut the same way at both ends. On a per-day chart both bookends will therefore look quiet, which tells you where the extract was cut and nothing about how the warehouse works.
- Timestamps are local warehouse time. The warehouse works around the clock.
- The data is synthetic but faithfully modelled on a real warehouse's logs; no real people or companies can be identified from it.
