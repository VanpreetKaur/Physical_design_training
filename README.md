Day 1:

**ASIC OPENLANCE FLOW overview**: 
Below is ASIC OpenLane Flow: 
![image](https://github.com/user-attachments/assets/ff0ad762-ca0d-4a73-bb43-07167fcd353e)

Below is the Run area(highlighted in red)for OpenLane flow: 
![image](https://github.com/user-attachments/assets/033655e9-be54-49c2-9616-b14995106182)
In this area, we will be excecuting OpenLane ASIC flow (RTL to GDS)



**PROCESS DESIGN KITS**:
Below is the directory structure for PDK(process design kits): 
![image](https://github.com/user-attachments/assets/0bfa1a24-4db9-40b4-b8f7-13b1f2079e24)
PDK used for this workshop is SKYWATER 130nm PDK. 
1. skywater-pdk: It includes all the information such as timing libraries, lef files, tech lef, tech files etc.
2. open_pdks: This is needed as all the silicon pdk files are compatible with commercial tools and not with opensource tools(such as magic, netgen etc). These pdk includes scripts which mitigate the compatibility issues of foundry pdks with open source tools.
3. Sky130A: this is a pdk which is made compatible for use with opensource tools. It includes files such as tool specific tech files, and technology process specific pdks.Below is the snap showing details for both the files:
![image](https://github.com/user-attachments/assets/6978d7c9-0cd3-4df3-b48d-c942afa7802d)
PDK used for this workshop is SKYWATER 130nm PDK. 
1. skywater-pdk: It includes all the information such as timing libraries, lef files, tech lef, tech files etc.
2. open_pdks: This is needed as all the silicon pdk files are compatible with commercial tools and not with opensource tools(such as magic, netgen etc). These pdk includes scripts which mitigate the compatibility issues of foundry pdks with open source tools.
m
sky130: process node i.e. sky130nm
fd: foundry abbreviation of skywate foundry
sc: represents standard cell
hd: high density



Getting Started with OPENLANE FLOW:
Below is the work area snapshot showing the main file which will be used for running openLance ASIC flow i.e. rtl to gds: 
![image](https://github.com/user-attachments/assets/02ee84fb-2066-4fe3-a6cb-b06ce72986e5)

1. for this, first we will need to type "docker" and it changes the terminal to bash mode. 
2. Secondly, we need to source ./flow.tcl in interactive mode for step by step understanding of each openlane flow steps.
3. After this, we need to import package using command: package require openlane 0.9
4. Above steps are shown in below snap:
![image](https://github.com/user-attachments/assets/302f323a-1960-41ec-b9cd-4c9cf5c9c5de)



**DATA PREPARATION STEP**:
In this step, we need to first prepare our data which includes merging lefs such as tlef(technology lef) and cell lefs. This can be done using below command as highlighted in the below snap i.e., 
prep -design picorv32a

![image](https://github.com/user-attachments/assets/cbfa96b2-46ad-4f48-8830-7047cefff5ce)
here, we have used design name as "picorv32a" and the data for this design is being fetched from "designs" folder present in our openLance work area

below is the output of above step
![image](https://github.com/user-attachments/assets/fc398f49-a05b-4073-aef2-1f7595fb893d)
here, under designs/picorv32a folder, a directory names "runs" is created. This includes reports, logs, config.tcl(used to check all the variables used in entire Openlane flow and it reflects on-the-fly variable as well), tmp directory etc. here, tmp dir includes merged lef and other files related to all the steps. 



**SYNTHESIS**:
For synthesis, we need to use command: run_synthesis
here, synthesis along with sta and abc run is performed. 
Below snap shows synthesis step completion:

![image](https://github.com/user-attachments/assets/11aedbe7-2042-4de2-ac32-f4da8d7fc97d)

below snap shows the design statistics :
![image](https://github.com/user-attachments/assets/130a18f3-b6f9-446a-84eb-e934f2daddba)

Flop Ratio = No. of D flip flops/Total number of cells 
hence, Flop Ratio = 1613/14876 = 0.108429 = 10.84%
Flop Ratio is 10.84%

Below is the output of synthesis: under results/synthesis dir: 
![image](https://github.com/user-attachments/assets/d5bf3a78-e501-4335-bb27-5d13a29d2290)

below is the snap showing reports for synthesis step:
![image](https://github.com/user-attachments/assets/cc9b813f-74ad-49c8-aec2-1ea710c6bf58)


**Chip Floorplanning**:	
![image](https://github.com/user-attachments/assets/2f19d734-6921-49c9-9e7b-905ba8e3f1c2)

Floorplan results are shown below:
![image](https://github.com/user-attachments/assets/df822ffe-1ba1-4d1e-b922-dc12c2b8cc8c)

below is the snap of design def: 
![image](https://github.com/user-attachments/assets/0ca214bf-0bfa-483a-bf61-c0184d909456)

Die area = (660685/1000) * (671405/1000) 
          = 660.685um *671.405um
          
**Die area = 4,43,587.212425um2**


Below is the command snap to load def in Magic tool:
![image](https://github.com/user-attachments/assets/29b9c9e4-7b6e-430e-99b9-60f867f522fd)



**Library Building and Placement**:
**Netlist binding and initial place design**:

Bind netlist with physical cells
![image](https://github.com/user-attachments/assets/194fab18-265b-4f84-aa45-5a503a040361)

Now, remove the wires,all the gates, flipflops and blocks are present in the shelf which is called as Library.
![image](https://github.com/user-attachments/assets/3c614dc6-36d5-4a12-b734-f6433d002e67)
 Library also has the timing information of the perticular book like delay of the gates. Library can be devides into two sublibraries, One library consist of shape and size and other library might consist only of the delay information. Library has the various flavours of each and every cell. Like same cell can have bigger in size in different self, bigger the size of cell lesser the resestnce path so it will work faster and will have lesser delay. We can pick up from these what we want based on the timing condition and available space on the floorplan.


**PLACEMENT:**
Once we have given proper shape and size to each and every gates the next step is to take those particular shapes and sizes and place it on the floorplan. We have the floorplan with inout and output ports, we have particular netlist, and we have particular size given to each component of this netlist. So, finally we have the physical view of the logic gates.
Next step is to place the netlist onto the floorplan. We have to take the connectivity information from the netlist and design the physical view gates on the floorplan.
![image](https://github.com/user-attachments/assets/df72b006-de73-4bb4-9bc2-de8d8060a727)

place the physical view of the netlist onto the floorplan in such a fashion that logical connectivity should be maintained and that particular circuit should interact with their input and output ports to maintain the timing and the delay will be minimal.
![image](https://github.com/user-attachments/assets/059e2262-ae73-45b4-99b2-bb17e7ff8183)

![image](https://github.com/user-attachments/assets/75172814-2bb9-4296-95cc-a9d2a3982fe0)

Optimize placement using estimated wire-length and capacitance
![image](https://github.com/user-attachments/assets/3c9f41ee-fd8f-4e3f-b764-cd56cc75c040)

![image](https://github.com/user-attachments/assets/0a2c874a-9d72-4709-bcba-0ea18baf9d4b)

![image](https://github.com/user-attachments/assets/3e1e45c6-c8ae-4e82-9216-c4d75ae774c2)



**Cell design and characterization flows**
In Cell Design Flow, Gates, flipflops, buffers are named as 'Standard Cells'. These standard cells are being placed in the section called as 'Library'.And in the library many other cells are available which have same functionality but the size is different.
![image](https://github.com/user-attachments/assets/bd61af1b-8fa0-4a78-a706-7b627e3b9bb1)

If we look into one of the inverter from the library the cell design flow is as follows
The inverter has to represented in form of the shape, drive strength, power charracteristic and so on.
Here cell design flow is devided into three parts.

1. Inputs
2. Design Steps
3. Outputs

![image](https://github.com/user-attachments/assets/ce4678db-5ddd-43cb-a549-3426730e30ef)

1. **Inputs**: Inputs required for cell design is PDKs, DRC and LVS rules SPICE models, library and user defined specs. In DRC & LVS rules tech file is provided which contains design rules and actual values. Rules can be converted in to code. SPICE MODEL tells about threshold voltage equation.
2. **design steps**: Design involves three steps which are circuit design, layout design, characterization.

In circuit Design there are two steps.
First step is to implement the function itself and second step is to model the PMOS nad NMOS transistor in such a fashion in order to meet the libraray.

3. **outputs**:The typical output what we get from the circuit design is CDL(circuit description language) file,GDSII,LEF,extracted spice netlist(.cir).
![image](https://github.com/user-attachments/assets/d8437f6c-c7c3-4d0a-8f4b-e7e950c05de3)

Layout design step: In Layout Design First step is to get the function implemented through the MOS transistor through a set of PMOS and NMOS transistor and the second step is to get the PMOS network graph and the nNMOS network graph out of the design that has been implemented.
![image](https://github.com/user-attachments/assets/de193e45-6b79-44fc-97e9-2bfadc1cf02d)

After getting the network graphs next step is to obtain the Euler's path. Eule's path is basically the path which is traced only once.
![image](https://github.com/user-attachments/assets/00209948-7998-4be4-bd9c-397dccf724e7)


Next step is to draw stick diagram based on the Euler's path. This stick diagram is derived out of the circuit diagram.
![image](https://github.com/user-attachments/assets/8909b098-282f-42e6-b742-18e18662fe85)


Next step is to convert this stick diagram into a typical Layout, into a proper layout and then get the proper rule
![image](https://github.com/user-attachments/assets/a17f9e6b-c702-4408-859a-1bc11422166a)


Next and Final step is to extract the parasatics of above shown layout and charaterize it in terms of timing. Output of the layout design will be GDSll format. Once we get the extracted spice netlist then we characterize the same which includes getting timing, noise and power information.


**Library Characterization Flow**:	
1. Read the spice model.
2. Read the extracted spice netlist
3. Define or recognize the behaviour of the buffer
4. Read the subcircuits of the inverter
5. Attach the necessary power supplies
6. Apply the stimulus
7. need to provide the necessary output capacitance
8. provide necessary simulation command for example if we are doing transent simulation so we need to give .tran command , if we are doing DC simulation then we give .dc command.

   ![image](https://github.com/user-attachments/assets/6f441ddc-4d13-4c05-b339-813dd01d9fa2)

Next step is to feed in all this inputs from 1 to 8 in a form of a configuration file to the characterization software "GUNA" .This software will generate power, noise and timing model.

![image](https://github.com/user-attachments/assets/a37f9111-9eba-4850-bb03-219c9aea54da)


**General timing characterization parameters**
![image](https://github.com/user-attachments/assets/86bb3bac-38e9-44a4-9193-9b40d23f07c6)

![image](https://github.com/user-attachments/assets/77fa454a-d60d-47a7-a1c0-5914ee7eeec8)

![image](https://github.com/user-attachments/assets/04284124-1e30-4cd7-a241-68eed7b3de9f)

![image](https://github.com/user-attachments/assets/f4697c27-0202-46cd-81ee-2b8664d03c51)

![image](https://github.com/user-attachments/assets/888f4630-29ef-4aad-b9cf-617a451c18ba)

![image](https://github.com/user-attachments/assets/98e5748d-9ab6-419e-9650-4ed172e634e9)

![image](https://github.com/user-attachments/assets/d1aea487-ed1f-4e6b-ba26-fae3f8c981ab)

![image](https://github.com/user-attachments/assets/b93e18d9-ad31-4bbd-904d-1385360795dd)



**Propagation delay and transition time**
![image](https://github.com/user-attachments/assets/55685a4e-669a-4d82-bcf1-c31679babb14)


Transition time= time(slew_high_rise_thr)- time(slew_low_rise_thr)

or

transition time = time(slew_high_fall_thr)- time(slew_low_fall_thr)

![image](https://github.com/user-attachments/assets/d4a49291-852f-43b9-b94b-7d538b7c9072)



**Labs for CMOS inverter ngspice simulations
IO placer revision**
In floor planning, pins are at equal distance and if we want to change it then we can also make it by Set command. For this, we have to check the swithes in the configuration and from that we have to take the syntax "env(FP_IO_MODE) 1". and make it to the "env(FP_IO_MODE) 2". then again run the floorplanning.

![image](https://github.com/user-attachments/assets/630e7543-ec50-41e6-9c28-d7dc073d4977)


**SPICE deck creation for CMOS inverter**

VTC- SPICE simulations:
1. create SPICE deck, it includes connectivity information about the netlist.It has input that are provided to the simulation and the deck points which will take the output.
2. Component connectivity:- In this we need to define the connectivity of the substrate pin. Substrate pin tunes the threshold voltage of the PMOS and NMOS.
3. Component values:- Values for the PMOS nad NMOS. We have taken the same size of both PMOS and NMOS.

![image](https://github.com/user-attachments/assets/a3b4d446-a4c8-4518-90fe-c4bc3e68529f)

![image](https://github.com/user-attachments/assets/c214ecc0-89f7-486c-96e4-3ce7f2af384b)

4. Identify the nodes:- Node mean the points between which there is a component.These nodes are required to define the netlist.

   ![image](https://github.com/user-attachments/assets/64d387af-c94a-449e-ba4b-871421e1ef99)

5. Name the nodes:- Now we will name these nodes as Vin, Vss, Vdd, out.

   ![image](https://github.com/user-attachments/assets/1f0da643-80fc-43b9-b6dc-41baab722843)

   Below is the SPICE deck:

    Drain- Gate- Source- Substrate

For M1 MOSFET drain is connected to output node, gate is connected to input node, PMOS transistor substrate and Source is connected to Vdd node.
For M2 MOSFET drain is connected to output node, gate is connected to input node, NMOS source and substrate are connected to 0. 
![image](https://github.com/user-attachments/assets/9ab0265f-61ca-4b4f-b172-47dce0b9f004)

Below snap shows the connectivity of load cap (Cload)
![image](https://github.com/user-attachments/assets/5376a34f-43c2-4eab-b5c1-4b74cd3fcdfe)

below is simulation commands in which we are swiping the Vin from 0 to 2.5 with the stepsize of 0.05.
 ![image](https://github.com/user-attachments/assets/433a0102-6dee-4466-83b3-85839255fd2d)

Final step is to model files. It has the complete description about NMOS and PMOS:
![image](https://github.com/user-attachments/assets/c7571e34-f202-479b-af4a-f2bc95e8214d)

![image](https://github.com/user-attachments/assets/13e913c9-8dbc-4593-8cb2-8d025c40be56)

SPICE simulation for the particular values and below is the output graph:
![image](https://github.com/user-attachments/assets/216a0be8-3c58-410c-b18d-d11cb468c86d)


**Switching Threshold Vm**:
below snap shows the correlation of switching threshold with other parameters:
![image](https://github.com/user-attachments/assets/9f76bc2a-0bc8-43b3-9cc8-e007c7799f36)

Switching thresold, Vm (the point at which the device switches the level) is the one of the parameter that defined the robustness of the Inverter. Switching thresold is a point at which Vin=Vout.
![image](https://github.com/user-attachments/assets/b2b24ce4-a48c-4aee-9551-cc4388c4cf0a)

In the graph below we can identify that the PMOS and NMOS are in which region. The direction of current flowing is different for NMOS nad PMOS.
![image](https://github.com/user-attachments/assets/08fa2316-5b3b-402c-89ba-65d717a68e3b)


Static and dynamic simulation of CMOS inverter:

input to dynamic simulation for checking and rise and fall delay:

![image](https://github.com/user-attachments/assets/a903eb5c-0f04-4e32-b21f-c0aa62ce31e3)



The graph Time vs Voltage will be plotted here from where we can calculate the rise and fall delay:
![image](https://github.com/user-attachments/assets/4b5bb180-69a7-4b22-84fc-73f527b28bd9)

![image](https://github.com/user-attachments/assets/bf6c42c9-06df-45ae-9283-6ab81d9008de)

Calculating Rise Prpagation delay as difference of time stamp for 50% VDD from output rise waveform to input fall waveform: 1.16277e-9-1.01446e-9 = 148ps

![image](https://github.com/user-attachments/assets/02a87caa-93da-43ad-b5b5-cb98a9fcb446)



****Inception of layout:
CMOS faabrication process:**

1. **Selecting a Substrate**:
   ![image](https://github.com/user-attachments/assets/43897e64-4372-472e-a17a-6f096297dad8)

2. **Create Active regions**:
![image](https://github.com/user-attachments/assets/fbfb258e-758a-413f-8b15-e7ac77ad256a)
![image](https://github.com/user-attachments/assets/e6e3730d-4e36-4c48-be13-4973c7be447a)
![image](https://github.com/user-attachments/assets/95610c72-1a91-438a-bf1b-27597dcfd7a3)
![image](https://github.com/user-attachments/assets/51dd0352-a8ec-4454-a7af-c7519bb0973f)
![image](https://github.com/user-attachments/assets/c882c660-3de2-429b-b858-4bfe62d41b8a)

3. **Formation of N-well and P-well**:
![image](https://github.com/user-attachments/assets/64cf88b0-842e-4e0a-9cb0-e3bd613a82a5)
![image](https://github.com/user-attachments/assets/29ed74f5-a026-4081-852c-a0002ef2906e)
![image](https://github.com/user-attachments/assets/ace57fb5-c636-4f0a-80e1-db343a938d71)
![image](https://github.com/user-attachments/assets/ebed081f-daaa-473d-b57d-4f433469db89)
![image](https://github.com/user-attachments/assets/da0a260f-5044-406e-96de-c37236ca54c7)
![image](https://github.com/user-attachments/assets/14cc97a5-98c2-468a-8272-537c2955225b)

4. **Formation of Gate**:
Factors controlling threshold voltage, which decides the turning on of GATE which finally decides gate formation:
 ![image](https://github.com/user-attachments/assets/0dbf7831-7980-47af-b9ec-991946f43aae)
![image](https://github.com/user-attachments/assets/640a3f36-d662-4efd-9ef3-6f7008e7a453)
![image](https://github.com/user-attachments/assets/4669f597-9cc4-4130-9ca9-f33ab1e45505)
![image](https://github.com/user-attachments/assets/9968f561-dd02-403d-8fca-b9447b81803f)
![image](https://github.com/user-attachments/assets/dc52dc93-346d-4aea-b4ce-9054edc79532)
![image](https://github.com/user-attachments/assets/6a6a5c40-52fb-4611-95d6-2a11bd03d8bc)

5. **Lightly doped Drain Formation(LDD)**:
![image](https://github.com/user-attachments/assets/411da397-873a-4248-88d9-c78ea00f9ddf)


**Introduction to delay tables**
Power Aware CTS:- If we make enable pin at logic '1' in the AND gate, then clock will propagate and if we make it 'logic 0' it will block the clock. Similarly in 'OR' gate if we make enable as 'logic 0' it will propagate and on making it 'logic 1' it will block the clock.

So the advantage of this blocking period is that we can save lot of power in clock tree.
![image](https://github.com/user-attachments/assets/f568caab-090c-4caa-aeb9-b6b6e2e361b1)
![image](https://github.com/user-attachments/assets/f623bdd7-4a76-4526-9b80-729adba5a422)
![image](https://github.com/user-attachments/assets/62a59596-7904-4869-ab86-5c39d998daaf)

![image](https://github.com/user-attachments/assets/9205cceb-f701-4ea3-9437-80c83d999eae)


**Setup Timing ananlysis**: 
![image](https://github.com/user-attachments/assets/56280d51-3958-44ae-95ff-914349dae9c8)
![image](https://github.com/user-attachments/assets/edb66cdc-06be-421c-b30b-116eb9e4ee6f)
![image](https://github.com/user-attachments/assets/869d7c5c-145b-4f0b-b8ed-e32fd31291a1)
![image](https://github.com/user-attachments/assets/9e7efe0c-eefd-4e31-bb52-d0f18c6a8279)

**Clock Jitter and uncertainty**:
![image](https://github.com/user-attachments/assets/1954eb42-71f4-4914-9f37-a89148a53db9)
![image](https://github.com/user-attachments/assets/6090a6e8-15e6-40d3-b91d-b6399fa808a9)
![image](https://github.com/user-attachments/assets/1ffebeac-92a9-43dc-b7e6-576f3998ba64)
![image](https://github.com/user-attachments/assets/5d1b421a-98d1-4038-a3db-21c4ef5de0d3)

**Clock Tree Synthesis**:
![image](https://github.com/user-attachments/assets/83f4a326-e4c4-453d-a4ae-8be5796b6be9)
![image](https://github.com/user-attachments/assets/c73af59f-3290-46f1-98e3-0612684f7cf9)
![image](https://github.com/user-attachments/assets/ce2e96bb-98bf-4dcf-a706-42e98c9e6bdb)
![image](https://github.com/user-attachments/assets/08904e40-3962-494c-a289-dbc134dbc06a)
![image](https://github.com/user-attachments/assets/a1a3bf7b-7b08-4fd6-bafb-19607e8543ce)
![image](https://github.com/user-attachments/assets/d919fdd5-8854-4287-a148-cd44f3ebc470)

**Need for clock Buffering**:
![image](https://github.com/user-attachments/assets/ba6fec53-b3a3-42f1-b905-c717235ac6db)


**Timing Analysis with Ideal clock**:
![image](https://github.com/user-attachments/assets/35983baf-17a0-4fdc-ba18-d8b66cf52aec)
![image](https://github.com/user-attachments/assets/dc8652e1-842d-4096-8876-192a72b20e74)

**Clock Net Shielding**:
![image](https://github.com/user-attachments/assets/1f28052b-0adc-43d7-8bd7-258368f0c7dc)
![image](https://github.com/user-attachments/assets/ac868da3-868f-45b0-8d7f-931b19fc77fa)
![image](https://github.com/user-attachments/assets/984a2ed7-6100-4539-b167-6d0fb89f2184)

**Timing Analysis with Real clocks**:

**SETUP TIMING ANALSYSIS**:
![image](https://github.com/user-attachments/assets/f1672a4b-beb1-4717-b276-963849bf938a)
![image](https://github.com/user-attachments/assets/d1141fd2-3bc4-471b-82bf-ab14a3275eb3)
![image](https://github.com/user-attachments/assets/c966824c-e0a6-4b35-b4da-a23b02ed9026)
![image](https://github.com/user-attachments/assets/04df9190-4d38-4f16-989b-a2006c3b0e0c)
![image](https://github.com/user-attachments/assets/dc15f4ff-1fd5-4ceb-9901-6261bab0c35d)

**HOLD TIMING ANALYSIS**:
![image](https://github.com/user-attachments/assets/b5162a19-3291-4ad1-842b-f73167a7dd36)
![image](https://github.com/user-attachments/assets/e5072bfe-bf8e-46f4-990c-5607b6cc7d2a)
![image](https://github.com/user-attachments/assets/7eda12d0-1d22-4b21-998b-9a9a98745a11)
![image](https://github.com/user-attachments/assets/c4cc7eb9-a0fd-4b49-aa93-b8bc4231a4eb)
![image](https://github.com/user-attachments/assets/eab97193-408d-45c0-88e0-af7987214411)
![image](https://github.com/user-attachments/assets/a3ec3116-a610-43ed-acf4-bb2bfb70cf25)
![image](https://github.com/user-attachments/assets/596ce615-7383-4039-92b3-082be74f62ba)
![image](https://github.com/user-attachments/assets/a0e91873-2141-4eca-b322-5641c5f3eb6e)


**ROUTING & DRC rule check**:
![image](https://github.com/user-attachments/assets/3ffdb503-6d65-44d6-b866-8a2589330641)

![image](https://github.com/user-attachments/assets/70eea392-8c75-45f6-b687-eabe2844f08b)







