# Radio Bottom

Interface board on the **radio** side of the PRC‑152A reproduction. It does
battery voltage/current/power monitoring (INA226 + shunt) and switches the load,
mating to *Battery Top* through spring‑loaded pogo‑pin contacts.

## Layout

```
Radio Bottom/
├── Kicad/                  # KiCad 10 project — open Radio Bottom.kicad_pro here
│   ├── Radio Bottom.kicad_pro / .kicad_sch / .kicad_pcb / .kicad_dru
│   ├── fp-lib-table        # registers the shared "Radio Library" footprint lib
│   └── fabrication-toolkit-options.json
└── BOM/
    └── Radio Bottom BOM V1.xls   # JLCPCB BOM spreadsheet
```

## Key components

| Ref | Part | Footprint | Purpose |
|-----|------|-----------|---------|
| U1 | **INA226** power monitor | `Package_SO:MSOP-10_3x3mm` | I²C voltage/current/power sense |
| R1 | 5 mΩ shunt | `Resistor_SMD:R_2512_6332Metric` | Current‑sense resistor for U1 |
| Q1 | AOD403 / FQP27P06 P‑MOSFET | `Package_TO_SOT_SMD:TO-252-2` | Load switch / reverse protection |
| D1 | Zener | `Diode_SMD:D_SOD-123` | Gate / rail clamp |
| C1 | 0.1 µF 0603 | `Capacitor_SMD:C_0603` | Decoupling |
| R4 | 100 kΩ 0603 | `Resistor_SMD:R_0603` | Bias/pull |
| J1 | JST XH 6‑pin vertical | `Connector_JST:...B6B-XH-A` | Harness to radio |
| J2, J3 | Pogo pin | `Radio Library:Pogo Radio Bottom 6MM` | Power/data contacts from battery |
| J4, J6 | Bolt pad | `Radio Library:Bolt Pad 4.25` | GND / mounting bolt |

The **Pogo Radio Bottom 6MM** targets here receive the **Pogo Battery 7MM**
contacts from *Battery Top*. The INA226 + 5 mΩ shunt measure battery
current/voltage and report over the data line.

## Working with it

1. Open `Kicad/Radio Bottom.kicad_pro` in **KiCad 10** or newer.
2. If the custom pogo/bolt footprints show as unresolved, re‑point the
   `Radio Library` entry — see [`../Kicad Footprint Libaries/README.md`](../Kicad%20Footprint%20Libaries/README.md).
3. After PCB changes, re‑run the **Fabrication Toolkit** plugin to refresh the
   fabrication output, then upload the zip + BOM to JLCPCB.
