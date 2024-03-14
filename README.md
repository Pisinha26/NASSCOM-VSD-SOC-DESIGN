# NASSCOM-VSD-SOC-DESIGN

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

Difference between FOUNDRY IP's and MACROS

![difference](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/assets/140955475/ea4cb08d-d182-45a0-a6be-262446f9d494)

In summary, while both foundry IPs and macros are essential for semiconductor design and manufacturing, foundry IPs encompass a broader range of intellectual properties provided by semiconductor foundries, while macros specifically refer to pre-designed functional blocks used in chip design, often provided by third-party vendors or developed in-house.

![complete chip](https://github.com/Pisinha26/NASSCOM-VSD-SOC-DESIGN/assets/140955475/102dbe26-7ad6-4753-a5cb-5a5daf465f42)
