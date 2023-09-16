# Advanced_Physical_Design
Advanced Physical Design using OpenLANE/Sky130 
## Table of Contents
* [1.Introduction](#1-introduction)
* [2.Day 1 - Inception of open-source EDA, OpenLANE and Sky130 PDK](#2-day-1---inception-of-open-source-eda--openlane-and-sky130-pdk)
  * [2.1 Fundamental terms](#2-1-fundamental-terms)
  * [2.2 SoC design and OpenLANE](#2-2-soc-design-and-openlane)
  * [2.3 OpenLane ASIC flow](#2-3-openlane-asic-flow)
* [3.Day 2 - Good floorplan vs bad floorplan and introduction to library cells](#3-day-2---good-floorplan-vs-bad-floorplan-and-introduction-to-library-cells)
  * [3.1 Floor Planning Considerations](#3-1-floor-planning-Considerations)
     * [3.1.1 Utilization factor and aspect ratio ](#3-1-1-utilization-factor-and-aspect-ratio)
     * [3.1.2 Pre-placed cells](#3-1-2-pre-placed-cells)
     * [3.1.3 Decoupling capacitors](#3-1-3-decoupling-capacitors)
     * [3.1.4 Power Planning](#3-1-4-power-planning)
     * [3.1.5 Pin Placement](#3-1-5-pin-placement)
  * [3.2 Performing Floor Planning and Placement in OpenLane](#3-2-performing-floor-planning-and-placement-in-openlane)
  * [3.3 Cell design and characterization flows](#3-3-cell-design-and-characterization-flows)
  * [3.4 General timing characterization parameters](#3-4-general-timing-characterization-parameters)
* [4.Day 3 - Design library cell using Magic Layout and ngspice characterization](#4-day-3---design-library-cell-using-magic-layout-and-ngspice-characterization)
  * [4.1 CMOS inverter ngspice simulations](#4-1-cmos-inverter-ngspice-simulations)
  * [4.2 Inception of Layout CMOS fabrication process](#4-2-inception-of-layout-cmos-fabrication-process)
  * [4.3 Sky130 Tech File Labs](#4-3-sky130-tech-file-labs)
* [5.Day 4 - Pre-layout timing analysis and importance of good clock tree](#5-day-4-pre-layout-timing-analysis-and-importance-of-good-clock-tree)
  * [5.1 Timing modelling using delay tables](#5-1-timing-modelling-using-delay-tables)
  * [5.2 Timing analysis with ideal clocks using openSTA](#5-2-timing-analysis-with-ideal-clocks-using-opensta)
  * [5.3 Clock tree synthesis TritonCTS and signal integrity](#5-3-clock-tree-synthesis-tritoncts-and-signal-integrity)
  * [5.4 Timing analysis with real clocks using openSTA](#5-4-timing-analysis-with-real-clocks-using-opensta)
* [6.Day 5 - Final steps for RTL2GDS using tritonRoute and openSTA](#6-day-5-final-steps-for-rtl2gds-using-tritonroute-and-opensta)
  * [6.1 Routing and design rule check (DRC)](#6-2-routing-and-design-rule-check-(drc))
  * [6.2 Power Distribution Network and routing](#6-2-power-distribution-network-and-routing)
  * [6.3 TritonRoute Features](#6-3-tritonroute-features)

## <a name="1-introduction"></a> 1.Introduction ##  
Electronic Design Automation (EDA) tools are essential for designing and simulating electronic circuits and systems. There are several open-source EDA tools available that provide various functionalities for electronic design.   
OpenLane is an open-source digital ASIC (Application-Specific Integrated Circuit) design flow that utilizes various open-source EDA (Electronic Design Automation) tools and resources to automate the design process for semiconductor manufacturing. OpenLane provides a complete RTL-to-GDSII (Register-Transfer Level to Graphic Data System II) design flow for digital ASICs. It leverages open-source EDA tools like Yosys for synthesis, ABC for technology mapping, and Magic for physical design. OpenLane streamlines the process of designing digital ASICs.  
SkyWater Technology Foundry offers semiconductor manufacturing services, including access to their 130nm technology node. This means that designers who have completed their ASIC designs using OpenLane can potentially use SkyWater's manufacturing services to produce physical chips based on their designs.  

## <a name="2-day-1---inception-of-open-source-eda--openlane-and-sky130-pdk"></a> 2.Day 1 - Inception of open-source EDA, OpenLANE and Sky130 PDK
### <a name="2-1-fundamental-terms"></a> 2.1 Fundamental terms ###
**Package:** In semiconductor technology, a package refers to the protective enclosure or casing that houses the semiconductor chip (IC). The package provides physical protection to the chip, as well as electrical connections to the outside world through pins or leads. Packages come in various shapes and sizes, and the choice of package can impact the performance and thermal characteristics of the IC. One such package is QFN-48 package.
_QFN-48 package_ - A QFN-48 package is a type of surface-mount integrated circuit (IC) package commonly used for various electronic components, such as microcontrollers, microprocessors, and other integrated circuits. "QFN" stands for "Quad Flat No-Lead," which describes the package's physical characteristics.
_Some key features of a QFN-48 package:_

1. Quad Flat: The package has a flat, square or rectangular shape with leads (connection points) on all four sides, which makes it easy to solder to a printed circuit board (PCB).
2. No-Lead: Unlike older IC packages like the Dual In-Line Package (DIP), QFN packages do not have traditional through-hole leads. Instead, they use surface-mount technology (SMT) with small metal pads on the bottom of the package. These pads make contact with corresponding pads on the PCB, and solder is applied to create the electrical connection.
3. 48 Pins: The "48" in QFN-48 indicates that this particular package has 48 electrical connection points (or pins) that interface with the PCB.
4. Low Profile: QFN packages are known for their low profile, which means they have a small height above the PCB. This is advantageous for applications where space is limited or when you need to minimize the overall height of the electronic device.
5. Thermal Performance: QFN packages often have exposed metal pads on the bottom, which can improve thermal performance by acting as a heat sink and dissipating heat away from the IC.
6. Package Variations: There are different variations of QFN packages, such as QFN-32, QFN-64, etc., with varying pin counts to accommodate different IC designs.  

**Chip:** A chip, also known as an integrated circuit (IC), is a miniaturized electronic circuit that consists of a collection of electronic components (such as transistors, resistors, capacitors, etc.) fabricated onto a single semiconductor substrate (usually silicon). The chip performs specific functions, such as data processing, amplification, or memory storage.  

**Pads:** Pads refer to the external connection points on an IC package. These pads are typically used for soldering the IC onto a printed circuit board (PCB) and for making electrical connections between the IC and other components on the PCB. Pads can be in the form of metal pins or pads with a grid array.  

**Core:** In the context of a microprocessor or CPU (Central Processing Unit), the core refers to the central processing unit itself. A multi-core processor may have multiple processing cores on a single chip, allowing it to perform multiple tasks simultaneously.  

**Die:** The term "die" refers to the small, individual semiconductor chip that is created during the manufacturing process. Before packaging, many identical chips are fabricated on a single silicon wafer. After fabrication, the wafer is diced into individual dies, each of which can become a separate IC when packaged.  

**IPs:** In the context of semiconductors and integrated circuits, IP can refer to intellectual property blocks, which are pre-designed and pre-verified functional units or cores that can be integrated into custom IC designs. These IP blocks can include things like processors, memory blocks, or interface modules, and they help speed up the development process by providing pre-tested and reusable building blocks for chip designers.  
Foundry IP's --> SRAM, ADC, PLL, DAC  

### <a name="2-2-soc-design-and-openlane"></a> 2.2 SoC design and OpenLANE ###
#### Components of open-source digital asic design ####
The design of a digital Application Specific Integrated Circuit (ASIC) typically requires three key enablers or elements:  

<p align="center">
 
  ![image](https://github.com/V-Pranathi/Advanced_Physical_Design/assets/140998763/9eba812f-2efd-47ab-b1b8-c47aa3e037de)

</p>
_1. Register Transfer Level Intellectual Property (RTL IPs):_ RTL IPs are pre-designed and pre-verified building blocks of digital logic circuits. These blocks are created using hardware description languages (HDLs) like Verilog or VHDL. RTL IPs can include components such as adders, multiplexers, flip-flops, memory blocks, and more. Designers can use these IPs to save time and effort when constructing complex digital circuits, as they don't have to design these building blocks from scratch. Instead, they can integrate RTL IPs into their designs, ensuring that these components are both functional and optimized for the intended application.  

_2. Electronic Design Automation (EDA) Tools:_ EDA tools are software applications that facilitate the design, analysis, simulation, and verification of electronic circuits and systems. In ASIC design, EDA tools are essential for tasks like RTL synthesis, logic optimization, floor planning, placement, routing, and timing analysis. These tools help automate many aspects of the design process, improve design productivity, and ensure that the ASIC meets its performance and power consumption requirements.  

_3. Process Design Kit (PDK):_ <p align="center"> **PDK = the interface between the FAB and the designers** </p>  
The PDK is a collection of data, files, and libraries provided by the semiconductor foundry. It contains information about the foundry's specific manufacturing process, including transistor models, design rules, device parameters, and other process-related data. PDK data is critical because it enables ASIC designers to create designs that are compatible with the foundry's manufacturing process. It ensures that the design can be fabricated successfully, taking into account the foundry's process characteristics, limitations, and requirements.  

* Opensource RTL Designs: github, librecores, opencores  
* Opensource EDA tools: QFlow, OpenROAD, OpenLANE  
* Opensource PDK data: Google Skywater130 PDK

ASIC flow objective : RTL to GDS II format used for final layout. The flow is often referred to as an automated Place and Route (P&R) process.  

#### Simplified RTL2GDS flow ####

  ![image](https://github.com/V-Pranathi/Advanced_Physical_Design/assets/140998763/50eabee3-2efc-4c60-96b6-cb7e35e8aaea)  

* **Synthesis** - The RTL code is synthesized to create a gate-level netlist. This process involves translating the RTL code into a (circuit)network of standard cells (AND, OR, flip-flops, etc.) that can be found in a Standard cell library.

  ![image](https://github.com/V-Pranathi/Advanced_Physical_Design/assets/140998763/d787dbf1-9de4-41e4-b985-ad63665f60eb)
 
* **Floor/Power Planning** - The objective is to plan the silicon area and robust power distribution network to power the circuits. By performing floorplanning we determine the approximate placement of the various logic elements on the silicon die. This step helps optimize the chip's area, power consumption, and performance.  
  _Chip-Floor Planning_ Partition the chip die between different system building blocks and place the I/O pads.  
  _Macro-Floor Planning_ - In macro-floor planning we define the macro dimensions and its pin locations. WE also define row definitions which is used in placement process.  
  _Power PLanning_ - Power planning is the process of managing and distributing electrical power within an IC to ensure proper functionality, performance, and reliability while minimizing power consumption.  
* **Placement** - Place the cells on the floorplan rows aligned with the sites. Placement is done in two steps 1) GLobal 2) Detailed.  
  _Global_ -  Global placement is the initial phase of placement in IC design. It involves placing the major functional blocks of the design onto the chip's floorplan while considering high-level objectives like chip area, wirelength, and approximate positions of the blocks.  
  _Detailed_ - Detailed placement follows global placement and focuses on refining the positions of individual cells, gates, and standard cells within the functional blocks. It involves optimizing the placement at a finer granularity.   
* **Clock Tree Synthesis** - Clock Tree Synthesis (CTS) is a crucial step in the physical design phase of integrated circuit (IC) design. It involves the generation and optimization of a clock distribution network within the chip to ensure that clock signals are delivered accurately and efficiently to all sequential elements (like flip-flops and latches) while meeting timing requirements. The primary goals of CTS are to minimize clock skew, reduce clock latency, and enhance overall chip performance and power efficiency.  
* **Routing** - Routing is a fundamental step in the physical design phase of integrated circuit (IC) design. It involves the creation of metal interconnects (wires) that connect various components, such as logic gates, memory cells, and input/output (I/O) pins, on the semiconductor chip's layout. Effective routing is crucial for ensuring the chip meets performance, power, and area (PPA) requirements. 
* **Signoff** - "signoff" refers to a critical phase in the design and verification process. Signoff is the final stage where various checks and analyses are performed to ensure that the design meets all specifications, performance requirements, and manufacturing criteria before it is sent for fabrication. The term "signoff" signifies the formal approval or endorsement of the design, indicating that it is ready for production.  
  _Physical Verification_ - The design layout is subjected to physical verification checks, which include  
       * **Design Rule Checking (DRC):** DRC ensures that the design adheres to the foundry's manufacturing rules.  
       * **Layout vs. Schematic (LVS) checks:** LVS confirms that the layout matches the intended schematic.  
  _Timing Verification_ - Static Timing Analysis (STA), Timing analysis tools are used to verify that all timing constraints, including setup and hold times, are met across the entire design. Any violations must be addressed before signoff.
  
### <a name="2-3-openlane-asic-flow"></a>2.3 OpenLane ASIC flow ###

**Various steps involve in OpenLANE ASIC Flow**  

* RTL Synthesis, Technology Mapping, and Formal Verification:  
        Tools: Yosys (for RTL synthesis), ABC (for technology mapping and formal verification).  

* Static Timing Analysis:  
        Tool: OpenSTA (for static timing analysis).  

* Floor Planning:  
        Tools: init_fp (initial floorplanning), ioPlacer (I/O placement), pdn (power distribution network planning), tapcell (tap cell insertion).  

* Placement:  
        Tools: RePLace (global placement), Resizer (optional for resizing cells), OpenPhySyn (formerly used for placement), OpenDP (detailed placement).  

* Clock Tree Synthesis:  
        Tool: TritonCTS (for clock tree synthesis).  

* Fill Insertion:  
        Tools: OpenDP (for filler placement).  

* Routing:  
        Global Routing: FastRoute or CU-GR (formerly used).  
        Detailed Routing: TritonRoute (for detailed routing) or DR-CU (formerly used).  

* SPEF Extraction:  
        Tools: OpenRCX (or SPEF-Extractor, formerly used) for Standard Parasitic Exchange Format (SPEF) extraction.  

* GDSII Streaming Out:  
        Tools: Magic and KLayout (for viewing and editing GDSII files).  

* Design Rule Checking (DRC) Checks:  
        Tools: Magic and KLayout (for DRC checks). 

* Layout vs. Schematic (LVS) Check:  
        Tool: Netgen (for LVS checks).  

* Antenna Checks:  
        Tool: Magic (for antenna checks).  

* Circuit Validity Checker:  
        Tool: CVC (for circuit validity checking).  

These open-source tools, when used collectively, provide a complete and automated ASIC design and verification flow through OpenLane. 


![image](https://github.com/V-Pranathi/Advanced_Physical_Design/assets/140998763/b9efd0fc-ac49-40f1-a0ca-0f0a5c7b2794)

#### Steps to install OpenLane ####
    
    cd $HOME
    git clone https://github.com/The-OpenROAD-Project/OpenLane --recurse-submodules 
    cd OpenLane
    make
    make test
    cd /home/pranathi/OpenLane/designs/ci
    cp -r * ../

#### Invoking and running synthesis OpenLane ####

    cd ~/OpenLane
    make mount
    ./flow.tcl -interactive
    package require openlane 0.9
    prep -design picorv32a
    run_synthesis

![image](https://github.com/V-Pranathi/Advanced_Physical_Design/assets/140998763/91e96179-e974-47f5-b9e7-1f6d65acd860)

**Viewing the netlist**  

    cd /home/pranathi/OpenLane/designs/picorv32a/runs/RUN_2023.09.10_05.15.33/results/synthesis
    gedit picor32a.v

![image](https://github.com/V-Pranathi/Advanced_Physical_Design/assets/140998763/363b2c29-175c-4380-99c5-5adb0c3c6f8c)

![image](https://github.com/V-Pranathi/Advanced_Physical_Design/assets/140998763/5db18bd3-2772-4c24-b3f7-1fefe532de62)  

**Calculating the flop ratio using synthesis reports**  

![image](https://github.com/V-Pranathi/Advanced_Physical_Design/assets/140998763/e92464fb-d463-455c-9a25-02aff3cd153f)

![image](https://github.com/V-Pranathi/Advanced_Physical_Design/assets/140998763/4eedeee2-d488-4ba5-afec-00378e90cfe3)

<p align="center"> Flop ratio =  Number of flops/Total number of cells =  1596/10104 = 0.1579 = 15.79% </p>  

## <a name="3-day-2---good-floorplan-vs-bad-floorplan-and-introduction-to-library-cells"></a> 3.Day 2 - Good floorplan vs bad floorplan and introduction to library cells ##
### <a name="3-1-floor-planning-Considerations"></a> 3.1 Floor Planning Considerations ###
#### <a name="3-1-1-utilization-factor-and-aspect-ratio"></a> 3.1.1 Utilization factor and aspect ratio ###  

In the context of floor planning in the field of integrated circuit design and layout, two important factors to consider are the "utilization factor" and the "aspect ratio."

    Utilisation Factor =  Area occupied by netlist
                         __________________________
                             Total area of core
                             

    Aspect Ratio =  Height
                   ________
                    Width

In practice, a utilization factor of 1 (100% utilization) is often unattainable due to various design considerations, including the need for buffer zones, routing channels, and other overhead. A utilization factor of 0.5 to 0.6 is much more typical and allows for these necessary design elements and potential future modifications. Regarding the aspect ratio, a value of 1 indicates a square-shaped chip, while any other value implies a rectangular shape. The choice of aspect ratio can depend on factors such as the chip's function, available package sizes, and manufacturing constraints.  

#### <a name="3-1-2-pre-placed-cells"></a> 3.1.2 Pre-placed cells ####  

Preplaced cells are fixed, predefined logic or functional blocks within an integrated circuit (IC) layout. They are positioned manually by design engineers at specific locations on the chip's layout canvas. Once placed, these cells maintain their fixed positions throughout subsequent stages of IC design, such as placement and routing. Preplaced cells typically contain complex logic or functional blocks that are critical to the chip's operation. These cells can include memory blocks, custom processors, analog circuitry, specialized accelerators, or other IP (Intellectual Property) components.  

#### <a name="3-1-3-decoupling-capacitors"></a> 3.1.3 Decoupling capacitors ####  
    
* Decoupling capacitors, often of significant capacitance, are strategically placed in close proximity to the logic circuits.  
* These capacitors are charged to the power supply voltage and serve as local energy reservoirs.  
* Their primary role is to decouple the circuit from the power supply, ensuring that the circuit receives the necessary amount of current during transient events, such as switching       activities.  
* Decaps help reduce crosstalk and electromagnetic interference (EMI) by stabilizing power supply voltages and minimizing voltage fluctuations caused by neighboring circuits.  

#### <a name="3-1-4-power-planning"></a> 3.1.4 Power Planning ####

* While pre-placed macros or cells can have dedicated decoupling capacitors for local power stability, it's not practical to provide each block or standard cell with its own decap.       Instead, a well-designed power planning strategy includes creating a power mesh to efficiently distribute power and ground (VDD and VSS) across the entire chip.  
* Multiple GND and VDD points are strategically placed throughout the IC layout to ensure even power distribution. This even distribution reduces the likelihood of voltage drops and      improves the efficiency of power delivery across the chip.  

#### <a name="3-1-5-pin-placement"></a> 3.1.5 Pin Placement ####

Pin placement, also known as I/O (Input/Output) planning or pin assignment, is a critical aspect of integrated circuit (IC) design. It involves determining the locations and assignments of pins or external connections on the chip package. Proper pin placement is essential for functionality, manufacturability, and overall performance. By carefully arranging pins, signal strength can be preserved, preventing signal degradation and ensuring that data can be transmitted accurately. Thoughtful pin placement can assist in managing heat within the device. Ensuring that power and ground pins are strategically located can help dissipate heat effectively. Well-planned pin placement enhances the reliability of the electronic system by reducing the risk of signal issues, overheating, and manufacturing errors.  

### <a name="3-2-performing-floor-planning-and-placement-in-openlane"></a> 3.2 Performing Floor Planning and Placement in OpenLane ###

    run_floorplan

![image](https://github.com/V-Pranathi/Advanced_Physical_Design/assets/140998763/10af4ad1-c38d-42b1-b0cf-b8b6377d56b3)  

Viewing the floorplan in magic:

      $ cd /home/pranathi/OpenLane/designs/picorv32a/runs/RUN_2023.09.12_16.49.43/results/floorplan
      magic -T ~/vsdstdcelldesign/libs/sky130A.tech lef read ../../tmp/merged.nom.lef def read picorv32.def &

![image](https://github.com/V-Pranathi/Advanced_Physical_Design/assets/140998763/9f94fafb-faad-4b3c-b7ce-8d0c11f375d1)

Zooming in we can see various components:

![image](https://github.com/V-Pranathi/Advanced_Physical_Design/assets/140998763/caabe03d-aa92-4634-8135-ed025150fb79)

    run_placement
    
![image](https://github.com/V-Pranathi/Advanced_Physical_Design/assets/140998763/dbf97265-5520-4e2a-88c8-831a52c1a923)  

Viewing the placement in magic:

     $ cd /home/pranathi/OpenLane/designs/picorv32a/runs/RUN_2023.09.16_04.23.58/results/placement
     magic -T ~/vsdstdcelldesign/libs/sky130A.tech lef read ../../tmp/merged.nom.lef def read picorv32.def &

![image](https://github.com/V-Pranathi/Advanced_Physical_Design/assets/140998763/504614c8-3525-4b7e-b612-ff8d67ee93b3)  

### <a name="3-3-cell-design-and-characterization-flows"></a> 3.3 Cell design and characterization flows ###  

Each cell that is placed on the layout is referred to as standard cell. Standard cells are pre-designed and pre-characterized logic gates, flip-flops, latches, and other digital components for which the definition is available in libraries.

**Standard Cell Design Flow**

_Standard cell design flow involves the following:_

  1. Inputs: PDKs, DRC & LVS rules, SPICE models, libraries, user-defined specifications
  2. Design steps: Circuit design, Layout design (Art of layout Euler's path and stick diagram), Extraction of parasitics, Characterization (timing, noise, power)
  3. Outputs: CDL (circuit description language), LEF, GDSII, extracted SPICE netlist (.cir), timing, noise and power .lib files

**Standard Cell Characterization Flow**

Characterization refers to the process of gathering and analyzing electrical and performance data for a specific cell or library element. The goal of characterization is to provide accurate and comprehensive information about how the cell behaves under various operating conditions. This information is essential for designing and optimizing digital circuits using these cells.

_A typical standard cell characterization flow includes the following steps:_

  1. Read in the models and tech files
  2. Read extracted spice netlist
  3. Recognise behaviour of the cell
  4. Read the subcircuits
  5. Attach power sources
  6. Apply stimulus to characterization setup
  7. Provide necessary output capacitance loads
  8. Provide necessary simulation commands the opensource software called **GUNA** can be used for characterization. Steps 1-8 are fed into the GUNA software which generates timing, noise and power models.

### <a name="3-4-general-timing-characterization-parameters"></a> 3.4 General timing characterization parameters ###

_Timing threshold Definitions_

| Timing defintion |	Value |
| ---------------- | ----- |
| slew_low_rise_thr |	20% value |
| slew_high_rise_thr |	80% value |
| slew_low_fall_thr |	20% value |
| slew_high_fall_thr |	80% value |
| in_rise_thr |	50% value |
| in_fall_thr |	50% value |
| out_rise_thr |	50% value |
| out_fall_thr |	50% value |

#### Propagation Delay and Transition time ####
_Propagation Delay_

Propagation delay refers to the time it takes for a change in an input signal to reach 50% of its final value to produce a corresponding change in the output signal to reach 50% of its final value of a digital circuit.  
     
    rise delay =  time(out_fall_thr) - time(in_rise_thr)

_Transition time_

Transition time refers to the time it takes for a digital signal to change its voltage level from one logic state (e.g., logic low or 0) to another logic state (e.g., logic high or 1) or vice versa. Transition time is typically measured as the time interval between the moment when the signal voltage reaches a specific percentage (e.g., 10% to 90% or 20% to 80%) of its final value during a voltage transition and the moment when it reaches the opposite percentage during the subsequent transition.  

    Fall transition time: time(slew_high_fall_thr) - time(slew_low_fall_thr)
    Rise transition time: time(slew_high_rise_thr) - time(slew_low_rise_thr)

## <a name="4-day-3---design-library-cell-using-magic-layout-and-ngspice-characterization"></a> 4.Day 3 - Design library cell using Magic Layout and ngspice characterization ##
### <a name="4-1-cmos-inverter-ngspice-simulations"></a> 4.1 CMOS inverter ngspice simulations ###
**SPICE Deck creation and simulation for CMOS Inverter**:

1. SPICE deck = component connectivity (basically a netlist) of the CMOS inverter.  
2. SPICE deck values = value for W/L (0.375u/0.25u means width is 375nm and lengthis 250nm). PMOS should be wider in width(2x or 3x) than NMOS. The gate and supply voltages are normally a multiple of length (in the example, gate voltage can be 2.5V)  
3. Add nodes to surround each component and name it. This will be used in SPICE to identify a component.  

_Notes:_
* Width is the length of source and drain. Length is the distance between source and drain.  
* PMOS hole carrier is slower than NMOS electron carrier mobility, so to match the rise and fall time PMOS must be thicker (less resistance thus higher mobility) than NMOS.

**SPICE Deck netlsit description**

   ![image](https://github.com/V-Pranathi/Advanced_Physical_Design/assets/140998763/ae6df423-467a-40a1-9f20-097819461902)

    ***syntax for PMOS and NMOS desription***
    [component name] [drain] [gate] [source] [substrate] [transistor type] W=[width] L=[length]

     ***simulation commands***
    .op --- is the start of SPICE simulation operation where Vin will be sweep from 0 to 2.5 with 0.5 steps
    tsmc_025um_model.mod  ----  model file containing the technological parameters for the 0.25um NMOS and PMOS 

The steps to simulate in SPICE:

    source [filename].cir
    run
    setplot 
    dc1 
    plot out vs in 

![image](https://github.com/V-Pranathi/Advanced_Physical_Design/assets/140998763/705aa94d-4ffc-4bc9-86a7-2de65c0d2fd0)

**Switching Threshold Vm**
_CMOS robustness depends on:_

1. Switching threshold = Vin is equal to Vout. This the point where both PMOS and NMOS is in saturation or kind of turned on, and leakage current is high. If PMOS is thicker than NMOS, the CMOS will have higher switching threshold (1.2V vs 1V) while threshold will be lower when NMOS becomes thicker.  
2. At this point, both the transistors are in saturation region, means both are turned on and have high chances of current flowing driectly from VDD to Ground called Leakage current.

DC transfer analysis is used for finding switching threshold. SPICE DC analysis below uses DC input of 2.5V. Simulation operation is DC sweep from 0V to 2.5V by 0.05V steps:  

    Vin in 0 2.5
    *** Simulation Command ***
    .op
    .dc Vin 0 2.5 0.05

Below is the result of SPICE simulation for DC analysis, the line intersection is the switching threshold:  

![image](https://github.com/V-Pranathi/Advanced_Physical_Design/assets/140998763/889560e3-a02b-47f4-99a4-ffeb6930a93c)

Meanwhile, transient analysis is used for finding propagation delay. SPICE transient analysis uses pulse input:

   1. starts at 0V
   2. ends at 2.5V
   3. starts at time 0
   4. rise time of 10ps
   5. fall time of 10ps
   6. pulse-width of 1ns
   7. period of 2ns

      ![image](https://github.com/V-Pranathi/Advanced_Physical_Design/assets/140998763/bbf732fe-607c-46f9-a927-9a5d0f839ef8)

The simulation operation has 10ps step and ends at 4ns:

    Vin in 0 0 pulse 0 2.5 0 10p 10p 1n 2n 
    *** Simulation Command ***
    .op
    .tran 10p 4n

Below is the result of SPICE simulation for transient analysis:

![image](https://github.com/V-Pranathi/Advanced_Physical_Design/assets/140998763/f6ed8ea7-243e-445f-b476-1995afd7f808)

#### Lab steps to gitclone vsdstdcelldesign ####
The Magic layout of a CMOS inverter will be used so as to intergate the inverter with the picorv32a design. To do this, inverter magic file is sourced from vsdstdcelldesign by cloning it within the home/OpenLane directory as follows:  

    git clone https://github.com/nickson-jose/vsdstdcelldesign

This creates a vsdstdcelldesign named folder in the openlane directory.  
Now, we can view the layout of inverter in magic using the below command:  

    magic -T libs/sky130A.tech sky130_inv.mag &

Ampersand at the end makes the next prompt line free, otherwise magic keeps the prompt line busy. Once we run the magic command we get the layout of the inverter in the magic window. The layout shown in magic is as below:

![image](https://github.com/V-Pranathi/Advanced_Physical_Design/assets/140998763/483e1de1-481c-4379-b981-b02356102c91)

### <a name="4-2-inception-of-layout-cmos-fabrication-process"></a> 4.2 Inception of Layout CMOS fabrication process ###  
#### 16 Mask CMOS Fabrication ####
The 16-mask CMOS process consists of the following steps:

1. _Selection of subtrate:_ Secting the body/substrate material.

![image](https://github.com/V-Pranathi/Advanced_Physical_Design/assets/140998763/745d9f5f-6cd3-46bb-966a-8f96b24ab116)

2. _Creating active region for transistors:_ Isolation between active region pockets by SiO2 and Si3N4 deposition followed by photolithography and etching.

![image](https://github.com/V-Pranathi/Advanced_Physical_Design/assets/140998763/9e88b152-8ad7-4c8a-a5d8-8cb6cf1d4e72)

  
3. _N-well and P-well formation:_ Ion implanation by Boron for P-well and by Phosphorous for N-well formation.

![image](https://github.com/V-Pranathi/Advanced_Physical_Design/assets/140998763/95fec549-d976-4193-a2c8-35def255a559)

4. _Formation of gate terminal:_ NMOS and PMOS gates formed by photolithography techniques.

![image](https://github.com/V-Pranathi/Advanced_Physical_Design/assets/140998763/35834db4-2557-44f1-8080-24e0dfadf9f5)

5. _LDD (lightly doped drain) formation:_ LDD formed to prevent hot electron effect.

  ![image](https://github.com/V-Pranathi/Advanced_Physical_Design/assets/140998763/3c4e17af-daba-4488-8e0f-df1b4fbb028b)

6. _Source & drain formation:_Screen oxide added to avoid channelling during implants followed by Aresenic implantation and annealing.

![image](https://github.com/V-Pranathi/Advanced_Physical_Design/assets/140998763/452abb4e-44bd-4a83-8b8f-cdea1584f441)

7. _Local interconnect formation:_ Removal of screen oxide by HF etching. Deposition of Ti for low resistant contacts.

![image](https://github.com/V-Pranathi/Advanced_Physical_Design/assets/140998763/d5580a95-d9a2-470f-9555-3a989d031858)

8. _Higher level metal formation:_ CMP for planarization followed by TiN and Tungsten deposition. Top SiN layer for chip protection.

![image](https://github.com/V-Pranathi/Advanced_Physical_Design/assets/140998763/1074b65d-7961-4bf5-8199-e05f345ea0b5)

##### The 16 masks used in the above process are: #####

* **Substrate Mask (Mask 1):** This mask defines the active regions on the silicon wafer where transistors and other devices will be formed. It specifies the boundaries of the N-well and P-well regions.  
* **Threshold Voltage Adjustment Mask (Mask 2):** This mask adjusts the threshold voltage of the transistors by defining the regions where threshold voltage implants are required.  
* **Gate Oxide Mask (Mask 3):** This mask defines the areas where gate oxide will be grown or deposited. The gate oxide acts as an insulator between the gate electrode and the silicon substrate.  
* **Poly-Silicon Gate Mask (Mask 4):** This mask defines the gate electrodes for both N-channel and P-channel transistors. It outlines the shape of the gates.  
* **N+ and P+ Diffusion Masks (Masks 5 and 6):** These masks define the source and drain regions for the N-channel and P-channel transistors, respectively. These regions are typically doped with impurities to create the necessary electrical characteristics.  
* **Contact Mask (Mask 7):** This mask defines the openings for contacts, which allow the metal layers to connect to the underlying silicon.  
* **First Metal Layer Mask (Mask 8):** This mask defines the first layer of metal interconnects that connect various components on the chip, such as transistors and contacts.  
* **Interlayer Dielectric (ILD) Mask (Mask 9):** This mask defines the dielectric material that insulates metal layers from each other. It also specifies the locations of vias for vertical connections.  
* **Via Mask (Mask 10):** This mask defines the openings in the ILD layer for vias, which enable vertical connections between metal layers.  
* **Second Metal Layer Mask (Mask 11):** This mask defines the second layer of metal interconnects, which connect to the underlying metal layer and vias.  
* **Barrier Layer Mask (Mask 12):** This mask defines layers used to improve adhesion between metal and dielectric, enhancing the reliability of the interconnects.  
* **Third Metal Layer Mask (Mask 13):** This mask defines the third layer of metal interconnects, which can connect to the lower metal layers through vias.  
* **Passivation Layer Mask (Mask 14):** This mask defines the protective passivation layer that covers the entire chip, protecting it from external factors and contamination.  
* **Bond Pad Mask (Mask 15):** This mask defines the locations of bond pads, which are used for external electrical connections and testing.  
* **Test Structure Mask (Mask 16):** This mask includes various test structures used for quality control, testing, and characterization during manufacturing.   

##### Basic layers layout and LEF using inverter #####

* From Layout, we see the layers which are required for CMOS inverter. Inverter is, PMOS and NMOS connected together.  
* Gates of both PMOS and NMOS are connected together and fed to input(here ,A), NMOS source connected to ground(here, VGND), PMOS source is connected to VDD(here, VPWR), Drains of PMOS and NMOS are connected together and fed to output(here, Y). The First layer in skywater130 is localinterconnect layer(locali) , above that metal 1 is purple color and metal 2 is pink color. If you want to see connections between two different parts, place the cursor over that area and press S one times. The tkson window gives the component name.  

 ![image](https://github.com/V-Pranathi/Advanced_Physical_Design/assets/140998763/69619408-1460-4e91-9d3d-b0bde28a9e1d)
 
 ### <a name="4-3-sky130-tech-file-labs"></a> 4.3 Sky130 Tech File Labs ###  

 Spice Extraction : Use the below commands in tkcon to achieve .mag to .spice extraction:  
 
* Make an extract file .ext by typing _extract all_ in the tkon terminal.  
* Extract the .spice file from this ext file by typing _ext2spice cthresh 0 rthresh 0_then _ext2spice_ in the tcon terminal.  

 ![image](https://github.com/V-Pranathi/Advanced_Physical_Design/assets/140998763/e4fdf4c4-e4e2-4db0-97ff-00a216afad25) 

 ![image](https://github.com/V-Pranathi/Advanced_Physical_Design/assets/140998763/c1b0e441-cb77-4ab9-85f0-fee96c876e87)

 We then modify the spice file to be able to plot a transient response:  

 ![image](https://github.com/V-Pranathi/Advanced_Physical_Design/assets/140998763/1d3e3663-cbab-42c8-a4b6-42910a9de090)

 ![image](https://github.com/V-Pranathi/Advanced_Physical_Design/assets/140998763/1f34c343-ec28-4138-bebd-bc1036c3687e)

Using this transient response, we will now characterize the cell's slew rate and propagation delay:  

* Rise Transition : 2.25421 - 2.18636 = 0.006785 ns/67.85ps  
* Fall Transition : 4.09605 - 4.05554 = 0.04051ns/40.51ps  
* Cell Rise Delay : 2.21701 - 2.14989 = 0.06689ns/66.89ps  
* Cell Fall Delay : 4.07816 - 4.05011 = 0.02805ns/28.05ps

##### Magic Tool options and DRC Rules #####
The technology file is a setup file that declares layer types, colors, patterns, electrical connectivity, DRC, device extraction rules and rules to read LEF and DEF files. Magic layouts can be sourced from opencircuitdesign.com using the command:  
 
    wget http://opencircuitdesign.com/open_pdks/archive/drc_tests.tgz
    tar xfz drc_tests.tgz

once extraction is done, drc_tests file is created and you will have all the information about magic layout for this lab exercise. The _.magicrc_ loads locally the tech file required by the user. Since this file sets up the tech file, sky130.tech need not be mentioned in the command used to invoke Magic. Hence Magic can be invoked more conveniently:

     magic -d XR

![image](https://github.com/V-Pranathi/Advanced_Physical_Design/assets/140998763/e0b9634e-ed2f-4fa1-b3c1-5fd2f028077d)

First load the poly file by _load poly.mag_ on tkcon window.

Finding the error by mouse cursor and find the box area, Poly.9 is violated due to spacing between polyres and poly.

First load the poly file by load poly.mag on tkcon window.

![image](https://github.com/V-Pranathi/Advanced_Physical_Design/assets/140998763/dff048a8-b1fa-451c-ade9-31c05f581ee1)

We find that distance between regular polysilicon & poly resistor should be 22um but it is showing 17um and still no errors . We should go to _sky130A.tech_ file and modify as follows to detect this error.  

![image](https://github.com/V-Pranathi/Advanced_Physical_Design/assets/140998763/eebb5ad6-72b2-4b2a-9450-ef64409f4e19)

Changes to be made are:

        spacing npres allpolynonres 480 touching_illegal \
	       "poly.resistor spacing to N-tap < %d (poly.9)"

        spacing xhrpoly,uhrpoly,xpc allpolynonres 480 touching_illegal \
	       "xhrpoly/uhrpoly resistor spacing to diffusion < %d (poly.9)"

![image](https://github.com/V-Pranathi/Advanced_Physical_Design/assets/140998763/ad50293d-f13b-4fd0-8b32-91e3a09e803d)







 

## <a name="references"></a> References ##

* https://openlane.readthedocs.io/en/latest/
* https://woset-workshop.github.io/PDFs/2020/a21.pdf
* https://github.com/Devipriya1921/Physical_Design_Using_OpenLANE_Sky130#components-of-opensource-digital-asic-design
* https://github.com/Rachana-Kaparthi/
* https://github.com/aasthadave9/Advanced-Physical-Design-Using-OpenLANE-Sky130/
* https://github.com/nickson-jose/vsdstdcelldesign/
* http://opencircuitdesign.com/magic/
* https://skywater-pdk.readthedocs.io/en/main/








  






