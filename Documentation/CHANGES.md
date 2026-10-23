# What changed compared with the original repository

Source: <https://github.com/Sylvreen-Npu/vlsi> (folder `Vlsi/`). The original repository was **not modified**.
Every original `.asc` file was first simulated unchanged in LTspice XVII, and its outputs were compared with the truth tables in the lab manuals.
The table below lists what was found, and what was changed only where a real problem was found.
"Measured" values come from LTspice simulations of the original and corrected files.

| Exp | Original file | New file(s) | Change | Reason (measured in LTspice) |
|---|---|---|---|---|
| 1 | `Exp1_NMOS_Transfer.asc`, `Exp1_NMOS_Drain.asc`, `Exp1_PMOS_Transfer.asc`, `Exp1_PMOS_Drain.asc` | `NMOS-Transfer-Characteristics.asc`, `NMOS-Drain-Characteristics.asc`, `PMOS-Transfer-Characteristics.asc`, `PMOS-Drain-Characteristics.asc` | **None** (renamed only, contents byte-identical) | Simulate correctly. |
| 2 | `Exp2_DC_VTC.asc`, `Exp2_Transient.asc` | `Inverter-DC-VTC.asc`, `Inverter-Transient.asc` | **None** (renamed only) | VOH = 5.00 V, VOL = 0.00 V, VM = 2.50 V; transient output = NOT input. |
| 3 | `Exp3_NAND_Gate.asc`, `Exp3_NOR_Gate.asc` | `NAND-Gate.asc`, `NOR-Gate.asc` | **Redrawn** (same transistor topology as manual Fig. 3.1–3.3) | Original NAND gave **4.42 V** for A = B = 1 (should be 0 V). Original NOR gave **3.73–3.77 V** when A = 1 (should be 0 V). Cause: 3-terminal MOSFET symbols tie each transistor's body to its *source*. In the pull-up PMOS the "source" is the output node, so the body–drain junction becomes forward-biased and holds the output up. The input pulses also never produced A = 0, B = 1, and several wires were drawn diagonally or overlapping. New files: 4-terminal MOSFETs with PMOS body → VDD and NMOS body → GND, clean orthogonal wiring, and inputs that count 00, 01, 10, 11. |
| 4 | `Exp4-(A+BC)’.asc`, `Exp4-[(A+B)C]’.asc` | `Y1-NOT-A-plus-BC.asc`, `Y2-NOT-A-plus-B-times-C.asc` | **None** (renamed only) | Correct for all 8 input combinations. Renamed because the curly apostrophe (’) and brackets in the old names stopped LTspice (under Wine) from writing its output files, and can break downloads/unzipping on some systems. |
| 5 | `Exp5-SR_latch.asc` | `SR-Latch-NAND.asc` | **Two net labels swapped** (`Q` ↔ `Q_`), plus title and truth-table text | Each gate was a correct NAND, but the label `Q` was on the gate driven by R'. With S' = 0, R' = 1 the circuit gave Q = 0, which is the opposite of the manual's truth table (S' = 0 → Q = 1). Wiring, devices and sources are unchanged. |
| 5 | (none) | `SR-Latch-NOR.asc` | **New** | NOR-based SR latch built from two cross-coupled static CMOS NOR gates, drawn like the NAND version. |
| 6 | `Exp6-Mux.asc` | `MUX-4to1-TG.asc` | **Redrawn**, same transmission-gate topology (2 inverters + 6 TGs, manual Fig. 6.2) | Original output HIGH level was only **0.56 V** (manual Fig. 6.3 shows the same 600 mV). Cause: every TG transistor's body was wired to a data node, so body–drain diodes clamp the signal. Also, inputs I2 and I3 were pulsed while a different input was selected, so they never reached the output. M1's body was floating. New file: PMOS body → VDD, NMOS body → GND, select lines S1 (MSB) / S0 (LSB) as in the manual's truth table, and distinct data patterns so the selected input is visible. Output now swings 0 V / 5 V. |
| 6 | (none) | `MUX-2to1-Gates.asc`, `MUX-2to1-TG.asc` | **New** | 2:1 MUX, Y = S'·D0 + S·D1: gate-level (NOT, AND, AND, OR) and CMOS transmission-gate versions. |
| 7 | `Exp7-HalfDDer.asc` | `Half-Adder.asc` | **One gate wire re-routed** (3 `WIRE` lines), plus title text | In the Carry NAND, both series NMOS (M3, M4) were driven by **B**; M3 must be driven by **A**. At A = 0, B = 1 the pull-up and pull-down conducted at the same time (internal node 3.66 V, static current), and Carry read 0.25 V instead of 0 V. M3's gate wire now goes to the A line. Everything else is unchanged. |
| 8 | `Exp8-Memory (1).asc` | `NOR-ROM-4x4.asc` | **Body connections completed, PMOS loads sized, rows labelled** | All 12 transistor bodies were floating: each body wire ended one grid step short, *along* the transistor's own drain/source line, and LTspice does not connect a wire drawn that way. The always-on PMOS loads were as strong as the NMOS pull-downs, so a stored "0" read **2.50 V**. The PMOS bodies are now tied to VDD with a small side loop, the NMOS bodies are tied to GND, and the loads are weak (`l=10u w=1u`, W/L = 1/10, the usual pseudo-NMOS ratio). A stored "0" now reads **0.26 V**, and the stored pattern is unchanged (0101 / 0011 / 1001 / 0110). Word lines are labelled R1–R4. |

The lab-manual `.docx` files are not copied here; they remain in the original repository.

## Note on the YouTube references
The manuals supplied with this task (Experiments 1–8, `.docx`) contain no YouTube links: neither in the visible text nor as hyperlinks
in the document relationships. The corrections above are therefore based on the manuals' circuit diagrams, theory and truth tables,
and on LTspice simulation.
