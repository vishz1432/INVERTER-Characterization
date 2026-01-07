# INVERTER-Characterization

**This project presents a complete CMOS inverter characterization using the SKY130 (1.8 V) PDK, designed and simulated in Xschem with Ngspice. DC, transient, and AC analyses are performed to evaluate voltage transfer characteristics, switching threshold, delay, rise/fall times, power consumption (static and dynamic), fanout, and inverter chain behavior. The results verify correct logic inversion, robust switching, and scalability under varying load and operating conditions**.

## Overview

**The CMOS inverter is the fundamental building block of digital integrated circuits. Accurate characterization of its behavior is essential for understanding delay, power consumption, noise margins, fanout capability, and scalability. This project focuses only on inverter characterization, making it ideal for beginners, academic labs, and standard‑cell pre‑characterization studies**.


 ## Technology & Tools
 - Process Technology: SkyWater SKY130 (1.8 V)
 - Schematic Capture: Xschem
 - Simulation Engine: Ngspice
 - PDK Type: Open‑source


   <img width="751" height="625" alt="image" src="https://github.com/user-attachments/assets/6d9cf120-5554-44b5-982f-85e6e6546f21" />


-----------------------------------------------------------------------------------------------------------------------------------------------

 # 1.Tools and PDK Setup

 ## 1.1 Tools Required

 For the simulation of circuits we will need the following tools.
 - Spice netlist simulation -[Ngspice](https://ngspice.sourceforge.io/).
 - Schematic Editor - [Xschem](https://xschem.sourceforge.io/stefan/index.html)

### Ngspice ###


<img width="200" height="80" alt="image" src="https://github.com/user-attachments/assets/8f2c2a5f-52c2-417e-883c-3446eb12e916" />


[Ngspice](https://ngspice.sourceforge.io/devel.html) is the open source spice simulator for electric and electronic circuits. Ngspice is an open project, there is no closed group of developers.

[Ngspice User Manual](https://ngspice.sourceforge.io/docs/ngspice-html-manual/manual.xhtml).Complete reference manual in HTML format.

### Xschem ###

<img width="200" height="80" alt="image" src="https://github.com/user-attachments/assets/831fb715-0a88-4c82-811f-ce6a320eb3e9" />

[Xschem](https://xschem.sourceforge.io/stefan/)is an open-source schematic capture tool for VLSI and electronics. It is designed to be lightweight, fast, and capable of handling large hierarchical circuits while remaining user-friendly.

[Xschem Reference Manual](https://xschem.sourceforge.io/stefan/xschem_man/xschem_man.html).Complete reference manual.


## 1.2 PDK Required ##

A process design kit (PDK) is a set of files used within the semiconductor industry to model a fabrication process for the design tools used to design an integrated circuit. The PDK is created by the foundry defining a certain technology variation for their processes. It is then passed to their customers to use in the design process.

The PDK is [Google Skywater 130 nm PDK](https://skywater-pdk.readthedocs.io/en/main/).


<img width="455" height="190" alt="image" src="https://github.com/user-attachments/assets/2afe1e47-67b5-48d4-a2b8-02763c6b650a" />

[All Details](https://skywater-pdk.readthedocs.io/en/main/rules/device-details.html).All basic details of PDK.


## 1.3 Install and Setup EDA Tools ##

### Windows Subsystem for Linux (WSL) for Open Source EDA tools ###

Windows Subsystem for Linux (WSL) is a feature of Windows that allows you to run a Linux environment on your Windows machine, without the need for a separate virtual machine or dual booting. With native X11 (graphics) support on WSL2, the latest WSL, in Winodws 10 version 2004+ (Build 19041+) or Windows 11, you can now run GUI apps including all the open-source EDA tools.

Now we will share instructions for installing WSL2 on Winodws 10/11 and install the EDA tools on a Ubuntu 24.04 distribution.


----------------------------------------------------------------------------------------------------------------------------------------------




# Inverter Schematic #

  <img width="600" height="550" alt="Screenshot 2026-01-07 203329" src="https://github.com/user-attachments/assets/db3ad126-0885-464f-8d9b-8ea2a42d87d9" />






 
