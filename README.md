# 4-bit Array Multiplier | RTL to GDSII

A complete **RTL-to-GDSII implementation of a 4-bit unsigned array multiplier** using Verilog, open-source VLSI tools, and the SkyWater SKY130 (130 nm) technology.

The multiplier uses a shift-and-add architecture consisting of **16 AND gates and 12 full adders**, with registered inputs and outputs. The project covers the complete digital ASIC design flow, from RTL verification to physical layout.

## What This Project Includes

- **RTL Design:** Implemented a 4-bit array multiplier in Verilog using partial-product generation and ripple-carry full-adder rows, without using the multiplication operator.
- **Functional Verification:** Developed a self-checking testbench covering all 256 input combinations, with waveform analysis using GTKWave.
- **Logic Synthesis:** Used Yosys to optimize the RTL and map it to SKY130 standard cells. Verified the synthesized netlist through gate-level simulation.
- **Static Timing Analysis:** Used OpenSTA to identify the critical path, calculate timing slack, and estimate the maximum operating frequency.
- **Physical Design:** Used LibreLane and OpenROAD for floorplanning, placement, clock tree synthesis, and routing.
- **Layout Verification:** Checked the final GDSII layout using DRC and LVS, with zero violations.

## Tools and Technologies

**Verilog** · Icarus Verilog · GTKWave · Yosys · OpenSTA · LibreLane · OpenROAD · KLayout · Magic · Netgen · SKY130

The design flow was run in the **IIC-OSIC-TOOLS Docker environment**.

## Key Results

| Metric | Result |
|---|---|
| RTL & Gate-Level Verification | 256/256 test vectors passed |
| Synthesized Cells | 63 |
| Total Cell Area | 740.71 µm² |
| Pre-layout Fmax | 384.0 MHz |
| Final Clock Period | 6.5 ns |
| Post-route Fmax | 155.9 MHz |
| Worst Post-route Setup Slack | +0.085 ns |
| DRC / LVS Violations | 0 / 0 |

## How to Run

Run these commands from the project root. Synthesis, timing analysis, and physical design require the SKY130 PDK and their respective tools, available through IIC-OSIC-TOOLS.

### 1. RTL Simulation

```bash
iverilog -g2012 -o sim tb/tb_mult.v rtl/full_adder.v rtl/mult_array.v
vvp sim
gtkwave mult.vcd
```

Expected result:

```text
PASS: 256 vectors checked, 0 errors
```

### 2. Logic Synthesis : Yosys

```bash
yosys -c synth/synth.tcl
```

Generates the synthesized gate-level netlist and cell statistics.

### 3. Static Timing Analysis : OpenSTA

```bash
sta sta/sta.tcl
```

Generates the critical-path timing report using the clock period specified in `sta.tcl`.

### 4. Physical Design : LibreLane

```bash
cd pnr
librelane config.json
```

Runs the RTL-to-GDSII flow using the SKY130 PDK.

## Project Report

For detailed explanations of the architecture, simulation results, synthesis statistics, critical-path analysis, and physical layout, refer to the **[Full Project Report](Report.pdf)**.
