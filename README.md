# NASSCOM-VSD-SOC-DESIGN

# Advanced-Physical-Design-Using-OpenLANE-Sky130
* [Day_1 - Inception of Open-Source EDA, OpenLANE & Sky130PDK](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/blob/main/README.md#day_1-inception-of-open-source-eda-openlane--sky130pdk)
* [Day-2 - Good floorplan vs Bad floorplan and introduction to library cells](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/blob/main/README.md#day-2-good-floorplan-vs-bad-floorplan-and-introduction-to-library-cells)
* [Day_3 - Design library cell using Magic Layout and ngspice characterization](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/blob/main/README.md#day_3-design-library-cell-using-magic-layout-and-ngspice-characterization)

## Day_1 Inception of Open-Source EDA, OpenLANE & Sky130PDK

### How to talk to Computers

* The `Arduino Leonardo` is a microcontroller board based on the `ATmega32u4`. It has `20 digital input/output pins` (of which 7 can be used as PWM outputs and 12 as analog inputs), a 16 MHz crystal oscillator, a micro USB connection, a power jack, an ICSP header, and a reset button. It contains everything needed to support the microcontroller; simply connect it to a computer with a USB cable or power it with an AC-to-DC adapter or battery to get started.
* The Leonardo differs from all preceding boards in that the ATmega32u4 has built-in USB communication, eliminating the need for a secondary processor. This allows the Leonardo to appear on a connected computer as a mouse and keyboard, in addition to a virtual (CDC) serial / COM port.


![leonardo aurdino](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/assets/140955475/362391d9-c07f-477b-866c-8177abbcc20b)

[leonardo pinout.pdf](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/files/14595132/leonardo.pinout.pdf)
</br>[leonardo schematics.pdf](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/files/14595134/leonardo.schematics.pdf)


<b>The below block diagram describes this Arduino Board.</b>
<br>The circled section is the processor/SOC and along with it we have all the interfaces-- `JTAG-UART`, `QSPI1-Flash`, `I2C0EEPROM`, `VCC/GND`, `ADC(QSPI3)`, `SDRAM(external chip)`
![block diagram of Arduino](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/assets/140955475/f41e7b48-4593-4fd2-a179-a3376aa07dbb)

* When we open up this particular IC, the whole thing is called a package which is shown below.
* The pin locations of this particular package are all driven by the Arduino board that we are trying to design. The size of this package is `7mm x 7mm`.
* The chip basically sits at the center of the package and the connection of the chip with the package is shown as wire bonds this way we will be able to transfer all these signals coming from the outside world to the interior of the chip.

![chip connect to package](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/assets/140955475/0e33ba91-14c5-4101-a6b8-c7d07d6005a5)

The chip will have various components--
1. `PADS` - A pad is the exposed region of the metal on a circuit board that the component lead is soldered to. Pads are the points through which the connection of peripherals on a board is made with the chip/processor, and the transfer of data takes place. Any signal can go outside or inside the chip through pads.

2. `CORE` - A core in a chip is a well-partitioned piece of logic capable of independently performing all functions of a processor. All the digital logic building blocks are present here.

3. `DIE` - This is what gets manufactured on the silicon wafer.
  
4. `FOUNDRY IP'S` - A foundry is a company that provides IC (integrated circuit) manufacturing services - basically, you give them your design, and they manufacture the chip for you. Intellectual Property. In this context, it’s the design of the parts of a chip. Nowadays, many chips are not wholly designed by the company that’s “designing” them.
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

5. `MACROS` -  Macro is an essential component in the VLSI design cycle before the final packaged chip is ready to use. Macro cells are the memory cells and intellectual property that an analog design team has designed. To break down the understanding of macro cells, consider macro cells as pieces of logic blocks, mainly intellectual properties (IP), which can be used in a design without the need to building them from scratch. Thus, these memory cells are instrumental in reducing the total time for the design engineers that are required to complete their entire design.

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

1. `RTL IPs` - RTL IP refers to a product in electronic format that represents an integrated circuit function that can be instantiated in an integrated circuit design. An IP Core is a product in an electronic format that represents an integrated circuit function that can be instantiated in an integrated circuit design, including the circuits and modules of such integrated circuit design(s) and associated firmware, application programming interfaces, Software drivers, application-specific Software, and all register transfer language (RTL), Verilog, and other source materials to instantiate, modify, support, and maintain any of the foregoing.

2. `EDA Tools` - Electronic Design Automation (EDA) tools are software tools used to design electronic systems such as integrated circuits and printed circuit boards. They have three key functions: simulation, design, and verification. EDA tools work together in a design flow that chip designers use to design and analyze entire semiconductor chips. They allow teams to predict circuit behavior, assemble circuit elements, and anticipate chip performance. EDA tools are used to verify that a design will meet all the requirements of the manufacturing process, known as design for manufacturability (DFM). Deficiencies in this area can cause the resultant chip to either not function or function at reduced capacity, and there are reliability risks.

3. `PDK` - It stands for <b>Process Design Kit</b>. It is basically an interface between FAB and the designers which is a collection of files used to model a fabrication process for the EDA tools used to design an IC.
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

We are actually interested in working in `openlane_working_dir` directory.

![image](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/assets/140955475/ddc1ded4-ea52-4f7e-b675-4cc84e00fbaa)

</br> But let's explore what is inside the `vsdflow` directory--

![Screenshot 2024-05-01 170426](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/assets/140955475/e2e60ebb-c13c-4f82-9c62-e0e926dcfcd8)
 

![Screenshot 2024-05-01 170645](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/assets/140955475/a15b7812-8b93-49a4-87ca-f9f6c93823f1)


* Actually in the `openlane_working_dir`, there will be two directories-- `openlane` and `pdks`.

![Screenshot 2024-05-01 175007](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/assets/140955475/c265bdf5-79d0-437c-a4ff-e1a3216946af)

* **pdk** stands for "process design kit".
* In the above  `openlane` directory, we will be actually doing everything.
* Coming to the `pdks` directory, This folder has all the information related to the pdk. The pdk which we will be using for this workshop is `SkyWater 130nm pdk`. This was recently made open source. So openlane is built around this pdk.
* In the `pdks` directory, there is a `skywater-pdk` folder which has all the pdk related files (timing libraries, lef files). So all these silicon foundry files are compatible to work with commercial EDA tools and not with open-source EDA tools.

![Screenshot 2024-05-01 180333](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/assets/140955475/fcb85180-e3ba-4ec6-8ccd-fe25eebc6bba)


* So `open_pdks` basically plans to solve that problem. They are basically sets of scripts and files that convert these foundry-level pdks to be compatible with open-source EDA tools like Magic, netgen, etc.
* `Sky130A` is that pdk that has been made compatible to work with an open-source environment.
* Inside the `Sky130A` directory we will see two directories-- `libs.ref` and `libs.tech`.

![Screenshot 2024-05-01 181429](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/assets/140955475/e6009640-9e5b-4f6e-88e9-a8f7ad864e57)

* **libs.ref** contains all the technology-specific files and **libs.tech** contains tools-specific files.

![Screenshot 2024-05-01 180658](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/assets/140955475/96821a62-eae1-4910-baa9-4cff91080601)

Inside the libs.ref directory, we will be working with sky130_fd_sc_hd  which contains all the technology files.

* Now, we will actually start our labs from the `openlane` directory.

```
vsduser@vsdsquadron:~/Desktop/work/tools/openlane_working-dir/openlane/$
```
* To invoke the openLane we have to use the two commands that are `docker` and `flow.tcl -interactive`. (interactive means all the process is done step by step.)
* Now the openLane has been invoked.
* The next command should be `package require openlane 0.9`..( this command basically imports all the packages which is required for the flow).
* </br>So these 3 steps have to be done every time.

* All the designs that are run by the openlane are extracted from the `designs` folder present inside the `openlane` directory.
* These are all the designs already built-in in the openlane. but we will be doing it for `picorv32a`.

![Screenshot 2024-05-01 183746](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/assets/140955475/89a070d9-6153-4fd2-bc7c-7ceb80fc7035)

* The `picorv32a` directory will have `src` folder and `config.tcl` file.
* The "src" folder will contain `.v` and `.sdc` files inside it.

![Screenshot 2024-05-01 185018](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/assets/140955475/b6f15e16-85ac-4370-9601-c7e2923b4205)

* `config.tcl` bypasses any configurations that have been already done into openlane.
* command to open config.tcl file--
```
$ less config.tcl
```
* In the config.tcl file, we actually set the design_name, Verilog files, sdc files, clock period, clock port, and filename variable and then we source this file. if suppose clock period is set to any other value in the flow as default, then we can override that value using config.tcl file.
* So, the precedence in which the openlane takes the value is first the "default value already set in the openlane", and second is the "value set in the config.tcl file" and the third is the "sky130A_sky130_fd_sc_hd_config.tcl" which means the last one has the highest priority.

**Now coming back to openlane--**

* The next command is `prep -design picorv32a` and after that, it will show `Preparation complete`.
* Here we actually prepare the design setup stage. we need to set up the file system specific to the flow. i.e. each & every step of the flow will be fetching files from a particular location. So that location needs to be created.
* `mergeLef.py` means it has merged both the `.lef` and `.tlef` files into one mergeLef.py file.
* After the **Preparation complete** state, we will first check if any directory is created in the picorv32 directory or not!!.. We will see that a `runs` directory is created in the picorv32 directory and inside the <b>"runs"</b> directory a folder with today's date will be created inside this directory.

![Screenshot 2024-05-01 194254](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/assets/140955475/17b29de1-69d8-4fa4-8c01-27e9705a59df)

  * Inside this directory, the folder structure required by the openlane will be present.
  * As per now except for the "tmp" folder, rest all the folders will be empty.
  * `tmp` folder is where temporary files are stored.
  * `results` folder is present for each of the stage. So as per now, nothing has been  run, so there will be nothing inside this folder.
  * `reports` folder will contain the report inside each of the stages in the flow.
  * The `config.tcl` file present here shows which all parameter is being taken by the run. i.e. "pdk", "lef information", "tracks information", "tlef information", "library information". So if we make any changes in the original configuration file, after running the floorplan, that will be updated here in this config.tcl file.
  * The `cmds.log` file takes the record of all the commands that we have used.

* After checking <b>"runs"</b> directory, now we will give command as `run_synthesis`.

![Screenshot 2024-03-15 081613](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/assets/140955475/2049f796-fcfb-4ead-b793-33f756e1a75c)

* After the synthesis is over, we will check how the result has been displayed in the <b>"runs"</b> directory.

<b>This is the synthesis result. we calculated the flop ratio.</b>

![Screenshot 2024-03-15 095813](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/assets/140955475/a812755b-55c0-4d0b-8506-f4eebb1780ad)

* To get more information about the openlane refer to this--
</br> [open lane/efabless github](https://github.com/efabless/openlane2)
</br> [open lane document](https://openlane2.readthedocs.io/en/latest/index.html)


## Day-2 Good floorplan vs Bad floorplan and introduction to library cells
### Chip floor planning considerations

### Utilization factor and aspect ratio
  ![photo_2024-03-19_21-32-08](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/assets/140955475/2362f638-49d5-4dce-8d95-6bbee1d2eac5)

  The diagram shown below is just a basic netlist that consists of two flip-flops ( launch flop and capture flop) and some combinational logic in between them. The dimensions of the chip will mostly depend on the dimensions of the logic gates/ standard cells, not the wires. Wires will play an important role in further stages.

  ![Screenshot 2024-03-20 000318](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/assets/140955475/2facc477-3d64-4201-b3c9-41ae5996f3a8)

  We are placing everything in one plate and roughly calculating the area for this netlist. The minimum area occupied by the netlist wherever it is placed is 
  4 sq. unit.

  ![Screenshot 2024-03-20 001809](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/assets/140955475/6e0da4d8-cf20-48ec-944c-d68a181586a6)

  Now the circuit above should be built inside this piece of area and should not exceed this area.
  <br>The below diagram shows the Core & Die section of a chip.
  * `Core` is the section of the chip where the fundamental logic of the design is placed.
  * `Die` which consists of a core, is a small semiconductor material specimen on which the fundamental circuit is fabricated. 

  ![photo_2024-03-20_00-31-09](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/assets/140955475/6a850be8-b3e7-43c2-997a-65a4e9404073)
  
  <b>Utilization factor</b>
  <br>If the above logic circuit completely fits in the core area, and there is no space left to place the other cells, then the utilization factor is 100%.
  <br> If U.F = 0.8 that means 80% is used for placing the cells and 20% is used for routing.

  ![photo_2024-03-20_00-36-44](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/assets/140955475/e51befee-57fb-461c-ad95-7de824967579)
  Ideally, the utilization factor should be 0.5 or 0.6, and the remaining area used for optimization purposes, i.e. to add any buffers or extra cells, routing etc.

  <b>Aspect Ratio</b>
  <br>It is defined as the ratio of height and width of the core.
  <br>if <b>A.R>1</b>-- height > width
  <br>if <b>A.R<1</b>-- height < width
  <br>if <b>A.R=1</b>-- square shape

  ![Screenshot 2024-03-20 005805](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/assets/140955475/548ca271-461f-4220-9be8-62be13fdc1ef)

  25% of our chip is occupied with the initial netlist which is completely connected by idle wires which don't have any shape or size. The remaining 75% is available for all placing additional cells and routing.

  
  ### Pre-Placed cells

  Preplaced cells are manually positioned components in integrated circuit layouts, strategically placed before automated placement and routing. They often include custom-designed or critical functional blocks requiring precise placement for optimized performance, power, or area. Unlike standard cells, preplaced cells provide designers with greater control over layout, facilitating hierarchical design methodologies and aiding in achieving design closure. These cells are commonly used for interface circuits or specialized logic blocks, ensuring they meet strict timing, power, and signal integrity requirements. Preplaced cells enhance flexibility and enable designers to balance competing design objectives effectively in large-scale integrated circuit designs.
  These pre-placed cells will be implemented once and can be instantiated multiple times. They are part of the netlist and receive some inputs and deliver output, but the functionality of these cells is that they are implemented only once. That's why we call it Pre-Placed cells because these cells are being placed only once in a chip.

  ![Screenshot 2024-03-20 030241](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/assets/140955475/2de21f30-a16b-4d63-bbca-9c4dbcf72d12)

![Screenshot 2024-03-20 033942](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/assets/140955475/cb470787-61e0-4557-aec3-3226eb1ca2c4)

These pre-placed cells have been placed in this area depending on the design scenario. So, the locations of these pre-placed cells are never touched while we go in the design cycle. So, the locations have to be very well-defined. Similarly, there are other IPs also available like memory, clock gating cell, comparator, and mux. So all of them can be implemented once and can be instantiated multiple times onto a netlist. That's why they are called preplaced cells because these cells are just placed once in the chip. The arrangement of these IPs in a chip is referred to as floorplanning. We have to define the locations of these cells before automated placement and routing. Automated placement and routing tools place the remaining logical cells in the design onto the chip.


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

![Screenshot 2024-05-05 164856](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/assets/140955475/4fb0ab23-a28d-4a43-872e-1d8f2da378e6)

![Screenshot 2024-05-05 170249](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/assets/140955475/fba7e5ca-a0eb-4b37-ac9d-9db71dd0839a)

When we say one particular line of 16-bit bus is logic 1 it says that the capacitor is being charged to Vdd, and whenever we say logic 0 it says that the capacitor is discharged to ground. Let consider this 16-bit bus connected to an inverter. So, all the capacitors that are initially charged will get discharged and vice-versa due to the inverter.

![Screenshot 2024-05-05 170606](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/assets/140955475/d513c5d5-4af0-4360-a979-ec16967824e1)

But the problem here is that all capacitor is connected to a single ground. This will cause a bump in the 'ground' tap point during discharging. That bump is called a "Ground Bounce". If the size of the bump exceeds the noise margin level might enter into an undefined state (can either go to logic 1 or logic 0). 

![Screenshot (68)](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/assets/140955475/45917359-0d04-491a-9633-11f53d298b2b)
 
A similar concept when capacitor which are at "0 volts" will have to charge to "V volts" through a single Vdd tap point. This is known as a "voltage droop" because all of them are demanding current at the same time. And as long as this voltage level is below the noise margin level rather than going to an unpredicted state(either 0 or 1), there is not an issue.
<br> The only solution of this problem is to have a multiple power supply. So, each block will take charge from the nearest power supply and similarly, dump the charge to the nearest ground. This type of power supply is called "Mesh". That's the reason there are multiple power supplies in the recent chips.

![Screenshot (69)](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/assets/140955475/fe0fbf05-31af-457f-802d-3455dce9e024)

This is the complete power planning of the chip---

![Screenshot (70)](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/assets/140955475/f936722e-d914-4cb7-b474-c1c6e4908192)


### Pin Placement and logical cell placement blockage

**Pin Placement**


Let's say, this is the design that we are looking to implement. The first circuit is driven by clk1 and the second circuit is driven by clk2. Both the circuits have different inputs Din1 and Din2 respectively and outputs as Dout1 and Dout2 respectively. Along with it, we have some pre-placed cells "block a" and "block b". So, we have 4 inputs ports (Din1, Din2, clk1, clk2) and 3 output ports (Dout1, clkOut, Dout2). 

![Screenshot (71)](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/assets/140955475/b8643367-5a92-4178-9e15-39628824970a)

let's there be one more section of the design that needs to be implemented as shown below--

![Screenshot (72)](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/assets/140955475/e98976a5-ffa5-4f37-9909-22f4f3a0c789)

So, this is the complete design "netlist"--

![Screenshot (73)](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/assets/140955475/c34cf6b1-caea-419b-8e33-02219cd0a635)

Now, this netlist will be placed in the chip that we are trying to design. The frontend team will decide the input and output connectivity while the backend team will do the pin placements. So, according to the pin placements, we have to locate the pre-placed cells on the chip. Also, we can observe is that the clock ports are bigger in size than the data ports because the input clk has to continuously provide the signals  to every elements of the chip, and the output clk has to take out the signals. So, both the call has to work at a fast rate. The main reason for the bigger size is to provide a less resistance path to the clock ports.

![Screenshot (74)](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/assets/140955475/c5b87e94-e3bb-4d20-a74e-0b71aa6a340a)

One more thing to be taken care of is this pin placement area is blocked for routing and cell placements, for that we need to do logical cell placement blockage.
</br>Now the floor plan is ready for the Placement and Routing step.


### Steps to run floorplan using Openlane

In the "configuration" folder present inside the "openlane" directory, there are "tcl" files and a "readme" file present. In the `readme.md` file,  we can see variables which are required for each stage i.e. synthesis, floorplan, placement, CTS, routing, etc. which can be set with the synthesis flow. In the `floorplan.tcl` file, there are basically the default parameters that are set for the floorplan stage in Openlane.

![Screenshot 2024-05-05 182303](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/assets/140955475/ef46d2a7-af7a-4dd1-81b7-30f9ebd8a8e4)

README.md file
![Screenshot 2024-05-05 182445](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/assets/140955475/2b0170e8-3c37-441c-a43f-a94ad5a23f76)

floorplan.tcl file
![Screenshot 2024-05-05 183216](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/assets/140955475/6f64894b-aabd-4502-8868-04e567e06632)

So, the first priority is for `floorplan.tcl` file, then for `config.tcl` file(present inside the picorv32a folder) and the next priority is `sky130A_sky130_fd_sc_hd_config.tcl`. 
</br>Now, after the synthesis is successful using `run_synthesis` command, we will give the command as `run_floorplan` and get the following output as shown below--

![Screenshot 2024-05-05 185504](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/assets/140955475/1865833c-61be-4d25-82a9-34e214381ce7)


### Review floorplan files and steps to view floorplan


![Screenshot 2024-05-05 190403](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/assets/140955475/77f001e1-4e26-463e-b745-d1176fe7cc3d)

To see all the configurations that are taken by the flow, we need to open "config.tcl" file which is present in the "runs" folder.

![Screenshot 2024-05-05 192507](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/assets/140955475/c4df1e6a-a39f-4bf0-ba1b-a196c3e4814b)

![Screenshot 2024-05-05 192822](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/assets/140955475/2e2fb43d-6d2d-4e23-8c15-a45f0edfa1de)

So, config.tcl has overridden the system defaults.
* Now, we will see the `floorplan` folder in the `results` directory, where a def(design exchange format) file is present. Inside this file, we can see all the information about die area.

![Screenshot 2024-05-05 194923](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/assets/140955475/2ad0daab-c4ae-47c2-a809-a2ca3a486ee1)

But looking at the "def" file doesn't make any sense. So, to see the actual layout after the flow plan, we do it in "Magic" using the command--
```
$ magic -T /home/kunalg123/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.floorplan.def &
```
"&" actually free up the prompt when magic launches.

### Review floorplan layout in Magic

* After the Magic gets launched, we will get something like as shown below--

![Screenshot 2024-05-05 200318](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/assets/140955475/23aa3ea1-ad9a-4c8c-98d9-074a952a7556)

To select on the screen, press "s", the object will get highlighted.
</br> To zoom in -- (click on object + press "z")
</br> To zoom-out -- (shift + z)
</br> We had set the i/o pins mode as 1 which means setting it equidistant.
</br> If we want to check the location or to know at what layer it is available, we have to open the "tkcon" terminal present and type `what` which will show all the details of that particular pin.

![Screenshot 2024-05-05 202617](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/assets/140955475/788e0781-17b0-4ad1-b955-44fcccf6c53b)


### Library building and Placement
### Netlist binding and initial place design

![Screenshot 2024-05-05 205301](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/assets/140955475/453e4674-4c49-4d07-9da2-b73de9371519)

**Binding netlist**
Actually, we have all of these gates in the square box shape with physical dimensions i.e. height and width. and these all cells and gates are placed at a place called "library". The library can be divided into two sublibraries- one library has information about shape and size, while the other library has information about delays.
</br>Library has different flavors for each and every cell. i.e. same cells can have bigger sizes on different shelves. So, the bigger the size of the cell, the lesser the resistance path, and will work faster and have a lesser delay.
</br> The below diagram shows different flavors of the same cell, and we can pick whatever we want based on the timing conditions and space on the floorplan--
![Screenshot 2024-05-05 210517](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/assets/140955475/70cb6d41-6e7d-430a-8747-86589e32c7da)

**Placement**
Once we have given proper shape and size to each and every gate, the next step is to take those particular shapes and sizes and place it on the floorplan. We have the floorplan with input and output ports, we have particular netlist, and we have particular size given to each component of this netlist. So we have the physical view of the logic gates. The next step is to place the netlist onto the floorplan using the  connectivity information from the netlist and design the physical view. The placement should be such that the pre-placed cell locations should not be affected and also no cell should be placed over the pre-placed cells. We need to place the physical view of the netlist onto the floorplan in such a fashion that logical connectivity should be maintained and that particular circuit should interact with their input and output ports  to maintain minimal timing and delay.

![Screenshot (77)](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/assets/140955475/01c16d1c-c9f0-4c2a-b081-510810e03305)

Now, we will go for optimized placement to reduce the distance between cells.

**Optimize placement using estimated wire length and capacitance**

If we look at the capacitance from Din2 to FF1 it is huge because the wire length is huge, in that case even the resistance will also be huge. If we send the signal from Din2 then it will be difficult for FF1 to catch that input because the distance is large. So we can place some intermediate steps to maintain the signal integrity. By this, the input is successfully driven to the FF1 from Din2. These intermediate steps are called here Repeaters. Repeaters are basically buffers that will recondition the original signal. By using repeaters, we resolve the problem of signal integrity but there will be a loss of area because more and more repeaters are used, so more area will be used in the particular floorplan.

* In stage 1, there is no need for any repeater to transmit the signal.

![Screenshot (78)](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/assets/140955475/5f3ae9a8-f341-44f8-8d1e-444161f3d09b)

* stage 2

![Screenshot (79)](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/assets/140955475/602f994e-e048-4854-b2c0-83d6e0d0cc7c)

* stage 3

![Screenshot (80)](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/assets/140955475/79880af1-1f48-484a-af3b-77aa3c5b991f)

* stage 4

![Screenshot (81)](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/assets/140955475/d672ac8f-cd4b-4ccc-b785-e11f52a5fe81)

Next what we have to do is to check whether what is done is correct or not by just checking the datapath. Considering the clocks are ideal, i.e. the time required for clk to reach any of the flipflops is 0. So, let's quickly do a  setup timing analysis and based on this, we will come to know whether the placement done is reasonable or not.

### Need for libraries and characterization

* Every IC design flow needs to go through several steps-- 
    * The first step is `Logic Synthesis` (converting the functionality into legal hardware). The output of the logic synthesis is the arrangement of gates that will represent the original functionality that has been described using an RTL.
    * The next step is `Floorplanning` where we import the output of logic synthesis and decide the size of the Core and Die.
    * The next step after floorplanning is `Placement`, where we take the particular logic cell and place it on the chip in such a fashion that initial timing is better.
    * The next step is `CTS(Clock tree synthesis)`, where we take care that clk should reach each and every signal at the same time also take care that each clk signal has equal rise and fall.
    * The next step is `Routing`, routing has to go through a certain flow dependent on the characterization of the flip flop.
    * The last step is `STA(Static timing analysis)`, where we try to set the set-up time, hold time, and maximum achieved frequency of the circuit.
    * One common thing across all stages is "GATES or Cells".

  ### Congestion aware placement using RePlAce

  We are actually doing a congestion-related placement, not considering the timing.
Placement in openlane occurs in two stages- `global placement`(the main purpose is to reduce the wire length) and `detail placement`.
Legalization happens in detail placement, i.e. the standard cells are placed in standard cells rows, they have to be exactly inside the rows and there should be no overlaps.

</br>Use the command --
```
run_placement
```
Now the placement is done, and we want to see how the design is post-placement. So, go to the placement directory and use the command--
```
$ magic -T /home/kunalg123/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.placement.def &
```
we will get the following placement output in "Magic" as shown below--

![Screenshot 2024-05-06 133525](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/assets/140955475/21c21e31-dcfc-438d-9529-506af543220e)

These many standard cells were actually at the initial layout of the floorplan. If we zoom in, we can see the placement of the standard cells in the standard cell rows.

![Screenshot 2024-05-06 134149](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/assets/140955475/5c8353b1-1e6c-446f-829f-b7446d14fd98)

### Cell design and characterization flow
### Inputs for cell design flow

![Screenshot (82)](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/assets/140955475/70ef2ff2-5369-484a-bec5-aa0b499e6372)

If we look into one of the inverters from the library, it has to go through a cell design flow which is divided into three parts-- `inputs`(needed to design the inverter), `design steps`, and `outputs`(used by the EDA tools) so that it is easily understandable by the EDA tools.
</br> Inputs required for the cell design are PDKs: DRC & LVS rules(provides a tech file with some rules by foundry), SPICE models(gives information about threshold voltage equation along with the required parameters), library & user-defined specs.

### Circuit design step

* There are some user-defined specifications:
  * The separation between the power rail and the ground rail defines the `cell height`, and it is the responsibility of the library developers to maintain the cell height.
  * The `cell width` depends on the timing and drive strength(if a cell has got a drive length of 10, it will be able to drive even more longer wires).
  * A typical inverter has to operate at a certain supply voltage which is being provided by the top-level designers and according to that the library developers have to take that supply voltage and design the library cell in such a fashion that it operates at this particular supply voltage and take care of the noise margin levels w.r.t this supply voltage.
  * There is a specification that certain libraries have to be built on certain metal layers themselves.
  * The library developers need to decide on the pin location according to the requirement.
  * For a typical gate length of 130 nm, the drawn gate length could be 125nm or 130 nm based on the library and user-defined specifications given to us.
 
* Now, coming to the design steps, since all the inputs are available with the library developers, it's their responsibility to take these inputs and based on these inputs, come up with a library cell that adheres to these inputs, so that when this particular library cell is getting plucked into the top level design i.e. in the CTS stage, placement stage, routing stage, it should not be a problem for the physical design steps.
* Design involves three different steps-- 
  * `Circuit design` - To design the pmos and nmos transistor in such a fashion that all the required values are maintained, the circuit design step is mainly based on spice simulation. The output that we get from the circuit design is called CDL(circuit description language).
  * `Layout design` - once we do the circuit design, i.e. we know the value of the transistor parameters, we need to implement these values as a layout. The first step is to get the function implemented through a MOS transistor. The second step is to get a pmos and nmos network graph and then to obtain the Euler's path)
 
![Screenshot 2024-05-06 150408](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/assets/140955475/7c505d34-491b-4c05-b3a8-bdac8f4796f2)

 After the stick diagram, the next step is to convert this stick diagram into a typical layout according to the DRC rules (given by the foundry) and the user-defined specifications using the "Magic tools".
The final step is to extract the parasitics of this particular layout and characterize it in terms of timing, so before that, the output of the layout design will be GDSII, LEF, and extracted spice netlist(.cir).
  * `Characterization` is the step that will help to get the timing, noise, and power information and the output of the characterization is the timing,noise,power(.libs), and the functionality of this particular circuit.
Let's say we did circuit design and layout design. So out of that, the inputs available with us let's say is the layout of the buffer using the "Magic" tool. Also, we have the description of the buffer along with the power sources connected.
Whatever we see in that particular layout, metal layers, contacts, each and every element will have associated resistances and capacitances with them. So we have actually extracted them in terms of spice netlist.

### Typical Characterization Flow
* Characterization Flow--
  * The first step is to read the model file which comes out of the foundry.
  * The second step is to read the extracted spice netlist.
  * The third step is to recognize the behavior of the buffer.
  * The fourth step is to read the sub-circuits of the inverters.
  * The fifth step is to attach the necessary power source.
  * The sixth step is to apply the stimulus.
  * The seventh step is to provide necessary output load capacitances.
  * The eighth step is to provide the necessary simulation commands.
 
  ![Screenshot 2024-05-06 155546](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/assets/140955475/1812935a-856b-4d17-957b-4070b2fed492)

  Now, the next step is to feed these all 8 steps in the form of a configuration file to the characterization software called "GUNA" and this software will generate timing, noise, and power models.

  ![Screenshot 2024-05-06 155838](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/assets/140955475/74f59359-0b00-4600-b814-a93310ff311c)

### General Timing Characterization Parameters
### Timing threshold definitions

* As seen in the previous section we have inverters connected back to back, we have power sources, and we have the stimulus applied to the inverter, all these things bring a very important point of understanding different threshold points of a waveform itself and it is called as "Timing threshold definitions'. 
* These are the inputs or variables related to any waveform we see.

![Screenshot 2024-05-06 160849](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/assets/140955475/1f4e17fa-e1b2-4ad3-b50e-2b615d0d36c3)

* **slew_low_rise_thr** -- its typical value is around 20%  from the low power supply.
* **slew_high_rise_thr** -- its typical value is 20% point from the top power supply.
![Screenshot 2024-05-06 163347](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/assets/140955475/e5b17bda-6115-47cf-b94b-143f18e7d9ee)

* **in_rise_thr** -- typical value is 50%.
* **out_rise_thr** -- typical value is 50%.
* **in_fall_thr** -- typical value is 50%.
* **out_fall_thr** -- typical value is 50%.
we need two points on the graph i.e. on the input waveform and the other on the output waveform to calculate the rise delay and the fall delay.
![Screenshot 2024-05-06 164313](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/assets/140955475/6fceed1f-3567-4bad-91fd-d91ed7cb9d73)
![Screenshot 2024-05-06 165112](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/assets/140955475/5a19f066-f879-45cb-b318-aa4fde9f813d)

### Propagation delay and transition time

**To calculate the timing delay**.
```
time delay = time(out_*_thr) - time(in_*_thr)
```
Now, let's take a sample waveform where the "in_rise_thr" and "out_fall_thr" is kept at 50%.

![Screenshot (89)](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/assets/140955475/e5ebef53-aa1a-4c0c-a210-1098f42a72a5)

But, let's suppose the threshold points are changed, then we will observe a negative delay. The reason behind having a negative delay is the poor choice of the threshold point. So, the choice of threshold point is really important.

![Screenshot (90)](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/assets/140955475/cf6b8292-3fab-4879-b145-974bf6c645db)

**To calculate the transition time**
```
For the rising waveform--
transition time = time(slew_high_rise_thr) - time(slew_low_rise_thr)

For the falling waveform--
transition time = time(slew_high_fall_thr) - time(slew_low_fall_thr)
```
![Screenshot 2024-05-06 172854](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/assets/140955475/4c10186e-8da6-4a4f-8569-e1e9adb7da78)

## Day_3 Design library cell using Magic Layout and ngspice characterization
### Labs for CMOS inverter ngspice simulations
### IO placer revision

* Now we will be going in depth into one of the cells. In our example, we took an inverter. We will be doing the post-layout simulation and post-characterizing our sample cell. We would be plugging this cell in the openlane flow in the picorv32 core and we will see whether it happens or not.
* Till now, we did till floorplan.
* So one of the features of openlane mentioned earlier is that if we make changes on the fly, let's say we have set our core utilization as 50% and we want to make it 65% and then run the flow again.
* If we want to change the configuration of how our input output pins are aligned around the core (earlier it is equidistant which we had set).
* `IO placer` is one of the opensource eda tools which is used to place io around the core.
* In the `floorplan.tcl` file, io mode has been set as 1 which means equidistant and we want to change it.
* So go to the terminal section after doing floorplan and give the command as
```
% set ::env(FP_IO_MODE) 2
```
* Now, again execute the `run_floorplan` command. We will see that the "def" file has been updated which can be verified as it shows the time of updation.
* Now let us check the floorplan using the "Magic" tool by giving the command as--
```
$ magic -T /home/kunalg123/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.floorplan.def &
```
then we will observe that the pins are not equidistant anymore. So.if we want to make the change in the openlane flow, we have to reset the variable and then run the floorplan again for the values to make a change.

###Spice deck creation for CMOS inverter

* **VTC-SPICE simulation**: the first step is to create a SPIKE deck which is a piece of connectivity information about the netlist. It has inputs which are provided to the simulation and the deck points that will take the output.
* **Component connectivity**: here, we need to define the connectivity of the substrate pins. Substrate pins tunes the threshold voltage of the PMOS and NMOS.
* **Component values**: we have taken the same size of both the PMOS and NMOS.

![Screenshot 2024-05-06 185750](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/assets/140955475/0103d945-abaa-4724-af3b-9a8e0f447ea3)

* **Identifying the nodes**: these nodes are required to define the netlist.

![Screenshot 2024-05-06 185928](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/assets/140955475/f7bc6d96-f968-4f95-af79-0dd5681ea6ad)

* **Naming the nodes**

![Screenshot 2024-05-06 190956](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/assets/140955475/0f370c41-16ba-41ba-b8fe-9db322fa1a69)

* The SPIKE deck is written in the format shown below:
<pre>
Drain-Gate-Substrate-Source
</pre>

![Screenshot 2024-05-06 193137](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/assets/140955475/4d59531d-09fb-4db5-9729-89bf26c1c4ca)

### Spice simulation lab for CMOS inverter

Now, we will describe the connectivity information of the other components of the above circuit.

![Screenshot 2024-05-06 194631](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/assets/140955475/451574bd-3f37-4289-9f48-4b41fc74937a)

Now, we will give the simulation commands in which we are swiping the Vin from 0 to 2.5 with the stepsize of 0.05 because we want to observe Vout while changing the Vin.

![Screenshot 2024-05-06 194820](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/assets/140955475/78455f0b-cf4e-4fff-8033-1d6ef8e361d1)

The final step is to describe the model files which will have a complete description of NMOS and PMOS.

![Screenshot 2024-05-06 195139](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/assets/140955475/ceabf7e8-e2f2-487e-8ab9-373cbe637ed8)

![Screenshot 2024-05-06 195608](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/assets/140955475/c8a3f7ab-ad74-48f1-9916-c0f40f3e3058)

Now, we will do the SPICE simulation for the given parameters using "ngspice" tool and observe the waveform--

`Wn=Wp=0.375u, Ln=Lp=0.25u, Wn/Ln=Wp/Lp=1.5`
![Screenshot 2024-05-06 195954](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/assets/140955475/dba36b3d-a0b9-4c20-9565-c68891333e32)

`Wn=0.375u, Wp=0.9375u Ln=Lp=0.25u, Wn/Ln=1.5, Wp/Lp=3.75`
![Screenshot 2024-05-06 200751](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/assets/140955475/04ce6270-a05e-4f73-a739-115a9e21d867)

The difference between these two graphs is that in the second graph, the transfer characteristic lies exactly in the middle of the graph, whereas in the first graph, it lies in the left part.

### Switching Threshold Vm

* These two models of different widths have their own applications. By comparing both waveforms, we can see that the shape of both waveforms is the same irrespective of the voltage level. It tells that CMOS is a very robust device. when Vin is at low, output is at high and when Vin is at high, the output is at low. so the characteristic is maintained at all kinds of CMOS with different sizes of NMOS or PMOS. That is why CMOS logic is very widely used in the design of gates.
* The switching threshold, Vm (the point at which the device switches the level) is one of the parameters that defines the robustness of the Inverter. Switching thresold is a point at which Vin=Vout.

![Screenshot (92)](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/assets/140955475/31774edf-798a-4bc2-8f52-aebfc5cb66c2)

Vm is a very critical point for CMOS because, at this point, there is a chance that both PMOS and NMOS get turned on. If both are turned on, then there may be a high chance of leakage current.
* The below graph identifies the region of PMOS and NMOS.

![Screenshot 2024-05-06 204032](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/assets/140955475/67b2cf2b-d366-4427-af27-df5f4cf10ec7)

### Static and Dynamic simulation of CMOS inverter

In Dynamic simulation we will know about the rise and fall delay of CMOS inverter and how does it varies with Vm. In this simulation, everything else will remain same except the input that is provided will be a pulse and the simulation command will be `.tran`.

![Screenshot 2024-05-06 204632](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/assets/140955475/af174ed9-624b-451e-b82c-97e4d27074b9)

The below graph defines the output voltage v/s time plot and from here, we can calculate the rise and fall delay.

![Screenshot 2024-05-06 211105](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/assets/140955475/158117e6-b2e1-4ffe-b3f5-312683ecc6b4)

So, take 50% of the voltage waveform, `[rise delay=(1.6277 - 1.01446)=0.14831=148 ps]`. Similarly, we can calculate `fall delay(=71ps)` where the output falls (50% of voltage waveform).

![Screenshot (95)](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/assets/140955475/d3dae0ca-8323-48ac-b186-30eaa2816400)

Similarly, we will do these for all the values of (Wp/Lp = x.Wn/Ln) and make a table report.

### Lab steps to git clone vsdtdcelldesign

we will clone this [github repository](https://github.com/nickson-jose/vsdstdcelldesign.git) into our "openlane" directory using the following command--
```
$ git clone https://github.com/nickson-jose/vsdstdcelldesign.git
```
* In this repository, there are .mag files for inverters, PMOS, NMOS, and .lib files. These libraries(.lib) files are Skywater SPICE model files for NMOS and PMOS.

![Screenshot 2024-05-06 214321](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/assets/140955475/0bff01f4-1221-4766-a788-5c2d919b8472)

* In this lab, we will be actually creating a new cell and plugging it into the openlane flow instead of removing and editing.
* Let's open the .mag files and see which layers are used in building an inverter. Actually, we don't need to build an inverter from scratch, we already have it, so we will be doing SPICE extraction and post-layout SPICE simulation.
* Before opening the ".mag" file, we need to open the "tech" file, and to open it--

![Screenshot 2024-05-06 215215](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/assets/140955475/d3c87440-ccc5-464e-8766-46309478aadb)

Now, we will copy the `sky130A.tech` file(present in the "magic" folder) into the "vsdstdcelldesign" path which we have cloned now using the command--

![Screenshot 2024-05-06 220621](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/assets/140955475/019c6c4e-0469-4d3e-92d7-d56873fd2bb1)

Now, use the command as shown below--

![Screenshot 2024-05-06 221222](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/assets/140955475/0fc760de-ea0c-4ae0-aac1-f8d6d4777f5a)

Then, we will get the layout of our inverter in the "Magic tool".

![Screenshot 2024-05-06 221040](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/assets/140955475/b899f95d-5dec-4201-a046-9a18269e351a)


### Inception of layout A CMOS fabrication process
### Creating Active regions

The first step for the 16-mask process is selecting a substrate(on which we fabricate our whole design). 
</br>We will use the most used substrate which we see on any chips and that is a p-type substrate which has the following property --
* High resistivity(5-50 ohms).
* Doping level(10<sup>15</sup>cm<sup>-3</sup>)..the reason for this doping level is that we have to maintain a doping level which is less than the "well doping".
* Orientation(100)

* The next step is to create a active region for transistors by creating a small pockets in a p-type substrate which will be called as active region and in those pockets, we will create NMOS and PMOS transistor, and these pockets will be connected in the higher metal layers. Also, the pockets(active region) on the p-type substrate should not interfere  with each other working. This can be make sure by creating isolation between each and every pockets.
* First step is to grow a SiO<sub>2</sub>(insulator) of 40nm on a p-type substrate.
* The next step is to deposit a layer of silicon nitrite(Si<sub>3</sub>N<sub>4</sub>) of 80nm.
* Before creating the pockets, we have to deposit a layer of photoresist(something like a negative film) of 1um. So when we talk about layout, those are nothing but called as `mask` in a fabrication term.

![Screenshot 2024-05-07 155051](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/assets/140955475/65fb1709-2856-4b02-9156-bb3e73ea54d6)

* The below diagram is the cross section view of the "mask or layout". This is nothing but a protection layer for the photoresist. The UV light doesn't hit the area which is under the mask. But some of the side areas and the middle area get hit by the UV light and go through chemical reaction. So, we just wash off that area in the developing solution. So now we have the protected area where we can do some process.

![Screenshot 2024-05-07 162429](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/assets/140955475/fc11a308-9c2d-4238-82fb-3558e1583d45)

The next step is to remove the mask and then the photoresist because the silicon matter will itself act as a very good protection layer to grow the oxides on the other area which will act as an isolation layer.

![Screenshot 2024-05-07 162844](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/assets/140955475/9bb24ff8-3c40-4de3-9720-19289c1a57df)

![Screenshot 2024-05-07 163038](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/assets/140955475/4bf08f72-cfbf-4345-a885-46bfce1797a7)

![Screenshot 2024-05-07 163229](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/assets/140955475/9f29abef-d55c-4a64-bbb4-995273dd1b2f)

* Now put this complete thing in the oxidation furnace to grow the oxide in the other area.
* After this, we can see below how the silicon nitrite metal has actually protected the area (90 to 95% area) underneath to grow while it was not able to protect the area which were at the edges because the growth was so strong and this area is called as isolation region. The whole process is called "locos ( local oxidation of silicon)". The grown SiO<sub>2</sub> in the middle will provide the perfect isolation between the PMOS and NMOS. This is how, we protect two transistors communicating with each other.

![Screenshot 2024-05-07 165137](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/assets/140955475/30fa730d-5d16-4533-a13f-59f8af4b01fb)


Next step is to remove the Si<sub>3</sub>N<sub>4</sub> using hot phosphoric acid.

![Screenshot 2024-05-07 165455](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/assets/140955475/3cafe091-bcaf-4ff9-8c75-d02421154b0e)


### Formation of N-well and P-well

we can not form P-well(used for NMOS fabrication) and N-well(used for PMOS fabrication) at the same time. We have to protect the one region area by photoresist while forming in other region. First deposit a layer of photoresist and then define the pattern of which layer we have to protect. In this case, we will use the mask2 which will protect the area underneath. Now the next step is to expose the photoresist to UV light, the light doesn't react with the area underneath the mask2 , it only reacts with the area exposed. Now the particular area is washed away and that area is available for any chemical reaction that we want to do over there.

![Screenshot (102)](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/assets/140955475/324f06ba-1311-427e-97d3-994f8f002ec3)

Next step is to remove the mask itself and we have to create a P-well using boron which is diffused into this p-substrate through the thin oxide with a very high energy called as ion implantation.

![Screenshot (103)](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/assets/140955475/36926958-b6d1-475f-b937-23797f39fa8b)

We will do a similar process to form N-well by using mask3 and diffusing phosphorous ions with a very high energy.

![Screenshot (104)](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/assets/140955475/d4d953a3-1acb-4db0-9805-3581b0b41099)

So now, we have created the P-well and N-well.
</br>Till now, the well depths are not defined. So, after creating the P-well and N-well, we will put it into high temperature furnace for drive-in diffusion and this is how we define the depth of the wells.

![Screenshot (105)](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/assets/140955475/689c56c6-c0d6-4dc0-8cac-fc145f65b703)

![Screenshot (107)](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/assets/140955475/315deefa-2ad6-4399-8e40-0f01e3c9393e)

### Formation of Gate terminal

Gate terminal is the most important terminal of the PMOS and NMOS because from the gate terminal only, we can control the thresold voltage (Vth). Doping concentration and oxide capacitance will control the thresold voltage as we can see from the Vth equation. So, first we will maintain the doping concentration and for that we use mask4 and  doing the ion implantation of boron ion at lower energy (~60kev) because we want the boron to penetrate just at the surface and this doping concentration will actually depend upon the value of Vth(threshold voltage).

![Screenshot (108)](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/assets/140955475/a4e77ff5-63d0-4a9c-be7e-51f3510b1ade)

Now, the similar step is repeated for the fabrication of PMOS on the N-well using mask5 and Arsenic ions at a low energy to dope just at this level which will help to control the threshold voltage of PMOS as well.

![Screenshot (109)](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/assets/140955475/64d028f6-142f-42d5-b9b6-326564e04a5f)

Next step is that we have to fix the oxide layer beacuse there already have been damage due to the implantation. So, first we will remove this oxide layer using Hydrofluoric acid solution and again re-grow the high quality oxide layer with same thickness (~10 nm). and this 10nm value can be controlled based on the Vth(threshold voltage) we need. Oxide capacitance is now controlled by the oxide thickness which is maintained over here.

![Screenshot (111)](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/assets/140955475/cec48ea4-5a50-4481-94c5-1c9b31e86cd6)

Now the final step where we have to look at the gate formation here is to deposit a thick  polysilicon layer of 0.4um , the gate area is supposed to be of less resistance, so we will dope it with more impurities of N-type.

![Screenshot (112)](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/assets/140955475/b2b0e2cf-cdd6-46dd-810f-8aadad9d456c)

![Screenshot (113)](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/assets/140955475/0a4d78e9-6ed6-4897-92e8-10695f3a03c9)

Next step is to deposit a photoresist over here and mask6 layer. Now remove the mask6 because of the UV light exposure and the remaining area from the polysilicon outside the photoresist can be edged away. In this way, we get a polysilicon gate and the final step is to remove the photoresist.

![Screenshot (114)](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/assets/140955475/c552a253-1280-4b1e-87a7-e87f9bc5c0d6)

![Screenshot (115)](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/assets/140955475/5a25c713-928b-4bb5-9fdc-98dddf27b78e)

![Screenshot (116)](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/assets/140955475/3c27efe4-14f0-4ad2-9b5b-66917c922d8b)

Now, in this complete procedure, we have actually controlled the gate voltage using the doping, oxide capacitances, and also made a low resistance gate by additional doping of polysilicon with N-type impurities like phosphorus and arsenic.


### Lightly doped drain (LDD) formation

Here, we actually want( P+,P-,N ) doping profile in the PMOS and (N+,N-,P) doping profile for NMOS. The reason  for which we want P- and N- is Hot electron effect and the Short channel effect.
</br>For the formation of LDD, we again have to do ion implantation in P-well by using mask7 and phosphorous ions for light doping.

![Screenshot (117)](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/assets/140955475/81399174-b13b-45b2-b989-840cfaf7c457)

We have to do similar process for N-well using mask8 and then using boron ions.

![Screenshot (118)](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/assets/140955475/f2c605a5-e5d6-4f97-8b46-c74b9221c881)

To protect this lightly doped structure, we have to create spacers and for that, we have to deposit a thick layer of silicon dioxide or silicon nitrite(~0.1 um).

![Screenshot (119)](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/assets/140955475/b7f64d30-714e-4449-b8ee-028e1a5dfeee)

Now, we will do Plasma anisotropic etching to form the side wall spacers which will not let that area affected by the source and drain.

![Screenshot (120)](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/assets/140955475/099ffdd6-454b-474f-8c10-5886cf3fc1dc)


### Source and drain formation











