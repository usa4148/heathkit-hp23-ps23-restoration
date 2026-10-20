# Heathkit HP-23 Series & PS-23 Power Supplies — Restoration Plan

A restoration plan for the Heathkit HP-23, HP-23A, HP-23B, HP-23C and PS-23 AC power supplies, which run the HW-100/101, SB-100/101/102 and SB-110/110A transceivers. It covers:

- the differences between models;
- which parts fail with age (filter capacitors, bleeders, the bias section, diodes, the switch and cord, the 11-pin cable);
- a phased recap and variac bring-up, with load-test values;
- the manuals' troubleshooting charts;
- how to configure the supply for each rig (low-voltage tap, bias, pinout, the pin 9–10 jumper);
- maintenance and parts sources.

**Status:** Rev. 1, checked against the original Heath **HP-23A** (595-924-01) and **PS-23** (595-2314) manuals. Each item is marked as manual-verified or **[field]** (owner and web sources). The groups.io rebuild threads are listed but couldn't be read for this revision.

## Files

- [`hp23-ps23-restoration-plan.md`](hp23-ps23-restoration-plan.md) — source (pandoc markdown)
- [`hp23-ps23-restoration-plan.pdf`](hp23-ps23-restoration-plan.pdf) — rendered PDF

## Build

```sh
pandoc hp23-ps23-restoration-plan.md -o hp23-ps23-restoration-plan.pdf \
  --pdf-engine=xelatex -V mainfont="DejaVu Serif"
```

## Safety

These supplies produce about **+820 V DC** from a voltage doubler, and the filter capacitors can hold a lethal charge after the cord is unplugged. Unplug, let the bleeders discharge, **measure**, then short every capacitor to chassis before working inside.

## Sources

- Heath HP-23A and PS-23 assembly manuals (owner-supplied scans); more HP-23 manuals are on the W5RKL manuals page
- AF6C, *Heathkit of the Month #26: HP-23C* (w6ze.org)
- Heath mailing list archive (puck.nether.net): HP-23 variant differences, replacement PCBs
- Latta (Frostburg State) HP-23/PS-23 restoration and circuit description
- Restoration write-ups: Radio Boat Anchor (HP-23A), awsh.org (HP-23B)
- groups.io: Heathkit-Radios, heathkit, HeathKit-HFAmps, HallicraftersRadios

## See also

[heathkit-sb301-restoration](https://github.com/usa4148/heathkit-sb301-restoration) · [heathkit-hw101-restoration](https://github.com/usa4148/heathkit-hw101-restoration) · [heathkit-sb102-restoration](https://github.com/usa4148/heathkit-sb102-restoration) · [heathkit-sb110-restoration](https://github.com/usa4148/heathkit-sb110-restoration) · [heathkit-sb220-restoration](https://github.com/usa4148/heathkit-sb220-restoration) · [heathkit-sb200-restoration](https://github.com/usa4148/heathkit-sb200-restoration)
