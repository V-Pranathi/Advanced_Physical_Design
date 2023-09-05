# Advanced_Physical_Design
Advanced Physical Design using OpenLANE/Sky130 
## Table of Contents
* [1.Introduction](#1-introduction)
* [2.Day 1 - Inception of open-source EDA, OpenLANE and Sky130 PDK](#day-1---inception-of-open-source-eda--openlane-and-sky130-pdk)
  * [2.1 Fundamental terms](#2-1-fundamental-terms)
  * [2.2 SoC design and OpenLANE](#2-2-soc-design-and-openlane)
## <a name="1-introduction"></a> 1.Introduction ##  
Electronic Design Automation (EDA) tools are essential for designing and simulating electronic circuits and systems. There are several open-source EDA tools available that provide various functionalities for electronic design.   
OpenLane is an open-source digital ASIC (Application-Specific Integrated Circuit) design flow that utilizes various open-source EDA (Electronic Design Automation) tools and resources to automate the design process for semiconductor manufacturing. OpenLane provides a complete RTL-to-GDSII (Register-Transfer Level to Graphic Data System II) design flow for digital ASICs. It leverages open-source EDA tools like Yosys for synthesis, ABC for technology mapping, and Magic for physical design. OpenLane streamlines the process of designing digital ASICs.  
SkyWater Technology Foundry offers semiconductor manufacturing services, including access to their 130nm technology node. This means that designers who have completed their ASIC designs using OpenLane can potentially use SkyWater's manufacturing services to produce physical chips based on their designs.  

## <a name="day-1---inception-of-open-source-eda--openlane-and-sky130-pdk"></a> 2.Day 1 - Inception of open-source EDA, OpenLANE and Sky130 PDK
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

![image](https://github.com/V-Pranathi/Advanced_Physical_Design/assets/140998763/9eba812f-2efd-47ab-b1b8-c47aa3e037de)

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

* Synthesis  
* Floor/Power Planning
* Placement  
* Clock Tree Synthesis
* Routing
* Signoff
  






