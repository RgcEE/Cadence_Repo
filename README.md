# Cadence Virtuoso — Analog IC Design Portfolio

VLSI analog circuit designs developed in Cadence Virtuoso using the AMS 0.35 µm (C35) PDK.
Simulations run via Spectre with BSIM3v3 device models on the University of Idaho ECE server.

---

## Technology

| Parameter | Value |
|---|---|
| PDK | AMS HIT-Kit 4.10 |
| Process | 0.35 µm CMOS (C35B4) |
| Tool | Cadence Virtuoso IC617 |
| Simulator | Spectre |
| Models | BSIM3v3 (`cmos53.scs`, section `cmostm`) |

---

## Module 01 — Device Characterization & CMOS Fundamentals

### NMOS I-V Characterization (`NMOS_iv`)

[Analysis](module01/NMOS_iv/analysis.md)

![NMOS I-V schematic](module01/NMOS_iv/schematic.png)

- W = 1.2 µm, L = 350 nm, W/L = 3.43 (AMS C35B4, `modn`)
- DC sweep: Id vs Vds family of curves (Vgs parametric)
- Vth extraction via constant-current method: **Vth = 0.523 V** (spec: 0.5 V ± 0.1 V)
- Confirmed correct PDK loading and BSIM3v3 model behavior

### CMOS Inverter VTC (`CMOS_inverter`)

[Analysis](module01/CMOS_inverter/analysis.md)

![CMOS inverter schematic](module01/CMOS_inverter/schematic.png)

- NMOS: `modn` W = 1.2 µm / PMOS: `modp` W = 2.4 µm, L = 350 nm, VDD = 3.3 V
- PMOS sized 2× for equal drive strength (mobility compensation)
- Vin swept 0 → 3.3 V, Vm extracted at Vout = Vin intersection

| Parameter | Value | Description |
|-----------|-------|-------------|
| Vm | 1.511 V | Switching threshold |
| Voh | 3.298 V | Output high |
| Vol | 2.936 mV | Output low |
| Vil | 1.223 V | Input low threshold |
| Vih | 1.716 V | Input high threshold |
| NMH | 1.582 V | High noise margin |
| NML | 1.220 V | Low noise margin |

Vm is 139 mV below VDD/2 (1.65 V ideal), consistent with AMS C35 electron/hole mobility ratio deviating from the 2× sizing assumption under BSIM3v3 short-channel effects.

---

## Module — Current Source Design

Four current source structures in PTM 180 nm CMOS, 3.3 V, BSIM3v3 Level 49. Target: IB = 2 µA at VDD = 3.3 V, T = 50°C, TT corner. PVT sweep across SS/TT/FF corners, −40°C to 140°C, VDD 2.7–3.3 V.

[Full module documentation](module_current_source/README.md)

### Circuit 1 — Simple current source

![Simple current source schematic](module_current_source/simple_current_source/simple_current_source_schematic.png)

- M1 diode-connected NMOS + RS1 = 1.35 MΩ sets Iref; M2 mirrors 1:1
- W = 1 µm, L = 500 nm (both transistors)
- TT: **IB1 = 2.013 µA** · FF: +3.4% · SS: −3.1%
- Positive temperature coefficient across all corners; VDD spread 1.35 µA over 2.7–3.3 V

### Circuit 2 — Self-biasing gm current source

![Self-biasing gm schematic](module_current_source/self_biasing_current_source/self_biasing_current_source_schematic.png)

- PMOS mirror forces equal current through source-degenerated M5 (RS2 = 6 kΩ) and M6
- Operating point: Iref = 1 / (2 · gm5 · RS2) — supply-independent by design
- TT: **IB2 = 2.045 µA** · FF: −2.8% · SS: −50.2% (fixed RS2)
- Negative temperature coefficient; 5× RS2 spread required across SS/FF corners

### Circuit 3 — Self-biasing gm with cascode stage

![Cascode stage schematic](module_current_source/self_biasing_current_source_cascode_stage/self_biasing_current_source_cascode_stage_schematic.png)

- Four independently sized regions; Rout ≈ 6.95 GΩ at L = 1 µm
- TT: **IB3 = 2.019 µA** · FF: +0.15% · SS: +0.49%
- 9× temperature stability improvement over Circuits 1–2; VDD spread reduced to 0.61 µA

### Circuit 4 — Self-biasing gm with start-up circuit

![Start-up circuit schematic](module_current_source/self_biasing_current_source_startup/self_biasing_current_source_startup_schematic.png)

- Adds PMOS injection stack (M29–M36) to resolve zero-current degenerate state at power-on
- PWL VDD ramp 0 → 3.3 V over 50 µs; transient verified across SS/TT/FF corners

### Cross-design summary

| Metric | Circuit 1 | Circuit 2 | Circuit 3 |
|--------|-----------|-----------|-----------|
| IB at TT, 3.3 V, 50°C | 2.013 µA | 2.045 µA | 2.019 µA |
| Temp swing −40 to 140°C | 45% | 45% | 5% |
| VDD spread 2.7–3.3 V | 1.35 µA | 1.35 µA | 0.61 µA |
| SS deviation from TT | −3.1% | −50.2% | +0.49% |
| FF deviation from TT | +3.4% | −2.8% | +0.15% |

---

