# handle

A handheld instrument that images fingertip contact during grip. Each side of the handle is a
glass plate, edge-lit by a blue LED so the plate runs on total internal reflection; a fingertip
pressing on it frustrates that reflection locally (FTIR), and a camera behind a mirror fold sees
the ridge pattern of the real contact area. The handle is two-sided and symmetric — index finger
on one plate, thumb on the other — and a six-axis force sensor sits under each plate, so contact
area and grip force are measured on the same plate at the same time. Piezo actuators at the short
ends of each plate drive an out-of-plane vibration, which modulates the friction the finger feels.

TU Delft · Cognitive Robotics / Human-Robot Interaction · Tactile Machine Lab
Q5 internship, 2026 · Po-Yu Hsieh

---

## What is in this repository

| Folder | Contents |
|---|---|
| [`stl/`](stl/) | Printable geometry — the mesh of each part as it was printed |
| [`bom/`](bom/) | Bill of materials — what the handle is made of, with suppliers, prices and lead times |

Both are filled by hand, file by file. Nothing is generated or synced automatically.

## What is deliberately not in here

| Not here | Where it is, and why |
|---|---|
| **The native SolidWorks model** — `*.SLDPRT`, `*.SLDASM` | `D:\Q5_Internship\_model`, un-versioned apart from the dated snapshot folders. Excluded on purpose, and `.gitignore` enforces it. See the note below on what that costs |
| **Raw test data** — FTIR image sequences, screen recordings, force logs | ~1.5 GB on `D:\Q5_Internship`. Git stores a whole new copy of a binary file on every change; image sequences would make this repository unusable within weeks |
| **Code** — the Pi capture scripts, the force-sensor driver, the analysis pipeline | Tracked with the project record, not here |
| **The project record** — design rationale, meeting notes, progress log | Tracked privately with the author's working notes |

> **An STL is a derived file, not a source file.** A mesh carries no sketches, no dimensions, no
> features and no parameters — it cannot be edited back into the design it came from. So this
> repository is an exact record of *what was printed*, and the editable design stays on `D:\`
> with no version control of its own. That is a deliberate trade: it keeps the repository small
> and portable, and it means the parametric model's history still lives in dated folders. If the
> native files should be versioned after all, delete the first block of `.gitignore`, keep
> `stl/` as it is, and put the parts and assemblies flat in a new `cad/` folder — flat, because
> a SolidWorks assembly stores a *path* to every part it contains, and moving a part either
> breaks that reference or silently re-binds it to another copy of the same filename.

---

## `stl/` — conventions

| | |
|---|---|
| **One file per part**, not per assembly | An assembly STL is one welded mesh: it cannot be printed part by part and cannot be compared against the previous version of a single part |
| **Naming** `YYYY-MM-DD_<part>_v<n>.stl` | e.g. `2026-11-06_glassholder_v4.stl`. The date is the freeze, not the export |
| **Say the print settings in the commit message** | An STL carries no material, no layer height, no orientation and no resolution. That information exists nowhere else once the slicer project is gone |
| **Re-export at the same resolution** | A finer export changes every triangle, so the diff tells you nothing about whether the geometry changed. Keep the SolidWorks export settings fixed |

Dated snapshot folders (`stl/20261030_ver7/` and friends) are ignored — the commit history is the
snapshot. Slicer output (`.gcode`) is ignored too; it is one printer's answer to the mesh, not the
mesh.

## `bom/` — conventions

Keep it as **`bom.csv`**, not a spreadsheet, so that git can diff it: a CSV shows exactly which
line changed, an `.xlsx` shows only that the file changed.

```csv
item,subsystem,part_number,manufacturer,supplier,qty,unit_price_eur,price_date,lead_time,status,link,notes
```

- **One row per orderable item**, not per modelled part. Four identical screws are one row with
  `qty = 4`.
- `subsystem` ∈ `optics`, `illumination`, `force`, `structure`, `compute`, `actuation`,
  `fasteners` — how the work is actually split.
- `status` ∈ `in hand`, `ordered`, `shortlisted`, `needed`. A part that is only a candidate still
  belongs in the file, with the link and the price on the day it was found.
- Dutch suppliers first — RS Online NL, Conrad, Kiwi Electronics; Thorlabs ships from Germany.
  Record the supplier actually used, not the cheapest one found.
- Prices in EUR excluding VAT, with `price_date`, so an old quote is recognisable as old.

---

## Everyday use

Stage only the files you mean to commit. `git add .` is not used in this repository.

```bash
git status                                    # what changed, and what is not yet tracked
git add stl/2026-11-06_glassholder_v4.stl     # one file at a time, on purpose
git commit -m "Glass holder v4: plate seat widened to 40 mm — PLA, 0.15 mm, flat on the bed"
git pull --rebase                             # always before pushing
git push
```

A commit message says **what changed about the part**, not that a file changed. For an STL there
is no diff at all, so the message is the only record that will ever exist: "update holder" is
worth nothing in six months.

## Size

Nothing here uses Git LFS and nothing here needs it. A printable part exports to a few MB of
mesh, and the BOM is a text file. GitHub warns on any single file over 50 MB and refuses one over
100 MB — a mesh that large has been exported at the wrong resolution.
