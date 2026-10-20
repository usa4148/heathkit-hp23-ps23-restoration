---
title: "Heathkit HP-23 Series & PS-23 Power Supplies — Restoration Plan"
subtitle: "Rev. 1 — checked against the original Heath HP-23A and PS-23 manuals"
date: "October 2026"
geometry: margin=0.9in
fontsize: 10pt
colorlinks: true
toc: true
---

<!-- Build: pandoc hp23-ps23-restoration-plan.md -o hp23-ps23-restoration-plan.pdf --pdf-engine=xelatex -V mainfont="DejaVu Serif" -->

> **Safety — read before opening the cabinet.** These supplies make **+820 V DC no-load** from a full-wave voltage doubler (C1/C2, 125 µF each). Heath's own words: *"The output voltage of this Power Supply can be lethal. Also, the potential stored in the large capacitors is dangerous"* (HP-23A p. 13; PS-23 p. 17). The PS-23 manual boxes the whole capacitor and diode area as a **hazardous-voltage location** (PS-23 Fig. 1, p. 17).
>
> - **Unplug the line cord** before any work. Unplugging the transceiver cable is not enough.
> - **Let the bleeders work, then prove it.** R1–R4 (100 kΩ 2 W) bleed the HV stack and R5 the LV. Wait several minutes, then **measure** pin 4 (HV), pin 3 (LV) and pin 1 (bias) to chassis with a meter before touching anything. Open or high bleeders, which are common (§3.2), mean the charge can stay for hours.
> - **Then short every capacitor to chassis.** Heath says to short the capacitor lugs to chassis with a screwdriver blade (HP-23A p. 13; PS-23 p. 17). A grounding stick with a series 1–10 kΩ power resistor, followed by a hard short, is kinder to the capacitors and the screwdriver. Short **both** C1 and C2: in a doubler each one holds about 400 V separately.
> - When live: stand on dry insulating material, work **one hand behind your back** (both manuals), and use clip leads attached with the power off.
> - The HP-23A switch only breaks one side of the line. The HP-23, HP-23A and HP-23C/PS-23 have no pilot light on most units, so **"dark" does not mean "off."**

# 1. Source documents

| Document | Where | Use |
|----------------------------------|--------------------------|----------------------------------------------|
| **Heath HP-23A Assembly Manual**, 595-924-01 (©1968), 19 pp. + schematic | Owner-supplied scan | **Primary reference** for HP-23/23A: parts list (p. 3), wiring (pp. 7–12), initial tests (pp. 12–13), operation (pp. 13–14), troubleshooting (p. 15), specs (p. 16), circuit description (p. 17), chassis photo (p. 18), schematic. |
| **Heath PS-23 Manual**, 595-2314 (©1979), 23 pp. | Owner-supplied scan | **Primary reference** for HP-23C/PS-23: parts list (p. 3), 350/275 VDC wiring (pp. 10–11), 3-wire cord (p. 13), cable (p. 14), initial tests (p. 15), operation (p. 17), troubleshooting (p. 19), specs (p. 20), circuit description (p. 21), photos (pp. 22–23). |
| AF6C, *Heathkit of the Month #26: HP-23C* | w6ze.org | Family history, years, variant differences. **[field]** |
| Heath list (puck.nether.net): K9STH and others on HP-23 variant differences (2007, 2009) | puck.nether.net | Variant feature matrix. **[field]** |
| Latta (Frostburg State), *HP-23/PS-23 Power Supply Restoration* and *Schematic & Circuit Description* | frostburg.edu | Measured failures, recap, variac bring-up. **[field]** |
| Radio Boat Anchor blog (HP-23A); awsh.org (HP-23B) | Web | Restoration write-ups with replacement part numbers. **[field]** |
| Restoration kits and boards: W8AOR (Scotty's Sled Shed), eBay kit sellers, Old Heathkit Parts board (K8GNZ design), WB8VGE HP-23R PCB | Web, Heath list | Parts sources (§9). **[field]** |
| **groups.io:** Heathkit-Radios, heathkit, HeathKit-HFAmps, HallicraftersRadios | groups.io | Rebuild discussions (§10). Topic pages would not load for this review; see §11. |

Notation: **[HP-23A p. n]** and **[PS-23 p. n]** cite the manuals' printed page numbers. **[field]** marks owner or web knowledge that the two manuals do not confirm.

# 2. The family at a glance

## 2.1 Models

| Model | Years **[field: AF6C]** | Line | Protection | LV B+ select | Bias | Filaments | Other |
|-----------|-----------|-----------|--------------|--------------------|-----------------|--------------|--------------------|
| **HP-23** | 1963–68 | 120 V only | **Fused plug** | Internal rewiring | Fixed −130 V + adjustable | 6.3 / 12.6 V | Front power switch and neon pilot; bleeders reportedly 50 kΩ **[field]** |
| HP-23E | — | 240 V 50 Hz | — | — | — | — | European version; made obsolete by the HP-23A's dual primary **[field]** |
| **HP-23A** | 1968–73 | **120/240 V** dual primary | **Circuit breaker** (65-17) | **Front DPDT "AC OFF" switch: right = 350 VDC, left = 275 VDC, center = off** | Fixed −130 V (pin 1) **+ adjustable −40 to −80 V (pin 11)** | 6.3 V @ 11 A (pin 8) / 12.6 V @ 5.5 A (pin 6) | **Manual-verified** [HP-23A pp. 2, 13–17]. 2-wire cord in this printing (89-19). |
| **HP-23B** | 1973–78 | 120/240 V | Breaker | Front switch (as HP-23A) | **Fixed only** | **12.6 V only** | **3-wire grounded cord** introduced **[field]** |
| **HP-23C** | 1978–80 | 120/240 V | Breaker | **Internal wiring again**; no switch | Fixed only | 12.6 V only | On/off only via the transceiver (pins 9–10) **[field]** |
| **PS-23** | 1979/80–83 | **120/240 V** | **2.92 A breaker** (65-17) | **Internal jumper: 350 VDC or 275 VDC wiring** | **Fixed −130 V only** | **12.6 V @ 5.5 A** | **Manual-verified** [PS-23 pp. 2–3, 10–11, 13, 20]. HP-23C renamed (Hewlett-Packard objected to "HP" model numbers) **[field]**. 3-wire cord standard. |

**Related:** the **HP-13 / 13A / 13B** are DC-to-DC *mobile* supplies (12–14 V DC in, about 25 A). They give the same rails: about 800 V HV, 310/265 V LV taps and an adjustable −40 to −130 V bias **[field: Heathkit Virtual Museum]**. They are not covered further here.

## 2.2 Specifications (identical in both manuals except as noted)

| Output | No load | Loaded | Ripple / duty | Pin |
|------------------|-----------|---------------------------|---------------------------------|-----|
| HV (doubler, 62.5 µF effective) | **820 V** | **700 V @ 250 mA** | < 1% @ 250 mA; continuous to 150 mA, 50% duty at 300 mA | 4 |
| LV high tap | **350 V** | **300 V @ 150 mA** (with 100 mA on HV) | < 0.05%; continuous to 175 mA | 3 |
| LV low tap | **275 V** | **250 V @ 100 mA** (with 100 mA on HV) | < 0.05%; continuous to 175 mA | 3 |
| Fixed bias | **−130 V** | **−100 V @ 20 mA** | — | 1 |
| Adjustable bias (**HP-23A only**) | −40 to −80 V | 1 mA max | — | 11 |
| Filaments | 12.6 VAC @ 5.5 A (both); **6.3 VAC @ 11 A (HP-23A only)** | | | 6 (12.6 V); 8 (6.3 V, HP-23A) |
| Line | 120/240 VAC, 50/60 Hz, **350 W max** | | | |
| Size / weight | 9 × 4¾ × 6¾ in.; 16 lb | | | |

Sources: [HP-23A p. 16; PS-23 p. 20].

## 2.3 Circuit (both manuals)

- **Primary:** breaker → (HP-23A: AC OFF switch) → dual primary, wired **parallel for 120 V, series for 240 V**. Output-socket **pins 9 and 10 are in series with the primary**, so the transceiver's power switch turns the supply on and off [HP-23A p. 17; PS-23 p. 21].
- **HV:** about **282 VAC** secondary → **full-wave voltage doubler** D1–D4 with C1, C2 (125 µF each). C1 and C2 charge on alternate half-cycles and discharge in series. **R1–R4** (100 kΩ 2 W) are the discharge path. **No further filtering** [HP-23A p. 17; PS-23 p. 21]. *[field: Latta]* Each capacitor sees about 400 V peak, and the diodes are used in series pairs because a 1N2071 is rated about 500 PIV.
- **LV:** the low-voltage winding is tapped at **95 or 125 VAC** → **half-wave voltage doubler** D5, D6 with C3, C4 → pi filter with C4 as input, **6 H choke L1**, and **C5** [HP-23A p. 17; PS-23 p. 21]. R5 (100 kΩ) bleeds the LV.
- **Bias:** about **95 VAC** from the low tap → **half-wave D7** → C6, R7 (1 kΩ 2 W), C7 → **−130 V fixed** [both].
  - HP-23A: R8 (10 kΩ 1 W), **R9 BIAS ADJUST (10 kΩ)** and R10 (10 kΩ 1 W) divide the bias to give **−40 to −80 V on pin 11** [HP-23A p. 17, schematic].
  - PS-23: **R8 is a 27 kΩ 1 W** bleeder on the bias line; there is no control [PS-23 p. 3, schematic].
- **Filaments:** a separate winding. The HP-23A's is center-tapped for 6.3 V or 12.6 V; the PS-23's is 12.6 V only [HP-23A p. 17; PS-23 p. 21].

## 2.4 Parts that matter (from the manuals)

| Ref | HP-23A [p. 3, schematic] | PS-23 [p. 3, schematic] | Role |
|--------|---------------------------------|---------------------------------|-------------------------|
| C1, C2 | 125 µF tab-mount (25-34), **voltage not printed** | **125 µF 500 V** (25-265) | HV doubler |
| C3, C4 | 125 µF (25-34) | 125 µF 500 V (25-265) | LV doubler; C4 is the pi-filter input |
| C5 | 40 µF (25-36) | **40 µF 450 V** (25-36) | LV pi-filter output |
| C6, C7 | **20 µF** (25-19), voltage not printed | **40 µF 150 V** (25-20) | Bias filter (**negative supply: + lead to ground**) |
| R1–R5 | 100 kΩ 2 W (1-24-2) ×5 | 100 kΩ 2 W (1-24-2) ×5 | HV bleeders (R1–R4), LV bleeder (R5) |
| R7 | 1 kΩ 2 W (1-15-2) | 1 kΩ 2 W (1-15-2) | Bias filter series resistor |
| R8 | 10 kΩ 1 W (1-9-1) | **27 kΩ 1 W** (1-46-1) | HP-23A: bias-control limiter; PS-23: bias bleeder |
| R9 | 10 kΩ tab-mount control (10-57) | — | HP-23A BIAS ADJUST |
| R10 | 10 kΩ 1 W (1-9-1) | — | HP-23A bias-control limiter |
| D1–D7 | Silicon diode (57-27) ×7 | **1N2071** (57-27) ×7 | D1–D4 HV, D5–D6 LV, D7 bias |
| L1 | Filter choke 46-22 (6 H per schematic) | Filter choke 46-56 | LV filter |
| T1 | 54-198 | 54-807 | Power transformer |
| Breaker | 65-17 (rating not printed) | 65-17, **2.92 A** | Primary protection |
| Switch | **DPDT toggle 61-12 (AC OFF / 350 / 275)** | — | — |
| Cord | 89-19 (2-wire in this printing) | 89-23 **flat 3-wire**, green to chassis lug | — |
| Output | 11-pin socket 434-118, 11-pin plug 438-29 with cap 440-1, 8-wire cable 347-18 | Same socket and plug; cable 347-52 | — |

Both manuals give **no voltage rating for the HP-23A's 125 µF and 20 µF parts**. The PS-23 parts list rates the same positions 500 V and 150 V; use those as the minimum for any replacement.

# 3. What fails, ranked

## 3.1 Electrolytic capacitors (the near-certain failure)

| Ref | Likelihood | Symptom | Action |
|--------|-----------|--------------------------------------------|----------------------------------------------|
| **C1, C2** (HV) | **Very high** | HV low or sagging; hum on transmit; one cap shorted gives **half HV** (PS-23 chart lists "Capacitor C1 or C2") [PS-23 p. 19]. Latta measured low capacitance on C1. | Replace **as a pair**: **150 µF 500 V** (Latta) or 125–150 µF **≥ 450 V**. Because the doubler puts each cap at about 400 V, 500 V parts give margin; see §11. Keep a **100 kΩ 2 W bleeder across each**. |
| **C3, C4** (LV) | High | Low or noisy B+, hum | 150 µF 450–500 V. |
| **C5** (LV output) | High | Hum or ripple on B+. **[field: HW-101-73]** An open C5 causes a tone on CW and relay chatter. | 47 µF 450 V. |
| **C6, C7** (bias) | **High** | **Low or no bias → the transceiver's finals run away** (excess plate current on receive/standby). Latta found **both open**. | Same µF as fitted (**20 µF HP-23A; 40 µF PS-23**) or 47 µF, **≥ 160 V**. **Negative supply:** the + lead goes to chassis. |

The originals are large twist-tab cans on phenolic and metal mounting wafers. Modern snap-in parts are much smaller **[field: AF6C]**. Options:

1. Mount new capacitors on the **original wafers** with lugs and hot-glue or clamps (Radio Boat Anchor).
2. **Restuff the cans**: open them, gut them, solder the new capacitor's leads to the original terminals (awsh.org).
3. Fit a **replacement PC board** (§9).

> **Wafer warning [field: Radio Boat Anchor]:** note which capacitors sit on **phenolic** (insulating) and which on **metal** (grounded) wafers before removal. In the doubler, C1's and C3's cans are **not at ground**. Putting a phenolic-wafer can on a metal wafer shorts the HV or LV section. The HP-23A manual calls for two metal (481-1) and two phenolic (481-3) wafers [HP-23A p. 3]; the PS-23 the same [PS-23 p. 3].

## 3.2 Resistors

| Ref | Failure | Action |
|--------------------|-------------------------------------------------|----------------------------------------------|
| **R1–R4** (HV bleeders, 100 kΩ 2 W) | Drift high (Latta: about **150 kΩ** each), so stored charge lasts longer. Run hot by design. | Replace all with **100 kΩ 2–3 W metal film or metal oxide**. Space them off the board or wafer (they run warm). |
| R5 (LV bleeder) | Same | Same. |
| **R7** (1 kΩ 2 W) | Open → **no bias** [both charts] | Replace, 2 W. |
| **R8** (PS-23: 27 kΩ **1 W**; HP-23A: 10 kΩ 1 W) | Open → no bias [both charts]. The PS-23's 1 W bleeder runs near its rating **[field: Latta upgraded it to 2 W]**. | Replace; 2 W on the PS-23. |
| R9, R10 (HP-23A bias control) | Open → no adjustable bias [HP-23A p. 15] | Replace the control if scratchy or open (only matters if you use pin 11). |

## 3.3 Diodes

| Ref | Failure | Action |
|-----------|-------------------------------------------|----------------------------------------------|
| D1–D4 | Open → **HV only half of indicated value at no load, less under load**; faulty → no HV [both charts] | Replace all with **1N4007** (1000 PIV, 1 A) **[field: Latta]**. Observe the banded (cathode) end exactly as the pictorials show [HP-23A p. 9; PS-23 p. 12]. |
| D5, D6 | Open → **LV half of indicated value**; or no LV [both charts] | 1N4007. |
| D7 | Open → **no bias** [both charts] | 1N4007, cathode as shown (the bias is negative). |

Silicon diodes rarely fail. Most restorers replace all seven while the board is open, because they're cheap and the 1N2071's 500 PIV leaves little margin in the HV string.

## 3.4 Primary, switch, line cord, breaker

| Item | Failure / risk | Action |
|-----------------------|------------------------------------------------|----------------------------------------------|
| **HP-23 fused plug** **[field]** | Two-fuse plug, no ground | Replace with a **3-wire grounded cord**. Fit a chassis-mounted fuse in the **hot** lead (Radio Boat Anchor used a single 4 A fuse; see §11). |
| **HP-23A 2-wire cord** (89-19) | No safety ground | **3-wire cord, green to chassis** (copy the PS-23 method: green to a solder lug on a scraped mounting hole [PS-23 pp. 4, 13]). |
| **Breaker** (65-17; PS-23 2.92 A) | Weak (nuisance trips) or welded | Test trip and reset. "Breaker opens when reset" means a short in the transformer, rectifier/filter, or the **connected equipment** [both charts]. |
| **HP-23A AC OFF switch** (DPDT 61-12) | Worn contacts. It switches **both the primary and the LV tap**, so a bad contact gives "completely inoperative" or "no low B+" [HP-23A p. 15]. | Clean or replace. **Label the position** if the supply is shared between rigs (§6). |
| Line-voltage wiring | Wrong primary connection → **all voltages half** [both charts] | Check the primary wiring against the manual: parallel for 120 V [HP-23A p. 7; PS-23 p. 8]. |

## 3.5 Transformer and choke

- **T1:** rarely fails unless a shorted capacitor or diode was run hard. A breaker that trips with the secondaries disconnected points to T1 [both charts]. Check for tar, smell and discoloration.
- **L1:** **open choke → no low B+** [both charts]. Replacement chokes are discussed on Heathkit-Radios ("HP-23A choke replacement").

## 3.6 Output socket, cable and plug

- **Pins 9–10 open** (cable, plug or the transceiver's switch) → **completely inoperative** [both charts]. This is the most common "dead supply" that isn't a supply fault.
- **Pin 3 or 4 open** → no LV or no HV [both charts]. Check the 8-wire cable for cracked insulation, especially the yellow HV conductor (pin 4).
- Re-solder the 11-pin plug pins; the 12.6 V pin carries 5.5 A. On the HP-23A at 6.3 V, more than 6 A needs the red conductor **paralleled with #18 wire** [HP-23A p. 12].
- **Wrong pins:** the HP-23A's **supply-end socket** differs from the transceivers' plug tables at pins 5, 8 and 11 (§6.2). Don't copy one onto the other.

# 4. Restoration plan

## 4.1 Phase 0: Identify and assess (unplugged)

1. **Identify the model** (§2.1): line cord and plug, breaker vs fuse, front switch, rear BIAS control, model label. The blue-and-white label on the bottom plate gives the model and series number [HP-23A p. 11; PS-23 p. 16].
2. **Record the LV configuration:** HP-23A switch position, or PS-23 / HP-23 / HP-23C internal wiring (**350 VDC**: brown jumper strip M lug 2 → capacitor K lug 2; **275 VDC**: strip C lug 2 → capacitor K lug 4 [PS-23 pp. 10–11]).
3. **Record the cable wiring:**
   - Green on pin 1 or 11.
   - Red on pin 6 or 8.
   - Brown/blue on pins 9–10, or a jumper fitted instead.
4. **Discharge** as in the safety box, then photograph everything, especially **which caps sit on phenolic vs metal wafers**.
5. **Inspect:** bulging or leaking cans, scorched bleeders, cord condition, breaker, switch, choke, and the transformer (tar).
6. **Run the manual's resistance check** (table below) on the as-found unit. Use a VTVM or analog VOM. **Solid-state ohmmeters don't forward-bias the diodes** [PS-23 p. 15].

**Initial-test resistance check**

To set the meter polarity, use R × 1 across the terminal strip:
- HP-23A: strip **J** lugs 3–4 [HP-23A p. 12].
- PS-23: strip **H** lugs 3–4 [PS-23 p. 15].

Find the lead orientation that reads about **10 Ω**, and put the matching lead on ground (pin 7, or capacitor E lug 4 on the PS-23). Then switch to **R × 10 k** and measure each socket pin. Wait for the needle while the capacitors charge.

| Pin | HP-23A [p. 13] | PS-23 [p. 15] |
|-----|-------------------|---------------------|
| 1 | 22 kΩ minimum | 22 kΩ or higher (reverse the leads if not) |
| 2 | ∞ | ∞ |
| 3 | 75 kΩ minimum | 75 kΩ or higher |
| 4 | 75 kΩ minimum | 75 kΩ or higher |
| 5 | ∞ | ∞ |
| 6 | ∞ | ∞ |
| 7 | **0 Ω** | **0 Ω** |
| 8 | ∞ | ∞ |
| 9 | ∞ | ∞ |
| 10 | ∞ | ∞ |
| 11 | **10–20 kΩ** (varies with BIAS ADJUST) | ∞ |

## 4.2 Phase 1: Recap and rebuild

1. Replace **C1–C7** (§3.1), observing the wafer and polarity notes. **C6 and C7 are reversed:** + to ground.
2. Replace **R1–R5** (bleeders), **R7** and **R8** (§3.2).
3. Replace **D1–D7** with 1N4007, cathodes as in the pictorials (§3.3).
4. Fit a **3-wire grounded cord** (§3.4); keep or add primary protection.
5. Clean the switch (HP-23A) and the bias control (HP-23A). Tighten the wafer, strip and socket hardware; **metal capacitor wafers and terminal-strip feet must be well grounded** [HP-23A p. 14; PS-23 p. 18].
6. Re-solder the 11-pin plug; check the cable.
7. **Re-run the §4.1 resistance check** before applying power.

## 4.3 Phase 2: Variac bring-up (unloaded)

**[field: Latta, Radio Boat Anchor, awsh.org]**

1. **Jumper pins 9 and 10** of the output socket (a clip lead is fine), so the supply runs without a transceiver.
2. Optionally lift the HV, LV and bias secondary leads first (Latta lifted the red, blue and brown-yellow leads), so you check only the transformer at low voltage.
3. Plug into a **variac**. Bring it up in steps of **10 → 25 → 50 → 75 → 100%**, pausing at each step. At each step, check that every output (or secondary) reads about that percentage of its full value. Watch for heat, smell or noise.
4. **New electrolytics benefit from a slow first charge.** Hold 25–50% for several minutes before going to 100%.
5. At 100%, unloaded, with an **11 MΩ VTVM/DVM**, expect about:
   - **+820 V** on pin 4;
   - **+350 V or +275 V** on pin 3, per the configuration;
   - **−130 V** on pin 1;
   - **12.6 VAC** on pin 6.

   The manuals' schematic voltages are with **no load** [HP-23A p. 14; PS-23 p. 18]. New capacitors read slightly **higher** than tired originals **[field]**.

## 4.4 Phase 3: Load test (optional but recommended)

The spec voltages are under load (§2.2). A resistive load shows regulation and reveals a weak capacitor or diode. The values below are **derived from the spec currents** (Ohm's law); they are not Heath figures.

**Use series strings,** so no single resistor sees more than its voltage rating (typically a few hundred volts for wirewound power resistors). Mount them in free air, test briefly, and **discharge after each test**.

| Output | Test load | Approx. resistance | Dissipation | Build (example) | Expect |
|-------------------|----------------|-------------|------------|-----------------------------------|--------------------------|
| HV (pin 4) | **100 mA** (continuous rating 150 mA) | ~7.5 kΩ | ~75 W | 4–6 × 50 W wirewound in series (e.g. 6 × 1.2 kΩ) | Between 820 V (no load) and 700 V (250 mA); roughly 770 V |
| LV high tap (pin 3) | 150 mA | **2.0 kΩ** | ~45 W | 2 × 1 kΩ 50 W in series | **≈ 300 V** (with ~100 mA on HV) |
| LV low tap (pin 3) | 100 mA | **2.5 kΩ** | ~25 W | 2 × 1.2 kΩ 25 W in series | **≈ 250 V** (with ~100 mA on HV) |
| Bias (pin 1) | 20 mA | **5 kΩ** | ~2 W | 5.1 kΩ 5 W | **≈ −100 V** |
| Filament (pin 6) | 5.5 A | ~2.3 Ω | ~70 W | 2.2 Ω 100 W, or two 12 V automotive lamps (~35 W each) | **12.6 VAC** |

Use **two meters** or clip-on probes. Never move a probe on a live HV load. For a quick check, a lighter HV load (e.g. 15 kΩ / 50 mA / ~40 W) is fine.

## 4.5 Phase 4: Configure and connect

Set the LV tap and make up or check the cable for the target rig (§6). Remove the 9–10 jumper if the rig switches the supply. Connect to the transceiver, then power on through the rig's switch with the rig's own initial-test procedure.

# 5. Symptom quick-reference

Combined from the manuals' troubleshooting charts [HP-23A p. 15; PS-23 p. 19], plus field notes.

| Symptom | Possible cause |
|-------------------------------|------------------------------------------------------------------|
| **Completely inoperative** | Breaker open. **Pin 9 or 10** (socket or plug) open or miswired. **Remote switch in the transceiver** faulty. Switch defective (HP-23A). Power transformer. |
| **Breaker opens when reset** | Short in the transformer windings; short in the rectifier or filter circuits (a **shorted electrolytic or diode**); **short or overload in the connected equipment**. |
| **No high B+** | D1–D4 faulty. Pin 4 on the socket or plug open or miswired. Power transformer. |
| **No low B+** | D5 and/or D6 open. **Choke L1 open.** Pin 3 open or miswired. HP-23A switch miswired or faulty. **Diode or capacitor polarity wrong** (after a recap!). |
| **No bias** | D7 open. **R7 or R8** open (HP-23A: also **R9**). Pin 1 (or 11) open or miswired. **[field]** C6/C7 open or shorted. |
| **HV only half, less under load** | D1, D2, D3 or D4 open; (PS-23) **C1 or C2**. |
| **LV only half** | D5 or D6 open. |
| **All voltages half** | Primary wired for 240 V on a 120 V line. |
| Hum on transmit, HV sags under load **[field]** | Tired C1/C2 (low µF, high ESR). |
| Tone on CW, relay chatter **[field: HW-101-73]** | C5 open. |
| Rig's relays stay energized; low TX power **[field: HW-101 manual p. 153; K4JPN]** | **LV on the 275 V tap** when the rig needs 350 V. |
| Finals draw excess current at idle **[field]** | Bias low: C6/C7, D7, R7/R8, or on the HP-23A the cable green wire on **pin 11** (adjustable, −40 to −80 V) instead of pin 1. |
| Shock from the chassis **[field]** | 2-wire cord, reversed polarity, no ground. Fit a 3-wire cord. |

# 6. Configuring for each rig

## 6.1 LV tap and bias, by rig

The supply manuals **do not name specific transceivers**. They say connection instructions come with the Heath equipment [HP-23A p. 13; PS-23 p. 17]. The settings below come from **the rig manuals** (checked in the companion plans) and the field.

| Rig | LV B+ needed | HP-23A switch | HP-23 / HP-23C / PS-23 wiring | Bias source | Source |
|-------------------|---------------------|-----------------------|------------------------------|-------------------------------|-------------------------------|
| **HW-101** | **300 V @ 150 mA** | **Right (350 VDC)** | **350 VDC wiring** | Fixed −130 V on **pin 1**; set with the **rig's BIAS** | HW-101 manual pp. 114, 153, 159 |
| **SB-102** | **300 V @ 150 mA** | **Right (350 VDC)** | **350 VDC wiring** | Pin 1; the rig's BIAS ADJUST | SB-102 manual pp. 98–100 |
| **SB-110A** | **+250 V @ 100 mA** (≤ 290 V) | **Left (275 VDC)** | **275 VDC wiring** | Pin 1 (covers −115 V @ 10 mA); the rig's BIAS ADJ | SB-110A manual pp. 86–87, 109, 123 |
| HW-100, SB-100, SB-101 | 300 V **[field]** | Right | 350 VDC | Pin 1 | Heath list; confirm in the rig manual |
| HW-12/22/32 (single-banders) | Low tap **[field]** | Left | 275 VDC | Per the rig manual | Heath list (Bernie, 2006); AF6C (the HP-23 was designed for them) |
| SB-301 / SB-401 | — | — | — | — | **Self-powered; they don't use an HP-23** |

**Why the rig manuals say "the HP-23A BIAS control is inoperative":** the HP-23A's R9 control only feeds **pin 11** (adjustable −40 to −80 V). When the cable is made up with the **green wire on pin 1** (fixed −130 V), as these rigs require, R9 does nothing. The rig divides the −130 V with its own BIAS control [HP-23A pp. 12, 17, schematic]. The PS-23, HP-23B and HP-23C have no adjustable bias at all.

**Sharing one supply:**
- **HP-23A:** the same lever is power *and* B+ select. Label it ("350 = HW-101/SB-102, 275 = SB-110A").
- **PS-23 / HP-23C / HP-23:** moving between an HW-101/SB-102 and an SB-110A means **rewiring the internal jumper**.
- **Measure pin 3 before connecting a different rig.**

## 6.2 11-pin output socket and cable (supply end)

| Pin | Cable wire | HP-23A [p. 12–13, schematic] | PS-23 [pp. 14, 17] |
|-----|------------|-------------------------------|-------------------------------|
| 1 | Green | **Fixed bias −130 V** (green here for these rigs) | **Fixed bias −130 V** |
| 2 | Black | Filament common | Filament common |
| 3 | Orange | LV **+350 V or +275 V** (switch) | LV **+350 V or +275 V** (wiring) |
| 4 | Yellow | **HV +820 V** | **HV +820 V** |
| 5 | — | NC | NC |
| 6 | Red | **12.6 VAC** filament | **12.6 VAC** filament |
| 7 | White | Ground | Ground |
| 8 | (Red alt.) | **6.3 VAC** filament (HP-23A only; red goes here for 6 V rigs) | NC |
| 9 | Brown | AC switch in equipment | AC switch in equipment |
| 10 | Blue | AC switch in equipment | AC switch in equipment |
| 11 | (Green alt.) | **Adjustable bias −40 to −80 V** | NC |

**Pins 9–10:** these are in series with the supply's primary. If the equipment has **no** AC switch provision, fit a **jumper between pins 9 and 10 of the cable plug** and leave the brown and blue wires **unconnected** (cut and tape them on the PS-23) [HP-23A p. 12; PS-23 p. 14]. The same jumper on the socket is the bench-test method (§4.3).

The table above is the **supply** end. Each transceiver's own power-plug table is the **rig** end, and the rig end uses pins 5, 8 and 11 differently (e.g. linear-amp relay contacts on the SB-110A). Make up the cable from the rig manual's figure.

# 7. Tips and tricks

- Keep the **original cans** as shells (restuff them) if appearance matters; otherwise a replacement board is the cleanest job (§9).
- **Hot-glue or clamp** the new capacitors and leads, and dress the wiring away from the bleeders, before closing up **[field: awsh.org]**.
- Rust on the hardware: soak it in vinegar for a few days **[field: awsh.org]**.
- Measure and **record** every no-load output before and after the rebuild. It's the quickest health check later.
- Mark the **LV setting on the bottom plate** (e.g. "350 VDC wiring — HW-101/SB-102").
- Keep a spare **11-pin plug and cap** (438-29 / 440-1 equivalents). Cracked plug caps are common.
- The supplies get hot under a 50% duty transmit load. Keep the top mesh clear and give them air.

# 8. Maintenance schedule

| Interval | Task |
|------------------|--------------------------------------------------------------|
| Each use | Feel for abnormal heat after long transmit sessions; listen for transformer buzz changes. |
| Yearly | Measure the no-load outputs (pins 1, 3, 4, 6) against your recorded values. Check that the breaker trips and resets. Inspect the cord and cable. |
| 2–3 years | Re-check bleeder values; retighten wafer and strip hardware (grounds); clean the HP-23A switch. |
| 5 years | ESR/leakage check on C1–C7; load test (§4.4). |
| ~15–20 years | Plan the next recap of the electrolytics. |
| Storage | Dry and room temperature. **Power up through a variac** after more than a year idle to re-form the capacitors. |

# 9. Parts sources

| Source | What | Notes |
|---------------------------|-------------------------------------------|-------------------------------------------|
| **W8AOR / Scotty's Sled Shed** | *HP-23 (all versions) / PS-23 Restoration Component Kit* (~US$65): capacitors, resistors, diodes **plus capacitor circuit boards** | Covers HP-23, 23A, 23B, 23C and PS-23. Values aren't listed online; ask the seller. |
| eBay kit sellers | "HP-23 A/B/C + PS-23 Rebuild Restoration Capacitor Kit" (V1/V2), "HP-23 PS-23 restoration kit" | Several sellers; check contents and ratings (500 V on C1/C2). |
| **Old Heathkit Parts board** (K8GNZ design) | Single PCB replacing the base wiring, four 450 V caps, 3 W 100 kΩ bleeders; parts list on CD | Reviewed on amateurradio.com (HP-23B). Vendor availability is uncertain (a 2021 comment points to webusa.us). |
| **WB8VGE (The Heathkit Shop / SunLight Energy Systems)** | HP-23R replacement PCB, mini or full kits (2005) | Heath list post; current availability unknown. |
| Mouser / Digi-Key | Individual parts | Examples used by restorers: **C1–C4** 150 µF 450–500 V snap-in (e.g. Cornell-Dubilier 381LX151M450K022); **C5** 47 µF 450 V; **C6/C7** 22–47 µF 160–200 V; **R1–R5** 100 kΩ 2 W metal film (e.g. Yageo FMP200JR-52-100K); **D1–D7** 1N4007. |
| Hardware stores | 18/3 SJT line cord, strain relief | — |

# 10. Sources

**Manuals (primary):**

- Heath, *Heathkit Assembly Manual, AC Power Supply Model HP-23A*, 595-924-01, ©1968.
- Heath, *Heathkit Manual for the AC Power Supply Model PS-23*, 595-2314, ©1979.

**Field and web:**

- AF6C, Heathkit of the Month #26 — HP-23C: https://www.w6ze.org/Heathkit/Heathkit_026_HP23C.pdf
- Heath list, "Heathkit HP-23 Power Supplies" (K9STH, Apr 2009): https://puck.nether.net/pipermail/heath/2009-April/024735.html and https://puck.nether.net/pipermail/heath/2009-April/024736.html
- Heath list, "Power Supply Differences" (Mar 2007): https://puck.nether.net/pipermail/heath/2007-March/022626.html
- Heath list, "Heathkit HP 23 Power Supplies" (Dec 2006): https://puck.nether.net/pipermail/heath/2006-December/022248.html
- Heath list, "Heathkit HP-23R replacement pcb kits" (WB8VGE, Jan 2005): https://puck.nether.net/pipermail/heath/2005-January/018677.html
- Latta, HP-23/PS-23 Power Supply Restoration: https://www.frostburg.edu/personal/latta/ee/hw101/pwrsuprestoration/pwrsuprestoration.html
- Latta, Power Supply Schematic and Circuit Description: https://www.frostburg.edu/personal/latta/ee/hw101/powsupschematic/pwrsupschematic.html
- Radio Boat Anchor, Heathkit HP-23A Power Supply Restoration: http://www.radioboatanchorblog.com/blog/heathkit-hp-23a-power-supply-restoration
- awsh.org, Heathkit HP-23B Power Supply Restoration: https://awsh.org/heathkit-hp-23b-power-supply-restoration/
- amateurradio.com, HP-23 upgrade board kit from Old Heathkit Parts: https://www.amateurradio.com/hp-23-upgrade-board-kit-from-old-heathkit-parts/
- W8AOR restoration kit: https://w8aor.com/online-store/ols/products/heathkit-hp-23-all-versionsps-23-restoration-component-kit
- Heathkit Virtual Museum, HP-13: https://heathkit-museum.com/ham/hkHP-13.html
- W5RKL manuals page (HP-23, 23A, 23B, 23C manuals and schematic): https://w5rkl.com/manuals/

**groups.io: where to look for rebuild discussions**

| Group | URL | What to search for there | Thread titles seen |
|-------------------|------------------------------------------|----------------------------------------|----------------------------------------|
| **Heathkit-Radios** | https://groups.io/g/Heathkit-Radios | "HP-23", "HP-23A", "PS-23", "choke", "breaker", "doubler", "bias", "recap" | "HP-23 power supply" (/topic/83964562); "HP-23 power supply — another…" (/topic/83968589); "Voltage doubling power…" (/topic/83969148); "HP-23E circuit breaker" (/topic/45642989); **"HP-23A choke replacement"** (/topic/109162368); "Recovering an HW-101" (/topic/86408555) |
| **heathkit** | https://groups.io/g/heathkit | "HP-23", "PS-23", "power supply", "HP-13" | **"Differences between HP-23A…"** (/topic/102721038); **"Heathkit PS-23 power supply"** (/topic/108728345) |
| **HeathKit-HFAmps** | https://groups.io/g/HeathKit-HFAmps | Amplifier-focused. Search "HP-23" and "power supply" for supply-related posts; no HP-23-specific threads were found in this search | — |
| HallicraftersRadios | https://groups.io/g/HallicraftersRadios | "HP-23" | "Healthkit HP-23" (/topic/15709800) |

# 11. What the manuals did and didn't confirm; conflicts

**Confirmed by the manuals:**
- Specifications (§2.2) are identical in both manuals; the HP-23A adds adjustable bias and the 6.3 V/11 A filament.
- HP-23A front switch: **right = 350 VDC, left = 275 VDC, center = off** [p. 14].
- PS-23: **jumper-selected** 350/275 VDC wiring [pp. 10–11]; **2.92 A breaker**; **flat 3-wire cord**; no bias control.
- The full parts lists, initial-test resistance tables, troubleshooting charts, circuit descriptions and the 11-pin pinout (§6.2).
- **Why the HP-23A bias control is "inoperative" with the HW-101, SB-102 and SB-110A:** R9 feeds only pin 11.

**Not in the manuals / unreadable:**
- **Voltage ratings for the HP-23A's 125 µF and 20 µF capacitors**, and its **breaker rating** (65-17). The PS-23 uses the same 65-17 part, rated **2.92 A**, so the HP-23A breaker is *probably* the same; inferred, not confirmed.
- Model-by-model years and feature differences (HP-23, B, C, E), the 50 kΩ HP-23 bleeders, and the reason for the PS-23 rename come only from field sources (§2.1).
- No **service bulletins** for the HP-23 family were found in the Heath list's transcribed bulletin series.
- Neither manual names the transceivers each setting is for (§6.1 comes from the rig manuals).

**Conflicts to be aware of:**

| Item | Source A | Source B | Resolution |
|---------------------------|---------------------------|---------------------------|----------------------------------------------|
| C1–C4 voltage rating | **PS-23 manual: 125 µF 500 V** | Field (Radio Boat Anchor; can markings): **450 V** | Replace with **500 V** parts (or ≥ 450 V with healthy bleeders). Each doubler cap sees about 400 V peak. |
| C1 original value | Both manuals: **125 µF** | Latta lists **100 µF 450 V** | Latta's unit was a homebuilt clone (PS-23 transformer); use the manual's 125 µF (150 µF replacement). |
| Bias caps C6/C7 | **HP-23A: 20 µF** | **PS-23: 40 µF 150 V** | Model difference; either value works. Replace with the same or larger µF, ≥ 160 V. |
| R8 | HP-23A: 10 kΩ 1 W (limiter) | PS-23: 27 kΩ 1 W (bleeder) | Different circuits. Don't swap. |
| HP-23A protection | **HP-23A manual: circuit breaker** | Radio Boat Anchor's "HP-23A" had a **two-fuse plug** | A fused plug indicates a plain **HP-23** (or a modified unit); field sources agree the breaker started with the HP-23A. |
| HP-23A adjustable bias range | **Manual: −40 to −80 V** | Radio Boat Anchor: "up to −130 V" | Use the manual. −130 V is the **fixed** output on pin 1. |
| "250/OFF/300" switch | Field shorthand | **HP-23A manual: labeled 275 / 350 VDC** (no-load) | Same switch. 275 VDC no-load = 250 V loaded; 350 = 300. |
| HP-23C introduction | AF6C: 1978 | AF6C also says replaced by PS-23 in 1979; PS-23 manual ©1979 | Dates approximate. |
| HV figure | Spec: 820 V no-load / 700 V at 250 mA | Latta computes ~796 V from 282 VAC | Within line-voltage tolerance. Modern 120–125 V lines push no-load HV higher. |

**groups.io:** every topic page on Heathkit-Radios, heathkit and HallicraftersRadios returned a fetch error during this review. The groups and visible thread titles are listed in §10, but **no content from them is quoted**. HeathKit-HFAmps showed no HP-23-specific threads in search. Read those threads directly, especially "HP-23A choke replacement" and "Differences between HP-23A…", and fold any findings into a Rev. 2.
