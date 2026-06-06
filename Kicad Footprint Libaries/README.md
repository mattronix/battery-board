# Kicad Footprint Libaries

Shared custom KiCad footprint library for the PRC‑152A interface boards. Both
*Battery Top* and *Radio Bottom* reference it under the nickname **`Radio Library`**.

## Layout

```
Kicad Footprint Libaries/
└── Radio Library.pretty/   # standard KiCad .pretty library (one .kicad_mod per footprint)
```

A `.pretty` folder is just a directory of `.kicad_mod` files — KiCad treats the
folder itself as one library.

## Footprints

These are the **non‑standard mechanical contacts** that aren't in KiCad's stock
libraries. Each is a single through‑hole pad — a mechanical/contact feature, not
a multi‑pin part — sized to fit a specific pogo pin or bolt.

| Footprint | Pad | Drill | Pad Ø | Used for |
|-----------|-----|-------|-------|----------|
| **Pogo Battery 7MM** | 1× THT circle | 1.6 mm | 2.5 mm | Pogo pin pressed from the battery side |
| **Pogo Radio Bottom 6MM** | 1× THT circle | 0.9 mm | 2.5 mm | Mating pogo target on the radio side |
| **Bolt Pad 4.25** | 1× THT circle | 4.3 mm | 7 mm | M4 mounting bolt + GND pad |

> `*.kicad_mod.rsls` and `Untitled*` files are editor scratch/temp artifacts, not
> real footprints (they are git‑ignored).

## How projects find the library

Each project's `fp-lib-table` registers this library under the nickname
**`Radio Library`** via an **absolute path**:

```
(lib (name "Radio Library") (type "KiCad")
     (uri "/Users/.../Kicad Footprint Libaries/Radio Library.pretty"))
```

Footprints are then referenced as `Radio Library:<name>` (e.g.
`Radio Library:Pogo Battery 7MM`) in the PCBs.

⚠️ **The path is hard‑coded and absolute.** On another machine — or if the repo
moves — KiCad won't find the library and the custom footprints show as unresolved.
Fix it one of two ways:

1. **Per project** — *Preferences → Manage Footprint Libraries → Project* and
   re‑point the `Radio Library` row at this repo's `Radio Library.pretty`, **or**
2. **Use a path variable** — define a custom `${RADIO_LIB}` environment variable
   and edit each `fp-lib-table` to use it, so the projects stay portable.

Footprints carry the KiCad 10 format (`version 20260206`, `generator_version 10.0`);
open with KiCad 10 or newer to avoid format warnings.
