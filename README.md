# NASSCOM-VSD-SOC-DESIGN

# Advanced-Physical-Design-Using-OpenLANE-Sky130

## Day_1 Inception of Open-Source EDA, OpenLANE & Sky130PDK

### How to talk to Computers

The Arduino Leonardo is a microcontroller board based on the ATmega32u4. It has 20 digital input/output pins (of which 7 can be used as PWM outputs and 12 as analog inputs), a 16 MHz crystal oscillator, a micro USB connection, a power jack, an ICSP header, and a reset button. It contains everything needed to support the microcontroller; simply connect it to a computer with a USB cable or power it with a AC-to-DC adapter or battery to get started.
The Leonardo differs from all preceding boards in that the ATmega32u4 has built-in USB communication, eliminating the need for a secondary processor. This allows the Leonardo to appear to a connected computer as a mouse and keyboard, in addition to a virtual (CDC) serial / COM port.


![leonardo aurdino](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/assets/140955475/362391d9-c07f-477b-866c-8177abbcc20b)

[leonardo pinout.pdf](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/files/14595132/leonardo.pinout.pdf)
[leonardo schematics.pdf](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/files/14595134/leonardo.schematics.pdf)


The below block diagram describes this Arduino Board.
the circled section is the processor/SOC and along with it we have all the interfaces-- JTAG-UART, QSPI1-Flash, I2C0EEPROM, VCC/GND, ADC(QSPI3), SDRAM(external chip)
![block diagram of arduino](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/assets/140955475/f41e7b48-4593-4fd2-a179-a3376aa07dbb)

When we open up this particular IC, the whole thing is called a package which is shown below.
the pin locations of this particular package are all driven by the Arduino board that we are trying to design. The size of this package is 7mm x 7mm.
chip basically sits at the center of the package and the connection of the chip with the package is shown as wire bonds and this way we will be able to transfer all these signals coming from outside world to the interior of the chip.

![chip connect to package](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/assets/140955475/0e33ba91-14c5-4101-a6b8-c7d07d6005a5)

The chip will have various component--
1. PADS- A pad is the exposed region of the metal on a circuit board that the component lead is soldered to. Pads are the points through which connection of peripherals on a board is made with the chip/porcessor, and transfer of data takes place. Any signal can go outside or inside the chip through pads.
2. CORE-  A core in a chip is a well-partitioned piece of logic capable of independently performing all functions of a processor. All the digital logic building blocks are present here.
3. DIE- this is what gets manufactured on silicon wafer.
  
4. FOUNDRY IP'S-  A foundry is a company that provides IC (integrated circuit) manufacturing services - basically, you give them your design, and they manufacture the chip for you. Intellectual Property. In this context, it’s the design for the parts of a chip. Nowadays, many chips are not wholly designed by the company that’s “designing” them. For example, in a typical mobile phone application chip:
The main CPU will probably be bought as IP from ARM.
The graphics processor (GPU) will be bought from one of a number of companies (e.g. ARM, Imagination Technologies, etc.)

Foundry IPs, or intellectual properties, encompass a range of assets that a semiconductor foundry owns or licenses to produce integrated circuits (ICs) for its clients. These IPs can be classified into several categories:

a) Process Technology IP: This includes the set of technologies and processes used in the fabrication of semiconductor devices. It encompasses everything from the design of the silicon wafers to the various steps involved in creating the circuitry on the chips.

b) Design IP: These are the designs of specific components or functional blocks used in ICs, such as CPUs, GPUs, memory modules, or communication interfaces. Design IPs are often licensed from third-party vendors or developed in-house by the foundry.

c) Verification IP: This type of IP includes the models, testbenches, and other verification components used to ensure that the designed IC functions correctly before manufacturing. It helps validate the functionality and performance of the chip design.

d) Packaging IP: Packaging IPs involve the technologies and designs related to the physical packaging of the ICs, including the substrate, interconnects, and external connections. Proper packaging is crucial for protecting the chips and enabling their integration into electronic devices.

e) Security IP: With the growing concerns over cybersecurity, security IPs have become increasingly important. These IPs include features such as encryption/decryption, authentication, and secure boot mechanisms to protect the data and functionality of the ICs from unauthorized access or tampering.

Overall, foundry IPs are essential for semiconductor foundries to differentiate themselves in the highly competitive semiconductor manufacturing industry. They enable foundries to offer unique technologies, services, and solutions to their clients, ultimately driving innovation and progress in the electronics industry.

5. MACROS-  Macro is an essential component in the VLSI design cycle before the final packaged chip is ready to use. Macro cells are the memory cells, intellectual property which an analog design team has designed. To break down the understanding of macro cells, consider macro cells as pieces of logic blocks, mainly intellectual properties (IP), which can be used in a design without the need to (of) building them from scratch. Thus, these memory cells are instrumental in reducing the total time for the design engineers that is required to complete their entire design.

![image](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/assets/140955475/fe9ac562-fa98-48b8-a4b1-ac26f78898f4)


![difference](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/assets/140955475/ea4cb08d-d182-45a0-a6be-262446f9d494)

In summary, while both foundry IPs and macros are essential for semiconductor design and manufacturing, foundry IPs encompass a broader range of intellectual properties provided by semiconductor foundries, while macros specifically refer to pre-designed functional blocks used in chip design, often provided by third-party vendors or developed in-house.

![complete chip](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/assets/140955475/102dbe26-7ad6-4753-a5cb-5a5daf465f42)

So if we want to manufacture this entire chip, we need to communicate with Foundry with the help of interface files which the Foundry passes to us.

![image](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/assets/140955475/95d24a92-c010-4892-9601-930bd42be84f)

![RISCV rtl to gds](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/assets/140955475/cf43df44-08ad-42a8-b7c7-171d92000dfc)

C-Program is first being compiled in its assembly language program which is the RISC-V assembly language program..then this assembly language program is converted into machine language program which is nothing but binary language program (1's and 0's) which is understood by the hardware of the computer i.e, these bits gets executed in this particular layout and we get the required output.
Another interface that needs to be present between RISC-V architecture and the layout is the hardware description language (HDL). So we need to implement this RISC-V specifications using some RTL and then finally RTL to GDS flow.
So from user point of view, we just execute this particular C-Program and that should get automatically executed by the chip of the hardware and we get the required output.

![image](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/assets/140955475/75aba07b-29cb-4089-89e0-2b59a71198df)

![flowchart app run laptop](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/assets/140955475/c177530d-4e25-4acc-8763-b4d1578ffda3)

This application software enters into the block called system software and here the system software converts the application software into binary language..
The major components of  the system software are operating system (O.S),  compiler and the assembler.
The Operating system handles memories, i/o operation, allocates memory..
The output of the operating system are the small functions in the c/c++/Java language and these are being taken by the respective compiler and converted into instructions. (.exe files)..
The syntax of these particular instructions depends upon what kind of the hardware (chip layout) available..for eg--intelx86, Arm, MIPS, RISC-V..
Now the job of the assembler is to take these instructions and convert it into respective binary numbers which the machine understands..these binary numbers are then fed to the hardware and according to the logic the hardware generates the output.

![Screenshot 2024-03-14 195929](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/assets/140955475/34874985-7e51-4006-999d-082265ae0aeb)


### SoC Design & OpenLANE

![asic and](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/assets/140955475/59658143-9d71-4af3-9b54-0e6ebe58488a)

![image](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/assets/140955475/7af4ef46-c41d-4ef3-9ab2-4d10c923bfb1)

1. RTL IP's- RTL IP refers to a product in electronic format that represents an integrated circuit function that can be instantiated in an integrated circuit design. An IP Core is a product in electronic format that represents an integrated circuit function that can be instantiated in an integrated circuit design, including the circuits and modules of such integrated circuit design(s) and associated firmware, application programming interfaces, Software drivers, application-specific Software, and all register transfer language (RTL), verilog, and other source materials to instantiate, modify, support, and maintain any of the foregoing.

2. EDA Tools- Electronic Design Automation (EDA) tools are software tools used to design electronic systems such as integrated circuits and printed circuit boards. They have three key functions: simulation, design, and verification. EDA tools work together in a design flow that chip designers use to design and analyze entire semiconductor chips. They allow teams to predict circuit behavior, assemble circuit elements, and anticipate chip performance. EDA tools are used to verify that a design will meet all the requirements of the manufacturing process, known as design for manufacturability (DFM). Deficiencies in this area can cause the resultant chip to either not function or function at reduced capacity, and there are reliability risks.

3. PDK- It stands for Process Design Kit. It is basically a interface between FAB and the designers which is a collection of files used to model a fabrication process for the EDA tools used to design an IC.
PDK data consists of primarily-->
-Process design rules i.e DRC
-Device models
-Digital Std. cell library
-I/O library
On june 30,2020 Google released a first ever open source PDK (FOSS 130 nm Production PDK)to the masses.
PDK has only the data information for successful ASIC implementationusing either open source or close source EDA tools.

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

