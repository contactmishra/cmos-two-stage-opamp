<h1 align="center">CMOS Two-Stage Op-Amp</h1>

<p align="center">
  <b>A Miller-compensated two-stage CMOS operational amplifier, sized by hand from a spec sheet and verified in LTspice.</b><br>
  University of Bologna · Analog Circuits and Sensor Systems
</p>

<p align="center">
  <img alt="LTspice" src="https://img.shields.io/badge/simulated%20in-LTspice-1D1D20?style=flat-square">
  <img alt="CMOS" src="https://img.shields.io/badge/technology-CMOS%20level%201-1D1D20?style=flat-square">
  <img alt="Supply" src="https://img.shields.io/badge/supply-%C2%B12.5%20V-1D1D20?style=flat-square">
  <img alt="Report" src="https://img.shields.io/badge/report-PDF-B31B1B?style=flat-square">
</p>

<p align="center">
  <img src="docs/circuit.png" alt="Two-stage CMOS op-amp: biasing network M10 to M14 on the left, differential pair M1 and M2 with current-mirror load M3 and M4, tail current source M5, second stage M6 and M7, Miller capacitor Cc with nulling transistor M9, load capacitor CL" width="80%">
</p>

## What this is

The op-amp is the building block behind almost every analog circuit: filters, sensor front-ends, ADC drivers. This project designs one from scratch: start from a list of required numbers, work out the width of every transistor with the square-law equations, then build it in LTspice and check that the real circuit behaves as designed.

The circuit has two parts:

1. **Biasing network** (M8, M10 to M14 and a resistor R<sub>B</sub>) that produces the reference currents.
2. **Two-stage amplifier**: an NMOS differential pair (M1, M2) with a PMOS current-mirror load (M3, M4) for gain, followed by a common-source stage (M6, M7) for more gain and output swing. A Miller capacitor C<sub>c</sub> with a nulling transistor M9 (in triode) keeps the loop stable.

## Target specification

| Parameter | Target |
|---|---|
| Supply V<sub>DD</sub> / V<sub>SS</sub> | +2.5 V / −2.5 V |
| Slew rate | ±5 V/µs |
| Load capacitance C<sub>L</sub> | 5 pF |
| Input common-mode range | −1.3 V to +2.1 V |
| Output swing | −2.2 V to +2.2 V |
| Gain-bandwidth | 5 MHz |
| Differential gain | > 80 dB |
| Phase margin | > 60° |

Process: level-1 MOSFET models (NMOS V<sub>T</sub> = 0.71 V, k′ = 182 µA/V²; PMOS V<sub>T</sub> = −0.901 V, k′ = 41.5 µA/V²; t<sub>ox</sub> = 9.6 nm, λ = 0.01).

## How it was designed

An 11-step hand calculation, each step feeding the next:

| Step | What is computed | Result |
|---|---|---|
| 1 | Compensation capacitor from the noise, GBW and slew-rate targets | C<sub>c</sub> = 1.002 pF, V<sub>ov3</sub> = 0.209 V |
| 2 | Second-stage current | I<sub>6</sub> = I<sub>7</sub> = 31.0 µA |
| 3 | Channel length from M6 (used for every transistor) | L = 4.30 µm |
| 4 | Size of M6 | W/L = 16.6, W = 71.4 µm |
| 5 | Tail current from the slew rate | I<sub>5</sub> = 6.0 µA |
| 6 to 11 | Input pair, tail, mirror load, output sink and nulling transistor, with perfect current balancing | table below |

Final widths (all L = 4.30 µm):

| M1 | M2 | M3 | M4 | M5 | M6 | M7 | M8 | M9 | M10 to M13 | M14 |
|---|---|---|---|---|---|---|---|---|---|---|
| 5.60 µm | 5.60 µm | 6.92 µm | 6.92 µm | 2.60 µm | 71.4 µm | 13.4 µm | 2.60 µm | 13.8 µm | 13.8 µm | 10.4 µm |

The bias resistor came out at R<sub>B</sub> = 2.75 kΩ.

## Simulation in LTspice

The amplifier was wrapped as a subcircuit and tested with four analyses.

<p align="center">
  <img src="docs/ltspice-schematic.png" alt="LTspice schematic of the full two-stage op-amp with biasing network, Cc = 1.002 pF and CL = 5 pF" width="85%">
</p>

| Input common-mode range (DC sweep) | Gain and bandwidth (AC sweep, 1 Hz to 1 GHz) |
|---|---|
| <img src="docs/input-common-mode-range.png" alt="DC sweep of the input common-mode voltage from -3 V to 3 V"> | <img src="docs/bode-gain-bandwidth.png" alt="Bode plot of gain and phase"> |
| **Output swing (DC sweep)** | **Slew rate (transient, 1 µs pulses)** |
| <img src="docs/output-swing.png" alt="Output voltage against input, rail to rail"> | <img src="docs/slew-rate.png" alt="Derivative of the output during a pulse response"> |

## What's in this repo

| File | What it is |
|---|---|
| [`Two-Stage-OpAmp-Report.pdf`](Two-Stage-OpAmp-Report.pdf) | The full report: specification, all 11 design steps with equations, LTspice schematics and simulation plots |
| [`docs/`](docs) | The figures used on this page, taken from the report |

## Skills shown

Analog IC design · CMOS square-law sizing · Miller compensation and phase margin · current mirrors and biasing · LTspice (DC, AC and transient analysis)

## Related

[CMOS Low-Voltage Op-Amp](https://github.com/theommishra-dev/cmos-low-voltage-opamp): the follow-up project, a folded-cascode op-amp that runs from a single 2 V supply.

---

<sub>Om Mishra · MSc Electronic Engineering, University of Bologna. This repository holds the project report.</sub>
