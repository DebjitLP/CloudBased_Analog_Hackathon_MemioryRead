
# Memory Read operation using 6T SRAM cell and Conventional Latch type Sense Amplifier 
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
| `SEN-Q Latch Delay (Read 0) ` | >150 ps | 
| `SEN-Q Latch Delay (Read 1) ` | >150 ps |
| `Bitline Load` |  100 fF |    
| `Output Load` |  10 fF |    

* **Reference Design**

![Reference Circuit Diagram](https://github.com/DebjitLP/CloudBased_Analog_Hackathon_MemioryRead/blob/main/Circuit%20Diagrams/reference_design.png?raw=true)

* **Reference Waveforms**

![Reference Circuit Diagram](https://github.com/DebjitLP/CloudBased_Analog_Hackathon_MemioryRead/blob/main/Waveforms/wav.png?raw=true)



##  SRAM Cell Design

The objective of SRAM cell design is to arrive at an optimal cell ratio and optimal pull up ratio. For performing a quick read operation, the cell current should be optimal so that bitline discharges within a given period of time. Signifying that the combination of passgate and pulldown should be less resistive. Also taking care of the bitline leakage when not doing any read operation, the minimum length of the technology is not used. Cell stability also becomes a very important factor so the pull downs should be Larger than the pass gate. So that the Vbump generated during read operation would be smaller than SNM and then we can accept more noise preventing any cell flip during read operation. For the same reason. Pass gates should be more resistive than pull down. Pull down should be large, but not too large. Also, the sizing of pull up devices becomes important in this case so as to complete the Write operation. To be Able to write into a cell the combination of Write driver and pass gate should be stronger than that of the pullup. Pullups are the weakest device of the SRAM cell. Pull up devices also need to be optimally sized so as to maintain the Writability and write speed of the SRAM cell. 

So, the strength of the devices is in the following order Pulldown > Passgates > Pullup. 

Length greater than minimum technology size is used to prevented any unwanted leakage.

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
| `Max BL Discharge` | 31 mV |

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
| Devices |  W/L     (u/u)           |
| :-------- |  :------------------------- |
| `Precharge Devies` |  0.12 / 0.03 |
| `Pullup Devices` | 0.1 / 0.04 | 
| `Pass Gates` | 0.5 / 0.03 |
| `Pull Down devies` | 1 / 0.04 |   
| `Load Inverter PMOS` | 0.9 / 0.03 |   
| `Load Inverter NMOS` | 0.5 / 0.03 |   
| `Output PMOS` | 0.9 / 0.03 |   
| `Output NMOS` | 0.5 / 0.03 |    
| `Latch PMOS` | 0.18 / 0.03 |   
| `Latch NMOS` | 0.1 / 0.03 |   

* **Achieved Read Specification**

| Parameter |  Specification                |
| :-------- |  :------------------------- |
| `SEN-Q Latch Delay (Read 0) ` | 50.5 ps |
| `SEN-Q Latch Delay (Read 1) ` | 63 ps |

* **Waveforms**

* BLB Discharging - Leading to a read 0.

![Reference Circuit Diagram](https://github.com/DebjitLP/CloudBased_Analog_Hackathon_MemoryRead/blob/main/Waveforms/read0_sense.png?raw=true)


* BLB Discharging - Leading to a read 0.

![Reference Circuit Diagram](https://github.com/DebjitLP/CloudBased_Analog_Hackathon_MemoryRead/blob/main/Waveforms/read1_sense.png?raw=true)


## Memory Read Operation



## Simulation Results
## API Reference

#### Get all items

```http
  GET /api/items
```

| Parameter | Type     | Description                |
| :-------- | :------- | :------------------------- |
| `api_key` | `string` | **Required**. Your API key |
| `api_key` | `string` | **Required**. Your API key |

#### Get item

```http
  GET /api/items/${id}
```

| Parameter | Type     | Description                       |
| :-------- | :------- | :-------------------------------- |
| `id`      | `string` | **Required**. Id of item to fetch |

#### add(num1, num2)

Takes two numbers and returns the sum.

