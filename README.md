# sky130-openlane-workshop-nasscom
Repository documenting my learning and implementation of SoC design and ASIC physical design using OpenLane and SKY130.
## 1. Inception of Open-Source EDA, OpenLANE and SKY130 PDK

### Introduction

In embedded systems and modern electronic devices, the visible chip mounted on a board is actually the **chip package**. The package acts as a protective enclosure for the fabricated silicon die present inside it. Electrical connections between the package terminals and the silicon die are established using the **wire bonding** technique.

At the center of the package lies the actual fabricated chip known as the **die**. The die consists of:

- **Pads** – Interface points through which signals enter and leave the chip.
- **Core** – Region where the complete digital logic circuitry is implemented.


Together, the core and pads form the complete die used for semiconductor manufacturing.

---

### Foundry, IPs and Macros

A **Foundry** is a semiconductor manufacturing facility where integrated circuits are fabricated. Foundries provide process-specific resources known as **Foundry IPs (Intellectual Properties)** which are optimized for a particular technology node.
<img width="1033" height="587" alt="Screenshot 2026-05-22 190420" src="https://github.com/user-attachments/assets/4f79b15d-54f4-42a3-86bb-1cc2e3699b25" />
Examples include:

- Standard Cell Libraries
- SRAM blocks
- I/O Cells
- PLLs and Analog IPs

Reusable functional digital blocks are commonly referred to as **Macros**.

---

### Introduction to RISC-V ISA

To execute software on hardware, a sequence of translation steps is required.

A high-level language program written in C/C++ is first converted into assembly instructions using a compiler. For RISC-V based systems, the generated instructions follow the **RISC-V ISA (Instruction Set Architecture)**.

The overall flow is:

```text
C Program
   ↓
RISC-V Assembly Instructions
   ↓
Machine Language (Binary)
   ↓
RTL Implementation
   ↓
RTL-to-GDSII Physical Design Flow
```
### Components of the Workshop

The workshop was mainly divided into three major domains covering both digital design fundamentals and ASIC physical design implementation.

#### 1. RISC-V Instruction Set Architecture (ISA)

Introduction to the RISC-V architecture, instruction formats, assembly language programming, and hardware-software interaction.
<img width="946" height="599" alt="Screenshot 2026-05-22 190634" src="https://github.com/user-attachments/assets/e27f9ec5-07a3-47e4-a1c4-cda6e853f2ff" />

#### 2. RTL Design and Synthesis of RISC-V based CPU Core

Implementation and synthesis of the RISC-V CPU core `picorv32` using Verilog HDL and open-source synthesis tools.

#### 3. Physical Design Implementation of picorv32a
<img width="1042" height="558" alt="Screenshot 2026-05-22 190806" src="https://github.com/user-attachments/assets/4b00795a-5042-457d-a2f5-bdc4affc02de" />

Complete RTL-to-GDSII ASIC implementation flow including:

- Floorplanning
- Placement
- Clock Tree Synthesis (CTS)
- Routing
- Timing Analysis
- Layout Verification

---

### Open-Source ASIC Design Requirements

For successful open-source ASIC implementation, the following components must be available as open-source resources:

- RTL Designs
- EDA Tools
- Process Design Kits (PDKs)

In the early days of semiconductor design, chip design and fabrication were tightly coupled and carried out only by a few companies such as Intel and Texas Instruments.

Later, structured VLSI methodologies enabled the separation of design and fabrication processes, leading to the rise of:

- Fabless Companies — focused only on chip design
- Pure Play Fabs — focused only on semiconductor manufacturing

The communication interface between the design and fabrication teams became the **Process Design Kit (PDK)**.

---

### SKY130 Open-Source PDK

A **Process Design Kit (PDK)** contains all the necessary files and technology information required for IC fabrication and physical design.

Typical contents of a PDK include:

- Device Models
- Technology Information
- Design Rules
- Standard Cell Libraries
- I/O Libraries
- SPICE Models
- LEF Files
- GDSII Layout Files

Traditionally, PDKs were protected under Non-Disclosure Agreements (NDAs), making them inaccessible to the public and academic communities.

In June 2020, Google collaborated with SkyWater Technology to release the first open-source 130nm Process Design Kit called **SKY130**, which marked a major milestone in open-source ASIC development.

---

### OpenLANE ASIC Design Flow

OpenLANE is an open-source automated RTL-to-GDSII ASIC design flow that integrates several open-source EDA tools to automate the chip implementation process.

The primary objective of the ASIC flow is:

```text
RTL → GDSII
```

where:

- **RTL (Register Transfer Level)** represents the hardware functionality using HDL.
- **GDSII** is the final layout format used for chip fabrication.

OpenLANE simplifies ASIC implementation by integrating synthesis, floorplanning, placement, CTS, routing, and sign-off verification into a single automated flow.

---

### ASIC Design Flow

ASIC implementation involves multiple stages to convert RTL code into a manufacturable physical layout.

---

#### 1. Synthesis

Synthesis converts the RTL description into a gate-level netlist using Standard Cell Libraries (SCLs).
<img width="1042" height="564" alt="Screenshot 2026-05-22 190928" src="https://github.com/user-attachments/assets/398fd791-66f2-462f-a984-3588c3892586" />

Output:
- Gate-level Netlist

The synthesized netlist is functionally equivalent to the original RTL design.

Standard cells contain multiple views used by different EDA tools, including:

- Liberty (.lib) Timing Models
- Verilog Behavioral Models
- SPICE/CDL Views
- LEF Abstract Layout Views
- GDSII Physical Layouts

---

#### 2. Floorplanning and Power Planning

Floorplanning defines:

- Chip dimensions
- Core area
- Macro placement
- Power distribution structure
<img width="1041" height="550" alt="Screenshot 2026-05-22 191023" src="https://github.com/user-attachments/assets/86e9029d-8b3b-48bc-9ad3-af1d2b94730a" />
<img width="1037" height="539" alt="Screenshot 2026-05-22 191100" src="https://github.com/user-attachments/assets/9314146d-f9aa-4826-ac09-0d198451716c" />

Power planning is generally implemented using upper metal layers because they have lower resistance and can reduce:

- IR Drops
- Electromigration effects
<img width="1045" height="565" alt="Screenshot 2026-05-22 191137" src="https://github.com/user-attachments/assets/e7a7eea5-95f0-4bb2-baa3-1a938082ae26" />

---

#### 3. Placement

Placement determines the physical location of standard cells inside the core area.
<img width="1043" height="547" alt="Screenshot 2026-05-22 191205" src="https://github.com/user-attachments/assets/6ac79491-7a06-42d5-ba81-a46dceb99224" />

##### Global Placement
Provides approximate locations based on connectivity.

##### Detailed Placement
Removes overlaps and legalizes the placement according to design rules.

---

#### 4. Clock Tree Synthesis (CTS)

Clock Tree Synthesis distributes the clock signal uniformly across the design.
<img width="1045" height="554" alt="image" src="https://github.com/user-attachments/assets/af507832-79c6-45bb-bf0c-665f07f21961" />

A major concern during CTS is:

- **Clock Skew** — difference in clock arrival time at different sequential elements.

Proper CTS helps improve timing reliability and synchronization.

---

#### 5. Routing
<img width="1040" height="574" alt="image" src="https://github.com/user-attachments/assets/d59dfb37-f528-4c6e-8ba8-81443b0facc4" />

Routing establishes all physical interconnections between placed cells.

The SKY130 PDK provides:

- 1 Local Interconnect Layer
- 5 Aluminum Metal Routing Layers
<img width="1045" height="554" alt="image" src="https://github.com/user-attachments/assets/9bf321dd-2162-454a-ac03-295e4a10eb8d" />

Routing is divided into:

##### Global Routing
Determines approximate routing paths.

##### Detailed Routing
Creates exact physical wire connections while satisfying design rules.

---

### Sign-Off Verification

After routing, the design undergoes multiple verification stages before fabrication.
<img width="1044" height="550" alt="image" src="https://github.com/user-attachments/assets/77f2d7ab-f643-4d08-b61c-48ec200c4cbf" />

#### Design Rule Check (DRC)

Verifies that the layout satisfies all fabrication design constraints.

#### Layout Versus Schematic (LVS)

Checks whether the generated layout matches the intended circuit netlist.

#### Static Timing Analysis (STA)

Ensures that the design operates correctly at the target clock frequency without timing violations.

---

## 2. Good Floorplan vs Bad Floorplan and Introduction to Library Cells

### Introduction

Floorplanning is one of the most important stages in the ASIC physical design flow. A proper floorplan directly impacts:

- Area utilization
- Routing congestion
- Timing performance
- Power distribution
- Overall chip manufacturability

A well-optimized floorplan improves design efficiency, while a poor floorplan can lead to timing violations, congestion, and routing failures.

---

### Chip Floorplanning

Floorplanning defines the physical structure of the chip, including:

- Core area
- Die area
- Placement of macros
- Standard cell regions
- Power distribution network
- Input/Output pad locations

The floorplanning stage is performed after synthesis and before placement.

---

### Good Floorplan vs Bad Floorplan

#### Characteristics of a Good Floorplan

- Proper macro placement
- Minimum routing congestion
- Efficient utilization factor
- Balanced aspect ratio
- Short interconnect lengths
- Proper power distribution
- Sufficient spacing between macros

Advantages:

- Better timing performance
- Reduced power consumption
- Easier routing
- Reduced congestion
- Improved manufacturability

---

#### Characteristics of a Bad Floorplan

- Congested routing regions
- Poor macro arrangement
- Excessive dead space
- Long interconnect paths
- Improper utilization
- Difficult routing channels
- Increased timing violations

Disadvantages:

- Routing failures
- Increased delay
- Higher power consumption
- IR drop issues
- Poor timing closure

---

### Utilization Factor and Aspect Ratio

#### Utilization Factor

The utilization factor represents the percentage of the core area occupied by standard cells.

```text
Utilization Factor = Area occupied by cells / Total core area
```

Higher utilization may increase congestion, while lower utilization wastes chip area.

---

#### Aspect Ratio

Aspect ratio is defined as:

```text
Aspect Ratio = Height / Width
```

An aspect ratio close to 1 generally results in a square floorplan which simplifies routing and placement.

---

### Pre-Placed Cells and Decoupling Capacitors

Certain cells are placed before standard cell placement begins.

These include:

- Macros
- Memory blocks
- IP blocks

Decoupling capacitors are added near switching circuits to reduce voltage fluctuations and improve power stability.

---

### Power Planning

Power planning ensures reliable power delivery across the chip.

Power is distributed using:

- Power rails
- Power rings
- Power straps

Upper metal layers are preferred because they have:

- Lower resistance
- Better current carrying capability

Proper power planning helps prevent:

- IR drops
- Electromigration
- Power noise

---

### Pin Placement

Pin placement defines the location of input and output ports around the chip boundary.

A good pin placement strategy helps:

- Reduce routing complexity
- Minimize wirelength
- Improve timing performance

---

### Introduction to Standard Cell Libraries

Standard Cell Libraries (SCLs) are collections of pre-designed and pre-characterized logic cells used during synthesis and physical design.

Examples of standard cells:

- AND Gate
- OR Gate
- NAND Gate
- NOR Gate
- Flip-Flops
- Buffers
- Inverters

---

### Standard Cell Characteristics

A standard cell is designed with fixed height and variable width to simplify placement and routing.

Each standard cell contains multiple views:

| View Type | Purpose |
|---|---|
| Liberty (.lib) | Timing and power models |
| Verilog Model | Functional behavior |
| SPICE/CDL | Circuit-level simulation |
| LEF | Abstract physical information |
| GDSII | Complete physical layout |

---

### Library Cell Design Considerations

Important parameters while designing library cells include:

- Cell height
- Cell width
- Power consumption
- Propagation delay
- Noise margin
- Drive strength

The placement of power rails and routing tracks must follow the technology design rules.

---

### Introduction to Tracks and Routing

Tracks are predefined routing paths used during detailed routing.

Proper alignment between:

- Pins
- Routing tracks
- Standard cell dimensions

ensures successful routing and DRC-clean layouts.

---

### Timing Considerations

Timing performance is affected by:

- Cell delay
- Wire delay
- Clock skew
- Signal transition time

Optimized floorplanning and proper standard cell selection help improve timing closure.

---
## 3. Design Library Cell using Magic Layout and ngspice Characterization

### Introduction

Standard cell design is one of the most important stages in ASIC physical design. A standard cell must satisfy both functional and physical design constraints while maintaining optimized timing, power, and area characteristics.

In this section, a CMOS inverter standard cell is designed using the **Magic Layout Tool** and characterized using **ngspice** simulation.

The complete process involves:

- CMOS inverter design
- Layout creation
- DRC verification
- SPICE extraction
- Circuit characterization
- Timing analysis

---

### CMOS Inverter

A CMOS inverter is the fundamental building block of digital integrated circuits.

It consists of:

- PMOS transistor connected to VDD
- NMOS transistor connected to GND

Operation:

- Input LOW → PMOS ON, NMOS OFF → Output HIGH
- Input HIGH → PMOS OFF, NMOS ON → Output LOW

---

### Standard Cell Design Flow

The standard cell design flow includes:

```text
Circuit Design
   ↓
SPICE Simulation
   ↓
Layout Design
   ↓
DRC Check
   ↓
LVS Verification
   ↓
Parasitic Extraction
   ↓
Post-Layout Simulation
   ↓
Characterization
```

---

### Introduction to Magic Layout Tool

Magic is an open-source VLSI layout editor used for:

- Layout creation
- Design Rule Checking (DRC)
- Extraction of parasitics
- Layout visualization

Magic supports the SKY130 technology and enables full-custom layout design.

---

### CMOS Inverter Layout Design

The inverter layout is created by placing:

- PMOS transistor in N-well region
- NMOS transistor in P-substrate region

Important layout considerations include:

- Proper transistor sizing
- Shared diffusion regions
- Minimum area usage
- Proper routing
- Power rail alignment

The layout is designed according to SKY130 design rules.

---

### Design Rules

Design rules define the manufacturing constraints that must be followed during layout creation.

These rules specify:

- Minimum width
- Minimum spacing
- Enclosure requirements
- Contact dimensions
- Metal overlap constraints

Violation of design rules can result in fabrication failures.

---

### DRC (Design Rule Check)

After layout creation, DRC is performed to ensure that all geometry rules are satisfied.

DRC helps identify:

- Spacing violations
- Width violations
- Overlap errors
- Contact violations

A DRC-clean layout is mandatory before fabrication.

---

### SPICE Extraction

Once the layout passes DRC, parasitic extraction is performed.

Extraction generates a SPICE netlist containing:

- Transistor information
- Interconnect parasitics
- Capacitances
- Resistances

This extracted netlist is used for accurate post-layout simulation.

---

### Introduction to ngspice

ngspice is an open-source circuit simulation tool used for transistor-level analysis.

It is used to perform:

- Transient analysis
- DC analysis
- Timing characterization
- Power analysis

The extracted SPICE netlist from Magic is simulated using ngspice.

---

### Timing Characterization

Timing characterization determines the electrical performance of the standard cell.

Important timing parameters include:

- Rise Time
- Fall Time
- Propagation Delay
- Transition Delay

These parameters are later used in timing libraries during synthesis and STA.

---

### Rise Time and Fall Time

#### Rise Time

Rise time is the time taken for the output signal to transition from:

```text
20% → 80% of VDD
```

#### Fall Time

Fall time is the time taken for the output signal to transition from:

```text
80% → 20% of VDD
```

Lower transition times improve circuit speed.

---

### Propagation Delay

Propagation delay is the time difference between input transition and output transition.

Two types:

- High-to-Low Delay (tphl)
- Low-to-High Delay (tplh)

Propagation delay directly impacts the operating frequency of the circuit.

---

### Cell Characterization

Characterization generates timing and power information for the standard cell.

The characterized data is stored in:

```text
.lib (Liberty File)
```

The liberty file contains:

- Timing models
- Delay information
- Power models
- Transition information

EDA tools use this data during synthesis and timing analysis.

---

### Layout Versus Schematic (LVS)

LVS verifies whether:

```text
Extracted Layout Netlist = Intended Schematic Netlist
```

LVS ensures that the physical layout matches the designed circuit functionality.

---

### Importance of Standard Cell Characterization

Proper characterization ensures:

- Accurate timing analysis
- Reliable synthesis
- Better power estimation
- Improved timing closure
- Manufacturable layouts

Poor characterization can lead to incorrect STA results and timing failures.

---
## 4. Pre-layout Timing Analysis and Importance of Good Clock Tree

### Introduction

Timing analysis is one of the most critical stages in digital ASIC design. Even if a circuit is functionally correct, improper timing can cause setup violations, hold violations, metastability, and unreliable operation.

Pre-layout timing analysis is performed before routing and physical implementation to estimate the timing performance of the design.

This stage helps in:

- Identifying timing violations
- Improving clock performance
- Optimizing combinational paths
- Reducing delay
- Achieving timing closure

---

### Timing Basics

Digital circuits operate synchronously using a clock signal.

Important timing parameters include:

- Setup Time
- Hold Time
- Clock Period
- Propagation Delay
- Clock Skew
- Clock Jitter

Proper timing analysis ensures reliable data transfer between sequential elements.

---

### Setup Time

Setup time is the minimum time before the clock edge during which the input data must remain stable.

Violation of setup time may result in incorrect data capture.

```text
Data must arrive before the active clock edge
```

---

### Hold Time

Hold time is the minimum time after the clock edge during which the input data must remain stable.

Violation of hold time may cause metastability.

```text
Data must remain stable after the clock edge
```

---

### Propagation Delay

Propagation delay is the time required for a signal to travel from input to output.

It includes:

- Gate delay
- Wire delay
- Buffer delay

Propagation delay directly affects the maximum operating frequency of the design.

---

### Clock Signal in Digital Design

The clock signal synchronizes all sequential elements in the circuit.

A reliable clock network is necessary for:

- Stable operation
- Timing synchronization
- Predictable data transfer

Poor clock distribution can lead to timing violations and unreliable functionality.

---

### Clock Tree Synthesis (CTS)

Clock Tree Synthesis is the process of distributing the clock signal evenly across the chip.

CTS inserts:

- Buffers
- Inverters
- Clock routing networks

to ensure balanced clock arrival at all sequential elements.

---

### Importance of Good Clock Tree

A well-designed clock tree helps achieve:

- Reduced clock skew
- Lower clock latency
- Improved timing closure
- Better synchronization
- Reduced timing violations

A poor clock tree may result in:

- Large clock skew
- Increased latency
- Setup and hold violations
- Excessive power consumption

---

### Clock Skew

Clock skew is the difference in clock arrival time between two sequential elements.

```text
Clock Skew = Difference in clock arrival times
```

Types of clock skew:

- Positive Skew
- Negative Skew
- Zero Skew

Excessive skew negatively impacts timing performance.

---

### Clock Latency

Clock latency is the delay between the clock source and the destination flip-flop.

It depends on:

- Routing length
- Buffer insertion
- Interconnect resistance and capacitance

Lower clock latency improves circuit performance.

---

### Clock Jitter

Clock jitter refers to temporary variations in clock edge timing.

Causes of jitter include:

- Noise
- Power supply fluctuations
- Crosstalk
- PLL instability

High clock jitter can reduce timing reliability.

---

### Timing Paths

Timing analysis is performed on different signal paths in the design.

#### Types of Timing Paths

##### 1. Input to Register Path

Path from input port to flip-flop.

##### 2. Register to Register Path

Path between two sequential elements.

##### 3. Register to Output Path

Path from flip-flop to output port.

##### 4. Input to Output Path

Combinational path without sequential elements.

---

### Slack

Slack determines whether timing constraints are satisfied.

```text
Slack = Required Arrival Time - Actual Arrival Time
```

#### Positive Slack
- Timing is satisfied

#### Negative Slack
- Timing violation exists

Achieving positive slack is essential for successful timing closure.

---

### Pre-layout STA (Static Timing Analysis)

Static Timing Analysis evaluates timing performance without requiring dynamic simulation.

STA checks:

- Setup violations
- Hold violations
- Arrival times
- Required times
- Critical paths

Pre-layout STA helps identify timing issues before physical routing.

---

### Critical Path

The critical path is the longest timing path in the circuit.

It determines:

```text
Maximum Operating Frequency
```

Reducing critical path delay improves overall performance.

---

### Buffer Insertion and Optimization

Buffers are inserted to:

- Reduce signal degradation
- Improve transition time
- Minimize delay
- Strengthen weak signals

Optimization techniques include:

- Gate sizing
- Buffer insertion
- Logic restructuring

---

### Importance of Timing Closure

Timing closure ensures that all timing constraints are satisfied before fabrication.

Successful timing closure results in:

- Reliable operation
- Stable clock behavior
- Improved performance
- Manufacturable design

---
## 5. Final Steps for RTL2GDS using TritonRoute and OpenSTA

### Introduction

The final stage of the ASIC physical design flow involves routing, timing verification, and sign-off analysis before generating the final GDSII layout for fabrication.

This stage ensures that:

- All cells are properly connected
- Design rules are satisfied
- Timing constraints are met
- The layout is manufacturable

Key tools used in this stage include:

- TritonRoute
- OpenSTA
- Magic
- Netgen

---

### Routing in ASIC Design

Routing is the process of creating physical electrical connections between all placed standard cells and macros.

The routing stage converts:

```text
Logical Connections → Physical Metal Interconnects
```

Routing must satisfy:

- Design Rules
- Timing Constraints
- Signal Integrity
- Power Requirements

---

### Types of Routing

#### Global Routing

Global routing determines approximate routing paths and routing resources.

Objectives:

- Estimate routing congestion
- Allocate routing tracks
- Determine routing regions

---

#### Detailed Routing

Detailed routing creates exact metal connections between cells.

It ensures:

- DRC-clean routing
- Correct via placement
- Accurate metal geometry

Detailed routing is more complex and must satisfy all fabrication rules.

---

### TritonRoute

TritonRoute is an open-source detailed routing engine used in the OpenLANE flow.

Features of TritonRoute:

- Detailed routing automation
- DRC-aware routing
- Multi-layer routing support
- Via optimization
- Pin accessibility analysis

TritonRoute generates the final routed layout after placement and CTS.

---

### SKY130 Routing Layers

The SKY130 PDK provides multiple routing layers.

These include:

- Local Interconnect Layer
- Metal1
- Metal2
- Metal3
- Metal4
- Metal5

The lower layers are generally used for local routing, while upper layers are used for global interconnects and power distribution.

---

### Routing Challenges

Major routing challenges include:

- Routing congestion
- DRC violations
- Crosstalk
- IR drops
- Antenna effects

Proper floorplanning and placement help reduce routing complexity.

---

### Antenna Effect

During fabrication, long metal wires can accumulate charge and damage transistor gates.

This phenomenon is called the antenna effect.

Antenna violations are fixed using:

- Antenna diodes
- Layer hopping
- Jumper insertion

---

### Power Distribution Network (PDN)

The Power Distribution Network supplies stable power throughout the chip.

PDN consists of:

- Power rails
- Power rings
- Power straps
- VDD and GND connections

A strong PDN minimizes:

- Voltage drops
- Electromigration
- Power noise

---

### OpenSTA

OpenSTA is an open-source Static Timing Analysis tool used to verify timing performance after routing.

OpenSTA performs:

- Setup analysis
- Hold analysis
- Slack calculation
- Clock analysis
- Critical path identification

It ensures that the routed design satisfies all timing requirements.

---

### Post-Layout STA

After routing, interconnect parasitics become significant.

Post-layout STA considers:

- Wire resistance
- Wire capacitance
- Buffer delay
- Clock delay

This provides more accurate timing results compared to pre-layout analysis.

---

### SPEF Extraction

SPEF (Standard Parasitic Exchange Format) files contain extracted parasitic information from the routed layout.

The SPEF file includes:

- Resistance values
- Capacitance values
- Interconnect parasitics

These parasitics are used during post-layout STA.

---

### Design Rule Check (DRC)

DRC verifies whether the final routed layout satisfies all fabrication design rules.

Checks include:

- Minimum spacing
- Minimum width
- Via rules
- Metal overlap constraints

A DRC-clean design is required before tape-out.

---

### Layout Versus Schematic (LVS)

LVS ensures that the final physical layout matches the intended circuit netlist.

```text
Layout Netlist = Schematic Netlist
```

LVS verification confirms functional correctness of the physical design.

---

### GDSII Generation

After successful routing and verification, the final layout is exported in:

```text
GDSII Format
```

GDSII is the industry-standard file format used for semiconductor fabrication.

This file contains all geometric information required for manufacturing the chip.

---

### Sign-Off Checks

Before fabrication, the design undergoes final sign-off verification.

Important sign-off checks include:

- DRC Verification
- LVS Verification
- Static Timing Analysis
- Antenna Checks
- IR Drop Analysis

Only after passing all sign-off checks is the design considered tape-out ready.

---

### Tape-Out

Tape-out refers to the final stage where the verified GDSII file is sent to the foundry for fabrication.

Successful tape-out indicates that:

- Timing is closed
- Routing is complete
- Verification checks are passed
- The chip is ready for manufacturing

---
