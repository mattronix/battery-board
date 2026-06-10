# Battery Cell Holder

The cell pack of the PRC‑152A reproduction. Two stacked PCBs sandwich the
cylindrical cells: one board carries the spring contacts at each cell end, and
the pack feeds the *Battery Top* interface board. The custom **spring‑contact**
footprint library used by both boards lives here too.

## Layout

```
Battery Cell Holder/
├── Battery Cell Holder Top PCB/
│   └── Kicad/                 # KiCad 10 project — open Battery Cell Holder Top PCB.kicad_pro
├── Battery Cell Holder Bottom PCB/
│   └── Kicad/                 # KiCad 10 project — open Battery Cell Bottom PCB.kicad_pro
└── BatterySpringContact-kicad/   # shared custom symbol + footprint library (see its own README)
```

> Note: the bottom board's project file is named `Battery Cell Bottom PCB.*`
> while its folder is `Battery Cell Holder Bottom PCB/` — open the `.kicad_pro`,
> not the folder name.

## The two boards

Each board carries six battery spring contacts (`BT1`–`BT6`) plus solder‑wire
test points (`TP1`–`TP4`). The top and bottom boards face the opposite ends of
the cells so the springs press axially onto the cell terminals.

| Ref | Part | Footprint | Purpose |
|-----|------|-----------|---------|
| BT1–BT6 | Battery spring contact | `BatterySpringContact:Keystone_5222_SpringContact_THM` | Per‑cell end contacts |
| BT | Battery spring contact (plate) | `BatterySpringContact:BatteryContact_uxcell_Plate_TabSolder_18650` | Plate‑style contact variant |
| TP1–TP4 | Test point | `Connector_Wire:SolderWire-1.5sqmm_1x01_D1.7mm_OD3.9mm` | Wire solder / probe points |

These are generic mechanical contacts with **no LCSC part number**, so JLCPCB's
pick‑and‑place can't assemble them — the board is fabricated with the correct
land and you **hand‑solder** the contacts afterward.

## Footprint library

`BatterySpringContact-kicad/` is a standard KiCad symbol + `.pretty` footprint
library for the stamped 18650/AA spring contacts (coil‑spring negative end,
conical‑button positive end, plus a 7 mm conical PCB spring). It carries its own
detailed README — including footprint dimensions to measure, mounting notes, and
the 3D‑model setup. **Read [`BatterySpringContact-kicad/README.md`](BatterySpringContact-kicad/README.md) before editing the footprints.**

⚠️ The 7 mm conical PCB spring uses 0.3 mm wire — low current capacity. Fine for
sense/monitor paths; do **not** route high pack‑discharge current through it.

## Working with it

1. Open the `.kicad_pro` in **KiCad 10** or newer (one project per board).
2. If the `Battery_Spring_Contact` symbol or its footprints show as unresolved,
   register the `BatterySpringContact` symbol/footprint libraries under that
   nickname — see [`BatterySpringContact-kicad/README.md`](BatterySpringContact-kicad/README.md).
3. The footprint references the STEP model via `${KIPRJMOD}`, so copy the
   `.step`/`.wrl` next to the `.kicad_pcb` (or re‑point the 3D‑model path) for
   the 3D viewer.
