# ADVANCED PHYSICAL DESIGN USING OPENLANE/SKY130
# Table Of Contents
## DAY 1
**Inception of Open-Source EDA, OpenLane and Sky130 PDK**
+ [How to Talk to Computers](#how-to-talk-to-computers)
+ [SoC Design and OpenLane](#soc-design-and-openlane)
+ [Open-Source EDA Tools](#open-source-eda-tools)
  
## DAY 2
**Good Floorplan vs Bad Floorplan and Introduction to Library Cells**
+ [Chip Floorplanning Considerations](#chip-floorplanning-considerations)
+ [Library Binding and Placement](#library-binding-and-placement)
+ [Cell Design and Characterization Flow](#cell-design-and-characterization-flow)
+ [General Timing Characterisation Parameters](#general-timing-characterisation-parameters)

## DAY 3
**Design Library Cell using Magic Layout and Ngspice Characterisation**
+ [Labs for CMOS Inverter Ngspice Simulations](#labs-for-cmos-inverter-ngspice-simulations)
+ [Inception of Layout](#inception-of-layout)
+ [Sky130 Tech File Labs](#sky130-tech-file-labs)
  
## DAY 4
**Pre-Layout Timing Analysis and importance of Good CLock Tree**
+ [Timing Modelling Using Delay Tables](timing-modelling-using-delay-tables)
+ [Timing Analysis with Ideal Clocks using openSTA](#timing-analysis-with-ideal-clocks-using-opensta)
+ [Clock Tree Synthesis TritonCTS and Signal Integrity](#clock-tree-synthesis-tritoncts-and-signal-integrity)
+ [Timing Analysis with Real Clocks using OpenSTA](#timing-analysis-with-real-clocks-using-opensta)

## DAY 5
**Final Steps for RTL2GDS**
+ [Routing and DRC](#routing-and-drc)
+ [Power Distribution Network and Routing](#power-distribution-network-and-routing)

# Day-1
## How to Talk to Computers
<details>
<summary> Introduction to QFN-48 Package, Chip, Pads, Core, Die and IPs </summary>  

**Arduino Board**
+ An Arduino board is a microcontroller-based development platform that allows you to create and prototype a wide range of electronics projects.
<p align='center'>

  ![image](https://github.com/KushalR17/pes_pd/assets/142383052/1c0e7e3d-90f0-42e3-9fc9-0c2d4ecb4f1e)

</p>
<p align="center">
  Fig 1. Typical Design of Arduino Board
</p>

**QFN-48 Package**
+ QFN-48 stands for **Quad Flat No-Leads 48**, which is a type of surface-mount integrated circuit (IC) package.
+ QFN packages are commonly used in electronics to house integrated circuits, microcontrollers, and other semiconductor devices.
+ The **48** in QFN-48 refers to the number of pins or leads on the package.
<p align="center">
![image](https://github.com/KushalR17/pes_pd/assets/142383052/ad7c0f19-d1ac-4d9e-83e1-fc7a58ca4921)

</p>
<p align="center">
  Fig 2. QFN-48 Package
</p>

**Chip**
+ In electronics and technology, a **chip** typically refers to a semiconductor device, which is a small piece of silicon that contains integrated circuits.
+ These chips can be microprocessors, memory chips, sensors, or other electronic components. For example, a microprocessor chip is the **brain** of a computer.

<p align="center">
![image](https://github.com/KushalR17/pes_pd/assets/142383052/2c806a74-5f78-4601-a197-ea3556e61aa8)

</p>
<p align="center">
  Fig 3. Chip
</p>

**Pads**
+ They refer to the areas on the chip's surface where electrical connections can be made.
+ These pads are typically metalized areas with a specific pattern that allows for the attachment of wires, leads, or other components to create electrical connections.
+ Pads serve as the interface between the internal circuitry of the chip and the external world, such as a printed circuit board (PCB) or other devices.

**Die**
+ It refers to a small, usually rectangular, piece of a semiconductor wafer that contains a single integrated circuit (IC) or microchip.
+ During the manufacturing process, multiple ICs are fabricated on a single semiconductor wafer, and each individual IC is referred to as a "die."
+ After manufacturing, these dies are typically cut from the wafer and then packaged into separate integrated circuits for use in electronic devices.

**Core**
+ It refers to a processing unit within the chip that can independently execute instructions and perform computations.
+ These cores are often referred to as **CPU cores**.
+  A chip may contain one or multiple CPU cores, each capable of running its own set of instructions and performing tasks concurrently.
+  The presence of multiple cores on a single chip is known as **multi-core processing**.

<p align="center">
![image](https://github.com/KushalR17/pes_pd/assets/142383052/942e89bb-cc43-4032-8976-019c47d59ba3)

</p>
<p align="center">
  Fig 4. Sample RISC-V SoC
</p>

**IPs**
+ It stands for **Intellectual Property** or **IP blocks**.
+ These are pre-designed and pre-verified functional blocks or components that are often licensed or acquired from third-party companies and integrated into a chip's design.
+ IPs help semiconductor companies save time and resources by incorporating well-tested and specialized functionality into their chips, rather than designing everything from scratch.

**Foundry**
+ It refers to a specialized manufacturing facility that produces semiconductor devices, integrated circuits (ICs), and other microelectronic components on behalf of other companies.
+ These facilities are also commonly known as semiconductor fabrication plants or fabs.

**Macros**
+ They refer to pre-designed and pre-verified blocks of logic or functional circuits that are often used for specific tasks within a chip's design.
+ Macros are similar to Intellectual Property (IP) blocks but are typically larger and more complex.
+ They are used to provide standardized, reusable, and well-optimized functionality within a chip.

</details>

<details>
<summary> Introduction to RISC-V </summary> 

**ISA (Instruction Set Architecture)**
+ ISA defines the interface between a computer's hardware and its software, specifically how the processor and its components interact with the software instructions that drive the execution of tasks.

**RISC-V (Reduced Instruction Set Computing - Five)**
+ It is an open-source Instruction Set Architecture (ISA) that has gained significant attention and adoption in the world of computer architecture and semiconductor design.
+ RISC architectures simplify the instruction set by focusing on a smaller set of instructions, each of which can be executed in a single clock cycle. This approach usually leads to faster execution of individual instructions. 

<p align="center">
![image](https://github.com/KushalR17/pes_pd/assets/142383052/a133c1a5-759e-4eb4-8a50-b3d67043fdd5)

</p>
<p align="center">
  Fig 5. Design Flow
</p>
</details>

<details>
<summary> From Software Applications to Hardware </summary>  

1. **Apps:** Application software, often referred to simply as **applications** or **apps**, is a type of computer software that is designed to perform specific tasks or functions for end-users.
2. **System software:** System software refers to a category of computer software that acts as an intermediary between the hardware components of a computer system and the user-facing application software. It provides essential services, manages hardware resources, and enables the execution of application programs. System software plays a critical role in maintaining the overall functionality, security, and performance of a computer system.'
3. **Operating System:** The operating system is a fundamental piece of software that manages hardware resources and provides various services for both users and application programs. It controls tasks such as memory management, process scheduling, file system management, and user interface interaction. Examples of operating systems include Microsoft Windows, macOS, Linux, and Android.
4. **Compiler:** A compiler is a type of software tool that translates high-level programming code written by developers into assembly-level language.
5. **Assembler:** An assembler is a software tool that translates assembly language code into machine code or binary code that can be directly executed by a computer's processor.
6. **RTL:** RTL serves as an abstraction level in the design process that represents the behavior of a digital circuit in terms of registers and the operations that transfer data between them.
7. **Hardware:** Hardware refers to the physical components of a computer system or any electronic device. It encompasses all the tangible parts that make up a computing or electronic device and enable it to perform various tasks.

</details>

## SoC Design and OpenLane
<details>
<summary> Introduction to all Components of Open-Source Digital ASIC Design </summary>  

**PDK**
+ It stands for **Process Design Kit**.
+  It is a collection of files, models, documentation, and tools that are provided by semiconductor foundries to assist integrated circuit (IC) designers in creating and verifying their designs for a specific semiconductor manufacturing process.
+  PDKs are essential for the design and development of semiconductor chips because they provide the necessary information and resources for designers to create circuits that are compatible with the foundry's fabrication process.

**EDA Tools**
+ EDA (Electronic Design Automation) tools are a set of software applications and tools used by electronics engineers and integrated circuit (IC) designers to design, simulate, verify, and analyze electronic circuits and systems.
+ These tools are essential for designing complex electronic devices, ranging from simple integrated circuits to advanced microprocessors and systems-on-chip (SoCs).
<p align="center">
![image](https://github.com/KushalR17/pes_pd/assets/142383052/30e72250-326b-441e-8343-e065cf671534)

</p>

**130nm**
+ It refers to a semiconductor manufacturing process technology node, which represents the minimum feature size or transistor gate length in that technology.
+ In semiconductor manufacturing, the feature size is a critical metric because it determines the size and performance characteristics of the transistors and other components that make up integrated circuits (ICs).

</details>

<details>
<summary> Simplified RTL2GDS Flow </summary>

**RTL to GDSII**
<p align="center">
![image](https://github.com/KushalR17/pes_pd/assets/142383052/aea3f8af-2dc9-4381-b9a4-4fb0c97828b9)

</p>
<p align="center">
  Fig 1. Simplified RTL to GDS FLow
</p>

+ **Synthesis**
  -  It refers to the process of converting a high-level hardware description into a gate-level representation that can be implemented on a specific hardware technology or semiconductor manufacturing process.
 
+ **Floor and Power Planning**
  - **Chip floor planning** is the process of partitioning the chip die between different system building blocks and place the I/O pads.
  - **Macro floor planning**, also known as block-level floor planning, is a specific aspect of chip floor planning that focuses on the arrangement and organization of large functional blocks or macros within an integrated circuit (IC) design. 
  - **Power planning** also known as power distribution network (PDN) design, focuses on the distribution and management of power and ground connections within the chip. It ensures that all components receive stable power supplies and that power is efficiently distributed throughout the chip.
 
+ **Placment**
  - It refers to the process of determining the physical locations of individual components, such as logic gates, flip-flops, memory cells, and other elements, on the semiconductor die or chip.
  - **Global placement** involves determining approximate positions or locations for major functional blocks, macros, and components on the semiconductor die or chip.
  - **Detailed placement** also known as fine-grained placement, is a critical step in the physical design of integrated circuits (ICs) that follows global placement.
During the detailed placement phase, the positions of individual components, such as logic gates, flip-flops, and memory cells, are determined with high precision within the semiconductor die or chip.

+ **Clock Tree Synthesis**
  - It involves the generation and optimization of a hierarchical tree-like network of clock distribution that ensures synchronized clock signals are delivered efficiently to all flip-flops and other clocked elements within the chip.
  - The primary objectives of clock tree synthesis are to minimize clock skew, reduce clock routing congestion, and meet strict timing requirements.

+ **Routing**
  - It is responsible for establishing the electrical connections (wires or metal traces) that allow signals to flow between different parts of the chip.
  - **Global routing**, also known as channel routing, is the first phase of routing. It determines the general paths for wires that connect different components or blocks on the chip. Global routers aim to minimize the total wirelength while adhering to design rules and constraints.
  - Once the general paths are defined in global routing, detailed routing comes next.
  - **Detailed routing** focuses on individual nets (connections) and determines the specific paths and wires that connect the pins of components or gates. It also resolves any conflicts or overlaps between wires.

+ **Sign-Off**
  - It signifies the point at which a specific design step or aspect has been completed, reviewed, and verified to meet certain criteria or standards.
  - **Physical Verification** sign-off phase covers a range of physical design verification checks, including DRC, LVS, and other manufacturing-oriented checks. It ensures that the design is ready for manufacturing and fabrication.
  - **Timing verification** sign-off specifically focuses on verifying that the chip meets the required timing constraints, such as setup and hold times, clock-to-q delays, and maximum clock frequency. This involves detailed timing analysis and simulation to ensure that the design operates correctly at the specified clock frequencies.
</details>

<details>
<summary> Introduction to OpenLane and Strive Chipsets </summary>

+ **OpenLane**
  - OpenLANE is an opensource tool or flow used for opensource tape-outs.
  - The OpenLANE flow comprises a variety of tools such as Yosys, ABC, OpenSTA, Fault, OpenROAD app, Netgen and Magic which are used to harden chips and macros, i.e. generate final GDSII from the design RTL. The primary goal of OpenLANE is to produce clean GDSII with no human intervention.
  -  OpenLANE has been tuned to function for the Google-Skywater130 Opensource Process Design Kit.

+ **Strive Chipsets**
<p align="center">
![image](https://github.com/KushalR17/pes_pd/assets/142383052/e298e352-9ee8-497f-99c6-dafd88a75723)

</p>
<p align="center">
  Fig 2. Strive SoC Family
</p>

</details>

<details>
<summary> Introduction to OpenLane Detailed ASIC Design Flow </summary>


<p align="center">
<img src="https://github.com/Veda1809/pes_pd/assets/142098395/e1f77b82-98df-43e2-b304-cfe6ec7276b1">
</p>

<p align="center">
  Fig 3. OpenLane ASIC Flow
</p>

+ **OpenLane Regression Testing**
 - Regression testing in the context of OpenLane refers to the process of running a set of predefined test cases or scripts on the OpenLane design automation framework to ensure that recent changes or updates to the framework have not introduced new bugs or regressions.

+ **Design for Test(DFT)**
  - Scan Insertion
  - Automatic Test Pattern Generation(ATPG)
  - Test Patterns Compaction
  - Fault Coverage
  - Fault Simulation
  
+ **Physical Implementation** (automated PnR(Place and Route)) (OpenRoad)
  - Floor/Power Planning
  - End Decoupling Capacitors and Tap cells insertion
  - Placement : Global and Detailed
  - Post placmnet optimisation
  - Clock Tree synthesis
  - Routing : Global and Detailed

 + **Logic Equivalence Check** (yosys)
   - Everytime the netlist is modified, verification must be performed.
   - LEC is used to formally confirm that the function did not change after modifying the netlist.
  
+ **Dealing with antenna rules violations**
  - When a metal wire segment is fabricated, it can act as an antenna.
  - Reactive ion etching causes charge to accumulate on the wire.
  - Transistor gates can be damaged during fabrication.
  - Two solutions:
    + Bridging attaches a higher layer intermediary.
    + Add antenna diode cell to leak away charges.
  - We took a preventive approach:
    + Add a fake antenna diode next to every cell input after placement.
    + Run the antenna check(**Magic**) on the routed layout
    + If the checker reports a violation on the cell input pin, replace the fake diode cell by a real one.
   
+ **Static Timing Analysis**
  - Static Timing Analysis (STA) is a critical step in the design and verification of integrated circuits (ICs) and other digital systems.
  - It is used to ensure that a digital design meets its required timing constraints and operates correctly within a given clock frequency.
  - STA is performed during the physical design phase of chip development and is crucial for assessing and optimizing the performance and reliability of digital systems.
 
+ **Physical Verification**
  - **LVS (Layout vs Schematic)** is a process that ensures that the physical layout of a chip or circuit matches its intended logical or schematic representation.
  - **DRC (Design Rules Checking)** is a process that ensures that the layout of a chip or circuit adheres to the specific design rules and constraints defined by the semiconductor manufacturing process. 
  - **Magic** is used for design rules checking and SPICE extraction from Layout.
  - **Magic** and **Netgen** are used for LVS

</details>

## Open-Source EDA Tools
<details>
<summary> OpenLane Directory Structure </summary>

+ PDK used in this workshop is Skywater 130nm PDK and OpenLane is built around this PDK.
<p align="center">'
![image](https://github.com/KushalR17/pes_pd/assets/142383052/9fcb8945-bdde-424b-9647-2ee7b970593d)

</p>
<p align="center">
  Fig 1.
</p>

+ **skywater-pdk** contains all the PDK related files.
+ **open_pdks** contains all the scripts and files that convert these foundry level PDKS to be compatible with the open-source EDA tools
+ **sky130A** is made compatible with our open-source environment.

<p align="center">
![image](https://github.com/KushalR17/pes_pd/assets/142383052/94356872-5afe-4877-b5d5-55ab46f0100b)

</p>
<p align="center">
  Fig 2.
</p>

+ **libs.ref** seems specific to technology.
+ **libs.tech** seems specific to the tool.

<p align="center">
![image](https://github.com/KushalR17/pes_pd/assets/142383052/4158a23b-56b9-400b-8010-f4f750e1524f)

</p>
<p align="center">
  Fig 3.
</p>

+ **sky130_fd_sc_hd** has all the technology files.
</details>

<details>
<summary> Design Preparation Step </summary>

+ To invoke OpenLane

<p align="center">
![image](https://github.com/KushalR17/pes_pd/assets/142383052/a37b2c5d-b782-4085-b000-f401ed60296b)

</p>
<p align="center">
  Fig 4.
</p>

+ Under **Designs** folder, we are going to use **picorv32a**.
+ **src** files contains verilog and sdc file.
<p align="center">
![image](https://github.com/KushalR17/pes_pd/assets/142383052/8ee769de-7b78-49ee-943c-dd6d2c77b578)

</p>
<p align="center">
  Fig 5.
</p>

+ `less config.tcl`

<p align="center">
![image](https://github.com/KushalR17/pes_pd/assets/142383052/3cbaa2b6-1f6f-47f8-8c70-8005efea9c7d)

</p>
<p align="center">
Fig 6.
</p>

+ We are going to prepare the design.
+ `prep -design picorv32a`.
<p align="center">
![image](https://github.com/KushalR17/pes_pd/assets/142383052/4064f927-865d-4b57-ad1c-713fa5c47cf4)

</p>
<p align="center">
  Fig 7.
</p>
</details>

<details>
<summary> Review Files after Design Prep and Run Synthesis </summary>

<p align="center">
![image](https://github.com/KushalR17/pes_pd/assets/142383052/0e4b6b2d-9373-4024-968f-64ab5b3ba5b8)

</p>
<p align="center">
  Fig 8.
</p>

<p align="center">
![image](https://github.com/KushalR17/pes_pd/assets/142383052/4231df97-d912-4e2a-8b49-c97e2781cd8b)

</p>
<p align="center">
  Fig 9.
</p>

<p align="center">
![image](https://github.com/KushalR17/pes_pd/assets/142383052/f99fd36a-3e88-45c8-853b-3ad569b7db63)

</p>
<p align="center">
  Fig 10.
</p>

+ `%run_synthesis`

<p align="center">
![image](https://github.com/KushalR17/pes_pd/assets/142383052/04f2cfe7-61cc-4406-baed-9f589e005ea2)

</p>
<p align="center">
  Fig 11.
</p>

</details>

<details>
<summary> OpenLane Project Git Link Description </summary>  

+ To know more about openlane

https://github.com/efabless/openlane2

</details>

<details>
<summary> Steps to Characterize Synthesis Results </summary>

+ To calculate the clock ratio, we need
  - the number of D Flipflops = 1613
  - the number of cells = 14876
+ The clock ratio is dff/cells = 0.108
<p align="center">
![image](https://github.com/KushalR17/pes_pd/assets/142383052/1d982d90-182e-4ea9-8d24-17d96531ab3a)

</p>
<p align="center">
  Fig 12.
</p>

+ To view the synthesised netlist

`less picorv32a.synthesis.v`

<p align="center">
![image](https://github.com/KushalR17/pes_pd/assets/142383052/fb10339d-de96-46d0-a600-cfa6fe33b33b)

</p>
<p align="center">
  Fig 13.
</p>

+ To view the actual statistical synthesis report

`less 1-yosys_4.stat.rpt`
<p align="center">
![image](https://github.com/KushalR17/pes_pd/assets/142383052/f0f67798-d3f0-4387-846c-ba2a974fc0ce)

</p>
<p align="center">
  Fig 14.
</p>

</details>

# Day-2
## Chip Floorplanning Considerations
<details>
<summary> Utilisation Factor and Aspect Ratio </summary>

+ Begin with a netlist.
+ Convert the symbols into physical dimensions.
+ Calculate the area occupied by the netlist on a silicon wafer.
+ Place all the logical cells inside the core.
+ If all the logical cells occupy the complete area of the area, then the utilisation is 100%.
+ We can calculate the utilisation factor and the aspect ratio by the formulae given below:

<p align="center">
![image](https://github.com/KushalR17/pes_pd/assets/142383052/e55cecc2-a4a6-4b77-9412-022d4422b31e)

</p>
<p align="center">
  Fig 1. Formulae
</p>

</details>

<details>
<summary> Concept of Pre-placed Cells </summary>

+ Consider a combinational logic which is converted into netlist
<p align="center">
![image](https://github.com/KushalR17/pes_pd/assets/142383052/baccad09-23fb-4eb0-8b7b-129243b358d9)

</p>
<p align="center">
  Fig 2.
</p>

+ Cut the circuit into two parts and separate them out.
<p align="center">
![image](https://github.com/KushalR17/pes_pd/assets/142383052/ee0dda8f-5776-41af-9956-0683b59dfe33)

</p>
<p align="center">
  Fig 3.
</p>

+ Extend the IO pins
<p align="center">
![image](https://github.com/KushalR17/pes_pd/assets/142383052/cff3d747-4dfb-4980-b37b-ea867cbf0b95)

</p>
<p align="center">
  Fig 4.
</p>

+ Black box the boxes
<p align="center">
![image](https://github.com/KushalR17/pes_pd/assets/142383052/b775e86e-58b3-4c5a-ac7d-5da36d0d9315)

</p>
<p align="center">
  Fig 5.
</p>

+ Separate the black boxes as two different IPs or modules
<p align="center">
![image](https://github.com/KushalR17/pes_pd/assets/142383052/106e7f00-4a08-4df0-9797-f2a1f3a64121)

</p>
<p align="center">
  Fig 6.
</p>

+ The arrangement of these IPs in the chip is referred as **Floorplanning**.
+ These IPs/blocks have user-defined locations and hence are placed in the cell before automated placement-and-routing and are called as **pre-placed-cells**.
+ Automated placement and routing tools place the remaining logical cells in the design onto the chip.
</details>

<details>
<summary> De-coupling Capacitors </summary>  

+ Define locations for pre-placed cells.
+ Surround the cells with de-coupling capacitors.
+ Decoupling capacitors, also known as bypass capacitors or noise-reduction capacitors, are electronic components used in electronic circuits to stabilize and improve the performance of integrated circuits (ICs) and other semiconductor devices.
+ They are a vital part of circuit design, especially in digital and mixed-signal electronics.
+ When a circuit is powered, especially in digital circuits where there are rapid transitions between logic states, the current demand can change rapidly. Decoupling capacitors are placed close to the power pins of ICs, such as microcontrollers or processors, to counteract these rapid changes.

<p align="center">
![image](https://github.com/KushalR17/pes_pd/assets/142383052/fe600405-850a-4591-9ada-d8fc45da4f02)

<p align="center">
  Fig 7.
</p>

</details>

<details>
<summary> Power Planning </summary>  
+ Power planning refers to the process of strategically managing and distributing electrical power within a circuit or system to ensure reliable and efficient operation.
+ Effective power planning is essential in modern electronics to meet performance, power consumption, and thermal constraints. 

<p align="center">
![image](https://github.com/KushalR17/pes_pd/assets/142383052/2e7f047f-feeb-48e9-a557-de9e472aa7bf)

</p>
<p align="center">
  Fig 8.
</p>

</details>

<details>
<summary> Pin Placement and Logical Cell Placement Blockage </summary>

**Pin Placment**
+ Pin placement, also known as I/O (Input/Output) placement, is a crucial step in the physical design of an integrated circuit (IC).
+ It involves determining the locations and positions of input and output pins on the chip's package or die.
+ Proper pin placement is essential to ensure that the IC can interface with the external world effectively, meet performance requirements, and adhere to manufacturability constraints.

<p align="center">
![image](https://github.com/KushalR17/pes_pd/assets/142383052/44124330-19f7-41a7-8674-861325071cd1)

</p>
<p align="center">
  Fig 9. Circuit Example
</p>

<p align="center">
![image](https://github.com/KushalR17/pes_pd/assets/142383052/62376e58-47d5-4be9-a6a4-f4518d13e873)

</p>
<p align="center">
  Fig 10. Pin placement for the Circuit
</p>

**Logical Cell Placement Blockage**
+ Logical cell placement blockage, often referred to as blockage constraints or blockage regions, is a concept used in the physical design of integrated circuits (ICs).
+ Blockage constraints are used to restrict or reserve specific areas of the chip's layout for various purposes, such as accommodating specialized circuitry, ensuring signal integrity, or meeting manufacturing requirements.

<p align="center">
![image](https://github.com/KushalR17/pes_pd/assets/142383052/46400557-1a18-4627-b2a1-d1db190950b9)

</p>
<p align="center">
  Fig 11. Logical Cell Placement Blockage for the Circuit
</p>

</details>

<details>
<summary> Steps to run Floorplan using OpenLane </summary>

+ `less floorplan.tcl`
<p align="center">
![image](https://github.com/KushalR17/pes_pd/assets/142383052/b2e68ff8-dc36-40e8-88aa-59e1dfc65995)

</p>
<p align="center">
  Fig 12.
</p>

+ `less config.tcl`
<p align="center">
![image](https://github.com/KushalR17/pes_pd/assets/142383052/cb12d842-91fb-4c9d-9619-ff04aa24348d)

</p>
<p align="center">
  Fig 13.
</p>

+ `%run_floorplan`
<p align="center">
![image](https://github.com/KushalR17/pes_pd/assets/142383052/4b38a36e-9fbb-400b-9fa9-f074c5d969ca)

</p>
<p align="center">
  Fig 14.
</p>

</details>

<details>
<summary> Review Floorplan Files and Steps to View Floorplan </summary>

<p align="center">
![image](https://github.com/KushalR17/pes_pd/assets/142383052/20f2f581-b88a-4b90-bac2-4e7dd7da80a7)

</p>
<p align="center">
  Fig 15.
</p>

<p align="center">
![image](https://github.com/KushalR17/pes_pd/assets/142383052/8684bde6-69fb-4b97-80a2-c8279ac2fe90)

</p>
<p align="center">
  Fig 16.
</p>

<p align="center">

</p>
<p align="center">
  Fig 17.
</p>

<p align="center">

</p>
<p align="center">
  Fig 18.
</p>

</details>

<details>
<summary> Review Floorplan Layout in Magic </summary>
  
 `magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.floorplan.def &`

<p align="center">
![image](https://github.com/KushalR17/pes_pd/assets/142383052/2b5ba0f9-ac9a-40ff-ac2e-29c07cc60666)

</p>
<p align="center">
  Fig 19.
</p>

+ When viewed the horizontal metal layer
<p align="center">
![image](https://github.com/KushalR17/pes_pd/assets/142383052/649e0a80-cdce-4e7f-8451-482a32fe27d0)

</p>
<p align="center">
  Fig 20.
</p>

+ When viewed the vertical metal layer
<p align="center">
![image](https://github.com/KushalR17/pes_pd/assets/142383052/9e34f3d2-3bf0-4058-a078-5a5051a0e30a)

</p>
<p align="center">
  Fig 21.
</p>

</details>

## Library Binding and Placement
<details>
<summary> Netlist Binding and Initial Place Design </summary>

**Netlist Binding**
+ Netlist binding is a crucial step in the process of transforming a high-level design description into a representation that can be physically implemented on a chip or printed circuit board (PCB).
+ This step involves associating the logical components and connections described in the netlist with physical components, such as gates, flip-flops, and interconnections, that will be used in the actual implementation.

<p align="center">
![image](https://github.com/KushalR17/pes_pd/assets/142383052/d4c35a82-1990-42aa-a659-811b050f2908)

</p>
<p align="center">
  Fig 1.
</p>

</details>

<details>
<summary> Optimise Placement using Estimated Wire-Length and Capacitance </summary>

+ We need to estimate the wire length and capacitance, and based on that insert repeaters.

<p align="center">
![image](https://github.com/KushalR17/pes_pd/assets/142383052/efb51e8e-e69e-4c6c-b35d-19802cad473a)

</p>
<p align="center">
  Fig 2.
</p>

</details>

<details>
<summary> Need for Libraries and Characterization </summary>

**Library Characterisation**
+ Library characterization is the process of creating a comprehensive and accurate characterization model for a library of standard cells.
+ These standard cells serve as the fundamental building blocks for designing digital circuits.
+ The goal of library characterization is to provide designers with essential information about how these cells behave under various operating conditions, allowing for accurate timing analysis and optimization.

</details>

<details>
<summary> Congestion aware Placement using RePLAce </summary>

+ `%run_placement`
<p align="center">
![image](https://github.com/KushalR17/pes_pd/assets/142383052/48fa94a8-d34a-4ef5-a475-63b5d5c67733)

</p>
<p align="center">
  Fig 3.
</p>

+`magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.placement.def &`

<p align="center">
![image](https://github.com/KushalR17/pes_pd/assets/142383052/91586fa0-2a38-4818-9090-627fabc0ece2)

</p>
<p align="center">
Fig 4.
</p>

</details>

## Cell Design and Characterization Flow
<details>
<summary> Inputs for Cell Design Flow </summary> 

**Cell Design Flow**
<p align="center">
![image](https://github.com/KushalR17/pes_pd/assets/142383052/ce1c1239-9e3d-432a-ad17-7ce32ba2e795)

</p>
<p align="center">
  Fig 1.
</p>

**Inputs:**
+ PDKs (Process Design Kits):
- PDKs are essential resources provided by semiconductor foundries.
- They contain information about the fabrication process, including the available semiconductor technology, transistor models, and design rules.
- PDKs enable IC designers to create layouts and perform simulations that are compatible with the specific manufacturing process of the foundry.

+ DRC (Design Rule Checking) and LVS (Layout vs. Schematic) Rules:
- DRC rules are a set of guidelines that ensure that the physical layout of a chip adheres to the foundry's manufacturing process requirements.
- LVS rules ensure that the electrical characteristics of the layout match the intended schematic design.
- Both DRC and LVS checks are crucial for identifying and rectifying design errors and ensuring manufacturability and functionality.

+ SPICE Models:
- SPICE (Simulation Program with Integrated Circuit Emphasis) models are mathematical representations of electronic components (transistors, resistors, capacitors, etc.).
- They describe how these components behave electrically under different conditions.
- SPICE models are used for circuit simulation to analyze the performance of an IC design and predict its behavior.

+ Library:
- A library in IC design contains a collection of pre-designed, standardized components (e.g., logic gates, flip-flops, analog blocks) that can be used to build more complex circuits.
- Libraries save time and effort by providing readily available building blocks for designing ICs.
- Libraries often include SPICE models for each component, allowing for accurate simulation.

+ User-Defined Specifications:
- User-defined specifications are custom requirements and constraints set by the IC designer for a specific design project.
- These specifications can include performance goals (e.g., speed, power consumption), design constraints (e.g., area, power budget), and unique functionality requirements.
- User-defined specifications guide the entire IC design process, influencing choices made in terms of circuit design, layout, and simulation.

</details>

<details>
<summary> Circuit Design Step </summary>

+ Circuit design involves creating the logical and functional representation of digital or analog circuits using hardware description languages (HDLs) like Verilog or VHDL.
+ This step defines the behavior of the circuit without specifying its physical layout.
+ Key tasks include defining circuit functionality, specifying input and output behaviors, selecting components like logic gates or transistors, and optimizing for desired performance metrics.

</details>

<details>
<summary> Layout Design Step </summary>

+ Layout design is the process of creating the physical arrangement of components, such as transistors, interconnections, and metal layers, on the silicon substrate to implement the circuit designed in the previous step.
+ Layout designers adhere to design rules and guidelines specific to the semiconductor process technology to ensure manufacturability.
+ Key tasks include transistor placement, routing of metal layers, ensuring signal integrity, and minimizing area while meeting performance requirements.

</details>

<details>
<summary> Typical Characterization Flow </summary>

+ Characterization involves the comprehensive evaluation and modeling of the circuit's behavior under various conditions, ensuring that it meets design specifications and performance goals.
+ This step generates timing models, power models, and other characterization data to describe how the circuit performs under different operating conditions (e.g., voltage, temperature, process variations).
+ Characterization data is crucial for accurate static timing analysis, power estimation, and integration of the circuit into larger designs.

</details>

## General Timing Characterisation Parameters
<details>
<summary> Timing Threshold Definitions </summary>

<p align="center">
![image](https://github.com/KushalR17/pes_pd/assets/142383052/4f6cd002-7fa1-40b8-bcd3-8d92baf29b73)

</p>
<p align="center">
  Fig 1.
</p>

<p align="center">
![image](https://github.com/KushalR17/pes_pd/assets/142383052/ca2858c2-a079-4007-b18a-a38687ae9e17)

</p>
<p align="center">
  Fig 2.
</p>

**Slew Low Rise Threshold (slew_low_rise_thr):**
+ This parameter defines the minimum input signal slope (rate of change) required to trigger a rising transition in the output signal.
+ It helps characterize how fast an input signal must rise to initiate a change in the output signal from low to high.

**Slew High Rise Threshold (slew_high_rise_thr):**
+ Similar to the slew_low_rise_thr, this parameter defines the minimum input signal slope required to trigger a rising transition in the output signal, but for signals that are already at a high logic level.

**Slew Low Fall Threshold (slew_low_fall_thr):**
+ This parameter defines the minimum input signal slope required to trigger a falling transition in the output signal.
+ It specifies how fast an input signal must fall to initiate a change in the output signal from high to low.

**Slew High Fall Threshold (slew_high_fall_thr):**
+ Like the slew_low_fall_thr, this parameter defines the minimum input signal slope required to trigger a falling transition in the output signal, but for signals that are already at a high logic level.

**Input Rise Threshold (in_rise_thr):**
+ This parameter represents the threshold voltage level at which an input signal is considered to be transitioning from low to high.
+ It is essential for accurate timing analysis and helps determine when inputs trigger changes in the circuit.

**Input Fall Threshold (in_fall_thr):**
+ Similar to in_rise_thr, this parameter represents the threshold voltage level at which an input signal is considered to be transitioning from high to low.

**Output Rise Threshold (out_rise_thr):**
+ This parameter defines the threshold voltage level at which an output signal is considered to be transitioning from low to high.
+ It is used to specify the timing behavior of the circuit's outputs.

**Output Fall Threshold (out_fall_thr):**
+ Similar to out_rise_thr, this parameter defines the threshold voltage level at which an output signal is considered to be transitioning from high to low.

</details>

# Day-3
## Labs for CMOS Inverter Ngspice Simulations
<details>
<summary> IO Placer </summary>

+ In order to change the distance between the IO pins:

  ` % set ::env(FP_IO_MODE) 2`
  `% run_floorplan`

<p align="center">
![image](https://github.com/KushalR17/pes_pd/assets/142383052/9480fe02-911e-413d-897d-67a728eab2f5)

</p>
<p align="center">
  Fig 1.
</p>

+ We can see that they are no more equidistant.

</details>

<details>
<summary> SPICE Deck Creation for CMOS Inverter </summary> 

**SPICE Deck**
+ Component connectivity
+ Component values
+ Identify nodes
+ Name nodes

<p align="center">
![image](https://github.com/KushalR17/pes_pd/assets/142383052/df0c31fd-78b2-431b-8486-8a5414d31513)

</p>
<p align="center">
  Fig 2.
</p>

CMOS_INVERTER.cir
```
*** MODEL DESCRIPTIONS ***
*** NETLIST DESCRIPTION ***
M1 out in vdd vdd pmos W=0.375u L=0.25u
M2 out in 0 0 nmos W=0.375u L=0.25u

cload out 0 10f

Vdd vdd 0 2.5
Vin in 0 2.5
*** SIMULATION Commands ***

.op
.dc Vin 0 2.5 0.05
*** include tsmc_025um_model.mod ***
.LIB "tsmc_025um_models.mod" CMOS_MODELS
.end
```

</details>

<details>
<summary> SPICE Simulation Lab for CMOS Inverter </summary>

+ Simulation steps
 - `cd <folder where the .cir file is present>`
 - `source CMOS_INVERTER.cir`
 - `run`
 - `setplot`
 - `dc1`
 - `display`
 - `plot out vs in`

<p align="center">
![image](https://github.com/KushalR17/pes_pd/assets/142383052/9ecc0c02-3165-4b54-837a-1d888b660383)

</p>
<p align="center">
Fig 3.
</p>

+ The output should be symmetric ie., the threshold voltage should be at vdd/2.
+ If it isnt, try to increase the PMOS width and run the simulation again.

</details>

<details>
<summary> Switching Threshold Vm </summary>

+ CMOS as a circuit itself is a very **Robust** device.
+ **Switching threshold** defines the robustness of CMOS.
+ **Vm** is the point where Vin=Vout.

</details>

<details>
<summary> Lab Steps to Gitclone vsdstdcelldesign </summary>

+ `git clone https://github.com/nickson-jose/vsdstdcelldesign.git`

<p align="center">
![image](https://github.com/KushalR17/pes_pd/assets/142383052/4d96e5a0-4346-44d4-aa2b-5dc4a91939ea)

</p>
<p align="center">
  Fig 4.
</p>

+ ` cp sky130A.tech /home/vsduser/Desktop/work/tools/openlane_working_dir/openlane/vsdstdcelldesign`

<p align="center">
![image](https://github.com/KushalR17/pes_pd/assets/142383052/4b81f536-dbfb-4354-b3aa-a37ee113daf8)

</p>
<p align="center">
  Fig 5.
</p>

+ We can see that the tech file is added.

<p align="center">
![image](https://github.com/KushalR17/pes_pd/assets/142383052/4345b957-97ce-40f9-85e6-540e2dd4aa24)

</p>
<p align="center">
Fig 6.
</p>

</details>

## Inception of Layout
<details>
<summary> Create Active Regions </summary>

+ Fabrication of CMOS is a 16 Mask process.

**Selecting the Substrate**
+ We go for a p-type substrate with
  - resistivity around : 5-50 ohm
  - doping level : 10^15 cm^-3
  - orientation : 100

**Creating Active region for transistors**

+ Grow a layer of SiO2(~40nm) on Psub.
+ Deposit a layer of ~80nm Si3N4 on SiO2.
+ Deposit 1um layer of photoresist(used to define regions).
+ Photolithography.
+ Etch out Si3N4 and SiO2 using a suitable solvent.
+ Place the obtained structure in oxidation furnace due to which field oxide is grown.This process is called LOCOS ( Local oxidation of silicon).
+ Etch out Si3N4 using hot phosphoric acid.

</details>

<details>
<summary> Formation of n-well and p-well </summary>

+ Deposit a layer of photoresist.
+ Apply mask to cover NMOS.
+ Expose to UV light, wash away the area which is exposed and remove mask.
+ Deposit Boron using ion implementation at an energy of 200keV.
+ Repeat the same steps for other half using phosphorous at an energy of 400keV.
+ Wells have been created but the depth is low, hence subject it to high temperature furnace which increases the well depth.

</details>

<details>
<summary> Formation of Gate Terminal </summary>
  
+ Deposit a layer of photoresist.
+ Apply mask to cover NMOS.
+ Expose to UV light, wash away the area which is exposed and remove mask.
+ Deposit Boron using ion implementation at an energy of 200keV cause we need boron at the surface.
+ Repeat the same steps for other half using arsenic.
+ Original oxide etched/stripped using hydroflouric solution.
+ Then re-grown again to give high quality oxide (~10nm thin).
+ Deposit ~0.4um polysilicon layer.
+ Dope N-type (phosphorous/ arsenic) ion implants for low gate resistance.
+ Deposit a layer of photoresist and repeat the same steps till removing the mask.

</details>

<details>
<summary> Lightly Doped Drain Formation </summary>

+ The doping profile near n-well is P+, P-, N.
+ Near p-well is N+, N-, P.
+ 2 reasons to do this:
  - hot electron effect
  - short cahnnel effect
+ On the surface of SiO2, near N-well, deposit a layer of photoresist, and mask it.
+ Expose to UV light, wash away the area which is exposed and remove mask.
+ Apply phosphorous to form N- implant on p-well.
+ Similarly do it on the other half, but apply boron to form P- implant on n-well.
+ LDD needs to be protected, hence deposit 0.1um thick SiO2 on full structure and etch out using plasma anisotropic etching.
+ This results in the formation of side-wall spacers.

</details>

<details>
<summary> Source and Drain Formation </summary>

+ On the surface of SiO2, near N-well, deposit a layer of photoresist, and mask it.
+ Expose to UV light, wash away the area which is exposed and remove mask.
+ Deposit arsenic at 75KeV that forms an N+ implant on Pwell.
+ Similarly do it on the other half, but apply boron to form P+ implant on n-well.
+ Subject it to high temperature furnace that results in required thickness of N+,P+,N-,P- implants.

</details>

<details>
<summary> Local Interconnect Formation </summary>

+ Etch thin SiO2 oxide in HF solution.
+ Deposit Titanium of wafer surface using sputtering.
+ Wafer heated at 650-700 degree celsius in N2 ambient for 60 sec.
+ Results in low resistant TiSi2.
+ At the other places, TiN is formed which is used only for local communication.
+ TiN is etched off using RCA cleaning.

</details>

<details>
<summary> Higher Level Metal Formation </summary>

+ Deposit 1um of SiO2 with phosphorous or boron (known as phosphoborosilicate glass) on wafer surface.
+ Use CMP (chemical mechanical polishing) technique for planarizing wafer surface.
+ TiN and blanket Tungsten layers are deposited and subjected to CMP.
+ An aluminum (Al) layer is added and subjected to photolithography and CMP.
+ Deposit a layer of Si3N4 that acts as dielectric to protect the chip.

<p align="center">
![image](https://github.com/KushalR17/pes_pd/assets/142383052/2791f88f-dc5c-42fc-8592-dfe8edef0e1f)

</p>

</details>

<detailS>
<summary> Lab Introduction to Sky130 Basic Layers Layout and LEF using Inverter </summary>

+ `magic -T sky130A.tech sky130_inv.mag &`

<p align="center">
![image](https://github.com/KushalR17/pes_pd/assets/142383052/24b3c73f-413d-41e7-b759-51c0e0a68d1e)

</p>
<p align="center">
Fig 7.
</p>

+ Click on the component and type `what` in the tkcon window.

<p align="center">
![image](https://github.com/KushalR17/pes_pd/assets/142383052/8d0d5749-a92d-4787-ba3a-e762a70b0273)

</p>
<p align="center">
Fig 8.
</p>

</detailS>

<details>
<summary> Lab Steps to Create std cell Layout and Extract SPICE Netlist </summary>

+ DRC errors in magic will be highlighted with white dotted lines.
<p align="center">
![image](https://github.com/KushalR17/pes_pd/assets/142383052/896b7dc4-7de4-49d6-88c6-6d4ff3d1d0c2)

</p>
<p align="center">
  Fig 9.
</p>

+ To identify DRC errors select `DRC find next error`.
+ It will be displayed on the tkcon window.

<p align="center">
![image](https://github.com/KushalR17/pes_pd/assets/142383052/34736e9e-bd53-49a3-8038-57330e28cf3f)

</p>
<p align="center">
  Fig 10.
</p>

+ Extracting to SPICE Command
  - `extract all`
  - `ext2spice cthresh 0 rthresh 0`
  - `ext2spice`
+ cthresh and rthresh are used to extract all parasatic capacitances.

<p align="center">
![image](https://github.com/KushalR17/pes_pd/assets/142383052/13b9c3ad-dc7c-4cd1-88a3-eaa70fb9b15a)

</p>
<p align="center">
  Fig 11.
</p>

+ We can see that the spice file is created in the folder.

<p align="center">
![image](https://github.com/KushalR17/pes_pd/assets/142383052/3164700c-8df8-474f-8e83-be13fe298755)

</p>
<p align="center">
  Fig 12.
</p>

+ Spice File

<p align="center">
![image](https://github.com/KushalR17/pes_pd/assets/142383052/ce12e687-903f-4990-b87b-4a75dbbd0502)

</p>
<p align="center">
Fig 13.
</p>

</details>

## Sky130 Tech File Labs
<details>
<summary> Lab Steps to Create Final SPICE Deck using Sky130 Tech </summary>

+ Grid size.
<p align="center">
![image](https://github.com/KushalR17/pes_pd/assets/142383052/8094c5ae-cfc7-45a1-8725-f037d4ad9a96)

</p>
<p align="center">
  Fig 1.
</p>

+ We modified the spice file.
<p align="center">
![image](https://github.com/KushalR17/pes_pd/assets/142383052/d3adeef3-4906-4915-a3c6-0bdb07ea7882)

</p>
<p align="center">
  Fig 2.
</p>

 - `ngspice sky130_inv.spice`
 - `plot y vs time a`

<p align="center">
![image](https://github.com/KushalR17/pes_pd/assets/142383052/4a096d23-607d-482e-a922-a85d52bbc6b1)

</p>
<p align="center">
  Fig 3.
</p>

</details>

<details>
<summary> Introduction to Magic Options and DRC rules </summary>
+ For reference : http://opencircuitdesign.com/magic/

**Magic**
+ Magic is a venerable VLSI layout tool, written in the 1980's at Berkeley by John Ousterhout, now famous primarily for writing the scripting interpreter language Tcl. 
+ Due largely in part to its liberal Berkeley open-source license, magic has remained popular with universities and small companies.
+ The open-source license has allowed VLSI engineers with a bent toward programming to implement clever ideas and help magic stay abreast of fabrication technology.
+ However, it is the well thought-out core algorithms which lend to magic the greatest part of its popularity.
+ Magic is widely cited as being the easiest tool to use for circuit layout, even for people who ultimately rely on commercial tools for their product design flow.

**DRC rules**
+ DRC (Design Rule Check) rules are a set of guidelines and constraints used in the field of semiconductor and integrated circuit (IC) design to ensure that the physical layout of a chip or circuit adheres to the manufacturing process's design rules.
+ These rules are essential for maintaining manufacturability and ensuring that the final ICs can be fabricated without defects.
+ The design rules used by Magic's design rule checker come entirely from the technology file.

</details>

<details>
<summary> Introdcution to Sky130 PDKs and Steps to Download Labs </summary>

**Sky130 PDK**
+ SKY130 is a mature 180nm-130nm hybrid technology developed by Cypress Semiconductor that has been used for many production parts.
+ SKY130 is now available as a foundry technology through SkyWater Technology Foundry.

+ `wget http://opencircuitdesign.com/open_pdks/archive/drc_tests.tgz`

<p align="center">
![image](https://github.com/KushalR17/pes_pd/assets/142383052/f8c4c0eb-e1f5-4f06-a3d6-630091d83b6e)

</p>
<p align="center">
  Fig 4.
</p>

</details>

<details>
<summary> Lab Introduction to Magic and Steps to Load Sky130 Tech-Rules </summary>

+ To open Magic
  - `magic -d XR`
 
+ Go to files then open `met3.mag` file.

<p align="center">
![image](https://github.com/KushalR17/pes_pd/assets/142383052/e11cd4f5-d512-42f6-a53c-d406ee0f117b)

</p>
<p align="center">
  Fig 5.
</p>

+ To check which DRC rule is being violated select area.
+ Type `drc why` in tkcon.

<p align="center">
![image](https://github.com/KushalR17/pes_pd/assets/142383052/7b0e0d3f-0a6f-4cd1-aa46-6ccd155f9570)

</p>
<p align="center">
  Fig 6.
</p>

+ To add contact cuts add met3 contact by selecting area and clicking on m3contact using middle mouse button.
+  Type  `cif see VIA2` in tkcon prompt.

<p align="center">
![image](https://github.com/KushalR17/pes_pd/assets/142383052/c2c7e3b5-2477-421f-b6e1-13ef3a68f723)

</p>
<p align="center">
  Fig 7.
</p>

</details>

<details>
<summary> Lab Exercise to fix poly.9 error in Sky130 Tech File </summary>

+ Type `load poly` in the tkon prompt.











# Day-4
## Timing Modelling Using Delay Tables
<details>
<summary> Lab steps to convert Grid info into Track info </summary>

+ ` ~/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/openlane/sky130fd_sc_hd/tracks.info`
+ `less tracks.info`

<p align="center">
![image](https://github.com/KushalR17/pes_pd/assets/142383052/bfa9999d-f965-418c-acae-2b6e9b1a42b4)

</p>
<p align="center">
Fig 1.
</p>

+ The 'tracks.info' file is used during the routing stage.
+ Routes are the metal traces.
+ Since the PNR is an automated flow, we need to specify where all we want the routes to go.

+ Now we converge the grid definition in the layout to track definition.

<p align="center">
![image](https://github.com/KushalR17/pes_pd/assets/142383052/b394948c-3838-46b4-b9fb-317218283229)

</p>
<p align="center">
Fig 2.
</p>

+ The next requirement is that the width of the cell should be the odd multiple of xpitch which is '0.46' as seen in the 'tracks.info' file.
+ As we can see it encloses two full boxes and two halves of one box, totally making three boxes as indicated by the white line.

<p align="center">
![image](https://github.com/KushalR17/pes_pd/assets/142383052/8b2485a3-76f5-4276-abdb-74386775e6df)

</p>
<p align="center">
Fig 3.
</p>

</details>

<details>
<summary> Lab steps to convert Magic Layout to Std Cell LEF </summary>

+ In the tkcon window, type `save sky130_vsdinv.mag`.
+ This is to make our own .mag file.
+ `lef write` to make .lef file
<p align="center">
![image](https://github.com/KushalR17/pes_pd/assets/142383052/63856cb5-dd72-460b-bd7c-d11b53b5a9d1)

</p>
<p align="center">
Fig 4.
</p>

+ `less sky130_vsdinv.lef`.
<p align="center">
![image](https://github.com/KushalR17/pes_pd/assets/142383052/8ec1ee32-21ca-4d92-b958-109baaa576be)

</p>
<p align="center">
Fig 5.
</p>

</details>

<details>
<summary> Introduction to Timing Libs and Steps to include New cell in Synthesis </summary>

+ We copy the lef file and the libraries.

<p align="center">
![image](https://github.com/KushalR17/pes_pd/assets/142383052/b5d9c2f7-10a8-4932-9f63-cf63e850c319)

</p>
<p align="center">
Fig 6.
</p>

<p align="center">
![image](https://github.com/KushalR17/pes_pd/assets/142383052/3de838b7-2871-4a1e-a28d-86264367861e)

</p>
<p align="center">
Fig 7.
</p>

+ Next we modify the 'config.tcl' file in the picorv32a folder.

<p align="center">
![Uploading image.png…]()

</p>
<p align="center">
Fig 8.
</p>

+ Open the OpenLANE interactive window and retrieve the 0.9 package.
 - `prep -design picorv32a -tag 14-09_10-42 -overwrite`
 - `set lefs [glob $::env(DESIGN_DIR)/src/*.lef]`
 - `add_lefs -src $lefs `
 - `run_synthesis`
<p align="center">
<img src="https://github.com/Veda1809/pes_pd/assets/142098395/87221315-74de-4b7d-8e95-db5a621b598d">
</p>
<p align="center">
Fig 9.
</p>

<p align="center">
<img src="https://github.com/Veda1809/pes_pd/assets/142098395/2e0fa894-ed3a-474e-b8fa-e6e23e0bc905">
</p>
<p align="center">
Fig 10.
</p>


<p align="center">
<img src="https://github.com/Veda1809/pes_pd/assets/142098395/f59efbaa-cf5b-430e-aafd-b8839411b5a3">
</p>
<p align="center">
Fig 11.
</p>

+ VLSI engineers will obtain system specifications in the architecture design phase. These specifications will determine a required frequency of operation. To analyze a circuit's timing performance designers will use static timing analysis tools (STA). When referring to pre clock tree synthesis STA analysis we are mainly concerned with setup timing in regards to a launch clock. STA will report problems such as worst negative slack (WNS) and total negative slack (TNS). These refer to the worst path delay and total path delay in regards to our setup timing restraint. Fixing slack violations can be debugged through performing STA analysis with OpenSTA, which is integrated in the OpenLANE tool. To describe these constraints to tools such as In order to ensure correct operation of these tools two steps must be taken:

+ Design configuration files (.conf) - Tool configuration files for the specified design
+ Design Synopsys design constraint (.sdc) files - Industry standard constraints file

</details>

<details>
<summary> Delay Tables </summary>

**Introduction**
+ Delay tables, often referred to as delay models or delay tables in the context of digital integrated circuit design, are data structures that provide information about the propagation delay of digital logic gates or cells under various conditions.
+ These tables are a fundamental component of static timing analysis (STA) and are used to predict the signal arrival times and meet timing constraints in digital designs.

**Purpose of Delay Tables:**
+ Delay tables are used to estimate the time it takes for a signal to propagate through a digital logic gate or cell.
+ This information is crucial for ensuring that signals meet their setup and hold time requirements and for calculating the overall timing behavior of a digital circuit.

**Types of Delay Tables:**
+ There are two main types of delay tables:
   - Library Delay Tables: These tables are part of a standard cell library and provide information about the delays of individual logic gates (AND, OR, XOR, flip-flops, 
    etc.) under various operating conditions (input transitions, voltage, temperature, etc.). Library delay tables are used to estimate the delays associated with 
    different gate types.
   - Interconnect Delay Tables: These tables describe the delay associated with routing signals between logic gates or cells on a chip. They account for wire resistance, 
   capacitance, and other physical properties that affect signal propagation.

**Data in Delay Tables:**
+ Delay tables typically include information such as:
   - Input conditions: Input transition times or slew rates.
   - Process corners: Variations in process technology, including worst-case and best-case scenarios.
   - Operating conditions: Voltage and temperature conditions.
   - Delay values: Delays for signal propagation through the gate or interconnect, often specified for different output loading conditions.

**Timing Analysis:**
+ Delay tables are used by STA tools to perform timing analysis on digital designs.
+ These tools use the delay tables to estimate the critical path delays, setup times, hold times, and other timing parameters.

**Corner Analysis:**
+ Corner analysis involves using delay tables for various process corners (e.g., slow, typical, fast) to account for manufacturing process variations.
+ This ensures that the design meets timing under a range of conditions.

**Clock Domain Crossing (CDC) Analysis:**
+ Delay tables are also used in CDC analysis to analyze signals that cross between different clock domains.
+ Understanding signal arrival times is crucial in preventing metastability issues.

**Optimization:**
+ Designers use delay tables to optimize their designs by selecting gates with appropriate delays to meet performance, power, and area goals.

**Iterative Process:**
+ During the design process, delay tables are used iteratively.
+ Designers may make adjustments to the design and rerun timing analysis to ensure that the design meets its timing constraints.

</details>

## Timing Analysis with Ideal Clocks using openSTA
<details>
<summary> Configure OpenSTA for Post-Synth Timing Analysis </summary>

+ We must create two files.
+ The first one must be in the openlane directory
+ This file is known as the 'pre_sta.conf' file.
<p align="center">
<img src="https://github.com/Veda1809/pes_pd/assets/142098395/ef4dc5de-b4d8-4daa-b24b-cccc92892422">
</p>
<p align="center">
  Fig 1.
</p>

+ The second is the my_base.sdc file.
+ This should be in the 'src/sky130' directory under the picorv32a directory.
<p align="center">
<img src="https://github.com/Veda1809/pes_pd/assets/142098395/8f3d851d-0ca4-4b23-b832-74947d2d2a0f">
</p>
<p align="center">
  Fig 2.
</p>

<p align="center">
<img src="https://github.com/Veda1809/pes_pd/assets/142098395/f1752a7e-99cf-4d19-9db6-ecbf25a71e1b">
</p>
<p align="center">
  Fig 3.
</p>

+ To run the timing analysis we type
+ `sta pre_sta.conf`

<p align="center">
<img src="https://github.com/Veda1809/pes_pd/assets/142098395/fd40565b-ac21-4a70-a5aa-e2119c578e85">
</p>
<p align="center">
  Fig 4.
</p>

+ There is a slack violation.
</details>

<details>
<summary> Optimise synthesis </summary>

+ Setting MAX_FANOUT value to 4 reduces the slack violation.
+ `set ::env(SYNTH_MAX_FANOUT) 4`
+ Then `run_synthesis`

<p align="center">
<img src="https://github.com/Veda1809/pes_pd/assets/142098395/d5aa0045-2fd8-4feb-80b0-7a5131e65e74">
</p>
<p align="center">
  Fig 5.
</p>

+ Since we have synthesised the core using our vsdinv cell too and as it got successfully synthesized, it should be visible in layout after `run_placement` stage which is followed after `run_floorplan` stage.

<p align="center">
<img src="https://github.com/Veda1809/pes_pd/assets/142098395/120370db-6dd6-4a5f-94aa-2790e3ea9555">
</p>
<p align="center">
  Fig 6.
</p>

</details>

## Clock Tree Synthesis TritonCTS and Signal Integrity

<details>
<summary> CTS </summary>

+ To run CTS we need to type the command.
+ `run_cts`
+ New .v is created.

<p align="center">
<img src="https://github.com/Veda1809/pes_pd/assets/142098395/56f2c27f-59e1-4609-bb92-538bfdc12a3d">
</p>
<p align="center">
  Fig 7.
</p>

</details>

## Timing Analysis with Real CLocks using OpenSTA

<details>
<summary> Lab steps to analyse Timing with Real CLocks</summary>

+ `openroad`
+ `read_lef /openLANE_flow/designs/picorv32a/runs/14-09_10-42/tmp/merged.lef`
+ `read_def /openLANE_flow/designs/picorv32a/runs/14-09_10-42/results/cts/picorv32a.cts.def`
+ `write_db pico_cts.db`
+ `read_db pico_cts.db`
+ `read_verilog /openLANE_flow/designs/picorv32a/runs/14-09_10-42/results/synthesis/picorv32a.synthesis_cts.v`
+ `read_liberty -max $::env(LIB_SLOWEST)`
+ `read_liberty -max $::env(LIB_FASTEST)`
+ `read_sdc /openLANE_flow/designs/picorv32a/src/my_base.sdc`

<p align="center">
<img src="https://github.com/Veda1809/pes_pd/assets/142098395/b154f855-0fbe-4474-a00e-018eea31e970">
</p>
<p align="center">
  Fig 8.
</p>

+ `set_propagated_clock [all_clocks]`
+ `report_checks -path_delay min_max -format full_clock_expanded -digits 4`

<p align="center">
<img src="https://github.com/Veda1809/pes_pd/assets/142098395/f9d97652-2a66-4da1-a0ac-86ee5b1b0d85">
</p>
<p align="center">
  Fig 9.
</p>

<p align="center">
<img src="https://github.com/Veda1809/pes_pd/assets/142098395/f9c6f227-2605-4981-923b-5b67d4d1884e">
</p>
<p align="center">
  Fig 10.
</p>

+ We perform it again for a more accurate result.

<p align="center">
<img src="https://github.com/Veda1809/pes_pd/assets/142098395/0ef49445-3ca6-431f-ac45-e4e79ea4e362">
</p>
<p align="center">
  Fig 11.
</p>

<p align="center">
<img src="https://github.com/Veda1809/pes_pd/assets/142098395/57d4836d-4a2d-48cb-817b-3ef317043769">
</p>
<p align="center">
  Fig 12.
</p>

</details>

<details>
<summary> Lab steps to Observe Setup and Hold Timing </summary>

+ `report_clock_skew -hold`

<p align="center">
<img src="https://github.com/Veda1809/pes_pd/assets/142098395/ae0f7352-156a-48bb-92b9-729c3491ffba">
</p>
<p align="center">
  Fig 13.
</p>

+ `report_clock_skew -setup`

<p align="center">
<img src="https://github.com/Veda1809/pes_pd/assets/142098395/abd5c628-ad24-4f32-a426-1973943f1dfc">
</p>
<p align="center">
Fig 14.
</p>

</details>

# Day-5
## Routing and DRC
<details>
<summary> Maze routing </summary>

+ Maze routing is a method used in electronic design automation (EDA) and integrated circuit (IC) design to determine efficient paths for interconnecting various components, such as logic gates, on a chip's layout. The goal is to find a path through a maze-like grid of obstacles while optimizing for factors like wire length, signal delay, and area utilization.

+ Lee's algorithm, also known as Lee's breadth-first search (BFS) algorithm, is a graph traversal and pathfinding algorithm that is commonly used in maze routing, maze solving, and other grid-based problems. Named after its creator, C. Y. Lee, the algorithm is particularly useful for finding the shortest path between two points in a grid while exploring the grid layer by layer.

</details>

<details>
<summary> DRC </summary>
  
Lambda rules are process-specific design rules used in semiconductor manufacturing to ensure that integrated circuit (IC) layouts adhere to the capabilities and constraints of a particular semiconductor process. These rules are expressed in terms of lambda (λ), a normalized unit of measurement relative to the process technology. Lambda rules can vary between semiconductor foundries and process nodes, but they typically cover various aspects of IC design. Here's a list of common lambda rules and design considerations:

+ Minimum Feature Size: Specifies the minimum allowed width and spacing for features such as transistors, metal tracks, and vias, often expressed as multiples of λ.
+ Aspect Ratio: Defines the acceptable aspect ratio (width-to-height ratio) for rectangular structures, ensuring manufacturability.
+ Metal Layer Constraints: Specifies minimum metal track widths, metal-to-metal spacings, and via sizes on metal layers.
+ Poly Pitch: Defines the minimum pitch (spacing between features) for the poly-silicon (poly) layer, which affects the size of transistors and gates.
+ Active Area Constraints: Specifies minimum active area dimensions, ensuring that transistors meet process requirements.
+ Well and Substrate Taps: Covers the placement and size of well and substrate taps for connecting to power and ground planes.
+ Gate Length: Specifies the minimum gate length for transistors, affecting their performance characteristics.
+ Contact and Via Rules: Defines the minimum size and spacing of contacts and vias used to connect different layers in the IC.
+ Local Interconnects: Provides rules for local interconnects, which are used for routing within a cell or macro.
+ Minimum Metal to Active Spacing: Sets the minimum separation between metal tracks and active areas.
+ Minimum Metal to Contact Spacing: Specifies the minimum distance between metal tracks and contacts.
+ Edge Exclusion Zones: Defines exclusion zones near the chip's edge, where certain design elements are not allowed.
+ Density Rules: Enforces limits on the density of features in different regions of the chip to ensure proper manufacturing and avoid over-congestion.
+ Well Proximity Rules: Governs the proximity of different well types (e.g., n-well and p-well) to prevent undesirable interactions.
+ Metal Layer Ordering: Specifies the order in which metal layers should be used in the design hierarchy.
+ Metal Filling: Addresses requirements for metal fill patterns to ensure planarity and manufacturability.
+ Antenna Rules: Addresses the issue of charge buildup (antenna effect) during manufacturing, providing guidelines for mitigating this effect.
+ Variation-Aware Rules: Accounts for process variations, statistical timing, and other variations in critical design rules.
+ Electromigration Constraints: Specifies limits on current densities to prevent electromigration issues in metal tracks.
+ Supply Voltage Constraints: Sets design guidelines for supply voltage levels and power distribution.
  
</details>

## Power Distribution Network and Routing

<details>
<summary> Power Distribution Network </summary>

+ After generating our clock tree network and verifying post routing STA checks we are ready to generate the power distribution network `gen_pdn` in OpenLANE:

<p align="center">
<img src="https://github.com/Veda1809/pes_pd/assets/142098395/8e498aeb-8efe-42bc-a1c9-a85f14be957c">
</p>
<p align="center">
  Fig 1.
</p>

<p align="center">
<img src="https://github.com/Veda1809/pes_pd/assets/142098395/7dde0890-0e24-4b1d-ac2b-153324d32d5d">
</p>
<p align="center">
  fig 2.
</p>

+ The PDN feature within OpenLANE will create:
   - Power ring global to the entire core
   - Power halo local to any preplaced cells
   - Power straps to bring power into the center of the chip
   - Power rails for the standard cells
+ We see that there is a change in the DEF.

<p align="center">
<img src="https://github.com/Veda1809/pes_pd/assets/142098395/c39633c9-f35a-4333-b5ed-7db6e771bd70">
</p>
<p align="center">
  Fig 3.
</p>

</details>

<details>
<summary> Global and Detailed Routing </summary>

+ OpenLANE uses TritonRoute as the routing engine for physical implementations of designs. Routing consists of two stages:
   - Global Routing - Routing guides are generated for interconnects on our netlist defining what layers, and where on the chip each of the nets will be reputed.
   - Detailed Routing - Metal traces are iteratively laid across the routing guides to physically implement the routing guides.

+ To run routing in OpenLANE:
  `run_routing`

<p align="center">![image](https://github.com/KushalR17/pes_pd/assets/142383052/fc82b070-780c-45db-9fbb-c24bf5b85000)

</p>
<p align="center">
  Fig 4.
</p>

+ If DRC errors persist after routing the user has two options:
  - Re-run routing with higher QoR settings.
  - Manually fix DRC errors specific in tritonRoute.drc file.
  
</details>

<details>
<summary> SPEF Extraction </summary>

+ After routing has been completed interconnect parasitics can be extracted to perform sign-off post-route STA analysis. The parasitics are extracted into a SPEF file.
+ The SPEF extractor is not included within OpenLANE as of now.

</details>
