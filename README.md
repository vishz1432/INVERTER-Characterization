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




# 2. Inverter Schematic #

  <img width="600" height="550" alt="Screenshot 2026-01-07 203329" src="https://github.com/user-attachments/assets/db3ad126-0885-464f-8d9b-8ea2a42d87d9" />


### key Points ###

- Technology: SKY130 (1.8 V low-Vt devices)
- Transistors Used: pfet_01v8_lvt (PMOS), nfet_01v8_lvt (NMOS)
- Simulation Tools: Xschem (schematic) + Ngspice (simulation)
- Input: Pulse source (0 → 1.8 V)
- Output: Clean digital inversion with sharp transition near VDD/2

 ## 2.1. Regions of Operations 


   <img width="651" height="317" alt="Screenshot 2026-01-07 112216" src="https://github.com/user-attachments/assets/fbc9d67d-d95b-4156-98c6-c69f23c1b2b4" />



## 2.2. DC Analysis 

**Objective:-**
- Obtain the Voltage Transfer Characteristic (VTC).
- Determine switching threshold and noise margins.

 **Method:**       The input voltage is swept from 0 V to 1.8 V while monitoring the output voltage.

 **Key Observations:** 
 - Sharp transition region.
 - Switching threshold near VDD/2.
 - Proper rail‑to‑rail output swing.


## VTC Curve : - Before using Tool

<img width="464" height="370" alt="Screenshot 2026-01-07 112049" src="https://github.com/user-attachments/assets/72ef79a3-1d14-4dbf-b35e-6e692c00a6a7" />


## Spice File : 
```bash
* CMOS Inverter DC Simulation – SKY130

.lib "/home/vishalvlsi/share/pdk/sky130A/libs.tech/ngspice/sky130.lib.spice" tt
.temp 25

VDD vdd 0 1.8
VIN in  0 0

XM1 out in 0   0   sky130_fd_pr__nfet_01v8 W=0.42 L=0.15
XM2 out in vdd vdd sky130_fd_pr__pfet_01v8 W=1.26 L=0.15

.dc VIN 0 1.8 0.01

.control
run
plot v(in) v(out)
.endc

.end
```

## Simulation :

<img width="1002" height="626" alt="Screenshot 2026-01-08 125647" src="https://github.com/user-attachments/assets/d8efdc2d-e641-45d4-97c3-44e379e4baf7" />

## 2.3 Transient Analysis

**Objective:-**
- Measure rise time and fall time.
- Estimate propagation delay.

**Method:** A pulse input is applied, and the output waveform is observed under capacitive loading.
**key Observation:**
- Clean digital inversion.
- Sub‑nanosecond rise and fall times.
- Delay increases with load capacitance.


## Spice File :
```bash
* CMOS Inverter Transient Simulation – SKY130

.lib "/home/vishalvlsi/share/pdk/sky130A/libs.tech/ngspice/sky130.lib.spice" tt
.temp 25

* Supply Voltage
VDD vdd 0 DC 1.8

* Input Pulse
VIN in 0 PULSE(0 1.8 2n 0.1n 0.1n 100n 200n)

* CMOS Inverter
XM1 out in 0   0   sky130_fd_pr__nfet_01v8 W=1.26 L=0.15
XM2 out in vdd vdd sky130_fd_pr__pfet_01v8 W=1.26 L=0.15

* Load Capacitance
Cload out 0 100f

* Transient Analysis
.tran 0.1n 500n

.control
run
plot v(in) v(out)
.endc

.end
```
## Simulation :
<img width="1002" height="650" alt="Screenshot 2026-01-08 131301" src="https://github.com/user-attachments/assets/7abe5c0a-ac61-40e8-aa1f-d552316a8b88" />



## 2.4 AC Analysis

**Objective:**
- Analyze small‑signal frequency response.
- Determine inverter gain and bandwidth.

**Method:**  Small‑signal AC analysis is performed with a capacitive load at the output.

**Key Observations:**
- High gain at low frequencies.
- Roll‑off at higher frequencies due to parasitics.


## Spice File :

```bash
* CMOS Inverter AC Feedthrough Response (Rising Plot)

.lib "/home/vishalvlsi/share/pdk/sky130A/libs.tech/ngspice/sky130.lib.spice" ss
.temp 125

* Supply (AC grounded)
VDD vdd 0 DC 1.8 AC 0

* Input with AC excitation ONLY (no DC bias)
VIN in 0 DC 0 AC 1

* CMOS inverter
XM1 out in vdd vdd sky130_fd_pr__pfet_01v8_lvt W=7 L=0.35
XM2 out in 0   0   sky130_fd_pr__nfet_01v8_lvt W=7 L=0.15

* Load capacitance
 out 0 100f

* AC sweep
.ac dec 10 1Meg 10e13

.control
run
plot vdb(out)
.endc

.end
```

## Simulation :

<img width="1002" height="650" alt="image" src="https://github.com/user-attachments/assets/eee5fde2-451d-49e6-af0a-73f1e3b83ab2" />

## 2.4. Static Power Analysis

**Objective:**
- Estimate leakage power in steady‑state conditions.

**Method:**
DC operating‑point analysis is used to measure supply current when the inverter input is at logic ‘0’ and logic ‘1’.

**Key Observation**
- Static power is extremely low.
- Leakage current dominates static consumption.

## Spice File :
```bash
******INVERTER-STATIC******
.lib "/home/vishalvlsi/share/pdk/sky130A/libs.tech/ngspice/sky130.lib.spice" ss
.temp 125

.global VDD GND

** PMOS case **
VDD      VDD     GND     DC 1.8
VIN      in      GND     DC 0

XP1      out     in      VDD     VDD    sky130_fd_pr__pfet_01v8_lvt L=0.35 W=7
XM1      out     in      GND     GND    sky130_fd_pr__nfet_01v8_lvt L=0.15 W=7
C1       out     0       1a

** NMOS case **
Vns     n1      0       DC 1.8
V1      i1      0       DC 1.8

XP2     o1      i1      n1      n1     sky130_fd_pr__pfet_01v8_lvt L=0.35 W=7
XM2     o1      i1      0       0      sky130_fd_pr__nfet_01v8_lvt L=0.15 W=7
C2      o1      0       1a

.OP

.control
run
print abs(I(VDD))
print abs(I(Vns))
let static_power = ((I(VDD)) + (I(Vns))) * 1.8
print abs(static_power)
.endc
.end
```
## Simulation :
<img width="802" height="600" alt="image" src="https://github.com/user-attachments/assets/e2be81eb-14f0-4698-9dcc-fa6bd97c783a" />

### Output Value :
<img width="535" height="390" alt="Screenshot 2026-01-08 164741" src="https://github.com/user-attachments/assets/6f0597ff-e477-48fd-9cce-e6e629544da3" />


## 2.5. Dynamic Power Analysis

**Objective:**
- Calculate average power during switching activity.

**Method:** Average supply current is measured during transient simulation and multiplied by supply voltage.

**Key Observation**
- Dynamic power increases with switching frequency.
- Load capacitance significantly affects power consumption.

## Spice File :
```bash
*  Dynamic power calculation of CMOS Inverter
.lib "/home/vishalvlsi/share/pdk/sky130A/libs.tech/ngspice/sky130.lib.spice" ss
.temp 125

Vdd Vdd 0 1.8
Vin In 0 PULSE(0 1.8 0 10n 10n 60n 120n)

XM1 Out In Vdd Vdd sky130_fd_pr__pfet_01v8_lvt w=7 l=.35
XM2 Out In 0   0   sky130_fd_pr__nfet_01v8_lvt w=7 l=.15

C1 out 0 1a

.tran 0.1n 240n
.op

.control
run
plot V(in) V(Out)
plot abs(i(Vdd))

meas tran i(avg) AVG  i(Vdd)
meas tran rise_time  TRIG v(out) VAL=0.18 RISE=1  TARG v(out) VAL=1.62 RISE=1
meas tran fall_time  TRIG v(out) VAL=1.62 FALL=1  TARG v(out) VAL=0.18 FALL=1
meas tran delay_time TRIG v(in)  VAL=0.9  RISE=1  TARG v(out) VAL=0.9  RISE=1
meas tran vmax MAX v(out)
meas tran vmin MIN v(out)

let power = i(avg)*1.8
print power
.endc

.end
```
## Simulation :
<img width="923" height="731" alt="image" src="https://github.com/user-attachments/assets/abcb3be6-bd7b-44f6-9c92-c777cb89d365" />

### Values :
- i(avg) = -1.040382e-05 from= 0.000000e+00 to= 2.400000e-07
- rise_time = 1.105302e-09 targ= 7.625172e-08 trig= 7.514641e-08
- fall_time = 1.073558e-09 targ= 4.883824e-09 trig= 3.810266e-09
- delay_time = 7.061907e-08 targ= 7.561907e-08 trig= 5.000000e-09
- vmax = 1.801978e+00 at= 1.328000e-09
- vmin = -1.456401e-04 at= 1.900300e-07
- Power = -1.87269e-05

<img width="702" height="558" alt="Screenshot 2026-01-08 165910" src="https://github.com/user-attachments/assets/3b535753-16cc-48b1-b893-7d021e92edbd" />



<img width="702" height="550" alt="Screenshot 2026-01-08 165845" src="https://github.com/user-attachments/assets/f91ee96a-e69a-4259-aa42-e1b2dfdc076e" />

### Theory & Derivation :

![WhatsApp Image 2026-01-08 at 5 10 48 PM](https://github.com/user-attachments/assets/67723532-6c64-4dd8-aa6c-f8e331880a17)


## 2.6. Inverter Fanout Analysis 

- Fanout Analysis evaluates how driving multiple identical inverter loads affects an inverter’s timing performance. As fanout increases, the effective capacitive load at the output rises, causing longer rise/fall times and increased propagation delay due to limited drive strength.

**Objective:**
Study the effect of multiple inverter loads on delay.

**Method:**  The inverter output is connected to several identical inverter inputs, and timing degradation is measured.

**Key Observations:**
- Rise/fall times increase with fanout.
- Propagation delay increases as load increases.

## Spice File :

```bash
* FANOUT
.lib "/home/vishalvlsi/share/pdk/sky130A/libs.tech/ngspice/sky130.lib.spice" ss
.temp 125

Vdd Vdd 0 1.8
Vin In 0 PULSE(0 1.8 0 10n 10n 60n 120n)

XM1 Out In Vdd Vdd sky130_fd_pr__pfet_01v8_lvt w=7 l=.35
XM2 Out In 0   0   sky130_fd_pr__nfet_01v8_lvt w=7 l=.15

XM11 Out1 Out Vdd Vdd sky130_fd_pr__pfet_01v8_lvt w=7 l=.35
XM21 Out1 Out 0   0   sky130_fd_pr__nfet_01v8_lvt w=7 l=.15

XM12 Out2 Out Vdd Vdd sky130_fd_pr__pfet_01v8_lvt w=7 l=.35
XM22 Out2 Out 0   0   sky130_fd_pr__nfet_01v8_lvt w=7 l=.15

XM13 Out3 Out Vdd Vdd sky130_fd_pr__pfet_01v8_lvt w=7 l=.35
XM23 Out3 Out 0   0   sky130_fd_pr__nfet_01v8_lvt w=7 l=.15

XM14 Out4 Out Vdd Vdd sky130_fd_pr__pfet_01v8_lvt w=7 l=.35
XM24 Out4 Out 0   0   sky130_fd_pr__nfet_01v8_lvt w=7 l=.15

.tran 0.1n 240n
.op

.control
run
meas tran rise_time  TRIG v(out) VAL=0.18 RISE=1  TARG v(out) VAL=1.62 RISE=1
meas tran fall_time  TRIG v(out) VAL=1.62 FALL=1 TARG v(out) VAL=0.18 FALL=1
meas tran delay_time TRIG v(in)  VAL=0.9  RISE=1 TARG v(out) VAL=0.9  RISE=1
meas tran vmax MAX v(out)
meas tran vmin MIN v(out)
.endc

.end
```
## Simulation :
<img width="702" height="550" alt="image" src="https://github.com/user-attachments/assets/82421285-375c-4794-8eab-6bec814f7162" />

## Output 
- rise_time = 1.373773e-09 targ= 7.662521e-08 trig= 7.525144e-08
- fall_time = 1.396628e-09 targ= 5.307938e-09 trig= 3.911310e-09
- delay_time = 7.116898e-08 targ= 7.616898e-08 trig= 5.000000e-09
- vmax = 1.801976e+00 at= 1.213500e-07
- vmin = -1.311942e-04 at= 7.015000e-08

# Conclusion 

- In this project, the CMOS inverter was analyzed and characterized to understand its DC and transient performance. Key parameters such as the Voltage Transfer Characteristics (VTC), noise margins, propagation delay, rise and fall times, and power dissipation were evaluated through simulation. The results demonstrate the inverter’s sharp switching behavior, high noise immunity, and low static power consumption, which are fundamental advantages of CMOS technology.

The characterization of the inverter provides critical insight into the impact of transistor sizing, load capacitance, and supply voltage on circuit performance. Since the CMOS inverter serves as the basic building block of digital integrated circuits, a thorough understanding of its behavior is essential for the design and optimization of more complex logic gates and digital systems. This study establishes a strong foundation for advanced CMOS circuit design and performance analysis.




















  












   
   



   







 
