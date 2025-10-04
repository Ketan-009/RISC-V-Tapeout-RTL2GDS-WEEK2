# Week 2: SoC Design Verification Methodology (Pre-Synthesis & Post-Synthesis Validation)

This documentation outlines a comprehensive verification methodology for a basic RISC-V System-on-Chip implementation. The focus is on establishing robust verification flows for both Register Transfer Level (RTL) and Gate-Level implementations, ensuring functional correctness throughout the design process.

**Note:** Click the sections below to expand.

---

<details>
<summary><strong>Part 1: Introduction to System-on-Chip Design</strong></summary>

### What is a System-on-Chip?

A **System-on-Chip (SoC)** represents an integrated circuit that consolidates multiple system components onto a single silicon die. Rather than implementing separate chips for different functions, an SoC provides a unified solution that optimizes performance, power consumption, and form factor for modern electronic applications.

<img width="1024" height="1024" alt="image" src="https://github.com/user-attachments/assets/de4a7e62-8d2c-4212-8e1e-6721798a4c20" />

**Block diagram of a System on Chip (SoC) architecture showing key components and their interconnections**

#### Essential SoC Components

1. **Processing Unit**: Central processing core responsible for instruction execution and system control.
2. **Memory Subsystem**: On-chip SRAM/DRAM for data storage and program execution.
3. **Peripheral Interfaces**: I/O controllers for external device communication.
4. **Graphics Engine**: Dedicated hardware for visual processing tasks.
5. **Signal Processing Units**: Specialized processors for audio/video signal manipulation.
6. **Power Management Unit**: Integrated power regulation and optimization circuits.
7. **Communication Modules**: Wireless connectivity options (Wi-Fi, Bluetooth, cellular).

#### Benefits of SoC Integration

- **Miniaturization**: Enables compact device designs with reduced PCB complexity.
- **Power Optimization**: Lower overall power consumption through integrated design.
- **Performance Enhancement**: Faster inter-component communication and data transfer.
- **Cost Reduction**: Economies of scale in manufacturing and reduced component count.
- **Improved Reliability**: Fewer interconnections reduce potential failure points.

#### Application Domains

- Mobile devices, IoT sensors, automotive systems, smart appliances, wearable technology.

#### Industry Examples

- Apple M-series processors, Qualcomm Snapdragon family, AMD Ryzen APUs, MediaTek Dimensity series.

#### Design Challenges

- Thermal management complexity, verification scalability, design flexibility constraints.

### SoC Categories

- **Control-Oriented SoCs**: Optimized for embedded control applications with low power requirements.
- **Performance-Oriented SoCs**: Designed for high-performance computing with advanced operating system support.
- **Domain-Specific SoCs**: Tailored for specialized applications requiring optimized performance characteristics.

<img width="1300" height="1261" alt="image" src="https://github.com/user-attachments/assets/6d0a43f8-5037-4ea1-8c69-c7552f71ad90" />

**Close-up view of a System-on-Chip (SoC) silicon die showing internal integrated circuit components and wire connections**

</details>

---

<details>
<summary><strong>Part 2: Practical Labs Implementation </strong></summary>

<details>
<summary><strong> Preresiquisites </strong></summary>

## Iverilog Setup Verification
<img width="1919" height="736" alt="Screenshot 2025-10-04 115636" src="https://github.com/user-attachments/assets/f8cd599f-dfab-451b-a9ab-d6c227dd5f26" />

## Gtkwave Setup Verification
<img width="1919" height="933" alt="image" src="https://github.com/user-attachments/assets/08f21760-8941-44eb-b74a-58d59c450632" />

## Repository Setup

```
# Clone the main VSDBabySoC repository (most comprehensive version)
git clone https://github.com/manili/VSDBabySoC.git

# Enter the project directory
cd VSDBabySoC
```

<img width="1905" height="589" alt="image" src="https://github.com/user-attachments/assets/23f05789-2408-401d-b093-280c658d0ed7" />

TLV → Verilog Conversion

```
# Install SandPiper-SaaS
pip install pyyaml click sandpiper-saas

# Convert TLV → Verilog
sandpiper-saas -i ./src/module/*.tlv -o rvmyth.v --bestsv --noline -p verilog --outdir ./src/module/
```

<img width="1919" height="754" alt="image" src="https://github.com/user-attachments/assets/8f0e7199-9a9b-46fe-9e6d-09b196515048" />

file structure

``` 
# First Install tree to view structure
sudo apt install tree

# Just tree to get file structure
tree
```

output
```
tree
.
├── images
│   ├── centralized_avsddac.png
│   ├── inside_dac.png
│   ├── inside_pll.png
│   ├── openlane_flow.png
│   ├── physical_design.png
│   ├── post_routing_sim.png
│   ├── post_synth_sim.png
│   ├── pre_synth_sim.png
│   ├── rvmyth_layout.png
│   ├── selected_dac.png
│   ├── selected_pll.png
│   ├── vsdbabysoc_block_diagram.png
│   └── vsdbabysoc_layout.png
├── LICENSE
├── Makefile
├── README.md
└── src
    ├── gds
    │   ├── avsddac.gds
    │   └── avsdpll.gds
    ├── gls_model
    │   ├── primitives.v
    │   └── sky130_fd_sc_hd.v
    ├── include
    │   ├── sandpiper_gen.vh
    │   ├── sandpiper.vh
    │   ├── sp_default.vh
    │   └── sp_verilog.vh
    ├── layout_conf
    │   ├── rvmyth
    │   │   ├── config.tcl
    │   │   └── pin_order.cfg
    │   └── vsdbabysoc
    │       ├── config.tcl
    │       ├── macro.cfg
    │       └── pin_order.cfg
    ├── lef
    │   ├── avsddac.lef
    │   └── avsdpll.lef
    ├── lib
    │   ├── avsddac.lib
    │   ├── avsdpll.lib
    │   └── sky130_fd_sc_hd__tt_025C_1v80.lib
    ├── module
    │   ├── avsddac.v
    │   ├── avsdpll.v
    │   ├── clk_gate.v
    │   ├── pseudo_rand_gen.sv
    │   ├── pseudo_rand.sv
    │   ├── rvmyth_gen.v
    │   ├── rvmyth.tlv
    │   ├── rvmyth.v
    │   ├── testbench.rvmyth.post-routing.v
    │   ├── testbench.v
    │   └── vsdbabysoc.v
    ├── script
    │   ├── sta.conf
    │   ├── verilog_to_lib.pl
    │   └── yosys.ys
    └── sdc
        ├── vsdbabysoc_layout.sdc
        └── vsdbabysoc_synthesis.sdc
```

<img width="1919" height="979" alt="image" src="https://github.com/user-attachments/assets/73272415-350d-46a6-83cf-dd0c6a4f9261" />
<img width="1919" height="942" alt="image" src="https://github.com/user-attachments/assets/4b4f1a33-931c-4c97-94ea-6b355c63e3d7" />

</details>


<details>
<summary><strong> RTL Functional Validation</strong></summary>


### Commands:

```
# Create Simulation directory
mkdir -p simulation

# Run the command for simulation
iverilog -o simulation/pre_synth_sim.out -DPRE_SYNTH_SIM src/module/testbench.v -I src/include -I src/module
```

<img width="1919" height="658" alt="image" src="https://github.com/user-attachments/assets/2284190e-14df-4c44-a544-9939a407ae8b" />

## Commands to View Waveform 

```
cd simulation
./pre_synth_sim.out
gtkwave pre_synth_sim.vcd
```

## Waveforms

<img width="1919" height="704" alt="image" src="https://github.com/user-attachments/assets/59bca4e8-276d-4280-9d54-43dea304e1f5" />

## CPU Core
<img width="1919" height="716" alt="image" src="https://github.com/user-attachments/assets/4a6ca1f1-c58a-4a80-a502-62f845bc0a16" />

- Shows internal pipeline/control signals (decode fields, dmem addr/enables).
- Continuous toggling ⇒ steady instruction issue, no stalls.
- Confirms proper decode of funct3/funct7 and active memory access.

## Top Level Waveform

<img width="1919" height="713" alt="image" src="https://github.com/user-attachments/assets/d3ea46dc-b836-40d1-86bc-4713e65b0bb0" />

- `reset` inactive; system running.
- `RV_TO_DAC[9:0]` presents descending sample codes (e.g., 0AB → 099 → 088 ...).
- Confirms core → DAC bus connectivity under stable clock.

## DAC Conversion Waveform

<img width="1919" height="721" alt="image" src="https://github.com/user-attachments/assets/a418b4c7-cfe1-4dad-9664-6d7bb15f428e" />

- `D[9:0]` codes map linearly to `OUT` (e.g., 0x0AB → ~0.167 of full-scale).
- `VREFH=1`, `VREFL=0`; enable asserted.
- Verifies ideal digital-to-analog scaling (OUT ≈ D/1023).

## PLL Waveform

<img width="1919" height="717" alt="image" src="https://github.com/user-attachments/assets/377a2475-8d7d-41ae-813c-105a8160c90a" />

- Stable `CLK` with measured period ≈35.4 ns.
- Reference period ≈283.3 ns ⇒ ~8× multiplication factor.
- Low, steady period variation ⇒ locked condition.

# BabySoC Simulation Analysis

## 1. Reset Operation
- Reset asserted, then deasserted cleanly.
- While asserted: all observed data buses remain idle (zeros).
- After deassertion: CPU pipeline activity begins (decode, memory enables, funct fields toggle).
- `RV_TO_DAC / D[9:0]` starts updating with valid sample codes.
- Conclusion: Reset sequencing and release are correct.

## 2. Clocking
- PLL enables `ENb_CP` and `ENb_VCO` held low (active) → PLL powered and engaged.
- Stable `CLK` period ≈ 35.4 ns.
- Reference period ≈ 283 ns → frequency multiplication ≈ 8×.
- No visible jitter spikes or missing edges in captured window.
- Conclusion: PLL locked and supplying a clean multiplied system clock.

## 3. Dataflow Between Modules
- Core shows continuous instruction execution (decode & memory addr/enables toggling).
- Generated sample codes propagate onto `RV_TO_DAC[9:0]` / `D[9:0]`.
- DAC output `OUT` scales proportionally (OUT ≈ D/1023 with VREFH=1, VREFL=0).
- Consistent decreasing code sequence visible across core → top-level → DAC.
- Conclusion: End-to-end data path (Core → Bus → DAC) functions correctly.

## Overall Verification
Reset behavior, clock generation, and inter-module dataflow are all validated and operating as intended.

</details>

</details>

---

# BabySoC Simulation Summary

## Summary Table

| Aspect | Key Signals / Fields | Core Observation | Conclusion |
|--------|----------------------|------------------|------------|
| Reset Operation | `reset`, `RV_TO_DAC[9:0]`, core decode & memory enables | Buses idle (0) while reset asserted; activity starts immediately after deassertion | Reset sequencing correct; system initializes cleanly |
| Clocking | `CLK`, `REF`, `ENb_CP`, `ENb_VCO`, measured period (~35.4 ns), ref period (~283 ns) | PLL enables active-low; stable multiplied clock (~8× reference), no jitter spikes seen | PLL locked and supplying clean system clock |
| Dataflow (Core → DAC) | Core pipeline signals, `RV_TO_DAC[9:0]` / `D[9:0]`, `OUT`, `VREFH=1`, `VREFL=0` | Sample codes generated in core propagate to DAC; `OUT ≈ D / 1023` (descending sequence matches digital values) | End‑to‑end path intact; DAC conversion linear and synchronized |

## Key Takeaways
- Reset works: no unintended transitions before release; immediate, valid post-reset activity.
- Clock domain stable: PLL multiplies reference cleanly (≈8×) with consistent period.
- Data integrity preserved: Values generated in the CPU arrive unchanged at the DAC input.
- DAC behavior matches ideal formula: `OUT = (D / 1023) * (VREFH - VREFL)`.
- No evidence of stalls, clock glitches, or bus contention in observed window.




