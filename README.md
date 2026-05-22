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

### Key Learnings

- Basics of chip packaging and die structure
- Understanding RISC-V ISA and hardware flow
- Introduction to open-source ASIC design
- Importance of SKY130 open-source PDK
- Understanding OpenLANE RTL-to-GDSII flow
- Overview of synthesis, placement, CTS, routing, and verification
