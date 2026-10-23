# Simulation verification report

## How the files were tested
* **Simulator:** the real **LTspice XVII (x64)** program (`XVIIx64.exe`), run under Wine 9.0 on Linux in batch mode
  (`XVIIx64.exe -Run -b <file>.asc`). LTspice itself netlisted every `.asc` file in this repository exactly as stored
  and ran its simulation command. No hand-written netlists were used.
* **Checks:** every log was scanned for errors and warnings, and **none** were reported for any of the 17 files.
  Every generated netlist was read back to confirm the connections: gate inputs, series/parallel networks,
  feedback paths, and body (bulk) connections.
* **Logic check:** LTspice's `.raw` output was resampled every 20 µs. Points within 0.3 ms of an input edge were skipped.
  At every remaining point each output was compared with the expected Boolean function (or the latch state machine).
  A logic 1 must be **≥ 4.5 V** and a logic 0 **≤ 0.5 V** (5 V supply).
* **Pictures:** `schematics/*.png` are screenshots of LTspice XVII displaying each `.asc` file.
  `waveforms/*.png` are plots drawn from LTspice's own `.raw` result data. They are not LTspice screenshots:
  LTspice's waveform window does not draw reliably under Wine, so its traces could not be captured faithfully.
* **Not tested here:** LTspice 24.x for Windows/macOS was not available in the test environment. The files use only
  standard symbols and built-in models that exist in every LTspice version, and the original files (made with LTspice 24, format
  "Version 4.1") opened and simulated in LTspice XVII without problems.

## Summary

| Experiment | File | Result | Notes |
|---|---|---|---|
| Experiment-03-NAND-NOR-Gates | `NAND-Gate` | **PASS** | 4/4 input combinations correct |
| Experiment-03-NAND-NOR-Gates | `NOR-Gate` | **PASS** | 4/4 input combinations correct |
| Experiment-04-Static-CMOS-Logic-Expressions | `Y1-NOT-A-plus-BC` | **PASS** | 8/8 input combinations correct |
| Experiment-04-Static-CMOS-Logic-Expressions | `Y2-NOT-A-plus-B-times-C` | **PASS** | 8/8 input combinations correct |
| Experiment-05-SR-Latch | `SR-Latch-NAND` | **PASS** | Set, Reset, Hold and Invalid states verified |
| Experiment-05-SR-Latch | `SR-Latch-NOR` | **PASS** | Set, Reset, Hold and Invalid states verified |
| Experiment-06-Multiplexer | `MUX-4to1-TG` | **PASS** | 7 conditions checked |
| Experiment-06-Multiplexer | `MUX-2to1-Gates` | **PASS** | 8/8 input combinations correct |
| Experiment-06-Multiplexer | `MUX-2to1-TG` | **PASS** | 8/8 input combinations correct |
| Experiment-07-Half-Adder | `Half-Adder` | **PASS** | 8/8 input combinations correct |
| Experiment-08-NOR-ROM | `NOR-ROM-4x4` | **PASS** | 5 conditions checked |
| Experiment-01-NMOS-PMOS-Characteristics | `4 files` | **RUNS** | DC sweeps simulate; curves plotted (no pass/fail truth table) |
| Experiment-02-CMOS-Inverter | `Inverter-DC-VTC` | **PASS** | VOH=5.00 V, VOL=0.000 V, VM=2.50 V |
| Experiment-02-CMOS-Inverter | `Inverter-Transient` | **PASS** | output = NOT input |

"Input combinations correct" counts every (input combination × output) pair that the stimulus applies.
Experiment 1 has no truth table; its four DC sweeps run, and the curves are in `waveforms/Experiment-01-Characteristics.png`.

# Detailed results
## Experiment 3 – NAND and NOR gates

### Exp 3(a): CMOS NAND gate, Y = (A.B)'

File: `Experiment-03-NAND-NOR-Gates/NAND-Gate.asc` — 3790 stable time points checked per output.

| Output | a | b | Expected | Measured (min…max) | Result |
|---|---|---|---|---|---|
| out | 0 | 0 | 1 | 5.00 … 5.00 V | PASS |
| out | 0 | 1 | 1 | 5.00 … 5.00 V | PASS |
| out | 1 | 0 | 1 | 5.00 … 5.00 V | PASS |
| out | 1 | 1 | 0 | 0.00 … 0.00 V | PASS |

### Exp 3(b): CMOS NOR gate, Y = (A+B)'

File: `Experiment-03-NAND-NOR-Gates/NOR-Gate.asc` — 3790 stable time points checked per output.

| Output | a | b | Expected | Measured (min…max) | Result |
|---|---|---|---|---|---|
| out | 0 | 0 | 1 | 5.00 … 5.00 V | PASS |
| out | 0 | 1 | 0 | 0.00 … 0.00 V | PASS |
| out | 1 | 0 | 0 | 0.00 … 0.00 V | PASS |
| out | 1 | 1 | 0 | 0.00 … 0.00 V | PASS |

## Experiment 4 – Static CMOS logic expressions

### Exp 4(a): Y = (A + B.C)'

File: `Experiment-04-Static-CMOS-Logic-Expressions/Y1-NOT-A-plus-BC.asc` — 4157 stable time points checked per output.

| Output | a | b | c | Expected | Measured (min…max) | Result |
|---|---|---|---|---|---|---|
| out | 0 | 0 | 0 | 1 | 4.94 … 5.00 V | PASS |
| out | 0 | 0 | 1 | 1 | 4.94 … 5.00 V | PASS |
| out | 0 | 1 | 0 | 1 | 4.94 … 5.00 V | PASS |
| out | 0 | 1 | 1 | 0 | 0.00 … 0.00 V | PASS |
| out | 1 | 0 | 0 | 0 | 0.00 … 0.03 V | PASS |
| out | 1 | 0 | 1 | 0 | 0.00 … 0.04 V | PASS |
| out | 1 | 1 | 0 | 0 | 0.00 … 0.03 V | PASS |
| out | 1 | 1 | 1 | 0 | 0.00 … 0.00 V | PASS |

### Exp 4(b): Y = ((A + B).C)'

File: `Experiment-04-Static-CMOS-Logic-Expressions/Y2-NOT-A-plus-B-times-C.asc` — 4156 stable time points checked per output.

| Output | a | b | c | Expected | Measured (min…max) | Result |
|---|---|---|---|---|---|---|
| out | 0 | 0 | 0 | 1 | 4.99 … 5.00 V | PASS |
| out | 0 | 0 | 1 | 1 | 4.95 … 5.00 V | PASS |
| out | 0 | 1 | 0 | 1 | 5.00 … 5.00 V | PASS |
| out | 0 | 1 | 1 | 0 | 0.00 … 0.00 V | PASS |
| out | 1 | 0 | 0 | 1 | 5.00 … 5.00 V | PASS |
| out | 1 | 0 | 1 | 0 | 0.00 … 0.06 V | PASS |
| out | 1 | 1 | 0 | 1 | 5.00 … 5.00 V | PASS |
| out | 1 | 1 | 1 | 0 | 0.00 … 0.04 V | PASS |

## Experiment 5 – SR latch

### Exp 5(a): NAND SR latch (inputs active-LOW S', R')

File: `Experiment-05-SR-Latch/SR-Latch-NAND.asc`. Each stable interval of the input sequence:

| t (ms) | S | R | Operation | Expected Q / Q_ | Measured Q / Q_ (V) | Result |
|---|---|---|---|---|---|---|
| 0.0–1.2 | 0 | 1 | Set | 1 / 0 | 5.00 / 0.00 | PASS |
| 1.8–4.7 | 0 | 0 | Invalid (forbidden) | 1 / 1 | 5.00 / 5.00 | PASS |
| 5.3–6.2 | 1 | 0 | Reset | 0 / 1 | -0.00 / 5.00 | PASS |
| 6.8–9.2 | 1 | 1 | Hold | 0 / 1 | 0.00 / 5.00 | PASS |
| 10.3–14.2 | 0 | 0 | Invalid (forbidden) | 1 / 1 | 5.00 / 5.00 | PASS |
| 15.3–17.2 | 1 | 1 | Hold | 1 / 0 | 5.00 / 0.00 | PASS |
| 17.8–19.7 | 1 | 0 | Reset | 0 / 1 | 0.00 / 5.00 | PASS |
| 20.3–22.2 | 0 | 0 | Invalid (forbidden) | 1 / 1 | 5.00 / 5.00 | PASS |
| 22.8–24.7 | 0 | 1 | Set | 1 / 0 | 5.00 / 0.00 | PASS |
| 25.8–29.7 | 1 | 0 | Reset | 0 / 1 | 0.00 / 5.00 | PASS |
| 30.8–33.2 | 0 | 1 | Set | 1 / 0 | 5.00 / 0.00 | PASS |
| 33.8–34.7 | 0 | 0 | Invalid (forbidden) | 1 / 1 | 5.00 / 5.00 | PASS |
| 35.3–38.2 | 1 | 0 | Reset | 0 / 1 | -0.00 / 5.00 | PASS |
| 38.8–39.7 | 1 | 1 | Hold | 0 / 1 | 0.00 / 5.00 | PASS |
| 40.3–41.2 | 0 | 1 | Set | 1 / 0 | 5.00 / 0.00 | PASS |
| 41.8–44.7 | 0 | 0 | Invalid (forbidden) | 1 / 1 | 5.00 / 5.00 | PASS |
| 45.3–46.2 | 1 | 0 | Reset | 0 / 1 | -0.00 / 5.00 | PASS |
| 46.8–49.2 | 1 | 1 | Hold | 0 / 1 | 0.00 / 5.00 | PASS |
| 49.8–50.0 | 1 | 0 | Reset | 0 / 1 | 0.00 / 5.00 | PASS |

### Exp 5(b): NOR SR latch

File: `Experiment-05-SR-Latch/SR-Latch-NOR.asc`. Each stable interval of the input sequence:

| t (ms) | S | R | Operation | Expected Q / QBAR | Measured Q / QBAR (V) | Result |
|---|---|---|---|---|---|---|
| 0.0–9.7 | 1 | 0 | Set | 1 / 0 | 5.00 / 0.00 | PASS |
| 10.3–19.7 | 0 | 0 | Hold | 1 / 0 | 5.00 / 0.00 | PASS |
| 20.3–29.7 | 0 | 1 | Reset | 0 / 1 | 0.00 / 5.00 | PASS |
| 30.3–39.7 | 0 | 0 | Hold | 0 / 1 | 0.00 / 5.00 | PASS |
| 40.3–49.7 | 1 | 1 | Invalid (forbidden) | 0 / 0 | 0.00 / 0.00 | PASS |
| 50.3–60.0 | 0 | 1 | Reset | 0 / 1 | 0.00 / 5.00 | PASS |

## Experiment 6 – Multiplexers

### Exp 6(a): 4:1 MUX, CMOS transmission gates

File: `Experiment-06-Multiplexer/MUX-4to1-TG.asc` — 647 stable time points checked.

| Condition | Expected OUTPUT | Measured OUTPUT (min…max) | Result |
|---|---|---|---|
| S1S0=00 (selects I0), I0=0 | 0 | 0.00 … 0.00 V | PASS |
| S1S0=00 (selects I0), I0=1 | 1 | 5.00 … 5.00 V | PASS |
| S1S0=01 (selects I1), I1=0 | 0 | 0.00 … 0.00 V | PASS |
| S1S0=01 (selects I1), I1=1 | 1 | 5.00 … 5.00 V | PASS |
| S1S0=10 (selects I2), I2=0 | 0 | 0.00 … 0.00 V | PASS |
| S1S0=10 (selects I2), I2=1 | 1 | 5.00 … 5.00 V | PASS |
| S1S0=11 (selects I3), I3=1 | 1 | 5.00 … 5.00 V | PASS |

### Exp 6(b): 2:1 MUX, logic gates

File: `Experiment-06-Multiplexer/MUX-2to1-Gates.asc` — 1789 stable time points checked per output.

| Output | s | d0 | d1 | Expected | Measured (min…max) | Result |
|---|---|---|---|---|---|---|
| y | 0 | 0 | 0 | 0 | 0.00 … 0.00 V | PASS |
| y | 0 | 0 | 1 | 0 | 0.00 … 0.00 V | PASS |
| y | 0 | 1 | 0 | 1 | 5.00 … 5.00 V | PASS |
| y | 0 | 1 | 1 | 1 | 5.00 … 5.00 V | PASS |
| y | 1 | 0 | 0 | 0 | 0.00 … 0.00 V | PASS |
| y | 1 | 0 | 1 | 1 | 5.00 … 5.00 V | PASS |
| y | 1 | 1 | 0 | 0 | 0.00 … 0.00 V | PASS |
| y | 1 | 1 | 1 | 1 | 5.00 … 5.00 V | PASS |

### Exp 6(c): 2:1 MUX, CMOS transmission gates

File: `Experiment-06-Multiplexer/MUX-2to1-TG.asc` — 1789 stable time points checked per output.

| Output | s | d0 | d1 | Expected | Measured (min…max) | Result |
|---|---|---|---|---|---|---|
| y | 0 | 0 | 0 | 0 | 0.00 … 0.00 V | PASS |
| y | 0 | 0 | 1 | 0 | 0.00 … 0.00 V | PASS |
| y | 0 | 1 | 0 | 1 | 5.00 … 5.00 V | PASS |
| y | 0 | 1 | 1 | 1 | 5.00 … 5.00 V | PASS |
| y | 1 | 0 | 0 | 0 | 0.00 … 0.00 V | PASS |
| y | 1 | 0 | 1 | 1 | 5.00 … 5.00 V | PASS |
| y | 1 | 1 | 0 | 0 | 0.00 … 0.00 V | PASS |
| y | 1 | 1 | 1 | 1 | 5.00 … 5.00 V | PASS |

## Experiment 7 – Half adder

### Exp 7: CMOS half adder

File: `Experiment-07-Half-Adder/Half-Adder.asc` — 3790 stable time points checked per output.

| Output | a | b | Expected | Measured (min…max) | Result |
|---|---|---|---|---|---|
| sum | 0 | 0 | 0 | 0.00 … 0.00 V | PASS |
| sum | 0 | 1 | 1 | 5.00 … 5.00 V | PASS |
| sum | 1 | 0 | 1 | 5.00 … 5.00 V | PASS |
| sum | 1 | 1 | 0 | 0.00 … 0.00 V | PASS |
| carry | 0 | 0 | 0 | 0.00 … 0.00 V | PASS |
| carry | 0 | 1 | 0 | 0.00 … 0.00 V | PASS |
| carry | 1 | 0 | 0 | 0.00 … 0.00 V | PASS |
| carry | 1 | 1 | 1 | 5.00 … 5.00 V | PASS |

## Experiment 8 – 4x4 NOR ROM

### Exp 8: 4x4 NOR ROM (word lines R1-R4, bit lines C1-C4)

File: `Experiment-08-NOR-ROM/NOR-ROM-4x4.asc` — 1900 stable time points checked.

| Condition | Expected C1 | Expected C2 | Expected C3 | Expected C4 | Measured C1 (min…max) | Measured C2 (min…max) | Measured C3 (min…max) | Measured C4 (min…max) | Result |
|---|---|---|---|---|---|---|---|---|---|
| R1 active | 0 | 1 | 0 | 1 | 0.26 … 0.26 V | 5.00 … 5.00 V | 0.26 … 0.26 V | 5.00 … 5.00 V | PASS |
| R2 active | 0 | 0 | 1 | 1 | 0.26 … 0.26 V | 0.26 … 0.26 V | 5.00 … 5.00 V | 5.00 … 5.00 V | PASS |
| R3 active | 1 | 0 | 0 | 1 | 5.00 … 5.00 V | 0.26 … 0.26 V | 0.26 … 0.26 V | 5.00 … 5.00 V | PASS |
| R4 active | 0 | 1 | 1 | 0 | 0.26 … 0.26 V | 5.00 … 5.00 V | 5.00 … 5.00 V | 0.26 … 0.26 V | PASS |
| no word line active | 1 | 1 | 1 | 1 | 5.00 … 5.00 V | 5.00 … 5.00 V | 5.00 … 5.00 V | 5.00 … 5.00 V | PASS |

## Experiment 1 – NMOS / PMOS characteristics (preserved, unchanged)

| File | Step | Sweep start → end | Id start → end |
|---|---|---|---|
| `NMOS-Transfer-Characteristics.asc` | 0 | 0 → 5 V | 0.000 → 0.250 mA |
| `NMOS-Drain-Characteristics.asc` | 0 | 0 → 10 V | 0.000 → 0.090 mA |
| `NMOS-Drain-Characteristics.asc` | 1 | 0 → 10 V | 0.000 → 0.160 mA |
| `NMOS-Drain-Characteristics.asc` | 2 | 0 → 10 V | 0.000 → 0.250 mA |
| `PMOS-Transfer-Characteristics.asc` | 0 | 0 → -5 V | -0.000 → -0.250 mA |
| `PMOS-Drain-Characteristics.asc` | 0 | 0 → -5 V | 0.000 → -0.000 mA |
| `PMOS-Drain-Characteristics.asc` | 1 | 0 → -5 V | 0.000 → -0.010 mA |
| `PMOS-Drain-Characteristics.asc` | 2 | 0 → -5 V | 0.000 → -0.040 mA |
| `PMOS-Drain-Characteristics.asc` | 3 | 0 → -5 V | 0.000 → -0.090 mA |
| `PMOS-Drain-Characteristics.asc` | 4 | 0 → -5 V | 0.000 → -0.160 mA |
| `PMOS-Drain-Characteristics.asc` | 5 | 0 → -5 V | 0.000 → -0.250 mA |

## Experiment 2 – CMOS inverter (preserved, unchanged)

* `Inverter-DC-VTC.asc`: Vout = 5.00 V at Vin = 0.0 V, Vout = 0.000 V at Vin = 5.0 V, switching threshold VM = 2.50 V.
* `Inverter-Transient.asc`: output is the logical inverse of the input at every stable point: **PASS**.
