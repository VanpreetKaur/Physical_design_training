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

libs.tech folder includes files specific to tools. 
libs.ref folder includes input files specific to process node and cell type such as verilog, libs, lef, cdl, gds etc.
Nomenclature for specific folder in libs.ref such as sky130_fd_sc_hd: 
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



**Library Binding, Placement** **& cell design Flow**:
 ![image](https://github.com/user-attachments/assets/838551ed-7f79-4a15-8f6a-893f06a893d3)

![image](https://github.com/user-attachments/assets/ec186a58-e795-4249-ab37-ddc0cf940cd4)

![image](https://github.com/user-attachments/assets/fecb3a5f-af0d-42e4-9a7d-ebee974ff5d4)

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
![image](https://github.com/user-attachments/assets/bbe61f03-1def-4746-8cdb-d73ba4b669d8)

 below is simulation commands in which we are swiping the Vin from 0 to 2.5 with the stepsize of 0.05. Because we want Vout while changing the Vin:
 ![image](https://github.com/user-attachments/assets/433a0102-6dee-4466-83b3-85839255fd2d)

Final step is to model files. It has the complete description about NMOS and PMOS:
![image](https://github.com/user-attachments/assets/c7571e34-f202-479b-af4a-f2bc95e8214d)

![image](https://github.com/user-attachments/assets/13e913c9-8dbc-4593-8cb2-8d025c40be56)

SPICE simulation for the particular values and below is the output graph:
![image](https://github.com/user-attachments/assets/216a0be8-3c58-410c-b18d-d11cb468c86d)
