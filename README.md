# PRC‑152A Repro — Battery / Radio Interface Boards

KiCad hardware project for the **battery-to-radio interface** of an AN/PRC‑152(A)
reproduction radio. It contains two small PCBs that mate through spring‑loaded
**pogo‑pin contacts**, plus a shared custom KiCad **footprint library** for those
mechanical contacts.

> All boards are designed in **KiCad 10** and built for fabrication/assembly at
> JLCPCB (Fabrication Toolkit export, JLCPCB BOM).

---

## Repository layout

```
battery-board/
├── Battery Top/                      # Board that sits on the battery
│   ├── Battery Top/                  # KiCad project (.kicad_pro/.kicad_sch/.kicad_pcb)
│   │   └── production/               # Gerbers/BOM/positions for JLCPCB
│   └── BOM/                          # JLCPCB BOM spreadsheet
├── Radio Bottom/                     # Board that sits on the radio body
│   └── Battery Bottom/               # KiCad project (folder name differs from board name)
├── Kicad Footprint Libaries/
│   └── Radio Library.pretty/         # ← the shared custom footprint library
└── README.md
```

> Note: a couple of folder names are inconsistent with the board names
> (`Radio Bottom/Battery Bottom/...`). The board you load is the `.kicad_pro`
> inside each, not the folder name.

---

## The two boards

### Battery Top
The interface board on the **battery** side. Carries power up to the radio and
exposes the data line.

| Ref | Part | Footprint | Purpose |
|-----|------|-----------|---------|
| J1 | JST XH 3‑pin vertical | `Connector_JST:...B3B-XH-A` | Battery power/data harness |
| J2, J3 | Pogo pin | `Radio Library:Pogo Battery 7MM` | VBAT+ contacts to radio |
| J5 | Pogo pin | `Radio Library:Pogo Battery 7MM` | DATA contact to radio |
| J4, J6 | Bolt pad | `Radio Library:Bolt Pad 4.25` | GND / M4 mounting bolt |
| F1 | 1206 fuse | `Fuse:Fuse_1206_3216Metric` | VBAT+ protection |

### Radio Bottom
The interface board on the **radio** side. It does battery current/power
monitoring and switches the load.

| Ref | Part | Footprint | Purpose |
|-----|------|-----------|---------|
| U1 | **INA226** power monitor | `Package_SO:MSOP-10_3x3mm` | I²C voltage/current/power sense |
| R1 | 5 mΩ shunt | `Resistor_SMD:R_2512_6332Metric` | Current‑sense resistor for U1 |
| Q1 | AOD403 / FQP27P06 P‑MOSFET | `Package_TO_SOT_SMD:TO-252-2` | Load switch / reverse protection |
| D1 | Zener | `Diode_SMD:D_SOD-123` | Gate / rail clamp |
| C1 | 0.1 µF 0603 | `Capacitor_SMD:C_0603` | Decoupling |
| R4 | 100 kΩ 0603 | `Resistor_SMD:R_0603` | Bias/pull |
| J1 | JST XH 6‑pin vertical | `Connector_JST:...B6B-XH-A` | Harness to radio |
| J2,J3 | Pogo pin | `Radio Library:Pogo Radio Bottom 6MM` | Power/data contacts from battery |
| J4,J6 | Bolt pad | `Radio Library:Bolt Pad 4.25` | GND / mounting bolt |

The two boards face each other: the **Pogo Battery 7MM** contacts on *Battery Top*
press onto the **Pogo Radio Bottom 6MM** contacts on *Radio Bottom*, carrying
VBAT+, GND and DATA across the mechanical joint. The INA226 + 5 mΩ shunt measure
battery current/voltage and report over the data line.

---

## The custom footprint library (`Radio Library`)

`Kicad Footprint Libaries/Radio Library.pretty/` is a standard KiCad `.pretty`
footprint library — a folder of `.kicad_mod` files, one per footprint. It holds
the **non‑standard mechanical contacts** that aren't in KiCad's stock libraries.

| Footprint | Pad | Drill | Pad Ø | Used for |
|-----------|-----|-------|-------|----------|
| **Pogo Battery 7MM** | 1× THT circle | 1.6 mm | 2.5 mm | Pogo pin pressed from the battery side |
| **Pogo Radio Bottom 6MM** | 1× THT circle | 0.9 mm | 2.5 mm | Mating pogo target on the radio side |
| **Bolt Pad 4.25** | 1× THT circle | 4.3 mm | 7 mm | M4 mounting bolt + GND pad |

Each is a single through‑hole pad — these are mechanical/contact features rather
than multi‑pin parts, so the drill size is chosen to fit the specific pogo pin or
bolt.

> `*.kicad_mod.rsls` and `Untitled*` files in the library folder are editor
> scratch/temp artifacts and are not real footprints.

### How the boards find the library

Each project's `fp-lib-table` registers the library under the nickname
**`Radio Library`** via an **absolute path**:

```
(lib (name "Radio Library") (type "KiCad")
     (uri "/Users/.../PCB Design/Kicad Footprint Libaries/Radio Library.pretty"))
```

Footprints are then referenced as `Radio Library:<name>` (e.g.
`Radio Library:Pogo Battery 7MM`) in the PCBs.

⚠️ **The path is hard‑coded and absolute.** On another machine (or if you move the
repo) KiCad won't find the library and the custom footprints will show as
unresolved. Fix it one of two ways:

1. **Per project** — open *Preferences → Manage Footprint Libraries → Project*
   and re‑point the `Radio Library` row at this repo's
   `Kicad Footprint Libaries/Radio Library.pretty`, **or**
2. **Use a path variable** — define `KIPRJMOD`‑relative or a custom
   `${RADIO_LIB}` environment variable and edit each `fp-lib-table` to use it,
   so the project is portable.

---

## Working with the project

### Open in KiCad
Open the `.kicad_pro` inside `Battery Top/Battery Top/` or
`Radio Bottom/Battery Bottom/`. If footprints are unresolved, fix the
`Radio Library` path as described above.

### Re‑generate fabrication output
Each project has a `production/` folder (gerbers zip, `bom.csv`,
`positions.csv`, `designators.csv`) produced by the **Fabrication Toolkit**
plugin (settings in `fabrication-toolkit-options.json`). Re‑run the plugin after
PCB changes to refresh these, then upload the zip + BOM to JLCPCB.

---

## Contributing

Improvements are welcome. Open an issue for anything non‑trivial, keep pull
requests focused on one change, and re‑export the `production/` output if you
touch a PCB. See [`CONTRIBUTING.md`](CONTRIBUTING.md) for the full workflow and
guidelines.

## License

This project is licensed under the **CERN Open Hardware Licence Version 2 –
Weakly Reciprocal (CERN‑OHL‑W v2)** — see [`LICENSE`](LICENSE).

You are free to use, study, modify, share and manufacture these designs. Because
the licence is *weakly reciprocal*, if you **distribute modified versions** of
these boards you must make your modified design (source) files **available under
the same licence**. The boards may be incorporated into a larger product without
that product as a whole having to be open.

`SPDX-License-Identifier: CERN-OHL-W-2.0`

## Notes

- `.gitignore` only excludes `.DS_Store`. KiCad lock files (`~*.lck`), `.history/`
  editor backups, and `production/` outputs are currently committed — consider
  ignoring the first two.
- Footprint files carry KiCad 10 (`version 20260206`, `generator_version 10.0`);
  open with KiCad 10 or newer to avoid format warnings.
