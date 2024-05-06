# NASSCOM-VSD-SOC-DESIGN

# Advanced-Physical-Design-Using-OpenLANE-Sky130

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

  * `Characterization` - 

  












