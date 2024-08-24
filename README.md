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



**Library Binading and Placement**
 ![image](https://github.com/user-attachments/assets/8f4aadee-93bf-41bb-bd43-125253981901)



**Cell Design and characterization flows**:
![image](https://github.com/user-attachments/assets/a76df220-a332-4b2d-a709-405b0f715464)







