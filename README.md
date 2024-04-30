# NASSCOM-VSD-SOC-DESIGN

# Advanced-Physical-Design-Using-OpenLANE-Sky130

## Day_1 Inception of Open-Source EDA, OpenLANE & Sky130PDK

### How to talk to Computers

* The `Arduino Leonardo` is a microcontroller board based on the `ATmega32u4`. It has `20 digital input/output pins` (of which 7 can be used as PWM outputs and 12 as analog inputs), a 16 MHz crystal oscillator, a micro USB connection, a power jack, an ICSP header, and a reset button. It contains everything needed to support the microcontroller; simply connect it to a computer with a USB cable or power it with an AC-to-DC adapter or battery to get started.
* The Leonardo differs from all preceding boards in that the ATmega32u4 has built-in USB communication, eliminating the need for a secondary processor. This allows the Leonardo to appear on a connected computer as a mouse and keyboard, in addition to a virtual (CDC) serial / COM port.


![leonardo aurdino](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/assets/140955475/362391d9-c07f-477b-866c-8177abbcc20b)

[leonardo pinout.pdf](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/files/14595132/leonardo.pinout.pdf)
[leonardo schematics.pdf](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/files/14595134/leonardo.schematics.pdf)


<b>The below block diagram describes this Arduino Board.</b>
<br>The circled section is the processor/SOC and along with it we have all the interfaces-- `JTAG-UART`, `QSPI1-Flash`, `I2C0EEPROM`, `VCC/GND`, `ADC(QSPI3)`, `SDRAM(external chip)`
![block diagram of Arduino](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/assets/140955475/f41e7b48-4593-4fd2-a179-a3376aa07dbb)

* When we open up this particular IC, the whole thing is called a package which is shown below.
* The pin locations of this particular package are all driven by the Arduino board that we are trying to design. The size of this package is `7mm x 7mm`.
* Chip basically sits at the center of the package and the connection of the chip with the package is shown as wire bonds this way we will be able to transfer all these signals coming from the outside world to the interior of the chip.

![chip connect to package](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/assets/140955475/0e33ba91-14c5-4101-a6b8-c7d07d6005a5)

The chip will have various components--
1. `<b>PADS</b>` - A pad is the exposed region of the metal on a circuit board that the component lead is soldered to. Pads are the points through which the connection of peripherals on a board is made with the chip/processor, and the transfer of data takes place. Any signal can go outside or inside the chip through pads.

2. `<b>CORE</b>` - A core in a chip is a well-partitioned piece of logic capable of independently performing all functions of a processor. All the digital logic building blocks are present here.

3. `<b>DIE</b>` - This is what gets manufactured on the silicon wafer.
  
4. `<b>FOUNDRY IP'S</b>` - A foundry is a company that provides IC (integrated circuit) manufacturing services - basically, you give them your design, and they manufacture the chip for you. Intellectual Property. In this context, it’s the design of the parts of a chip. Nowadays, many chips are not wholly designed by the company that’s “designing” them.
* For example, in a typical mobile phone application chip:
The main CPU will probably be bought as IP from ARM. The graphics processor (GPU) will be bought from one of a number of companies (e.g. ARM, Imagination Technologies, etc.)

* <b>Foundry IPs, or intellectual properties,</b> encompass a range of assets that a semiconductor foundry owns or licenses to produce integrated circuits (ICs) for its clients.
<b>These IPs can be classified into several categories:</b>

a) `Process Technology IP`: This includes the set of technologies and processes used in the fabrication of semiconductor devices. It encompasses everything from the design of the silicon wafers to the various steps involved in creating the circuitry on the chips.

b) `Design IP`: These are the designs of specific components or functional blocks used in ICs, such as CPUs, GPUs, memory modules, or communication interfaces. Design IPs are often licensed from third-party vendors or developed in-house by the foundry.

c) `Verification IP`: This type of IP includes the models, testbenches, and other verification components used to ensure that the designed IC functions correctly before manufacturing. It helps validate the functionality and performance of the chip design.

d) `Packaging IP`: Packaging IPs involve the technologies and designs related to the physical packaging of the ICs, including the substrate, interconnects, and external connections. Proper packaging is crucial for protecting the chips and enabling their integration into electronic devices.

e) `Security IP`: With the growing concerns over cybersecurity, security IPs have become increasingly important. These IPs include features such as encryption/decryption, authentication, and secure boot mechanisms to protect the data and functionality of the ICs from unauthorized access or tampering.

Overall, foundry IPs are essential for semiconductor foundries to differentiate themselves in the highly competitive semiconductor manufacturing industry. They enable foundries to offer unique technologies, services, and solutions to their clients, ultimately driving innovation and progress in the electronics industry.

5. `<b>MACROS</b> -  Macro is an essential component in the VLSI design cycle before the final packaged chip is ready to use. Macro cells are the memory cells and intellectual property that an analog design team has designed. To break down the understanding of macro cells, consider macro cells as pieces of logic blocks, mainly intellectual properties (IP), which can be used in a design without the need to building them from scratch. Thus, these memory cells are instrumental in reducing the total time for the design engineers that are required to complete their entire design.

![image](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/assets/140955475/fe9ac562-fa98-48b8-a4b1-ac26f78898f4)

![difference](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/assets/140955475/ea4cb08d-d182-45a0-a6be-262446f9d494)

In summary, while both <b>foundry IPs</b> and <b>Macros</b> are essential for semiconductor design and manufacturing, foundry IPs encompass a broader range of intellectual properties provided by semiconductor foundries, while macros specifically refer to pre-designed functional blocks used in chip design, often provided by third-party vendors or developed in-house.

![complete chip](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/assets/140955475/102dbe26-7ad6-4753-a5cb-5a5daf465f42)

So if we want to manufacture this entire chip, we need to communicate with the Foundry with the help of interface files that the Foundry passes to us.

![image](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/assets/140955475/95d24a92-c010-4892-9601-930bd42be84f)

![RISCV rtl to gds](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/assets/140955475/cf43df44-08ad-42a8-b7c7-171d92000dfc)

* <b>C-Program</b> is first compiled in its assembly language program which is the <b>RISC-V assembly language program</b>..then this assembly language program is converted into a machine language program which is nothing but a <b>binary language program (1's and 0's)</b> that is understood by the hardware of the computer i.e, these bits get executed in this particular layout and we get the required output.
Another interface that needs to be present between RISC-V architecture and the layout is the <b>hardware description language (HDL)</b>. So we need to implement these RISC-V specifications using some RTL and then finally RTL to GDS flow.
* So from the user's point of view, we just execute this particular C-Program and that should get automatically executed by the chip of the hardware and we get the required output.

![image](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/assets/140955475/75aba07b-29cb-4089-89e0-2b59a71198df)

![flowchart app run laptop](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/assets/140955475/c177530d-4e25-4acc-8763-b4d1578ffda3)

* This application software enters into the block called system software and here the system software converts the application software into binary language.
* The major components of  the system software are the operating system (O.S),  compiler, and assembler.
* The Operating system handles memories, i/o operation, and allocates memory.
* The output of the operating system is the small functions in the c/c++/Java language and these are being taken by the respective compiler and converted into instructions. (.exe files).
* The syntax of these particular instructions depends upon what kind of hardware (chip layout) is available..for eg--intelx86, Arm, MIPS, or RISC-V.
* Now the job of the assembler is to take these instructions and convert them into respective binary numbers which the machine understands..these binary numbers are then fed to the hardware and according to the logic, the hardware generates the output.

![Screenshot 2024-03-14 195929](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/assets/140955475/34874985-7e51-4006-999d-082265ae0aeb)


### SoC Design & OpenLANE

![asic and](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/assets/140955475/59658143-9d71-4af3-9b54-0e6ebe58488a)

![image](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/assets/140955475/7af4ef46-c41d-4ef3-9ab2-4d10c923bfb1)

1. `<b>RTL IPs</b>` - RTL IP refers to a product in electronic format that represents an integrated circuit function that can be instantiated in an integrated circuit design. An IP Core is a product in an electronic format that represents an integrated circuit function that can be instantiated in an integrated circuit design, including the circuits and modules of such integrated circuit design(s) and associated firmware, application programming interfaces, Software drivers, application-specific Software, and all register transfer language (RTL), Verilog, and other source materials to instantiate, modify, support, and maintain any of the foregoing.

2. `<b>EDA Tools</b>` - Electronic Design Automation (EDA) tools are software tools used to design electronic systems such as integrated circuits and printed circuit boards. They have three key functions: simulation, design, and verification. EDA tools work together in a design flow that chip designers use to design and analyze entire semiconductor chips. They allow teams to predict circuit behavior, assemble circuit elements, and anticipate chip performance. EDA tools are used to verify that a design will meet all the requirements of the manufacturing process, known as design for manufacturability (DFM). Deficiencies in this area can cause the resultant chip to either not function or function at reduced capacity, and there are reliability risks.

3. `<b>PDK</b>` - It stands for <b>Process Design Kit</b>. It is basically an interface between FAB and the designers which is a collection of files used to model a fabrication process for the EDA tools used to design an IC.
<br>PDK data consist of primarily-->
* Process design rules i.e DRC
* Device models
* Digital Std. cell library
* I/O library
On June 30, 2020, Google released the first ever open-source PDK `(FOSS 130 nm Production PDK)` to the masses.
PDK has only the data information for successful ASIC implementation using either open-source or close-source EDA tools.

![Screenshot 2024-03-14 232618](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/assets/140955475/27cedcbe-cec2-4797-9718-0b7d7c10a6bb)

![Screenshot 2024-03-14 233128](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/assets/140955475/25ed5fae-ba2a-425b-aaf7-c85f04f4f84e)

![image](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/assets/140955475/8f493685-5a26-4674-8b47-9d9c32d2d9d8)

![Screenshot 2024-03-14 233712](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/assets/140955475/824503ed-8d41-4175-bbbd-56c0190e2ce1)

![Screenshot 2024-03-14 234035](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/assets/140955475/fcc6e2a9-1449-4202-8223-1348f8f151a0)

![Screenshot 2024-03-14 235313](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/assets/140955475/74058a99-0165-462f-91f8-b5876b5edaaa)

![Screenshot 2024-03-14 235011](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/assets/140955475/9db02423-74c6-4252-a9fb-96495038edb4)

![Screenshot 2024-03-15 000002](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/assets/140955475/d3c210f9-d04b-4a8a-9ea4-9b184b0a0119)

![Screenshot 2024-03-15 000513](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/assets/140955475/8ac9616c-680c-4357-9020-904882456bd7)

![Screenshot 2024-03-15 001100](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/assets/140955475/930eea0a-1eb6-4217-a7e8-5e4755386519)

![Screenshot 2024-03-15 001354](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/assets/140955475/c041a9a3-eeff-4c9f-a75d-23a3b44c7eca)

### Introduction to OpenLANE

<b>OpenLane</b> is an automated `RTL to GDSII flow` based on several components including `OpenROAD`, `Yosys`, `Magic`, `Netgen`, `CVC`, `SPEF-Extractor`, `KLayout`, and a number of custom scripts for design exploration and optimization. The flow performs all ASIC implementation steps from RTL all the way down to GDSII.

![Screenshot 2024-03-15 030342](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/assets/140955475/7dff59ee-8ee2-48c3-acbc-a93826d91b60)

![Screenshot 2024-03-15 031036](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/assets/140955475/e5ed8425-6787-4ece-b870-0c391cae439f)

![Screenshot 2024-03-15 031552](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/assets/140955475/ecef32fe-1d9a-4f3b-a9bf-973448451508)

<b>The objective of OpenLANE is to produce a clean GDSII with no human intervention, i.e. `No LVS violations`, `No DRC violations`, and `No Timing violations`.</b>

* It is tuned for SkyWater 130nm Open PDK and also supports `XFAB180` and `GF130G`.
* It is containerized.
    * Functional out of the box.
    * Instructions to build and run natively will follow.
* Can be used to harden Macros and chips.
* Two modes of operation-- `Autonomous` & `Interactive`.
* Design space exploration-- Find the best set of flow configurations.
* Large number of design examples</b>-- 43 Designs with their best configuration and more details to be added.

![Screenshot 2024-03-15 033218](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/assets/140955475/2949751f-9566-422b-90d0-a0ccac9db5b1)


![Screenshot 2024-03-15 033240](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/assets/140955475/302620f4-ad86-47ae-a83e-ec22811fc07e)


![Screenshot 2024-03-15 033301](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/assets/140955475/7e5ca505-70dc-407d-a5fb-0f83d19a7808)


![Screenshot 2024-03-15 033323](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/assets/140955475/12ac4c54-7cdb-4188-a437-f8d420a8588a)


![Screenshot 2024-03-15 033344](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/assets/140955475/92c0a9fe-58db-4581-a175-911c48e35c90)


![Screenshot 2024-03-15 033410](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/assets/140955475/bf7c3821-57e3-4512-8c39-d49d02cd1352)


![Screenshot 2024-03-15 033429](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/assets/140955475/542c37e6-c60a-4715-83ec-03e5de83df9d)


![Screenshot 2024-03-15 034057](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/assets/140955475/7a73ac6d-319e-454e-97e5-30daef9a3f6d)

![Screenshot 2024-03-15 034246](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/assets/140955475/8d684442-d2b2-45a8-b7fc-93774f9ee99e)


![Screenshot 2024-03-15 034414](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/assets/140955475/ceef16aa-6f70-4925-bc9f-cf18226e4659)


### Get familiar with open-source EDA Tools

<b>go to Terminal--</b>
```
vsduser@vsdsquadron:~/Desktop/work/tools/openlane_working-dir/openlane/$
```
* To invoke the openLane we have to use the two commands that are `docker` and `flow.tcl -interactive`. (interactive means all the process is done step by step.)
* Now the openLane has been invoked.
* The next command should be `package require openLane 0.9`..( this command basically imports all the packages which is required for the flow).
</br>So these 3 steps have to be done every time.
* The next command is `prep -design picorv32a` and after that, it will show <b>"Preparation complete"</b>.
* Here we actually prepare the design setup stage. we need to set up the file system specific to the flow. i.e. each & every step of the flow will be fetching files from a particular location. so that location needs to be created.
* `mergeLef.py` means it has merged both the `.lef` and `.tlef` files into one mergeLef.py file.
* After the <b>"Preparation complete"</b> state, we will first check if any directory is created in the picorv32 directory or not!!.. We will see that a `runs` directory is created in the picorv32 directory and inside the <b>"runs"</b> directory a folder with today's date will be created. inside this directory, all the folder structures that is required by openlane will be present.
* After checking <b>"runs"</b> directory, now we will give command as `run_synthesis`.
* After the synthesis is over, we will check how the result has been displayed in the <b>"runs"</b> directory.

![Screenshot 2024-03-15 081613](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/assets/140955475/2049f796-fcfb-4ead-b793-33f756e1a75c)

<b>This is the synthesis result. we calculated the flop ratio.</b>

![Screenshot 2024-03-15 095813](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/assets/140955475/a812755b-55c0-4d0b-8506-f4eebb1780ad)


## Day-2 Good floorplan vs Bad floorplan and introduction to library cells
### Chip floor planning considerations
###   Utilization factor and aspect ratio
  ![photo_2024-03-19_21-32-08](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/assets/140955475/2362f638-49d5-4dce-8d95-6bbee1d2eac5)

  The below diagram is just a basic netlist that consists of two flipflops( launch flop and capture flop) and some combinational logic in between them. The dimensions of the chip will mostly depend on the dimensions of the logic gates/ standard cells not the wires. Wires will play important role in further stages.

  ![Screenshot 2024-03-20 000318](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/assets/140955475/2facc477-3d64-4201-b3c9-41ae5996f3a8)

  We are placing everything in one plate and roughly calculating the area for this netlist. The minimum area occupied by the netlist wherever it is placed is 
  4 sq.unit.

  ![Screenshot 2024-03-20 001809](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/assets/140955475/6e0da4d8-cf20-48ec-944c-d68a181586a6)

  Now the circuit above should be built inside this piece of area and should not exceed this area.
  <br>The below diagram shows the Core & Die section of a chip.

  ![photo_2024-03-20_00-31-09](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/assets/140955475/6a850be8-b3e7-43c2-997a-65a4e9404073)
  
  <b>Utilization factor</b>
  <br>If the above logic circuit completely fits in the core area, and there is no space left to place the other cells, then the utilization factor is 100%.
  <br>if U.F = 0.8 that means 80% is used for placing the cells and 20% is used for routing.

  ![photo_2024-03-20_00-36-44](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/assets/140955475/e51befee-57fb-461c-ad95-7de824967579)
  Ideally the utilization factor should be 0.5 or 0.6.

  <b>Aspect Ratio</b>
  <br>It is defined as the ratio of height and width of core.
  <br>if <b>A.R>1</b>-- height > width
  <br>if <b>A.R<1</b>-- height < width
  <br>if <b>A.R=1</b>-- square shape

  ![Screenshot 2024-03-20 005805](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/assets/140955475/548ca271-461f-4220-9be8-62be13fdc1ef)

  25% of our chip is occupied with the initial netlist which is completely connected by idle wires which don't have any shape or size. The remaining 75% is available for all placing additional cells and routing.

  
  ### Pre-Placed cells

  Preplaced cells are manually positioned components in integrated circuit layouts, strategically placed before automated placement and routing. They often include custom-designed or critical functional blocks requiring precise placement for optimized performance, power, or area. Unlike standard cells, preplaced cells provide designers with greater control over layout, facilitating hierarchical design methodologies and aiding in achieving design closure. These cells are commonly used for interface circuits or specialized logic blocks, ensuring they meet strict timing, power, and signal integrity requirements. Preplaced cells enhance flexibility and enable designers to balance competing design objectives effectively in large-scale integrated circuit designs.
  These pre-placed cells will be implemented once and can be instantiated multiple times. They are part of netlist and receive some inputs and deliver output, but the functionality of these cells is that they are implemented only once. That's why we call it Pre-Placed cells because these cells are being placed only once in a chip.

  ![Screenshot 2024-03-20 030241](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/assets/140955475/2de21f30-a16b-4d63-bbca-9c4dbcf72d12)

![Screenshot 2024-03-20 033942](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/assets/140955475/cb470787-61e0-4557-aec3-3226eb1ca2c4)

These pre-placed cells have been placed in this area depending on the design scenario. So, the locations of these pre-placed cells are never touched while we go in the design cycle. So, the locations have to be very well defined.


### De-coupling capacitors  

We have to also surround the pre-placed cells with de-coupling capacitors.

![Screenshot 2024-03-20 063524](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/assets/140955475/42096025-ec94-4dba-8696-8c1ceb157695)

![Screenshot 2024-03-20 063817](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/assets/140955475/109b99f7-c129-41f7-909b-c1935952dd14)

![Screenshot 2024-03-20 112507](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/assets/140955475/ef011ba4-87ce-4d82-8d5c-8627269fa688)

If we are expecting a logic1, so we can't guarantee we will get logic1 due to the long physical distance from the power supply to this piece of circuit. we can solve this problem using a de-coupling capacitor.
<br>De-coupling capacitor is a huge capacitor that is completely filled with charge. The voltage stored in this capacitor is equivalent to the supply voltage. The de-coupling capacitor actually de-couples this circuit from the main supply. So, whenever the switching activity happens, the amount of current required by the circuit is provided by the de-coupling capacitor. that's why the de-coupling capacitor is placed close to the  circuit. And whenever there is no switching activity, the de-coupling spends its time replenishing its own charge.


![Screenshot 2024-03-20 112717](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/assets/140955475/e36c24b0-2b06-4fdd-a1a4-9f12c277b363)

![Screenshot 2024-03-20 112859](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/assets/140955475/1100e86f-0e10-425a-a163-2fb89d10d469)

So now it ensures that these pre-placed cells get their supply from these de-coupling capacitors. Now these blocks will behave as there will be no switching activity that will get missed, also there won't be any crosstalk issues, i.e there will be no problem where the logic1 will not be treated as logic1 because the de-coupling capacitors placed around these blocks will supply the required amount of charge to these blocks.
With this, we have taken care of the local communication.
<br>We will take care of Global communication with the help of power planning.


### Power Planning


