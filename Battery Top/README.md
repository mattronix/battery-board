# Battery Top

Interface board on the **battery** side of the PRC‑152A reproduction. It carries
power up from the battery to the radio and exposes the data line, mating to
*Radio Bottom* through spring‑loaded pogo‑pin contacts.

## Layout

```
Battery Top/
├── Kicad/                 # KiCad 10 project — open Battery Top.kicad_pro here
│   ├── Battery Top.kicad_pro / .kicad_sch / .kicad_pcb / .kicad_dru
│   ├── fp-lib-table       # registers the shared "Radio Library" footprint lib
│   ├── fabrication-toolkit-options.json
│   └── production/        # JLCPCB output (gerbers zip, BOM, positions) — git-ignored
└── BOM/
    └── JLPCB BOM v1.xls   # JLCPCB BOM spreadsheet
```

## Key components

| Ref | Part | Footprint | Purpose |
|-----|------|-----------|---------|
| J1 | JST XH 3‑pin vertical | `Connector_JST:...B3B-XH-A` | Battery power/data harness |
| J2, J3 | Pogo pin | `Radio Library:Pogo Battery 7MM` | VBAT+ contacts to radio |
| J5 | Pogo pin | `Radio Library:Pogo Battery 7MM` | DATA contact to radio |
| J4, J6 | Bolt pad | `Radio Library:Bolt Pad 4.25` | GND / M4 mounting bolt |
| F1 | 1206 fuse | `Fuse:Fuse_1206_3216Metric` | VBAT+ protection |

The **Pogo Battery 7MM** contacts here press onto the **Pogo Radio Bottom 6MM**
targets on *Radio Bottom*, carrying VBAT+, GND and DATA across the joint.

## Working with it

1. Open `Kicad/Battery Top.kicad_pro` in **KiCad 10** or newer.
2. If the custom pogo/bolt footprints show as unresolved, re‑point the
   `Radio Library` entry — see [`../Kicad Footprint Libaries/README.md`](../Kicad%20Footprint%20Libaries/README.md).
3. After PCB changes, re‑run the **Fabrication Toolkit** plugin to refresh
   `production/`, then upload the zip + BOM to JLCPCB.
