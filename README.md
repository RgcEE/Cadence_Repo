# Cadence Virtuoso — Analog IC Design Portfolio

VLSI analog circuit designs developed in Cadence Virtuoso using the AMS 0.35 µm (C35) PDK.
Simulations run via Spectre/SPICE with BSIM3 device models on the University of Idaho ECE server.

---

## Technology

| Parameter | Value |
|---|---|
| PDK | AMS HIT-Kit 4.10 |
| Process | 0.35 µm CMOS (C35B4) |
| Tool | Cadence Virtuoso IC617 |
| Simulator | Spectre / ngspice |
| Models | BSIM3 |

---

## Projects

### Sense Amplifier (`sense_amp_bsim3`)
Latch-type SRAM sense amplifier characterization using BSIM3 models.

- Cross-coupled PMOS/NMOS latch topology
- Parametric sweep of device width, length, and threshold voltage
- Transient analysis: decision delay vs. differential input
- Process variation sensitivity study

---

## Repository Structure

```
cadence_repo/
├── sense_amp_bsim3/        # Sense amplifier project
│   ├── schematics/         # Virtuoso schematic exports
│   ├── testbenches/        # Simulation testbench netlists
│   ├── results/            # Simulation output and plots
│   ├── scripts/            # OCEAN / parametric sweep scripts
│   └── docs/               # Reports and analysis
├── models/                 # Shared BSIM3 model files
└── README.md
```

---