
# Memory Read operation using 6T SRAM cell and Conventional Latch type Sense Amplifier 





## Table of Contents

* [Abstract](https://github.com/DebjitLP/CloudBased_Analog_Hackathon_MemoryRead#abstract)
* [Tools Used](https://github.com/DebjitLP/CloudBased_Analog_Hackathon_MemoryRead#tools-used)
* [Working](https://github.com/DebjitLP/CloudBased_Analog_Hackathon_MemoryRead#working)
* [SRAM Cell Design](https://github.com/DebjitLP/CloudBased_Analog_Hackathon_MemoryRead#sram-cell-design)
* [Sense Amplifier Design](https://github.com/DebjitLP/CloudBased_Analog_Hackathon_MemoryRead#sense-amplifier-design)
* [Memory Read Operation](https://github.com/DebjitLP/CloudBased_Analog_Hackathon_MemoryRead#memory-read-operation)
* [Netlist](https://github.com/DebjitLP/CloudBased_Analog_Hackathon_MemoryRead#netlist)
* [Author](https://github.com/DebjitLP/CloudBased_Analog_Hackathon_MemoryRead#author)
* [Acknowledgements](https://github.com/DebjitLP/CloudBased_Analog_Hackathon_MemoryRead#acknowledgements)
* [References](https://github.com/DebjitLP/CloudBased_Analog_Hackathon_MemoryRead#references)






## Abstract

Integrated SRAMs take about 70 percent of the chip area in SoCs. SRAM constitute of 6T cell which is the
functional single bit storage element. Sense Amplifiers along with
the latching circuit are present in the IO region of the memory unit used to complete the read operation of a SRAM cell. In this
paper the read operation of memory composed of SRAM cell is described.




## Tools Used

- Synopsys Custom Compiler
- Synopsys Primewave
- Synopsys 28nm PDK
## Working

The reference circuitry is made up of SRAM cells connected to bitlines, which are connected to the precharge circuit and the sense amplifier's pass gates. A precharge circuit is included in the sense amplifier to set the internal nodes to logic 1 before each read operation. The internal nodes of the Sense Amplifier are connected to a latching circuit that generates the read operation's final output.

Bitlines are precharged to Vdd before the wordline arrives. Following the WL, the BL/BLB discharges occur, depending on which side logic 0 is stored and which side's Bitline discharges. The differential across the bitlines is also transmitted to the sense amplifier's internal nodes. After the internal nodes have been discharged sufficiently, the Sense Enable signal is raised, decoupling the bitlines from the internal nodes of the sense amplifier and turning on the footer NMOS discharging the internal nodes.

Internal nodes are resolved to 1 and 0 based on differential polarity via positive feedback. Based on the data stored in the cell, the latching circuit completes the read operation by setting Dout to 1/0. Internal nodes and bitlines are precharged to Vdd before the next read operation.


* **Target Specification**

| Parameter |  Specification                |
| :-------- |  :------------------------- |
| `Operating Voltage` |  1 V |
| `SEN-Q Latch Delay (Read 0) ` | < 150 ps | 
| `SEN-Q Latch Delay (Read 1) ` | < 150 ps |
| `Bitline Load` |  100 fF |    
| `Output Load` |  10 fF |    

* **Reference Design**

![Reference Circuit Diagram](https://github.com/DebjitLP/CloudBased_Analog_Hackathon_MemioryRead/blob/main/Circuit%20Diagrams/reference_design.png?raw=true)

* **Reference Waveforms**

![Reference Circuit Diagram](https://github.com/DebjitLP/CloudBased_Analog_Hackathon_MemoryRead/blob/main/Waveforms/wavef.png?raw=true)



##  SRAM Cell Design

The objective of SRAM cell design is to arrive at an optimal cell ratio and optimal pull up ratio. For performing a quick read operation, the cell current should be optimal so that bitline discharges within a given period of time. Signifying that the combination of passgate and pulldown should be less resistive. Also taking care of the bitline leakage when not doing any read operation, the minimum length of the technology is not used. Cell stability also becomes a very important factor so the pull downs should be Larger than the pass gate. So that the Vbump generated during read operation would be smaller than SNM and then we can accept more noise preventing any cell flip during read operation. For the same reason. Pass gates should be more resistive than pull down. Pull down should be large, but not too large. Also, the sizing of pull up devices becomes important in this case so as to complete the Write operation. To be Able to write into a cell the combination of Write driver and pass gate should be stronger than that of the pullup. Pullups are the weakest device of the SRAM cell. Pull up devices also need to be optimally sized so as to maintain the Writability and write speed of the SRAM cell. 

So, the strength of the devices is in the following order Pulldown > Passgates > Pullup. 

Length greater than minimum technology size is used to prevented any unwanted leakage.
All the simulations are performed using Synopsys 28nm PDK and at TT 25 C.

* **SRAM Circuit Diagram**

![Reference Circuit Diagram](https://github.com/DebjitLP/CloudBased_Analog_Hackathon_MemoryRead/blob/main/Circuit%20Diagrams/SRAM_schematic.png?raw=true)

* **Device Sizes**


| Devices |  W/L     (u/u)           |
| :-------- |  :------------------------- |
| `Precharge Devies` |  1.6 / 0.03 |
| `Pullup Devices` | 0.1 / 0.04 | 
| `Pass Gates` | 0.2 / 0.04 |
| `Pull Down devies` | 0.25 / 0.04 |   

* **Achieved Read Specification**

| Parameter |  Specification                |
| :-------- |  :------------------------- |
| `Max Vbump` | 206 mV |
| `Max BL Discharge` | 30 mV |

* **Waveforms**

* Internal Node Q/QB set to 0/1 leading to the discharge of BL.

![Reference Circuit Diagram](https://github.com/DebjitLP/CloudBased_Analog_Hackathon_MemoryRead/blob/main/Waveforms/read_BL_dis.png?raw=true)


* Internal Node Q/QB set to 1/0 leading to the discharge of BLB.

![Reference Circuit Diagram](https://github.com/DebjitLP/CloudBased_Analog_Hackathon_MemoryRead/blob/main/Waveforms/read_BLB_dis.png?raw=true)




## Sense Amplifier Design

Before the WL comes the Precharge Circuit should go off, so Pch signal is taken high.
As WL comes the differential voltage is created (offset) in BL/BLB.
The same offset is reflected in Sat/Saf node.
Sense Enable signal is (SEN) taken high -> PG turns off -> BL/BLB gets decoupled from Sat/Saf. Simultaneously Footer nmos turns ON.
Node having Vdd - offset is pulled down to Gnd.
Node having approximately Vdd after suffering some discharge is pulled up to Vdd.
Time taken for latching from the point SEN goes high is called Sense Enable-Qlatch Delay.
The time taken by Sense Amplifier to write into the latch after the offset in one of the nodes is called Reaction Time.

Read Time = Reaction time + (Total time taken from bitcell to SA)

* **Sense Amplifier Circuit Diagram**

![Reference Circuit Diagram](https://github.com/DebjitLP/CloudBased_Analog_Hackathon_MemoryRead/blob/main/Circuit%20Diagrams/Sense_schematic.png?raw=true)

* **Device Sizes** 

| Devices |  W/L (u/u)           |
| :-------- |  :------------------------- |
| `Precharge Devies` |  0.12 / 0.03 |
| `Pullup Devices` | 0.1 / 0.04 | 
| `Pass Gates` | 0.5 / 0.03 |
| `Pull Down devies` | 1 / 0.04 |   
| `Footer NMOS` | 1.5 / 0.03 |   
| `Load Inverter PMOS` | 0.9 / 0.03 |   
| `Load Inverter NMOS` | 0.5 / 0.03 |   
| `Output PMOS` | 0.9 / 0.03 |   
| `Output NMOS` | 0.5 / 0.03 |    
| `Latch PMOS` | 0.24 / 0.03 |   
| `Latch NMOS` | 0.15 / 0.03 |   

* **Achieved Read Specification**

| Parameter |  Specification                |
| :-------- |  :------------------------- |
| `SEN-Q Latch Delay (Read 0) ` | 50.5 ps |
| `SEN-Q Latch Delay (Read 1) ` | 63 ps |

* **Waveforms**

* BLB Discharging - Leading to a read 0.

![Reference Circuit Diagram](https://github.com/DebjitLP/CloudBased_Analog_Hackathon_MemoryRead/blob/main/Waveforms/read0_s.png?raw=true)


* BL Discharging - Leading to a read 1.

![Reference Circuit Diagram](https://github.com/DebjitLP/CloudBased_Analog_Hackathon_MemoryRead/blob/main/Waveforms/read1_s.png?raw=true)


## Memory Read Operation


For demonstrating the memory read operation, 6T SRAM cell is connected to a conventional last type sense amplifier via bitlines with a load of 100 fF representing the load of a column of bitcells . First, for demonstrating read 1, Node Q of the SRAM cell is initialized to a logic 0 which results in the discharge of BL and thus the offset appears at the SAF node of the sense amplifier. As soon as SEN (Sense Enable) signal is taken high SAF node discharges and SAT node is taken to VDD resulting in DOUT node to a logic 1.

Similarly, for demonstrating the read 0 operation, QB node of the SRAM cell is initialized to the logic 0 which leads to the discharge of it BLB, Therefore the offset appears at the SAT node of the sense amplifier as soon as the SEN signal is taken high, the SAT node discharges SAF node is taken to VDD. Resulting in DOUT node of the latch to a logic 0. The DOUT node of the latch is connected to a buffer to drive a higher Output load of 10 fF. 


* **Signals** 

| Devices |  Pulse Width    (ps)        |  Time Period     (ps)      |
| :-------- |  :------------------------- |  :------------------------- |
| `BPCH (Bitline Precharge)` |  300 |  1000 |
| `WL (Wordline)` | 250 |  1000 | 
| `PCH (Precharge)` | 360 | 1000 | 
| `SEN (Sense Enable)` | 100 |  1000 | 


* **Stimulus of various Signals are given as follows**

 - Wordline:

![Reference Circuit Diagram](https://github.com/DebjitLP/CloudBased_Analog_Hackathon_MemoryRead/blob/main/Stimulus/BPCH.png?raw=true)


 - Bitline Precharge:

![Reference Circuit Diagram](https://github.com/DebjitLP/CloudBased_Analog_Hackathon_MemoryRead/blob/main/Stimulus/WL.png?raw=true)


 - Precharge:

![Reference Circuit Diagram](https://github.com/DebjitLP/CloudBased_Analog_Hackathon_MemoryRead/blob/main/Stimulus/PCH.png?raw=true)


 - Sense Enable:

![Reference Circuit Diagram](https://github.com/DebjitLP/CloudBased_Analog_Hackathon_MemoryRead/blob/main/Stimulus/SEN.png?raw=true)


* **Circuit Diagram for demonstrating Read Operation**

![Reference Circuit Diagram](https://github.com/DebjitLP/CloudBased_Analog_Hackathon_MemoryRead/blob/main/Circuit%20Diagrams/Read_schematic.png?raw=true)


* **Waveforms**

* Internal Node Q/QB set to 1/0 leading to Read 0 operation.

![Reference Circuit Diagram](https://github.com/DebjitLP/CloudBased_Analog_Hackathon_MemoryRead/blob/main/Waveforms/read0.png?raw=true)


* Internal Node Q/QB set to 0/1 leading to Read 1 operation.

![Reference Circuit Diagram](https://github.com/DebjitLP/CloudBased_Analog_Hackathon_MemoryRead/blob/main/Waveforms/read1.png?raw=true)


## Netlist

* **Netlist for characterizing SRAM**


```bash
  *  Generated for: PrimeSim
*  Design library name: cp_sram
*  Design cell name: sram
*  Design view name: schematic
.lib 'saed32nm.lib' TT

*Custom Compiler Version S-2021.09
*Fri Feb 25 18:51:01 2022


********************************************************************************
* Library          : cp_sram
* Cell             : sram
* View             : schematic
* View Search List : hspice hspiceD schematic spice veriloga
* View Stop List   : hspice hspiceD
********************************************************************************
c11 blb gnd_1 c=100f
c10 bl gnd_1 c=100f
xm9 bl bpch blb vdd p105 w=1.6u l=0.03u nf=1 m=1
xm8 bl bpch vdd vdd p105 w=1.6u l=0.03u nf=1 m=1
xm7 blb bpch vdd vdd p105 w=1.6u l=0.03u nf=1 m=1
xm1 q qb vdd vdd p105 w=0.1u l=0.04u nf=1 m=1
xm0 qb q vdd vdd p105 w=0.1u l=0.04u nf=1 m=1
xm6 qb wl blb gnd_1 n105 w=0.2u l=0.04u nf=1 m=1
xm5 q qb gnd_1 gnd_1 n105 w=0.25u l=0.04u nf=1 m=1
xm4 qb q gnd_1 gnd_1 n105 w=0.25u l=0.04u nf=1 m=1
xm3 q wl bl gnd_1 n105 w=0.2u l=0.04u nf=1 m=1



.tran '1f' '2.5n' name=tran

.option primesim_remove_probe_prefix = 0
.probe v(*) i(*) level=1
.probe tran v(bl) v(blb) v(bpch) v(q) v(qb) v(wl)

.temp 25
.include './stimGen/stimGen.sp'

.ic  v(q)=1  v(qb)=0  v(bpch)=0  v(wl)=0  v(bl)=1  v(blb)=1

.option primesim_output=wdf

.option parhier = LOCAL

.end

```


* **Netlist for characterizing Sense Amplifier**


```bash

*  Generated for: PrimeSim
*  Design library name: cp_sense_amplifier
*  Design cell name: sense_amplifier
*  Design view name: schematic
.lib 'saed32nm.lib' TT
.param diff='v(/BL) - v(/BLB)'
*Custom Compiler Version S-2021.09
*Sat Feb 26 05:52:47 2022


********************************************************************************
* Library          : cp_sense_amplifier
* Cell             : sense_amplifier
* View             : schematic
* View Search List : hspice hspiceD schematic spice veriloga
* View Stop List   : hspice hspiceD
********************************************************************************
xm20 vdd net70 dout vdd p105 w=0.24u l=0.03u nf=1 m=1
xm18 vdd dout net70 vdd p105 w=0.24u l=0.03u nf=1 m=1
xm16 vdd net51 dout vdd p105 w=0.9u l=0.03u nf=1 m=1
xm14 net55 sat vdd vdd p105 w=0.9u l=0.03u nf=1 m=1
xm12 net51 net5 vdd vdd p105 w=0.9u l=0.03u nf=1 m=1
xm11 saf sen bl vdd p105 w=0.5u l=0.03u nf=1 m=1
xm10 sat sen blb vdd p105 w=0.5u l=0.03u nf=1 m=1
xm6 saf pch sat vdd p105 w=0.12u l=0.03u nf=1 m=1
xm5 saf sat vdd vdd p105 w=0.1u l=0.04u nf=1 m=1
xm4 saf pch vdd vdd p105 w=0.12u l=0.03u nf=1 m=1
xm3 sat saf vdd vdd p105 w=0.1u l=0.04u nf=1 m=1
xm2 sat pch vdd vdd p105 w=0.12u l=0.03u nf=1 m=1
xm0 net5 saf vdd vdd p105 w=0.9u l=0.03u nf=1 m=1
xm21 gnd_1 net70 dout gnd_1 n105 w=0.15u l=0.03u nf=1 m=1
xm19 gnd_1 dout net70 gnd_1 n105 w=0.15u l=0.03u nf=1 m=1
xm17 gnd_1 net55 dout gnd_1 n105 w=0.5u l=0.03u nf=1 m=1
xm15 net55 sat gnd_1 gnd_1 n105 w=0.5u l=0.03u nf=1 m=1
xm13 net51 net5 gnd_1 gnd_1 n105 w=0.5u l=0.03u nf=1 m=1
xm9 net35 sen gnd_1 gnd_1 n105 w=1u l=0.03u nf=1 m=1
xm8 saf sat net35 gnd_1 n105 w=1u l=0.04u nf=1 m=1
xm7 sat saf net35 gnd_1 n105 w=1u l=0.04u nf=1 m=1
xm1 net5 saf gnd_1 gnd_1 n105 w=0.5u l=0.03u nf=1 m=1








.tran '1f' '7.5n' name=tran
.measure tran mydelay TRIG AT='0' TARG v(dout) VAL='0.9' RISE='0.9' TD='0'
.measure tran offset FIND 'v(offset)' AT='mydelay'

.option primesim_remove_probe_prefix = 0
.probe v(*) i(*) level=1
.probe tran v(bl) v(blb) v(dout) v(pch) v(saf) v(sat) v(sen)

.temp 25
.include './stimGen/stimGen.sp'

.ic  v(dout)=0

.option primesim_output=wdf


.option parhier = LOCAL
.option replicates
.option primesim_mcseed
.option primesim_write_mcparam = 0
.option primesim_identical_mc_instance_file
.option primesim_mcbrief = 1






.end


```


* **Netlist for Demostrating Read Operation**


```bash


*  Generated for: PrimeSim
*  Design library name: memory_read_sram_SA
*  Design cell name: read_operation
*  Design view name: memoryreadd
.lib 'saed32nm.lib' TT

*Custom Compiler Version S-2021.09
*Sat Feb 26 05:51:56 2022


********************************************************************************
* Library          : memory_read_sram_SA
* Cell             : read_operation
* View             : memoryreadd
* View Search List : hspice hspiceD schematic spice veriloga
* View Stop List   : hspice hspiceD
********************************************************************************
xm50 d net12 vdd vdd p105 w=1.35u l=0.03u nf=1 m=1
xm31 net12 dout vdd vdd p105 w=1.35u l=0.03u nf=1 m=1
xm30 bl bpch blb vdd p105 w=1.6u l=0.03u nf=1 m=1
xm29 bl bpch vdd vdd p105 w=1.6u l=0.03u nf=1 m=1
xm28 blb bpch vdd vdd p105 w=1.6u l=0.03u nf=1 m=1
xm23 q qb vdd vdd p105 w=0.1u l=0.04u nf=1 m=1
xm22 qb q vdd vdd p105 w=0.1u l=0.04u nf=1 m=1
xm20 vdd net2 dout vdd p105 w=0.24u l=0.03u nf=1 m=1
xm18 vdd dout net2 vdd p105 w=0.24u l=0.03u nf=1 m=1
xm16 vdd net5 dout vdd p105 w=0.9u l=0.03u nf=1 m=1
xm14 net1 sat vdd vdd p105 w=0.9u l=0.03u nf=1 m=1
xm12 net5 net4 vdd vdd p105 w=0.9u l=0.03u nf=1 m=1
xm11 saf sen bl vdd p105 w=0.5u l=0.03u nf=1 m=1
xm10 sat sen blb vdd p105 w=0.5u l=0.03u nf=1 m=1
xm6 saf pch sat vdd p105 w=0.12u l=0.03u nf=1 m=1
xm5 saf sat vdd vdd p105 w=0.1u l=0.04u nf=1 m=1
xm4 saf pch vdd vdd p105 w=0.12u l=0.03u nf=1 m=1
xm3 sat saf vdd vdd p105 w=0.1u l=0.04u nf=1 m=1
xm2 sat pch vdd vdd p105 w=0.12u l=0.03u nf=1 m=1
xm0 net4 saf vdd vdd p105 w=0.9u l=0.03u nf=1 m=1
xm51 d net12 gnd_1 gnd_1 n105 w=0.75u l=0.03u nf=1 m=1
xm32 net12 dout gnd_1 gnd_1 n105 w=0.75u l=0.03u nf=1 m=1
xm27 qb wl blb gnd_1 n105 w=0.2u l=0.04u nf=1 m=1
xm26 q qb gnd_1 gnd_1 n105 w=0.25u l=0.04u nf=1 m=1
xm25 qb q gnd_1 gnd_1 n105 w=0.25u l=0.04u nf=1 m=1
xm24 q wl bl gnd_1 n105 w=0.2u l=0.04u nf=1 m=1
xm21 gnd_1 net2 dout gnd_1 n105 w=0.15u l=0.03u nf=1 m=1
xm19 gnd_1 dout net2 gnd_1 n105 w=0.15u l=0.03u nf=1 m=1
xm17 gnd_1 net1 dout gnd_1 n105 w=0.5u l=0.03u nf=1 m=1
xm15 net1 sat gnd_1 gnd_1 n105 w=0.5u l=0.03u nf=1 m=1
xm13 net5 net4 gnd_1 gnd_1 n105 w=0.5u l=0.03u nf=1 m=1
xm9 net3 sen gnd_1 gnd_1 n105 w=1.5u l=0.03u nf=1 m=1
xm8 saf sat net3 gnd_1 n105 w=1u l=0.04u nf=1 m=1
xm7 sat saf net3 gnd_1 n105 w=1u l=0.04u nf=1 m=1
xm1 net4 saf gnd_1 gnd_1 n105 w=0.5u l=0.03u nf=1 m=1
c52 d gnd_1 c=10f
c11 blb gnd_1 c=100f
c10 bl gnd_1 c=100f








.tran '1f' '1000p' name=tran

.option primesim_remove_probe_prefix = 0
.probe v(*) i(*) level=1
.probe tran v(bl) v(blb) v(bpch) v(dout) v(pch) v(q) v(qb) v(saf) v(sat) v(sen)
+ v(wl)

.temp 25
.include './stimGen/stimGen.sp'

.ic  v(dout)=0  v(pch)=0  v(bpch)=0  v(q)=0  v(qb)=1

.option primesim_output=wdf


.option parhier = LOCAL






.end


```


## Author


```bash
Debjit Batabyal , Indraprastha Institute of Information Technoology Delhi

```


## Acknowledgements


 - Kunal Ghosh, Co-founder, VSD Corp. Pvt. Ltd. - kunalpghosh@gmail.com
 - https://www.iith.ac.in/events/2022/02/15/Cloud-Based-Analog-IC-Design-Hackathon/
 - Synopsys India
 - [VSD VLSI System Design](https://awesomeopensource.com/project/elangosundar/awesome-README-templates)
 - Chinmay panda, IIT Hyderabad
 - Sameer Durgoji, NIT Karnataka
## References

- A. Saxena et al., "Design Of High Density Memory Cell Library For Low Voltage Operation In 65nm LSTP Technology," 2021 IEEE 18th India Council International Conference (INDICON), 2021, pp. 1-7, doi: 10.1109/INDICON52576.2021.9691642.
- V. Patil, A. Grover and A. Parashar, "Design of Sense Amplifier for Wide Voltage Range Operation of Split Supply Memories in 22nm HKMG CMOS Technology," 2020 33rd International Conference on VLSI Design and 2020 19th International Conference on Embedded Systems (VLSID), 2020, pp. 37-42, doi: 10.1109/VLSID49098.2020.00024.
