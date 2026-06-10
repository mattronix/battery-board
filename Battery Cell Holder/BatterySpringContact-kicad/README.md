# Battery Contact — KiCad library (uxcell plate-style spring/button)

KiCad symbol + footprints for the uxcell-style stamped 18650/AA battery contact set
(flat plate, solder tab with end hole; coil spring = negative end, conical button =
positive end). One footprint serves BOTH ends — they mount identically; only the
cell-side feature differs.

## Contents
- `BatterySpringContact.kicad_sym` — symbol `Battery_Spring_Contact` (1 pin, passive)
- `BatterySpringContact.pretty/`
  - `BatteryContact_uxcell_Plate_TabSolder_18650.kicad_mod`  ← use this for your part
  - `Keystone_5222_SpringContact_THM.kicad_mod`              ← coil-spring alt (legacy)

## JLCPCB reality (read this)
This is a generic mechanical contact with NO LCSC part number, so JLCPCB's pick-and-place
CANNOT assemble it. JLCPCB will fabricate the board with the correct land; you HAND-SOLDER
the contacts afterward. Want JLCPCB to actually place a spring during assembly? You must
switch to an LCSC-catalogued SMD battery clip instead.

## How the footprint mounts
- Lay the metal tab FLAT onto the SMD pad and solder it (iron or paste+hotplate).
- The plated through-hole at the tab end is a solder/wire anchor.
- The plate + spring/button stands UP off the board toward the cell.
- Both the pad and the hole are tied to net "1" (single contact = one node).

## Footprint dimensions — NOMINAL, MEASURE TO CONFIRM
Built for the ~16 mm 18650 plate variant. Check these 5 numbers with calipers and edit
the `.kicad_mod` (values in mm):
1. Plate width (across) — sets fab/silk/courtyard circle. Nominal 16 (radius 8).
2. Tab length — sets SMD pad X (`size 9 6`) and pad center (`at 10.5 0`). Nominal ~9.
3. Tab width — sets SMD pad Y. Nominal 6.
4. Tab-end hole diameter — sets drill (`drill 1.8`). Nominal 1.8.
5. Plate-edge → hole-center distance — sets PTH `at 13 0`. Nominal 13.

Common size families if unsure: AA ≈ 12–13 mm plate; 18650 ≈ 16×16 or 18.5×16 mm.

## Install (KiCad 8)
1. Symbol: Preferences → Manage Symbol Libraries → add `BatterySpringContact.kicad_sym`
   (nickname `BatterySpringContact`).
2. Footprint: Preferences → Manage Footprint Libraries → add the `BatterySpringContact.pretty`
   folder under the SAME nickname so the symbol's default-footprint link resolves.
3. Place `Battery_Spring_Contact` once per cell end; assign +/− nets as needed.

## Mechanical (your 3D-printed shell)
- Capture the plate in the printed shell; don't rely on the solder joint for retention,
  especially under recoil/vibration (milsim use).
- Size the shell around the spring's COMPRESSED height so flat-top and protected cells
  both seat under contact pressure.


## RECOMMENDED footprint: BatterySpringContact_ConicalPCB_7mm
Conical PCB coil-spring contact (the 7 mm one). Verified dimensions:
- base coil OD 7 mm  -> 7.5 mm circular solder pad
- top coil OD 5 mm   -> what the cell end presses on
- free height 7 mm, wire 0.3 mm
Mount: solder base coil flat to the round pad; cell compresses the top axially.
This is the symbol's default footprint now.
WARNING: 0.3 mm wire is thin -> low current capacity. Great for sense/monitor or
low-current paths; do NOT route high pack-discharge current through it. Hand-solder
with flux (nickel-plated steel wets poorly); NOT JLCPCB pick-and-place.

## 3D model (STEP + WRL)
Files `BatterySpringContact_ConicalPCB_7mm.step` (66 KB, true mm) and `.wrl` (render).
The footprint already references the STEP via:
  (model "${KIPRJMOD}/BatterySpringContact_ConicalPCB_7mm.step" ...)
So the SIMPLEST setup: copy the .step (and .wrl) file into your KiCad PROJECT folder
(next to your .kicad_pcb) and ${KIPRJMOD} resolves automatically.

If you keep the models inside the .pretty folder instead, re-point the path:
  PCB editor -> double-click the footprint -> 3D Models tab -> remove the entry ->
  Add -> browse to the .step file. KiCad writes the correct absolute path for you.

Model is authored base-at-origin, +Z up (sits on top copper, spring points at the cell),
so offset/rotate are zero and scale is 1. KiCad 8's 3D viewer renders STEP directly.
