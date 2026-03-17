# AMS C35 PDK Cadence Spectre Troubleshooting Log

Errors encountered during Module 01 setup on the AMS C35 350nm PDK with Cadence Virtuoso / Spectre. Each entry covers the symptom, root cause, and fix.


## Issue 1 Wrong Model File Format: Eldo vs Spectre

**Symptom:** PDK model files found under `eldo/c35/` (e.g. `cmos53tm.mod`) would not load in ADE L.

**Root cause:** The `eldo/` directory contains Mentor Eldo SPICE-format files. Cadence Spectre requires `.scs` (Spectre native format). The two are not interchangeable.

**Fix:** Use the Spectre model file:
```
/tools.new/examples/pdk/ams035/spectre/c35/cmos53.scs
```

**Reference:**
- `.mod` / `.spi` SPICE/Eldo format, `.MODEL` syntax
- `.scs` Spectre native format, `model` syntax (no dot prefix)


## Issue 2 Wrong Device Name: mosinsub vs modn

**Symptom:**
```
ERROR (SFE-23): The instance `M0' is referencing an undefined model or subcircuit, `mosinsub'.
```

**Root cause:** `mosinsub` is the raw BSIM3v3 model card it is not directly instantiable. The correct device is `modn`, an `inline subckt` that wraps `mosinsub` and handles parameter passing.

**Fix:** Set the instance `model` parameter to `modn` (NMOS) or `modp` (PMOS). Always set `w` and `l` explicitly:
```
model = modn
w = 1.2u
l = 350n
```


## Issue 3 SFE-2001: modn=1 Process Flag Undefined

**Symptom:**
```
ERROR (SFE-2001): Cannot run the simulation because an unknown parameter `modn'
has been specified in expression `modn'.
```

**Root cause:** Inside the `modn` subckt, a conditional `if (modn==1)` selects between the real device and a dummy resistor. This `modn` is a global process flag not the subckt name. It must be set to `1` before `cmos53.scs` loads. The flag lives in the processOption files, not in `cmos53.scs` itself.

**Fix:** Add two files to ADE L Model Libraries **before** `cmos53.scs`:

| Order | File | Section |
|-------|------|---------|
| 1 | `.../spectre/c35/process.scs` | (blank) |
| 2 | `.../spectre/c35/processOption/C35B4C0.scs` | (blank) |
| 3 | `.../spectre/c35/cmos53.scs` | `cmostm` |

Available corners in `cmos53.scs`: `cmostm` (typical mean), `cmoswp`, `cmosws`, `cmoswo`, `cmoswz`, `cmostmwn`, `cmosmc`.


## Issue 4 Stale Netlist Cache (OSSHNL-109)

**Symptom:**
```
ERROR (OSSHNL-109): The cellview has been modified since the last extraction.
Run Check and Save to correct this error.
```

**Fix:** In the schematic window press **Shift+S** (Check and Save), then in ADE go to **Simulation → Netlist → Recreate**.


## Issue 5 SSH Git Failure: Cadence OpenSSL Conflict

**Symptom:**
```
ssh: libcrypto.so.10: version `OPENSSL_1.0.2' not found
fatal: Could not read from remote repository.
```

**Root cause:** The Cadence environment sets `LD_LIBRARY_PATH` to its own SSL libraries, which conflict with the system SSH binary.

**Fix:** Add to `~/.bashrc`:
```bash
alias git='LD_LIBRARY_PATH="" git'
```


## Issue 6 Parametric Sweep Hanging

**Symptom:** Parametric sweep ran indefinitely without completing.

**Root cause:** DC convergence failure caused by undefined source voltages Spectre cannot find a valid bias point and iterates indefinitely.

**Fix:**
- Set explicit DC values on all `vdc` sources in the schematic
- Set default variable values in ADE after **Variables → Copy From Cellview**
- In **Simulation → Options → Analog** set: `homotopy=all`, `gmin=1e-12`, `reltol=1e-3`


## Device Quick Reference AMS C35B4

| Device | Subckt | Notes |
|--------|--------|-------|
| NMOS 3.3V | `modn` | Standard device, min W/L = 1.2u/350n |
| PMOS 3.3V | `modp` | Size 2x NMOS width for equal drive strength |
| NMOS medium Vth | `modnm` | |
| NMOS low Vth | `modnl` | |
| NMOS 1.8V | `modn18t` | |
| NMOS 3.0V | `modn30m` | |

All standard devices call `mosinsub` (BSIM3v3) internally. Never instantiate `mosinsub` directly.


## Module 01 Results Summary

**NMOS I-V**
- W=1.2u, L=350n, W/L=3.43
- Vth = 0.523V (constant current method, 343nA at Vds=100mV)
- AMS C35 typical spec: 0.5V ± 0.1V within range

**CMOS Inverter VTC**
- NMOS: modn W=1.2u L=350n / PMOS: modp W=2.4u L=350n
- Vm = 1.511V, Voh = 3.298V, Vol = 2.936mV
- Vil = 1.223V, Vih = 1.716V
- NMH = 1.582V, NML = 1.220V
