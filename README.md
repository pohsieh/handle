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
