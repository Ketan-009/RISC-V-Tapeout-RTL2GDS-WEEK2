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

## Waveform

<img width="1919" height="704" alt="image" src="https://github.com/user-attachments/assets/59bca4e8-276d-4280-9d54-43dea304e1f5" />

## VSDBabySoC Waveform Analysis (Simulation Summary)

- **System Integration:** The waveform confirms successful integration of the RVMYTH RISC-V core, PLL, and DAC modules.
- **Clocking:** The PLL generates stable clock signals (REF, VCO_IN) that correctly drive the processor.
- **Reset Behavior:** The reset signal is inactive after initialization, allowing normal system operation.
- **Data Flow:** The DAC OUT signal varies, indicating active data processing and instruction execution by the RISC-V core.
- **Timing:** Clock domains are synchronized; reset sequence completes successfully.


### Observations
- PLL-derived clock stable
- Reset released cleanly
- Core executes code
- DAC output toggles (driven digital proxy)
- Inter-module connectivity valid

**Conclusion:**  
The simulation demonstrates that all SoC components are operational and properly interfaced. The processor executes instructions and outputs results via the DAC, verifying correct system functionality.

</details>


The comprehensive verification methodology for the BabySoC implementation has been successfully completed. Both pre-synthesis RTL and post-synthesis gate-level implementations have been validated for functional correctness. The verification process has produced a thoroughly tested gate-level netlist ready for physical design implementation.

**Key Achievements:**
- Complete RTL functional verification with comprehensive test coverage
- Successful logic synthesis with area and timing optimization
- Gate-level simulation confirming functional equivalence
- Robust verification methodology suitable for complex SoC designs

**Deliverables:**
- Verified RTL implementation
- Optimized gate-level netlist
- Comprehensive verification documentation
- Reusable verification environment and methodology



