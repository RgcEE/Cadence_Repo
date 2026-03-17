# ADE L — DC Sweep Notes
## NMOS I-V Setup (AMS C35)

**Schematic checklist before opening ADE:**
- V0 gate vdc = `Vgs`, V1 drain vdc = `Vds`
- Source, bulk both to gnd
- Drain has an output label (e.g. `Nmos_out`)

---

**1. Load model**
Setup, Model Libraries, add:
`/tools.new/examples/pdk/ams035/spectre/c35/cmos53.scs`
Section: blank

**2. Add design variables**
Variables, Edit, add `Vgs = 0.8` and `Vds = 0`

**3. DC analysis**
Analyses, Choose, dc
- Component Parameter, select V1, vdc
- Start: 0, Stop: 3.3

**4. Parametric sweep**
Tools, Parametric Analysis
- Variable: `Vgs`, Type: List
- Values: `0.8 1.2 1.6 2.0`

**5. Output**
Outputs, To Be Plotted, Select on Schematic, click the drain wire

**6. Run — green button**

---

**If it breaks:**
- `model not found` — model file not loaded
- flat single line — Vgs variable not linked to V0 vdc property
- no waveform — output not selected
