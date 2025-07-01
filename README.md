# DIGITAL_VLSI_SOC_DESIGN_AND_PLANNING
**### Sky130 Day 1 - Inception of open-source EDA, OpenLANE and Sky130 PDK**
## How to talk to computers
# Introduction to QFN-48 Package, chip, pads, core, die and IPs


![Image](https://github.com/user-attachments/assets/ca642cb4-506f-42e8-9df4-9ad36a755874)
The above board can be defined in the block diagram shown below

![Image](https://github.com/user-attachments/assets/a9406d1b-1291-4f60-9c5f-29037692133a)
The yellow circled part is the processor. When we open up the yellow circled IC it will look something shown as below. this is called package Quad flat no lead. Chip is sitting at the center of the package and is connected to the package with wire bonds. Through these wire bonds we transfer all the external signal to the internal of the chip.  The designing of this chip from abstract level all the way down to the fabrication is done by RTL to GDSll flow

![Image](https://github.com/user-attachments/assets/34e7eea1-f5f3-4163-81e5-a92f17d0c1de)
Pads are something through which we send the signal inside the chip.
Core- where all the digital logics like MUX, AND etc are placed.
Die: Present at the corner. it is the size of the entire chip.
Die is getting manufactured at the silicon wafer.

![Image](https://github.com/user-attachments/assets/b3f9b472-7ae7-4139-8cdf-e521ca445a16)
Typical chip consists of RISCV, SRAM, ADC etc as shown below.
Foundry IPs(intellectual properties)- SRAM+adc+dac+PLL
Foundry is a big factory which has got machines and where are chips get manufactured. 
![Image](https://github.com/user-attachments/assets/f8fcadb7-fb0d-49eb-9f87-8ef77e3b730a)

![Image](https://github.com/user-attachments/assets/c5aa9c33-d3c4-48a6-bef6-7d35f38e804b)
Macros are pure digital logics. if i want to manufacture entire chip i need to communicate with Foundry and that is done by interface. 
now we will learn how to connect to foundry from the design point.
## Introduction to RISC-V
This is a way we are going to talk to the computer. the c program shown is first compiled in assembly level language which is RISC V assembly language program.now this ssembly language program is converted to machine language program( binary language) understood by hardware of computer. Originally it's in hexadecimal format which needs to be converted to binary format which finally gets executed on layoutand we get required output. ( for eg we want to swap two numbers).

![Image](https://github.com/user-attachments/assets/fab276a6-d3e3-487a-92dd-70921571d840)
## **From Software Applications to Hardware
System software converts the application software into binary language. 
Major components of system software is 
OS-Handles IO operations, allocates memory, low level system functions

Compiler takes the output from the operating system as C,C++,Java and convert them into intstructions. These instructions depends upon hardware.

Assembler take the instructions from compiler and convert them into respective binary numbers. This binary language now send to hardware and hardware performs output based on the function it receive and gives the output.

![Image](https://github.com/user-attachments/assets/4db98e67-27f6-4e03-806a-3f69ee69fe95)
### Soc design and OpenLANE
## **Introduction** to all components of open-source digital asic design
To design Digital ASIC, few tools or things which are required from the day one. These are

![Image](https://github.com/user-attachments/assets/6f4a27c7-4c13-450b-872e-4ac99c2d7387)
OpenLANE is a EDA tool.
RTL Design EDA tools PDK data what is RTL design? In digital circuit design, register-transfer level (RTL) is a design abstraction which models a synchronous digital circuit in terms of the flow of digital signals (data) between hardware registers, and the logical operations performed on those signals.for this designs many open sorces are available. like, librecores.org, opencores.org, github.com, etc...

What is EDA tools? The term Electronic Design Automation (EDA) refers to the tools that are used to design and verify integrated circuits (ICs), printed circuit boards (PCBs), and electronic systems, in general. many open sorces tools are available like Qflow, OpenROAD, OpenLANE, etc...

What is PDK Data? PDK is process design kit. It is interface between FAB and design. This data is collections of files like,

process design rules: DRC, LVS, REX Digital standerd cell libreries i/o librerirs etc..... which are used to model a fabrication process for the EDA tools used to design an ICs. for example, in 2020, google release the open source PDK for FOSS 130nm production with the skywater technology. But right now it is at cutting age of the 5 nm also. But in many applications, the advance node is not required, and the cost of advanced node is also high as compared to 130nm processors. This 130nm processors are also fast processor. for example,

intel: P4EE @3.46 GHz(Q4'o4)

sky130_OSU (single cycle RV32i CPU) pipeline version can achieve more than 1 GHz clock.
## Simplified RTL2GDS flow

![Image](https://github.com/user-attachments/assets/6515eca7-48af-4573-9cc0-b36344f9586c)
Step 1. Synthesis:- In the synthesis, the design RTL is translated to a circuit out from the SCL. The resultant circuit is describes in HDL and usualy refered to the gate level netlist. the gate level netlist is functionaly equivelent to the RTL. "standard Cells" have regular layouts like Electrical. HDL,SPICE

Step 2. Floor/Power Planning:-The main objective here is that to plan silicon area and distribute the power to the whole circuit. In the chip floor planning, the partition chip die between different system building blocks and place the i/o pads. In micro floor planning, we define the dimensions, pin locations, rows. In power planning, the power network is connstructed. tipically, the chip is power by multiple VDD and GND. so, total components are connected to power supply horizontaly and vertically by metal streps. here parallel structures are used to reduce the resistance. To address the electromagnetization problem, power distribution network uses upper metal leyers, which are thicker than lower metal layers. Hence have less resistance.

Step 3. Placement:- In this process, we place the gate level netlist on the floor planning rows, alligned with the sites. cells should be placed very closed to eachother to reduce the interconnnect delay. Usually placement is done in 2 steps:

Global placement:- It is very first stage of the placement where cells are placed inside the core area for the first time looking at the timing and congestion. Global Placement aims at generating a rough placement solution that may violate some placement constraints while maintaining a global view of the whole Netlist.

Detailed placement:- In detailed placements, we determined the exact route and layers for each netlist. the objective of detailed placement is valid routing, minimize area and meet timing constrains. Additional objective is minimum via and less power.

![Image](https://github.com/user-attachments/assets/798b65f6-8ef4-4e3a-b4cf-e0fcbf3ba74c)
Step 4. Clock Tree Synthesis:- Before routing the signals, we have to route the clock. In the process of clock synthesis, we have distribute the clock to the every sequential elements. for example flipflops, registers, ADC, DAC etc. basically clock netwroks looks likes a tree. where the clock source is roots and the clock elements are end leaves. Synthesization should be done in a manner that with minimum skew and in a good shape. To minimize the clock skew by using the low-skew global routing resources for clock signals. Microsemi devices provide various types of global routing resources that significantly reduce skew. Usually a tree is a H tree, X tree etc.

![Image](https://github.com/user-attachments/assets/505645c4-6665-46d2-abbb-91ccee58db7f)
Step 5. Routing:- After routing the clock, the signal routing comes. Making physical connections between signal pins using metal layers are called Routing. Routing is the stage after CTS and optimization where exact paths for the interconnection of standard cells and macros and I/O pins are determined. There are two types of nets in VLSI systems that need special attention in routing:

Clock nets Power/Ground nets The sky130 PDK defines the 6 routing layers. the lowest layer is called local interconnect layer (titanium nitride layer). Other five layers are aluminium layers In the process of routing, metal tracks forms a routing grids and these grids are huge. so, divide and conquer approach is use for routing. The two types of routing is used:
Global routing: Generates the routing guides
Detailed Routing: Uses the routing guides to implement the actual wiring. 

Step 6. Sign Off:- Once the routing is done, we can construct the final layout. This final layout will goes under the verification. Two types of verifications are there:

Physical verification: Here design rule checking will done and it will check the final layout and owners layout Timing Verification: Here Static Timing Analysis will done.
## **Introduction** to OpenLANE and Strive chipsets

**_Everything About OpenLANE_**
OpenLANE is a fully open-source, automated digital ASIC implementation flow that takes a design from RTL (Register Transfer Level) all the way to GDSII, ready for chip fabrication. It is designed to democratize silicon design, making advanced chip design accessible to academia, startups, and hobbyists by integrating a suite of open-source EDA tools and providing a streamlined, highly configurable workflow.
**_Key Features and Philosophy_**
End-to-End Flow: Automates all major ASIC design steps: synthesis, floorplanning, placement, clock tree synthesis, routing, signoff, and GDSII generation.

Open Source: Built entirely from open-source tools (Yosys, OpenROAD, Magic, Netgen, etc.), under permissive licenses.

Customizable and Extensible: Especially in OpenLANE [2](https://www.globenewswire.com/news-release/2024/04/18/2865597/0/en/Efabless-Announces-the-Release-of-the-OpenLane-2-Development-Platform-Transforming-Custom-Silicon-Design-Flows.html), users can script and customize flows, add new steps, and tailor the process to their needs using Python.

Community Driven: Maintained and improved by a large, active community, with extensive documentation and support resources
OPENLANE is an automated RTL to GDSII flow that is composed of several tools such as OpenROAD, Yosys, Magic, Netgen, Fault, CVC SPEF-Extractor, CU-GR, Klayout and a number of scripts used for design exploration and optimization. It is started as an Open-source flow for a true Open Source tape-out Experiment. striVe is a family of open everything SoCs: Open PDK, Open EDA, Open RTL

striVe SoC Family

![Image](https://github.com/user-attachments/assets/06b07df5-45fd-4be6-a095-0907f6b6fa3d)
## **Introduction** to OpenLANE detailed ASIC design flow

![Image](https://github.com/user-attachments/assets/814ae8dc-8e54-4d8a-b448-f8143cfcbd27)

## **Get** familiar to open-source EDA tools
### **OpenLANE** Directory structure in detail

Linux commands
cd : change directory. It opens the folder

ls -ltr : It lists the content of folder in chronological order

ls : lists the content of the folder

pwd : shows the present working directory

mkdir : to make a new directory

ls --help : This will list the switches with their meanings

clear : To clear the screen
pdk is process design kit. Here we are working in Sky130_fd_sc_hd PDK varient. where, "sky130" is process name or node name."fd" is a foundary name (skyWater foundary)."sc" means standerd cell librery files and the last one "hd" stands for high density(basically one type of varient).

Sky130_fd_sc_hd varient contains many technology files like verilog, spice, techlef, meglef, mag, gds, cdl, lib, lef, etc. (techlef file contains the layer information).
 
![Image](https://github.com/user-attachments/assets/b7676285-dd77-4809-a766-c22636874e82)
libs.tech is specific to the tool

![Image](https://github.com/user-attachments/assets/f42183e3-cd6d-4006-8a61-d84b6a3b167d)

![Image](https://github.com/user-attachments/assets/36d9b88a-6c25-48ae-9dc1-ea00d1e9f3d0)
### **Design** Preparation Step
when we enter in the OpenLANE, we have to use flow.tcl because as a name says, it will goes with the flow using the script. And by using interactive switch, we will do step by step process. without interactive switch, it will run complete flow from RTL to GDSII. Now OpenLANE is open and we can see that prompt will change now.

![Image](https://github.com/user-attachments/assets/dd064767-c30a-49a2-bc08-5b92c0198119)
Now we are required to import all the packages. All the designs are in designs folder. 
here there are many designs already. we can make our design too. Currently we'll open picorv32.
src file where our verilog RTL file will be present. 
![Image](https://github.com/user-attachments/assets/b2f9e5bb-fbe3-47df-9433-8517bb61427a)

 In this design we can see many files are available. i.e., scr, config.tcl, etc. This config.tlc file contains every details about the design. for example, details about enrollment, clock period, clock period port etc. This 'config.tcl' which bypasses any configuration that has been done in openlane. Many of the switches use the default that has been present in the openlane source.It overwrites the settings and become specific to the design. Here RTL file, SDC file, clock period has already been set. Also the filename has been given, but when we run our custom file 'sky130_fd_sc_hd_config.tcl' file won't be there. Openlane takes the value in the following order: First is the default value which is already set in openlane, Second is config.tcl and third is sky130_fd_sc_hd_config.tcl. So the highest priority is sky130_fd_sc_hd_config.tcl, it will overwrite the default and config.tcl. This was the design part
Now we need to set up the file system specific to the flow that will be fetched from a particular location in openlane using the command 'prep -design picorv32a'.
![Image](https://github.com/user-attachments/assets/7a434e6e-7ffe-47f8-a961-dceeb38fa277)
we can see in below fig from where verilog file is being picked up. clock period as 5ns. 
let's go back to the open lane prompt to different tab. Now, in openlane, we are going to run the synthesis, but before synthesis, we have to prepare design setup stage. for that command is  prep -design picorv32a.

![Image](https://github.com/user-attachments/assets/33f499b8-a9e5-4b4d-8b0d-3de30852b447)
### **Review** files after design prep and run synthesis
folder will be created with today's date. we will open it. inside it apart from tmp folder all the folders will be empty as of now. In the temp file, merged.lef file is available which was created in preparation time. if we open this merged.lef file, we get all the wire or layer level and cell level information.  In tmp--> write command less merged.lef, it is the file which was created during preparation time which includes wire. layer levels, vias, cell level information and so on.

![Image](https://github.com/user-attachments/assets/ae81f1fd-00bc-4f65-9308-9f260fca1bac)
This config.tcl contains default value.

![Image](https://github.com/user-attachments/assets/29906ca7-b37d-4f39-92db-4205baf1ef44)

![Image](https://github.com/user-attachments/assets/fda077e8-0652-4d2c-8664-9e3385cf05aa)
now coming back to Openlane promt preparation is already done. now we have to do the synthesis. run_synthesis will run wire synthesis. this will take 3 to 5 mins. 

![Image](https://github.com/user-attachments/assets/4f6173e4-1269-4b46-8432-31469321151d)

![Image](https://github.com/user-attachments/assets/ca7650f8-d6be-450e-817e-ed3cc72c329a)
Synthesis was done.
### **OpenLANE** Project Git Link Description
search openlane efabless on google. click on github link.Read for better understanding on openlane. Watch on youtube ([FOSSi Dial-Up] Tim Ansell - Skywater PDK: Fully open source manufacturable PDK for a 130nm process). 
### **Steps** to characterize synthesis results
on scroling up the synthesis completed tab we will find d flip flop. no of cells.

![Image](https://github.com/user-attachments/assets/94972de0-f876-4f6a-bcfd-4bedb411a616)
After synthesis our first step would be to calculate the Flip Ratio;
Flip Ratio=no. of D flip flops/ No. of cells
1613/14876= 0.108

# **DAY-2** Sky130 - Good floorplan vs bad floorplan and introduction to library cells

### **Utilization** factor and aspect ratio
# **Define** width and height of core and die
In this the first step in the physical design is to DECIDE THE HEIGHT AND WIDTH OF CORE AND DIE. We will start with the basic netlist. 
![Image](https://github.com/user-attachments/assets/faf34b02-1ff3-4908-8ac0-f8dab0d911e0)
We are dependent on the dimensions of logic gates and FFs. We will try to give proper length and breadth to this particular gate.
![Image](https://github.com/user-attachments/assets/15cf6902-4b36-465c-a0e5-49898b404b7b)
Next, we are actually interested in finding the dimensions of core and die rather than the wires as of now.So, we will find the dimensions of the standard cells first.
Considering the dimensions of standard cells as 1unit X 1unit, the Area we get = 1 sq. units
With the help of netlist, we will identify the Area occupied by the std. cells on the silicon wafer.
![Image](https://github.com/user-attachments/assets/3af6f37a-442a-4566-a1a7-930a65403a37)
Before that Let's remove the wires and place them together(as shown below).
Now the area of the netlist becomes Area= 2units X 2units= 4 sq.units

![Image](https://github.com/user-attachments/assets/b0bb866f-4979-48cd-9d2e-1bb5d3281522)

What is a core and a die?
On a silicon wafer, one section is die--> inside doe there is a core(a core is a section of the chip where fundamental logic of the design is placed).
(a die which consists of a core, is a small semiconductor material specimen on whcih the fundamental circuit is fabricated).

![Image](https://github.com/user-attachments/assets/64b05459-f67b-4dca-9a87-e8626d1d1da0)

![Image](https://github.com/user-attachments/assets/8623f4c8-aa1f-4e2a-9936-9362f35c0502)
In the above example the Utilization factor=1, but practically only 50-60% utilization is possible.

Another important term is 'Aspect Ratio', which is Height/Width. So in this case the aspect ratio is 1 that means chip is square. If aspect ratio is not equal to 1 that means the chip is rectangle. In such case , the remaining place is optimized by using some other circuitry.

![Image](https://github.com/user-attachments/assets/4f9b35ed-bcde-468f-82b3-b13c30ab5ed5)

# **Concept** of pre-placed cells

Let's take another example where the width and height of die is 4 units by 4 units and we have the netlist of 2units by 2 units, so if we calculate the utilization factor it will come around 25%. That means 75% of the core is empty and can be used for other optimazation, routing and wires. 

Define locations of Preplaced Cells:- Lets take a combinational logic which does some amount of function and assume its a huge circuit having some N Logic gates so let's devide it into some small numbers of gates. We will cut the whole circuit into two parts, and separate both of them into two blocks and both block will be implemented seperately.

![Image](https://github.com/user-attachments/assets/3147e7bd-5098-4a8f-815f-7165b12b66e0)

Considering the two blocks, we separate the input output pins of both the blocks. Separate the blocks and make them a black box, the I/O pins will also be used separately. The advantage of this kind of system is that we don't have to implement the circuit multiple times, the same black box can be sent to different users for separate usage. This will reduce the number of logic gates. This a concept of Reused models.

![Image](https://github.com/user-attachments/assets/8d6503f7-6e1d-433a-93df-c20e446f67b1)

![Image](https://github.com/user-attachments/assets/d66e5c65-dc34-4409-8a24-b0ac9134db5b)

Advantage of doing this is we can reuse them multiple times after implimenting once only. Similary there are other IP's also available for eg. Memory, Clock-gating cell, Comporator, MUX all of these are part of the top level netlist.They recieve some signals and perform functions and deliver the outputs but the functionality of the cell is implemented only once.

The arrangement of these IP's in a chip is refferd as floorplanning.

These IP's have user-defined locations, and hence are placed in chip before automated placement and routing are called "pre-placed cells".

These cells are placed in such a way that, the placement and routing tool do not touch the location of the cell.

# **De-coupling** capacitors

Earlier we discussed about the locations of the pre-placed cells, that is it needs to be fixed and fill not change in the further processes also.
After this THE PREPLACED CELLS WILL BE SURROUNDED BY DECOUPLING CAPACITORS. Now, what are decoupling capacitors and why do we need them..?
Consider a piece of circuit as shown below. Whenever this circuit switches, e.g: AND gate switches from logic 0 to Logic 1 there is a demand of current. For this a small capacitor is placed so whenever transition from 0-->1 the capacitor charges to show 1. It is the responsibility of applied voltage(Vdd) to supply current to all the logics. Similarly when logic switches from 1-->0 the capacitor discharges to 0, for this Vss is responsible. But every physical wire has some equivalent resistance, inductance and capacitance of it's own, which leads to drop in the supply voltage.

So if the supply voltage is suppose 1V then due to resistances of wire due to voltage drop the voltage reached is 0.8 or 0.7V (Vdd').
The capacitor will now charge till 0.7V only. Now if the 0.7V lies between the high and low margin region, then it will be a problem as it can switch to 0 or 1 irrespective of the requirement.
This is the problem of having a large distance between the main power supply and the physical circuit.

![Image](https://github.com/user-attachments/assets/33dd57e3-6e53-4d44-a170-6a06c8920ae9)

![Image](https://github.com/user-attachments/assets/aef9d8b5-2e2a-4642-8728-949c247396ce)

This problem can be solved using De-coupling capacitors, De-coupling capacitors are large capacitors which are charged till the applied voltage. They are placed very close to the main circuit so that there is hardly any voltage drop. The capacitor acts as a shock absorber for the chip's power supply. It smooths out sudden jolts in voltage just like a damper absorbs mechanical shocks. So, as the name suggests it decouples from the main circuit.
 Every time the circuit switches, it draws current from Cd, whereas, the RL network is used to replenish the charge into Cd. And the amount of current needed for the circuit is supplied by the De- Coupling Capacitor.

![Image](https://github.com/user-attachments/assets/e2d8bccf-6dec-4667-a127-0c9d42cc22f3)

Below image shows how the main circuit blocks are surrounded by the decoupled capacitors. This ensures that there is proper switching and no cross-talk.

![Image](https://github.com/user-attachments/assets/74bb3246-44e3-4657-9f5e-821f78db1683)

# **Power** planning

Let us suppose there are multiple macros, and we have connected the decoupling capacitor to all the macros individually. There is a driver connected to load. The macros are connected to the main power supply(Vdd). As in the diagram we can see the driver and load are connected with a 'red' wire. We want the logic operation going on in driver to be transmitted to load. But here also there will be voltage drop due to wire's resistance. Furthermore, we can't connect decoupling capacitors here as it is not feasible to connect the decouple capacitors everywhere.

![Image](https://github.com/user-attachments/assets/7a7f238c-9121-45fd-9c68-31eb5e687496)

Let the 'red' wire represents a 16 bit bus, suppose we are giving a 16 bit signal, where for 1 the capacitors have to be fully charged till V, and for 0 the capacitors have to be discharged. Now, if we connect an inverter at the load, the 1 has to turn to 0, that means Capacitors voltage needs to be discharged to ground simultaneously. Due to discharge at single 'Ground' tap point, there will be a bump at the Ground called as 'Ground Bounce'. Which will lie in between the noise margin levels causing the disrupt in the output values. when the capacitors are charging from 0 to 1 so they are demanding current supply at the same time, this will create a 'Voltage Droop'.


![Image](https://github.com/user-attachments/assets/0e5af927-a68a-41ff-9f9f-63b19ffcf5db)


![Image](https://github.com/user-attachments/assets/23b3c622-14e9-49c8-8075-a32db67339fb)

![Image](https://github.com/user-attachments/assets/d173be88-73b5-4817-97da-1ec3e3cc23e4)

These problems occur only because there is only one power supply, if there would have been multiple power supplies then this wouldn't happen. For example as shown below.Therefore, while designing chips we give multiple power supplies. So that any logic will take it's power from it's nearest power supply and dump it's current to it's nearest ground.

![Image](https://github.com/user-attachments/assets/63697fec-843e-4846-9a05-7ffb49706b1f)

![Image](https://github.com/user-attachments/assets/ef037677-84bb-4670-9441-6260dc939e92)
This is how we do Power Planning by giving horizontal and vertical lines and the interconnects are the contacts.

# **Pin** placement and logical cell placement blockage

Let's consider the example design which needs to be implemented, with input-output terminals and individual clocks. Later connecting the pre-placed blocks to the below logic gates.
Lets take below designs for example that needs to be implemented. Here first circuit is driven by clk1 and second circuit is driven by clk2 and both has different inputs Din1 and Din2 respectively and outputs as Dout1 and Dout2.Along with that we have some preplaced cells as well as Blocka which recieves inputs from Din1 second input from Din2. We have another preplced cell as Blockb Which recieves input from clk1 and clk2 and provides a clk output. So currently we have 4 input ports Din1,Din2,Clk1,Clk2 and 3 output ports Dout1,ClkOut,Dout2

![Image](https://github.com/user-attachments/assets/6f00cf12-bd8d-4cad-98a1-2e9d53db8343)

![Image](https://github.com/user-attachments/assets/1be8945b-41fc-477e-849f-ed509ba4106c)

Now, taking one more section of the same circuitry with two different clocks for different FFs, showcasing the concept of 'Interclocks Timing Analysis'.Also, including the pre-placed cells in between.

![Image](https://github.com/user-attachments/assets/1abd1aef-b70f-446a-86da-5e9e86d57b31)

complete design becomes like given below which has 6 input ports and 5 output ports. The connectivity information between the gates is coded using VHDL/Verilog language and is called as 'Netlist'.

![Image](https://github.com/user-attachments/assets/38ac2eca-850d-4381-8171-49a25c402298)

generally the trend is we place all the input ports on the left side and output ports on the right side but it can vary.

![Image](https://github.com/user-attachments/assets/05e633f6-0694-4f70-bf46-c8d14d24d4f6)

Now let's see how will bw the pin placement.We need to place the logic circuit between the gap of core and die. Here, the input port is on the left and output port is on the right, but it can vary. We observe few things here, First the ordering of input and output ports are random depending upon the requirements. As block a is connected to D1 and D4 inout so they are placed near to that and block b is at Dout1 and Dout 3. Also as the blocks are placed at certain area so we ensure that the cell placement is not in that area.This is where the hand-checking between frontend and backend teams comes into picture. Frontend team defiens the netlist connectivity and backend defines the pin placement. Second thing to observe is the size of clocks is much bigger than the size of inputs and output pins. This is because the clocks is the driving source of the I/O pins, FFs and the complete chip. So we need the least resistance path, therefore bigger the size lower is the resistance. Next we will ensure that where the pin placement has been done, we need to block that area for routing and placement tool ,This is done by 'Logical cell placement blockage'.

After the logical cell placement blockage step our floorplan is done for placement and routing step.

# **Steps** to run floorplan using OpenLANE

We will doing the floorplanning in openlane. For Floorplanning we require some switches which we will get in 'configuration' file in openlane.
Inside the configuration there is a README file--> enter into that.

![Image](https://github.com/user-attachments/assets/557fd86e-7fd4-444a-90a5-663bac4c6bb7)

Here you will see the variables required for each stage, such as global variable with design name, synthesis variables and so on.
Different switches are given under floorplanning as shown.

![Image](https://github.com/user-attachments/assets/d28880b4-6edf-4232-a273-48a02820c8c5)

PDN stands for power distribution network.
Now we need to set the switches, for that go back to the README file, in the floorplan.tcl directory we will see the default switches which are being set already, We can see (FP_IO_MODE):1 means that the i/o pins is positioned randomly but at equidistant, for 0 means not positioned equidistant.
open less floorplan.tcl 

![Image](https://github.com/user-attachments/assets/68245d23-585c-46dc-86d2-99d4e63f777f)
 
if openlane closes follow the steps from docker this synthesis completed. write run_floorplan. this will take few seconds

![Image](https://github.com/user-attachments/assets/0daaf6d0-3487-43af-aae2-2588e75bdc51)

![Image](https://github.com/user-attachments/assets/7ace37f7-3f9d-48e1-9718-6ca303612748)
Our floorplan was successful.

# **Review** floorplan files and steps to view floorplan

As we have run the floorplan, just like we did for synthesis we will go inside picorv32a and check for the present date when the floorplan file was created. Then we will go into the floorplan, and open the directory 'ioplacer.log' and we did the placements in input output.

![Image](https://github.com/user-attachments/assets/5910a7b0-4f2d-401c-bb26-40f5980fb7b8)

![Image](https://github.com/user-attachments/assets/f9829bee-b03c-436e-ac09-ed3745ade86f)


![Image](https://github.com/user-attachments/assets/88714e9a-547e-4dc1-9be7-4f87b811f940)

![Image](https://github.com/user-attachments/assets/0af8beb1-d8f9-458e-8a40-578a1ea41932)

![Image](https://github.com/user-attachments/assets/9e65cd04-ba0e-4e16-b501-2a57258d347b)
Here utilization factor is 35 because of sky_130_ has the highest priority.
![Image](https://github.com/user-attachments/assets/b7348af4-b334-4999-8ce5-e65e205c2c6a)

![Image](https://github.com/user-attachments/assets/69b7abba-ec3e-458e-b441-ad10e4ae5012)

![Image](https://github.com/user-attachments/assets/5d68bfc1-eefc-471e-97f7-8c57ae5d2b48)

![Image](https://github.com/user-attachments/assets/ea2dacca-18db-4e42-b8e5-88adcd7dcf4f)
Move the matrix in center by selecting the matrix and press V. To select the area on matrix press left mouse and then right mouse and then press Z. To select particular object keep the cursor on that object and press S.

![Image](https://github.com/user-attachments/assets/8b7b5e46-fb93-4de3-94fa-b7ffa8e60e69)
There's tkcon window. when we type "what" it will show in which layer this specific pin is in.

![Image](https://github.com/user-attachments/assets/53013c16-0c7c-4e69-8534-ab3bc73989ee)
This shows it's in metal layer 3.
Along with this we can also see the Decaps(decoupled capacitor) arranged along the border or side rows.Then we have tap cells, which are used to avoid latch-up conditions in CMOS devices, they connect n-well to VDD and substrate to GND.

![Image](https://github.com/user-attachments/assets/259f5b38-1efb-4181-b7f5-b509cf2d3771)
If we notice these cells are diagonally equidistance. This is our floorplan. Zoom the left bottom corner black cell. This is the standard cell at the lower left corner which represents the AND, OR,etc logic gates.

![Image](https://github.com/user-attachments/assets/27186aa1-8ac6-4b09-bb41-3c9ad979401e)
Whenever making any changes make sure to do in config file. It's more flexible. To bring the design to normal size press S+V. To quit press on File and then quit.
## **Library** building and Placement

# **Netlist** binding and initial place design

In Placement and Routing , the first step to bind the physical netlist.In reality the logic gates do not actually have the shape as in which they are shown instead they are represented as a box with certain width and height which is given during designing. So now each and every component of the netlist is now given a proper dimension. Bind netlist with physical cells:- Lets we have the netlist of gates and shape of these gates represents the functionality of this gates. Foe example we have NOT gate as a tringular shape but in reality it is a box with physical dimensions it has width and height.Similarly for AND gate it also has a box shape in reality, Flipfops are also square boxes.So, we have given the physical dimensions to all the gates and flipflops. For everycomponent of the netlist we will give the particular shape with particular dimensions because ir real world the shapes like AND,OR gates does not exists so we make them as square all the blocks also have the width and height and proper shape.

![Image](https://github.com/user-attachments/assets/32410819-5199-4373-a105-19b5061eef1f)

Now we will remove the wires,all the gates, flipflops and blocks are present in the shelf which is called as Library.

![Image](https://github.com/user-attachments/assets/a2ace364-b0ae-4e3a-923d-eb052c7e7107)

A library is a place where you can find all kind of books all the gates,f/f are books here. Library also has the timing information of the perticular book like delay of the gates. Library can be devides into two sublibraries, One library consist of shape and size and other library might consist only of the delay information. Library has the various flavours of each and every cell. Like same cell can have bigger in size in different self, bigger the size of cell lesser the resestnce path so it will work faster and will have lesser delay. We can pick up from these what we want based on the timing condition and available space on the floorplan. So, the library will have the delay of the particular shell, it's width and height, also it's particular information at which condition it will be operated.
Library provides various options about the size.Like in the second case, the gates are bigger in size-->less resistance path-->so faster. Similarly in third case it is even faster.

![Image](https://github.com/user-attachments/assets/b8e5d222-f20e-4879-9f2d-fd9a8cdbbe00)

Now next comes the Placement of the desired netlist on the floorplan. So we have the floorplan, the netlist and the shape of components in netlist.

![Image](https://github.com/user-attachments/assets/e790ab8e-3ce8-4643-ac88-e6ae6361d9d7)

Considering the floorplan that we have along with preplaced cells, we will start placing the FFs by looking at the netlist. As in the netlist the FFs1 is near to Din1 and FF2 is at Dout1 so we will place accordingly.They are placed closed to each other to avoid timing delay.
Also, the stage 2 of logic, you can see that all the FFs and gates are placed together.

Placement:- Once we have given proper shape and size to each and every gates the next step is to take those particular shapes ans sizes and place it on the floorplan. We have the floorplan with inout and output ports, we have particular netlist, and we have particular size given to each component of this netlist. So we have the physical view of the logic gates. Next step is to place the netlist onto the floorplan. We have to take the connectivity information from the netlist and design the physical view gates on the floorplan.
Now, we have the floorplan where we have the preplaced cells from the previous slides, Plcement will make syre that the pre placed cells locations are not affected they are kept as it as and the second thing which will be taken care of that is no cell should be placed over the pre-placed cells. We need to place the physical view of the netlist onto the floorplan in such a fashion that logical connectivity should be maintained and that particular circuit should interact with their input and output ports to maintain the timing and the delay will be minimal.
Now, in the netlist we can see that FF1 is close to Din2 and FF2 is close to Dout3, the distance is quite large.They are arranged diagonally.

![Image](https://github.com/user-attachments/assets/abb08cf4-2467-424c-a29a-d9105d3c1600)


![Image](https://github.com/user-attachments/assets/e646fe1e-d03b-41e9-8cee-f05484ae879c)
Here first we will see the arrangement of the remaining parts from the netlist onto the floorplan.We have placed all the element in such manner that all elements are closed to it's input and output pins. But, the distance of FF1 of Stage 4 and Din4 is still far them others. By optimizing the placement, we can solve this problem.

# **Optimize** placement using estimated wire-length and capacitance

Optimize Placement:- In optimize placement we will resolve the problem of distancing.Lrt's take the example of FF1 to Din2. There must be a wire going from Din2 to FF1 but before going into routing the desing or wiring we will try to estimate the capacitances. If we lokk the capacitance from Din2 to FF1 it is every huge because wire length is huge in that case even the resutance will also be huge because of that length. If we send the signal from Din2 then it will be difficult for FF1 to catch that input because distance is large. So we can place some intermediate steps to maitain the Signal integrity. By this the input is succesfully driven to the FF1 from Din2. These intermediate steps are called here Repeaters , Repeaters are basically buffers that will recondition the original signal and make a few signal which replicate the original signal and send it forward this process repeates untill we reach to the actual cell where we want to send the input in this way signal integrity is maintained. By using repeaters we resolve the problem of signal integrity but there will be a loose of area because more and more repeaters are used more area will be used of the particular floorplan.

![Image](https://github.com/user-attachments/assets/9772becb-54d4-4045-8bd7-d427582a96cc)
In the stage 1, there is no need of any repeater to transmit the signal. But in stage 2, due to high distance, the lenth of wire is high and signal is not transmitted in particular range. so we required repeater.

# **Final** placement optimization

In stage 2 you can see that there no space between the FFs and Logic gates, this is called 'Abetment in placement optimization', this is done when a particular circuit has to run very fast(high frequency application) so there should'nt be any wire placement in between to avoid delay.
As similar to stage 2, in Stage 3 also we required the buffer between gate2 and FF2.

![Image](https://github.com/user-attachments/assets/465bea11-9a61-4799-a86d-935824fc3000)

Stage 4 is bit tricky as compared to other stages. We placed 2 buffers in between, and also there is a criss-cross with other connections in between. So we need to deal with that also further . Now we have to check that, what we have done is correct or not. For that we need to do Timing analysis by considering the ideal clocks and according to the data of analysis, we will understand that, the placement is correct or not.

![Image](https://github.com/user-attachments/assets/328114aa-5244-4e24-a2b9-e4567f2bf0be)

Now we will try to do the Setup Timing Analysis, considering the clocks to be ideal that means giving clock to all the FFs at the same time.

# **Need** for libraries and characterization

Every ICdesign Flow needs to go through the several steps. First step to go through is Logic Synthesis, let's say if we have a functionality which is coded in a form of an RTL so first we need to convert the functionality into legal hardware is refered to as Logic Synthesis. Ouput of the logic synthesis is arrangement of gates that will represent the original functionality that has been described using an RTL. Next step of logic synthesis is Floorplaning, in this we omport the output of logic synthesis and decide the size of the Core and Die. The next step after floorplaning is Placement, in this we take the particular logic cell send place them on the chip in such a fashion that initial timing is better. Next step is CTS(Clock tree synthesis), in this we take care that clk should reach each and every signal at the same time also take care of each clk signal has equal rise and fall.Next step is Routing, routing has to go through the certain flow dependendent on the characterization of the flip flop.And now comes the last step STA(Static timing analysis), in this we try to see the set up time, hold time, maximum achieved frequency of the circuit. One common thing across all stages 'GATES or Cells'.

# **Congestion** aware placement using RePlAce

At present we are more interested in ensuring that the congestion free placement, later we will consider the timing analysis. We have earlier seen that placement occurs in two stages- global placement--> detailed placement
In Global Placement legalization does not happens, it happens in Detailed Placement. Legalization means the standard cells are placed in standard cell rows, they have to be exactly inside and abeted(closely packed) with each other. Also no overlapping, Legalization involves timing.
So while we do run_placement in openlane-->1st global placement happens-->the main objective of this is to reduce wire length and in openlane we use concept og HPWL(Half parameter wire length).Also, Our main motive is to converge the overflow, if it does then the placement is done.
To physically check if placement is done, go into results folder and check for placment, a placement.def file will be created. Open the file in magic using the same tech file as used earlier.

![Image](https://github.com/user-attachments/assets/60d663b9-9915-4085-a83a-cb0312f3b723)

When we run the placement, first Global placement is happens. main objective of glibal placement is to reducing the length of wires.

Now opening the Magic file to see actual view of standerd cells placement.And the actual view in the magic file is given below.
![Image](https://github.com/user-attachments/assets/0601e586-66c9-45cc-b977-195309f551b6)

If we zooom into this, we find the buffers, gates, flip flops in this.

![Image](https://github.com/user-attachments/assets/79bab568-cc57-4692-9e88-c9c30a1e5ff9)

## **Cell** design and characterization flows

# **Inputs** for cell design flow
What are standard cells in typical IC design flow?
Standard cells are pre-designed and pre-characterized logic gates and other fundamental building blocks used in the physical design of integrated circuits (ICs). They form the foundation of digital IC design
Standard cells are: Logic gates (e.g., AND, OR, NOT),Flip-flops, latches,Buffers, inverters, multiplexers, and Special cells (e.g., tie-high/low, filler cells).
In Cell Design Flow, Gates, flipflops, buffers are named as 'Standard Cells'. These standard cells are being placed in the section called as 'Library'.And in the library many other cells are available which have same functionality but the size is different.

![Image](https://github.com/user-attachments/assets/8e3d44c7-1baf-423c-8e82-0d78176e954f)

![Image](https://github.com/user-attachments/assets/f890f29c-db82-4fe8-a2e8-df30b9fe8bd7)

Let's take one particular inverter-->see the cell design flow, this inverter should be understood by a particular EDA tools.It has to be represented in form of shape, size and various cell design flow.
Cell design flow is divided into 3 parts: a)Inputs
b)Design steps
c)Outputs

![Image](https://github.com/user-attachments/assets/36a9a3aa-3e9f-4365-a465-38d5187367b9)

![Image](https://github.com/user-attachments/assets/86dec730-544b-4dbe-b21c-9e8eec772340)

# **Circuit** design step
The seperation between the power rail and the ground rail defines the cell height. Cell width depends upon the timing and drive strength.

2)design steps:- Design involves three steps which are circuit design, layout design, characterization.

In circuit Design there are two steps.

First step is to implement the function itself and second step is to model the PMOS nad NMOS transistor in such a fashion in order to meet the libraray.

3)Outputs

The typical output what we get from the circuit design is CDL(circuit description language) file,GDSII,LEF,extracted spice netlist(.cir).

![Image](https://github.com/user-attachments/assets/d20e49d9-2a6c-4a24-9453-e956f71176e4)

# **Layout** design step

In Layout Design First step is to get the function implemented through the MOS transistor through a set of PMOS and NMOS transistor and the second step is to get the PMOS network graph and the nNMOS network graph out of the design that has been implemented. The first we already discussed that is implementation of the given function, the second step to derive the pmos and nmos network graphs. This is done by 'Art of Layout-Euler's path and stick diagram'. It will give the best layout and best performance.
After we are done with the network graphs, we get the Euler's path. Euler's path is the path which is being traced only once. Based on Euler's path we draw a stick diagram out of it.

![Image](https://github.com/user-attachments/assets/ab80fc6b-6457-43bc-99c7-e84100e2381a)
Next step is to draw stick diagram based on the Euler's path. This stick diagram is derived out of the circuit diagram.

![Image](https://github.com/user-attachments/assets/0a584076-6fba-4939-9820-bad9b4aa845b)

Next step is to convert this stick diagram into a typical Layout, into a proper layout and then get the proper rule we have discissed earlier. Once we get the particular layout then we have the cell width, cell length and all the specifications will be there like drain current, pin locations and so on.

![Image](https://github.com/user-attachments/assets/9487929e-d473-417e-bdc5-3d06121bcac9)

Next and Final step is to extract the parasatics of that particular layout and charaterise it in terms od timing. So before that the output of the layout design will be GDSll. Once you get the extracted spice netlist then we characterize it. Characterization helps in getting timing, noiseand power information.

# **Typical** characterization flow

Let us try to built in characterisation flow from the inputs we have, there are certain steps we need to follow:
i) Reading the model files
ii) Read the Extacted SPICE netlist
iii) Recognizethe behaviour of buffer
iv) Read the sub-circuit of inverter
v) Attach the power sources
vi) Apply the stimulus given to the characterisation step
vii) Provide the necessary output capacitors
viii)Provide the necessary simulation command i.e. For transition simulation-->.tran and for DC simulation-->.dc

![Image](https://github.com/user-attachments/assets/78f7574b-8c1a-4155-8dbc-bbf1b18409b9)
Next is to feed all these steps in characterisation software called GUNA.This software will generate timing,noise and power.libs outputs.

![Image](https://github.com/user-attachments/assets/c0a8dfea-6f1c-4368-8642-b9fc5d9cb35f)

## **General** timing characterization parameters

# **Timing** threshold definitions

As seen in the previous section we have inverter connected back to back, we have power sources, we have the stimulus applied to the inverter all these things brings a very important point of understanding differenet threshold points of a waveform itself and it is called as "Timing threshold definitions'.
Waveform of output of 1st inverter is given as input to 2nd inverter.
slew_low_rise_thr It is voltage level below which a rising signal is considered to have started it's transition. Or we can say that slew low rise threshold depicts the value close to 0.slew_low_rise_thr is typically 20% from bottom power supply.

![Image](https://github.com/user-attachments/assets/3ca828fe-1c5e-452b-b0ba-abb0479f3101)

in the figure below the term 'Slew_low_rise-thr' depicts the value close to 0. and the typically value of this is about 20% it could be 30% as well.

![Image](https://github.com/user-attachments/assets/c43497b0-81b2-4b8d-b077-a56936898159)

Slew_high_rise_thr

![Image](https://github.com/user-attachments/assets/91a2323a-12e3-4276-ae76-635e5a6f67f3)

![Image](https://github.com/user-attachments/assets/10ae69f3-58bf-463c-b49e-807555bcd352)

Slew_low_fall_thr

![Image](https://github.com/user-attachments/assets/e22ca71c-0135-4196-b754-31071ca8c6fd)

Slew_high_fall_thr

![Image](https://github.com/user-attachments/assets/36d4fe57-f8a8-4455-9975-d1f2594e092c)

NOw, taking the waveform of input stimulus which is input of the first buffer and with that taking output of the first buffer.Similar as a slew, thresolds are for delay also available. for that same as slew, we have to take some rise and fall points from the waveforms. this tresolds are almost 50%.

in_rise_thr

![Image](https://github.com/user-attachments/assets/e3383ba7-3026-4277-bd4f-5bc72c4f98f5)

in_fall_thr , its typical value is 50%

![Image](https://github.com/user-attachments/assets/c66e6d69-81e9-4ba4-ba25-1d0cc6989d8b)

out_rise_thr

![Image](https://github.com/user-attachments/assets/cb446761-2d1c-43e9-baee-4b059ef9ee4b)

out_fall_thr

![Image](https://github.com/user-attachments/assets/1572cced-72cf-431a-8644-3a733a933c05)

# **Propagation** delay and transition time

Based on these above values we are going to calculate the further values like propogation delay, current,slews etc.

If we want to calculate the delay of anything we need to subtract the out_rise_thr from in_rise_thr. Here let's take typical value 50%, let's see on the particular waveform how does it works Time delay = Time(out_thr)-time(in_thr).

![Image](https://github.com/user-attachments/assets/e6d065a5-816e-48b6-9105-51bbe87b1d68)

In the above example in_rise_thr and out_fall)thr was kept at 50%. But if the threshold ponit moves to the top the the output comes before the input and we see negative delay and negative delays are not accepted. So the reason behind having this negative delay is poor choice od threshold point so thr choice of the threshold point is really important.

![Image](https://github.com/user-attachments/assets/58d6d125-9c09-4354-bc35-562389afbcaf)

Let's take another example where we have choosed threshold point correctly but still can get a negative delay. Because uotput comes before the input that's why we are getting negative delay here, which is not accepted.

![Image](https://github.com/user-attachments/assets/ff4ca00c-170c-499a-8080-11b139d7ea66)

### **Sky130** Day 3 - Design library cell using Magic Layout and ngspice characterization

# **Labs** for CMOS inverter ngspice simulations

# **IO** placer revision
Till now, we have done floor planning and run placement also. But if we want to change the floorplanning, for example, in our floor planning, pins are at equal distance and if we want to change it then we can also make it by Set command.

For that first we have to check the swithes in the configuration and from that we have to take the syntax "env(FP_IO_MODE) 1". and make it to the "env(FP_IO_MODE) 2". then again run the floorplanning.

![Image](https://github.com/user-attachments/assets/61bc16a5-ab8a-48fd-9205-89cd31198722)

Then check the changes in the pins location through magic -T.


![Image](https://github.com/user-attachments/assets/dcd6acd1-167f-461b-a71c-75eb077724de)

![Image](https://github.com/user-attachments/assets/6edcdfdb-7c11-4a28-a8d6-7f3b0c0f4b07)

![Image](https://github.com/user-attachments/assets/f32efd05-e063-49aa-b106-37cc01b7c5c9)

# **SPICE** deck creation for CMOS inverter
Now we will be doing some SPICE simulations and deriving the real time mosfets.
1st step is SPICE deck, it is the connectivity information about the netlist. It has got the inputs provided for simulation, tap points at which we'll take the outputs and so on. So we will create the SPICE deck for complete netlist with pmos and nmos.In this case we looking at the 'static behaviour' of the cmos.Next, we will define the component value, where the pmos and nmos are given W/L values, and output capacitance load value.(Although we know pmos should be wider than nmos, but here we will take the same values for both).

![Image](https://github.com/user-attachments/assets/daa95df8-1dc6-4f31-8aa9-59bc6be6fd47)

Next step is to give the input values. Usually the voltage kept is in multiples of channel length. Also assume the drainvoltage.

![Image](https://github.com/user-attachments/assets/41fdae28-000e-47cd-bafe-067238685a27)

Next step is to Identify the Nodes, when between two points there is a component then that is specified as a node.

![Image](https://github.com/user-attachments/assets/069a24e2-b08a-4742-97c3-6bcba4814a0c)

![Image](https://github.com/user-attachments/assets/51aa2c6f-d855-4f53-be76-595695b99847)

We will start writing the SPICE deck code.
Stars define the commands, The syntax will be Drain-Gate-Source-Substrate

![Image](https://github.com/user-attachments/assets/9e945b21-8e56-41af-9b94-acaa869f6be6)

# **SPICE** simulation lab for CMOS inverter

Till now we have described the connectivity information about CMOS inverter now we will describe the other components connnectivity information like load capacitor, source. Let's seee the connectivity of output load capacitor.

It is connected between out and the node 0. And it's value is 10ff. Supply voltage(Vdd) which is connected between Vdd and node 0 and value of it is 2.5 , Similarly we have input voltage which is connected between Vin and node 0 and its value is 2.5.

![Image](https://github.com/user-attachments/assets/2bbdc7c7-5d5e-433c-b462-586808b3d95b)

Now we have to give the simulation commands in which we are swiping the Vin from 0 to 2.5 with the stepsize of 0.05. Because we want Vout while changing the Vin.

![Image](https://github.com/user-attachments/assets/eb502e4c-841f-426e-94d4-0d0a6b8a0b24)

SImilarly the Vin connected between in and 0.
Now we will write the simulation commands.
Given command means sweep the input voltage from 0 to 2.5 at steps of 0.05. reason of doing this , we need to calculate the voltage at the output while we sweep the input this will give the Transfer Characteristics.

![Image](https://github.com/user-attachments/assets/e8db802d-3603-4a25-82c6-e9a84b5650ad)

Final step is to 'Describe the Model file', this is very important step.This has got the complete description of pmos and nmos transistors including all the technological paramters. From this file only it will take all the description.

![Image](https://github.com/user-attachments/assets/6730a70f-41b7-440d-81ec-3cfc457f2625)

We will now try to plot the waveform in the ngspice using the model file we have.
Here Wn,p=0.375microns, Ln,p=0.25microns; Wn/Ln=Wp/Lp=1.5W
We get the required Transfer Characteristics.

![Image](https://github.com/user-attachments/assets/abc50e80-7b8f-4e1f-99da-45e92599355e)

Next keeping the Wn=0.375 microns, Wp=0.9375microns; Ln,p=0.25 microns; Wn/Ln=1.5, Wp/Lp=3.75.

![Image](https://github.com/user-attachments/assets/cb87674b-a8a2-49d0-b39f-45b33a4e6a30)

We can see in dc1 the waveform is a bit shifted left from it's center, whereas dc2 is accurate.

# **4.** Switching Threshold Vm

These both model of different width has their own application. By comparing this both waveform, we can see that the shape of the both waveform is same irrespective of the voltage level.It tells that CMOS is a very roboust device. when Vin is at low, output is at high and when Vin is at high, the output is at low. so the charactoristic is maintain at all kind of CMOS with different size of NMOS or PMOS. That is why CMOS logic is very widely used in the design of the gates.

Switching threshold, Vm (the point at which the device switches the level) is the one of the parameter that defined the robustness of the Inverter. Switching threshold is a point at which Vin=Vout.

![Image](https://github.com/user-attachments/assets/07a89a24-07a9-41da-8aaa-697ab38fbb63)

Even though we changed the width/length ratio we saw the graphs is same in both the cases, this shows that CMOS Inverter is a robust device.The behaviour of the inverter remains same despite of the changes.
We will do the Static behaviour Evaluation showing the robustness of the CMOS Inverter. The parameters which define the same are;

![Image](https://github.com/user-attachments/assets/c80f4e48-b668-48e1-a40b-e578c2182867)

Switching Threshold, Vm- It is a point where Vin=Vout, we will draw a tangent and see at what point Vin=Vout. This will give Vm. Also Vm is the point when both PMOS and NMOS are ON(Saturation Region). In this region there is a leakage current which flows from Vdd to ground.
Given below we got the Vm in both the cases.

![Image](https://github.com/user-attachments/assets/f4fc604c-45ca-4775-9498-ef4f4f2e17e4)

At this point Vgs=Vds, that means Vgs>>Vt, also the current which flows from PMOS and NMOS are same It's just that the direction of current are different.

![Image](https://github.com/user-attachments/assets/74a4a115-5d3a-41f1-b346-9c520b5045d3)

# **Static** and dynamic simulation of CMOS inverter

Now we will try to prove the robustness of CMOS Inverter with different W/L ratios in SPICE simulator and calculating the Vm.

![Image](https://github.com/user-attachments/assets/caa32091-d171-43a8-aef6-8adb4cd06758)

Earlier we did the static simulation by command .dc, now we will be doing the dynamic simulation by writing the command .tran and the input provided will be a pulse.

![Image](https://github.com/user-attachments/assets/3852bef8-13e6-4279-b059-8ec1154607a0)

![Image](https://github.com/user-attachments/assets/b59efb0d-1a59-4922-b47e-021ede4cdced)

will calculate the rise delay and fall delay from the graph we obtained.
So at Wp/Lp=Wn/Ln, we got the rise and fall delay as shown below, with switching threshold Vm=0.99V

![Image](https://github.com/user-attachments/assets/f56d9a39-db74-4630-97ab-21071bf9a796)

# **Lab** steps to git clone vsdstdcelldesign






## **Inception** of Layout – CMOS fabrication process

# **Create** Active regions

1) selecting a substrate:- we have a p-type silicon substrate having high resistivity(5-50ohm) well dopped, and orintation(100).

2) creating active region for transistor:- Region where you see PMOS and NMOS. On p-type substrate we are going to create some small pockets which will be called as active region and in these pockets we are going to create PMOS and NMOS transistor. Will cretae isolation between each and every pockets. Active regions are the pockets where we will dope with n type.
For this we need to create the isolation so that the pockets do not interact with each other, so we will grow a ~40nm SiO2 layer on the substrate.
Next we will deposite a ~80nm layer of Si3N4 on top of SiO2.
Now to make the active region pockets we will deposit the ~1micron layer of photoresist to create the masks.
Where we want to create the wells there will put masks.
And UV light drom the top.

![Image](https://github.com/user-attachments/assets/613ea361-9da7-4755-a6a2-a2b5a69d60eb)

![Image](https://github.com/user-attachments/assets/4d1bf7b2-573c-46ee-93d8-347ac86e4545)

![Image](https://github.com/user-attachments/assets/2b225c8d-2ec4-4fd3-af94-6db26a709c88)

![Image](https://github.com/user-attachments/assets/8f74597c-39fe-4fd8-a542-b892d87e94a4)

We create the isolation layer by depositing the Sio2 layer (~40nm) on the substrate. Now, we are depositing the Si3N4 layer (~80 nm) on the Sio2 layer.

![Image](https://github.com/user-attachments/assets/6c4736d3-d3d4-44f6-8162-fefeb9013feb)
Before creating the pocket identify the region where we need to crete the pocket. Now will deposite a layer of photoresist(~1um) on which we will create some mask1 using UV light.

![Image](https://github.com/user-attachments/assets/27ff5e64-e24a-4dfd-9a8c-1a3aafb16267)
Unwanted area has been exposed using UV light. And we get pattern the exposed area is getting washed away.

![Image](https://github.com/user-attachments/assets/50246380-e4b1-4126-a176-032c5ea55dee)
In the next step mask will be removed and doing etching of Si3N4 layer on the exposed area.

![Image](https://github.com/user-attachments/assets/74dca025-625d-4f06-b333-25b665b3ea6e)
Now, next step is to remove photoresist by chamical reaction, because now to Si3N4 layer itslef behaves like good protecting layer for Sio2 layer. now,We will place it in the oxidation furnace. if we do LOCOS (local oxidation of silicon) process, the exposed sio2 part will grow and bird break also form. This grown sio2 will provide the perfect isolation between two PMOS and NMOS. This is how we protect two transistor communicating with each other.

![Image](https://github.com/user-attachments/assets/290677b0-78eb-4d91-8162-c213bcb0d8d0)
Next step is to remove the Si3N4 using hot phospheric acid.

![Image](https://github.com/user-attachments/assets/b483abb8-9c02-4237-b20f-96b8ab9186da)

# **Formation** of N-well and P-well

N-well and P-well formation:- we can not form P-well and N-well at a same time. we have to protect a region while forming one of the region by photoresist. And then using mask 2 and UV light, we will do patterning of photoresist to form P-well.

![Image](https://github.com/user-attachments/assets/d716e707-4034-401e-a776-19bc663ed012)

Now, the area where we want to form the P-well is exposed. now we remove the mask and by applying the ion implantaton method (~200kev)to form P-well using Boron. But still it is P implant. After performing the high temparature anneling, it will become P-well.

![Image](https://github.com/user-attachments/assets/0ac0893c-ec5f-40cd-81a6-42b8ab28d306)
We will do a similar process to form N-well by using mask 3 and using Phosphorus ions.

![Image](https://github.com/user-attachments/assets/5c4f138e-2b98-498b-bc29-8ca30d30d770)
Till now depth of wells are not define. so, by putting into the high temparature furnace (drive-in diffusion), we will define the depth of wells.

![Image](https://github.com/user-attachments/assets/bd3f2415-0f7a-4484-8fb6-190293988715)

# **Formation** of gate terminal

Gate formation:- Gate terminal is the most important terminal of the PMOS and NMOS because from the gate terminal only we can control the threshold voltage. doping concentration and oxide capacitance will control the thresold voltage.so, first we are maintain the doping concentration here. for that we use mask 4 and again doing the ion implantation of boron ion at lower energy (~60kev).

![Image](https://github.com/user-attachments/assets/601372b3-335b-45c5-ae71-804e303621c4)

same process we will repeat for N-well also by using mask 5 and Arsenic ion.

![Image](https://github.com/user-attachments/assets/1f054c42-c2f3-439c-9e5c-2845d92bc204)

Next step is that we have to fix the oxide layer. but before that we have to remove the oxide layer because this layer is got dammeged because of the privious processes. so,first we remove the layer using HF solution and again re-grown the high quality oxide layer with same thickness.

The final step is the deposition of polysilicon layer over oxide layer with more impurities for low resistance gate terminal.Then etched out this polysilicon layer by using mask 6 and photoresist.

![Image](https://github.com/user-attachments/assets/5304e80c-6a73-44ee-b8c2-168765256f97)

After etching, remove the photoresist and gate terminal looks like

![Image](https://github.com/user-attachments/assets/49b3fdcc-d38a-4e67-86f8-66bd0663ef2c)

# **Lightly** doped drain (LDD) formation
5) LDD formation:- Here, we actully want P+,P-,N doping profile in the PMOS and N+,N-,P doping profile for NMOS. Reason for that is

Hot electron effect

short channel effect

For the formation of LDD, we again do ion implantation in P-well by using mask 7 and here we use phosphorus as a ion for light doping.

![Image](https://github.com/user-attachments/assets/802550d5-7ea4-42f9-b5e1-0842cc30d8f3)

Same process we will repeat for N-well. there we use mask 8 and Boron Ion.

![Image](https://github.com/user-attachments/assets/40dbacf5-afa3-45a5-aa6d-fb3aed9979e2)

Now, by creating the spacers, we can protect the actual structure remain constant of P-implant and N-implant. For that we deposit a thick Sio2 or Si3N4 layer over the gate terminal.

![Image](https://github.com/user-attachments/assets/0328cc03-c452-439f-b9a3-7195b8ea4b8f)

Now, we do Plasma anisotropic etching. By that side-wall spacers are formed.

![Image](https://github.com/user-attachments/assets/d462f2d9-d67b-41a8-9f83-800d769f6818)

# **Source** and drain formation

Before the formation of Source and Drain, a thin screen layer of oxide is deposited to avoid channeling effect.

![Image](https://github.com/user-attachments/assets/e0024546-5ff5-4b7f-859c-a9697e3f6a53)

![Image](https://github.com/user-attachments/assets/afff58ec-3abb-40b4-a70b-4ab27a7fd8ba)

For Source and Drain formation, we deposit make Mask9 on n substrate and exposing the p substrate to Arsenic with energy ~75eV. The side wall spacers will protect the LDD so that channeling does not happen. We will get the N+ structure required.

![Image](https://github.com/user-attachments/assets/c843a50a-aea6-4008-9e24-cdaf7d71f5ff)

Similarly we will mask this layer using Mask10 and expose the n substrate to Boron.

![Image](https://github.com/user-attachments/assets/b94297e2-600b-49cf-a359-4b1d590e03d0)

Now we will put the structure under high temperature for Annealing, it will push the dopants more inside the substrate and there will be uniform distribution.

![Image](https://github.com/user-attachments/assets/067c14f4-cf01-408b-bfd5-13bc83ace0a6)

 # **Local** interconnect formation

Steps to form contacts and interconnects(local)
Contacts are really important, as these are the only users a user can connect to the circuit.For thsi first we will etch out the thin oxide layer for avoiding channeling effect using HF.

![Image](https://github.com/user-attachments/assets/c5cc2c68-c73d-4344-8942-47f1a077b6fd)

For creating local interconnects, first we will deposit Titanium suing sputtering process all over the substrate.

![Image](https://github.com/user-attachments/assets/a9a7013b-a21c-4791-aba5-e083570d1ab6)

Next step is to create the connects between titanium and source drain. This is done by heating the wafer at an ambient temperature of 600-700 degreese celcius under N2 gas for 60sec. This will result in TiSi2(a low resistive metal contact on gate) contacts created on source and drain. Also TiN layer will be formed, it is used only for local communication.

![Image](https://github.com/user-attachments/assets/adfaba9d-1f8e-46a6-8634-739fddf27b8a)
To bring up the required contacts on top, we will put Mask11 and etch out the area we want to be coming out. We want Source, Drain and Gate area to be coming out.

![Image](https://github.com/user-attachments/assets/99982b49-a524-4b5b-a314-b24b78a2c7ba)

We will etch out the extra TiN layer using RCA cleaning.

![Image](https://github.com/user-attachments/assets/b13ef614-c9b6-4d11-b07c-57167df535cb)

![Image](https://github.com/user-attachments/assets/47d44dc6-cca2-4483-80f7-fe492da84735)

# **Higher** level metal formation

Higher level metal formation
Here we observe there is non planarity which is not good for depositing higher metal interconnects. So we will planarise this surface by using thick layer of SiO2 which is doped with phosphorus and boron. The reason of doping is that phosphorus protects the layer from reactive sodium ions and boron is used to reduce the temperature as this wafer will be exposed to certain high temperature so boron will help in reducing the temperature.

![Image](https://github.com/user-attachments/assets/2d3ac00f-43d9-4877-bbbb-05753e9979c1)

To remove the hills and bumps we do polishing, CMP.

![Image](https://github.com/user-attachments/assets/14d8a5e4-a237-449a-897e-5308002754b6)

Next is creating the metal contacts by drilling, so this also done by photlithography technique. By using Mask12.

![Image](https://github.com/user-attachments/assets/356a2cbb-0916-40f0-baed-046b9045dc65)

Now we will remove the mask by washing away the photoresist. We will create thin layer ~10nm of TiN, it acts as Adhesion layer between SiO2 and acts a barrier layer for bottom and top interconnects.

![Image](https://github.com/user-attachments/assets/00dcf564-4fb7-4e59-b6e0-a69de966b23b)

Then we will deposit a blanket of Tungsten(W) layer, this will help to create a very good contact from bottom to top.

![Image](https://github.com/user-attachments/assets/3b56f430-42bd-4244-b50c-87af2992a161)

Next, is CMP, removing the extra tungsten from top.

![Image](https://github.com/user-attachments/assets/b7a0ed65-7d8a-4069-a8f0-0c7cb79956a4)

Now we will deposit metal Aluminium layer on top to take the metal contacts above. Further we will mask to expose the specific areas.

![Image](https://github.com/user-attachments/assets/869dc850-2c5b-492e-a758-822db88c394d)

![Image](https://github.com/user-attachments/assets/5e76172c-668b-4d12-bd98-c98096f82070)

We got the first layer of metal interconnect below.

![Image](https://github.com/user-attachments/assets/6c817176-8c65-4857-be99-6e20a42a1c37)

We will repeat the above processes to get further layer of metal interconnects.

![Image](https://github.com/user-attachments/assets/2db274f2-b2c5-473a-9f6f-2922fda590b1)

![Image](https://github.com/user-attachments/assets/0858a89a-154e-4274-bbff-21acefb9b366)

After Mask14 again a thin layer or TiN is deposited.

![Image](https://github.com/user-attachments/assets/4e8023cc-a093-43aa-be89-35819b73c7bf)
Now again depositing Tungsten(W) on top.

![Image](https://github.com/user-attachments/assets/adb88b25-d3e2-46db-8720-1018641c8068)

Now we will deposit the third layer of interconnect using Mask15. Also the thickness is more than the bottom layer. When we go from bottom to top the thickness of metal layers increase.

![Image](https://github.com/user-attachments/assets/881696f5-91e7-42a7-aefd-11631f561062)

After this we will deposit the Si3N4 layer, we use Si3N4 to protect the chip as this is a good protectant layer from the outside world.

![Image](https://github.com/user-attachments/assets/9e9a56fa-480c-4e63-a84a-99911bdbde8d)
Finally, we will use Mask16 to drill out the final contacts outside.

![Image](https://github.com/user-attachments/assets/72f4cead-d618-4f4c-8e8d-10cf313eff66)
# **Lab** steps to git clone vsdstdcelldesign
![Image](https://github.com/user-attachments/assets/c37db63a-052b-4169-b7d8-35f7c78fe026)

![Image](https://github.com/user-attachments/assets/9d0d0a93-a724-4691-9d16-62672122613a)

![Image](https://github.com/user-attachments/assets/4e26d0d7-3faa-4549-b578-a40c3446c318)

![Image](https://github.com/user-attachments/assets/16bb5fb8-cb4a-49f7-8e07-d4ccead92a0a)

![Image](https://github.com/user-attachments/assets/fddc8181-d751-4614-8cdd-ba5beead53a9)

# **Lab** introduction to Sky130 basic layers layout and LEF using inverter

# **Lab** steps to create std cell layout and extract spice netlist

![Image](https://github.com/user-attachments/assets/e0662d72-157a-427c-9eac-0c9edd6e3866)

![Image](https://github.com/user-attachments/assets/7534dae0-d528-4bd2-bf8e-907b522dc1d9)

![Image](https://github.com/user-attachments/assets/5b6c2b0a-ebfa-46ae-889e-3eed95a20196)

![Image](https://github.com/user-attachments/assets/cbb400c0-8171-4552-89ee-09ff51ad6e19)

## Sky130 Tech File Labs

# **Lab** steps to create final SPICE deck using Sky130 tech

![Image](https://github.com/user-attachments/assets/06778828-cae1-4ab8-8902-8fcae94a1d48)

![Image](https://github.com/user-attachments/assets/04d21504-8ff2-4c48-9c79-74437e80ea12)

![Image](https://github.com/user-attachments/assets/af639778-04e5-4849-8361-b2638864192f)

![Image](https://github.com/user-attachments/assets/474fe13a-06ee-4c19-84e9-97f5fdae0f51)

![Image](https://github.com/user-attachments/assets/708f5821-dcf3-461c-a387-515b4bf9c93d)

![Image](https://github.com/user-attachments/assets/72c765d1-00e1-48f9-b1aa-19a65841dd23)

![Image](https://github.com/user-attachments/assets/4df370eb-80ed-4ee4-a53e-bd085f35fca9)

![Image](https://github.com/user-attachments/assets/68f7aab6-6906-496b-b1a5-d3909d8a210c)

![Image](https://github.com/user-attachments/assets/5fc734d9-1b4e-45a8-bc80-dc3372c747c1)

## **Lab** introduction to Sky130 pdk's and steps to download labs

![Image](https://github.com/user-attachments/assets/57dff082-f4f7-455a-ba71-142ca5444b71)

![Image](https://github.com/user-attachments/assets/6d5019d9-524c-495a-ac25-3e54b802de84)


![Image](https://github.com/user-attachments/assets/96d223e8-0259-43a9-ba92-238fda468d7c)

![Image](https://github.com/user-attachments/assets/6c5de5e4-87ab-40b1-8d2f-9af1152685c8)

![Image](https://github.com/user-attachments/assets/c03a9d3f-eeff-499c-9ae4-26227d342ef0)

![Image](https://github.com/user-attachments/assets/80fb952b-9d22-4130-84ba-41a55ec6216d)

DAY4

Timing modelling using delay tables
Lab steps to convert grid info to track info

![Image](https://github.com/user-attachments/assets/e7b4b0d5-5347-475e-8904-c5026f7027fe)

![Image](https://github.com/user-attachments/assets/7bb902f2-5969-41af-abad-b8e03db34237)

![Image](https://github.com/user-attachments/assets/ea2133a8-0877-463f-87f9-008d26ebbd30)

![Image](https://github.com/user-attachments/assets/0ae630a5-b5c6-45d8-b711-723f24eed320)

![Image](https://github.com/user-attachments/assets/64b1fea0-242f-4050-8da7-f1d1295af622)

![Image](https://github.com/user-attachments/assets/cc86847a-8973-4b8a-a632-733cf06527fb)

![Image](https://github.com/user-attachments/assets/089f140e-7282-464f-a8b1-a55f15fa1c56)
# **Lab** steps to convert magic layout to std cell LEF

![Image](https://github.com/user-attachments/assets/15de5e7b-3189-40aa-ad6c-a0efb2ac220c)

![Image](https://github.com/user-attachments/assets/68faf2df-987d-4994-9e27-840b8aa6ede7)

![Image](https://github.com/user-attachments/assets/474aa378-6778-4019-81a9-3ab888dd3f60)

![Image](https://github.com/user-attachments/assets/8f417f85-f60e-4d2c-a707-d7c8217230da)

![Image](https://github.com/user-attachments/assets/e817d2db-6f37-487a-b0a4-a6ce3f4cd079)

![Image](https://github.com/user-attachments/assets/f70f1d1a-38ad-4972-b05f-870600ca8ea5)

# **Introduction** to timing libs and steps to include new cell in synthesis

![Image](https://github.com/user-attachments/assets/ea4cf6c9-9ccd-4c53-b905-346b17253bc2)

![Image](https://github.com/user-attachments/assets/278f5bea-7fba-4646-8194-72bedc7da549)

![Image](https://github.com/user-attachments/assets/600b4c28-b910-402b-94d6-9382a8ae399b)

![Image](https://github.com/user-attachments/assets/26fbb5ba-204e-4e61-92b5-8e0c2d71b5cf)

![Image](https://github.com/user-attachments/assets/33cc1a87-0489-40e5-8f28-bac0d70734ea)

# **Lab** steps to configure synthesis settings to fix slack and include vsdinv

![Image](https://github.com/user-attachments/assets/5524a327-2511-4c41-b093-c37710ca7094)

![Image](https://github.com/user-attachments/assets/07d65261-15f9-4107-9a0c-506d03f5115f)

![Image](https://github.com/user-attachments/assets/354c58fe-aef8-42f2-a04b-7f52e8d8306a)

![Image](https://github.com/user-attachments/assets/c7d62ddc-b52e-4b3a-8273-c61f014fe761)

![Image](https://github.com/user-attachments/assets/54ed64c4-9061-457f-9781-ffab2976ac6b)

# **Lab** steps to configure OpenSTA for post-synth timing analysis

![Image](https://github.com/user-attachments/assets/628b4f02-c4e8-4edc-b9fe-4c07069e6891)

![Image](https://github.com/user-attachments/assets/85cfbad6-c854-4e1e-9413-736020423c0c)

![Image](https://github.com/user-attachments/assets/8df27343-a0cf-4d0d-ae14-daa78730c08b)

![Image](https://github.com/user-attachments/assets/1c1b943f-b957-44f3-8fa5-904d376c441c)

![Image](https://github.com/user-attachments/assets/8b51b04e-fd0f-4de4-ba5e-95d2afb5c068)

# **Lab** steps to optimize synthesis to reduce setup violations

![Image](https://github.com/user-attachments/assets/81566801-e9b7-4d04-8f7a-f7394f560fc8)

![Image](https://github.com/user-attachments/assets/b60516ec-a5b6-465d-a23b-24b343c0ed74)

![Image](https://github.com/user-attachments/assets/ad832574-a78d-4a2d-9468-30c216936feb)

# **Lab** steps to do basic timing ECO

![Image](https://github.com/user-attachments/assets/eeaff805-1a81-46b8-bd17-22f2bb6629fd)

![Image](https://github.com/user-attachments/assets/04de2cdb-1350-4319-ae47-4015ed865abc)

## **Clock** tree synthesis TritonCTS and signal integrity

# **Lab** steps to run CTS using TritonCTS

![Image](https://github.com/user-attachments/assets/b96da2ff-3262-4536-a7fc-ee9a7b5ddf24)

![Image](https://github.com/user-attachments/assets/28c23cc6-6195-4285-a677-c4604b624573)

![Image](https://github.com/user-attachments/assets/2b243ff0-ef54-4699-9155-d6af6ce84919)

![Image](https://github.com/user-attachments/assets/e57127ff-c5c4-4e67-be74-98507799ca27)

![Image](https://github.com/user-attachments/assets/d6488673-bb76-4769-8871-ff418dbfacf9)

![Image](https://github.com/user-attachments/assets/0b368f8a-9e97-43f9-aa37-eac9039fa406)

# **Lab** steps to verify CTS runs

![Image](https://github.com/user-attachments/assets/0619ad7e-36f0-4225-b5be-63c707cb2abd)

![Image](https://github.com/user-attachments/assets/86719ac5-7752-4514-ba75-886daeb512d6)

# **Lab** steps to execute OpenSTA with right timing libraries and CTS assignment

![Image](https://github.com/user-attachments/assets/1ccd656d-e57b-43ba-a39b-287b1c5ce75c)

![Image](https://github.com/user-attachments/assets/1fc74a7c-4b64-4580-872d-5e1eab2aca7c)

![Image](https://github.com/user-attachments/assets/ef8d6f31-df75-4fd2-aa3d-7f15446f5428)

![Image](https://github.com/user-attachments/assets/d8a07aef-fffd-4e68-9305-426e3f2bb6f1)

![Image](https://github.com/user-attachments/assets/accc41d6-176a-4ed9-9a4e-0823e0589cf8)


### **Sky130** Day 5 - Final steps for RTL2GDS using tritonRoute and openSTA

## **Routing** and design rule check (DRC)

# **Introduction** to Maze Routing – Lee’s algorithm

So the next and final stage in the physical design is Routing and DRC.

Routing:- It is finding the best shortest possible connection between two end points with one point being the source and other point being the target and with less number of twist and turns.

Maze-Routing(Lee's Algorithm):- These should not be zig-zag lines of connections most of the connections should be in L shape or in Z shape. So according to algorithm first it create some grids and grids are routing at the backend. It's called as routing grid. There are some numbers of grids on this routig having some dimensions. SO here we are having two points one is 'Source' and the other is 'Target'. With the help of this routing grid algorithm has to find out the best possible way between them.

First step is algorithm tries to label all of the grids surrounded. Only the adjacent horizontal and vertical grids are labeled not the digonal one as shown in the image below.
All the adjacent grids are labelled as 1.

![Image](https://github.com/user-attachments/assets/dd51a975-a1b7-4436-8b66-7f32db486602)

# **Lee’s** Algorithm conclusion

Now we will lable the grids to the next integer untill we reach to the target. In the example we reached the target after integer 9.
Because of blockage the top 5 is not visible which shows it can't be used.

![Image](https://github.com/user-attachments/assets/203e3960-f5fc-4e6e-a4d6-48795dc56738)

SO now there are so many ways to reach to target from source but we have to choose the best shortest possible way to reach the target.And we need to avoid the zig-zag way better to choose 'L' shape routing'. Any route with single bend is the most preferrable one.

![Image](https://github.com/user-attachments/assets/0de1bb64-28b2-446e-9049-b7d670296999)

Now take one more example for routing, and will follow the exact same step as follows in the above example.

![Image](https://github.com/user-attachments/assets/88b0e20f-f4b8-423f-b17b-adc82cc10a13)
We have already discussed the routing problem, now we'll try to figure out the issues while routing.

# **Design** Rule Check

Let's consider two parallel wires so the rule says that whenever we choose two wires there should be minimum distance between these two wires.

![Image](https://github.com/user-attachments/assets/d11cbb66-6fe8-4458-acf2-c7944e8b3870)

Rule 1) Wire width:- Width of the wire should be minimum that derived from the optical wavelength of lithography technique applied.
   Optical photolithography uses light to build these wires. Light has got some minimum wavelength with that it can draw certain width of the line. 
Rule 2) Wire Pitch:- The minimum pitch between two wire should be this much as shown in the figure below.

![Image](https://github.com/user-attachments/assets/b0ed5e5a-3adc-4c12-9f96-9d949758f9a7)

![Image](https://github.com/user-attachments/assets/a103e4df-e537-442b-a0f5-11f20378a4d3)

Rule 3) Wire Spacing:- The wire spacing between two wires should be as shown in the image below. It can be more than this but not less.

![Image](https://github.com/user-attachments/assets/e31baf3c-6bae-4086-a0df-dd186d92f428)

Solution of this signal short problem is take one of the wire and put it on the other metal layer. usually upper metal is wider than the lower metal.
![Image](https://github.com/user-attachments/assets/f57748c9-6ba9-4f73-a36a-be340bd2b065)

![Image](https://github.com/user-attachments/assets/de6ff5cd-d79e-4297-958a-bb1f864b9c42)
Rule 1) Via Width:- via width should be some minimum value.

![Image](https://github.com/user-attachments/assets/0cdc5ef0-aaa3-4546-a3c4-8b030ad08b87)
Rule 2) Via Spacing:- Via spacing should be minimum value.

After routing and DRC the next step is Parasitic extraction. Resistance and capacitance present on every wire should be extracted and use for further process.

![Image](https://github.com/user-attachments/assets/5225758c-8d11-49bc-9bef-1315a9b8a060)

## **Power** Distribution Network and routing

# **Lab** steps to build power distribution network

![Image](https://github.com/user-attachments/assets/318005dc-f8ed-4995-aade-150712e73fdf)

![Image](https://github.com/user-attachments/assets/8176f4a8-8cca-4b4d-ae9a-06ad61aedab0)

![Image](https://github.com/user-attachments/assets/93217d79-f5b6-4bdd-b72c-374ce71afc0e)
It seems like the net VGND displays the total number of nodes on the grid matrix, indicating that it has been successfully created.

The chip receives power from the VDD and GND pads, which then travels through the tracks and ultimately reaches the cells to power them.

## **Lab** steps from power straps to std cell power

![Image](https://github.com/user-attachments/assets/039a4f8a-13fb-4633-a5c9-20f85a29ba3b)

Green is picorv.
Yellow is IO pads and ground pads. 
Square one's at the corner are corner pads. 
Red pad is power pad. 
Blue pad is ground pad.
Power is transfered to the rings from the pads through the black dots shown in the image on the cross section points of the ring and pads.

We have vertical and horizontal tracks which ensures that the power is being transferred from the ring to chip this is shown by the red and blue color. This is how power planning works in physical design of any device.

![Image](https://github.com/user-attachments/assets/670f3889-4bda-4c79-acff-b534b6b21b9f)

The usage of the def command in the image above is to indicate that the latest completed step was the generation of PDN.

The resulting file 17-pdn.def contains the information from cts.def as well as the power distribution network.

# **Basics** of global and detail routing and configure TritonRoute

![Image](https://github.com/user-attachments/assets/3c419013-96d2-4625-b209-8aaf85459104)

![Image](https://github.com/user-attachments/assets/2bece39e-05a6-4e6b-85ab-77d080bb14b0)

The total process of routing is devided into two part.

![Image](https://github.com/user-attachments/assets/14bd030b-6141-4ce1-ba03-39e719f3470d)

Fast route (Global route)- engine which is used for global route
Detailed Route- done by triroute
In the Global route, the routing region is devided into the rectangular grids cells as shown in the figure above. And it is represented as cores 3D routing graph. Global route is done by FAST route engine.The detailed route is done by TritonRoute engine. A,B,C,D are four pins which we want to connect through routing. and this whole image of A,B,C,D shows the net.

## **TritonRoute** Features

![Image](https://github.com/user-attachments/assets/ad92fea6-6c3f-42cf-ab32-c86e11d579f7)

![Image](https://github.com/user-attachments/assets/26e06b87-00de-4e2b-9e93-ec4c80fe4f16)

Requirements of preprocessed guides

-Should have unit width

-Should be in preferred direction

-Assumes route guides for each net satisfy inter-guide connectivity
Two guides are connected if

-They are on the same metal layer with touching edges.

-They are on neighbouring metal layers with a non-zero vertically overlapped area.

-Works on proposed MILP-based panel-routing scheme with intra-layer parallel and inter-layer sequential routing framework

# **TritonRoute** Feature2 & 3 - Inter-guide connectivity and intra- & inter-layer routing

Each unconnected terminal i.e, pin of a standerd-cell instance should have its pin shape overlapped by a route guide.

![Image](https://github.com/user-attachments/assets/4da38982-48dc-407e-8f67-97d9de4f84c3)
Here we can see that black dots are pins of the cells and it is overlapped by route guide. if you have pins on the intersection of the vertical and horizontal tracks that will ensure that it will be overlapped by route guides.

![Image](https://github.com/user-attachments/assets/f68d65fc-7b71-4b29-bb0b-cf15600b710f)

# **TritonRoute** method to handle connectivity

INPUTS:-LEF
OUTPUTS:-detailed routing solution with optimized wore-length and via count
CONSTRAINTS:-Route guide honouring, connectivity constraonts and design rukes
Now we have to defined the space where detailed routing take spaced.

Handling connectivity:-

![Image](https://github.com/user-attachments/assets/aa305d2e-53a6-42f3-98a3-f00fb11c0df0)

Access point(Ap):- An on-gride point on the metal layer of the route guide, and is used to connect to lower-layer segments, upper-layer segments, pins or IO ports.

Access point cluster (APC):- A union of all access points derived from same lower-layer segment,upper-layer guide, a pin or an IO port.

Here in the figure shown above, the illustration of access points:

(a)To a lower-layer segment

(b)To a pin shape

(c)To upper layer

Routing topology algorithm and final files list post-route

![Image](https://github.com/user-attachments/assets/0592221c-0992-4344-89cc-df2f328e4380)

The algorithm requires the determination of the cost associated with each APC and the calculation of the minimum spanning tree between the APCs to find the optimal points between two APCs.

The next step involves post-routing STA analysis, which requires the extraction of parasitic effects (SPEF).

Since OpenLANE does not have a SPEF extraction tool, this process needs to be done outside of OpenLANE.

The resulting .spef file can be located in the routing folder under the results folder.

### **Routing** topology algorithm and final files list post-route

![Image](https://github.com/user-attachments/assets/90b0ddd9-b94b-4f80-8214-81782ee3a3c1)

![Image](https://github.com/user-attachments/assets/d1584b15-f33b-4695-93a9-3c3cb4b09a15)

Now remains is the post routing test.

![Image](https://github.com/user-attachments/assets/1e549bea-a9b8-4b4c-b98d-17e90b0ed1fe)

![Image](https://github.com/user-attachments/assets/8b54d3f5-7606-467d-8ff8-fb8b9681693f)

![Image](https://github.com/user-attachments/assets/f7af1e88-73b4-46fc-ac4c-67e00790cc20)

References
Workshop Github material
https://github.com/google/skywater-pdk
https://github.com/nickson-jose/vsdstdcelldesign
ttps://sourceforge.net/projects/ngspice/
https://github.com/
https://www.vlsisystemdesign.com/wp-content/uploads/2017/07/Introduction-to-Industrial-Physical-Design-Flow.pdf
