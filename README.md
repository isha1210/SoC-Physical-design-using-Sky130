# SoC-Physical-design-using-Sky130
 A 5-day lab-based hands-on workshop that gives knowledge of hierarchical physical design flow on how to build a chip from scratch using multiple opensource tools. This workshop also gives an understanding of all the steps in RTL to GDSII flow in detail.

# Overview of Physical Design flow -

Place and Route (PnR) is the core of any ASIC implementation and Openlane flow integrates into it several key open source tools which perform each of the respective stages of PnR. Below are the stages and the respective tools (in ( )) that are called by openlane for the functionalities as described:

1. Synthesis: (tools)

 Generating gate-level netlist. (yosys)

 Performing cell mapping. (abc)

 Performing pre-layout STA. (openSTA)

2. Floorplanning

 Defining the core area for the macro as well as the cell sites and the tracks (init_fp).
 
 Placing the macro input and output ports (ioplacer).
 
 Generating the power distribution network (pdn).

3. Placement
 
 Performing global placement (RePLace).
 
 Perfroming detailed placement to legalize the globally placed components (OpenDP).

4. Clock Tree Synthesis (CTS)

 Synthesizing the clock tree (TritonCTS).

5. Routing
 
 Performing global routing to generate a guide file for the detailed router (FastRoute).
 
 Performing detailed routing (TritonRoute)

6. GDSII Generation
 
 Streaming out the final GDSII layout file from the routed def (Magic).

# OpenLANE (An open source automated flow) - 

OpenLANE is an automated RTL to GDSII flow based on several tools including OpenROAD, Yosys, Magic, Netgen, Fault, OpenPhySyn, SPEF-Extractor and custom methodology scripts for designing and optimization.

Here's a picture of the OpenLANE flow diagram :

![1](https://user-images.githubusercontent.com/92804006/144555026-cb198c0d-db4b-4b9d-8c57-f472cd7cbfb0.jpg)

Process Design Kit(PDKs) contains device models, digital standard cell libraries, process design rules, input-output libraries and other important collection of files used for fabrication.

The command to start OpenLANE : 

![open](https://user-images.githubusercontent.com/92804006/144558185-a0aabead-0592-4750-812f-cb64abb0f640.jpg)

 The current design is picorv32a.so we need to issue prep -design picorv32a which will merge all the LEF and also complete padding:

![designprep](https://user-images.githubusercontent.com/92804006/144558861-117e2eca-89b6-45de-b440-487d7767734e.jpg)

run_synthesis is the command for start synthesis. Synthesis takes a few minutes(according to the depth of design), below is the result summary with the flop ratio:

![succsyn](https://user-images.githubusercontent.com/92804006/144559474-4a9d53ea-280e-498d-b2e7-f97d5868da61.jpg)

![flopratio](https://user-images.githubusercontent.com/92804006/144560332-fe5ab49c-917d-43d8-8ad7-d1d9e5505c33.jpg)

Here, flop ratio. the flop ratio = Total number of flip-flops / Total number of Cells

the total number of flip-flops = 1398 and total number of Cells = 14876 and the ratio = 0.094

All the runs available in working_dir/openLANE/design/picorv32a/runs directory names as date.

Floorplan

Floorplan determines the chip quality. It includes :

Define the size of your chip/block and Aspect ratio
Defining the core area and IO core spacing
IO Placement/Pin placement
Allocates power routing resources
Place the hard macros and reserve space for standard cells.
Defining Placement and Routing blockages blockages
Creating Rings

![FPcomp1](https://user-images.githubusercontent.com/92804006/144569416-d7eb5dfc-803c-4802-8aa4-53e870a51ead.jpg)

Command -

![magiclayout_command](https://user-images.githubusercontent.com/92804006/144570218-27e0d39c-4fb0-4bc4-b548-f0cf45d9698a.jpg)

![floorplansuccess](https://user-images.githubusercontent.com/92804006/144569134-f9033bda-e7d9-4709-b39c-007b9172c494.jpg)

Floorplan using Magic tool -

Output -

![magiclayout](https://user-images.githubusercontent.com/92804006/144570240-47017121-da95-4b6e-b317-e6f3f46ae6e3.jpg)

Placement -

![Placement_layout_str](https://user-images.githubusercontent.com/92804006/144581268-6b819c9c-d698-4466-8980-30a57b8e2b98.jpg)

After changing values - 

run_placement is the command for start placement.Below is a snapshot after completing placement using magic: 

![Placement](https://user-images.githubusercontent.com/92804006/144580447-22db3242-1c49-4aa0-aab1-46dfce0eaa38.jpg)

STANDARD CELLS

There are multiple standard cells available in github or skywater. In this workshop let's cloning from github as below:

![Gitclone1](https://user-images.githubusercontent.com/92804006/144571044-e9a7ea7c-ff5f-4554-b1fa-015babe52d0e.jpg)

The standard cell inverter(CMOS) layout looks as below: 

Command -

![Layout_command](https://user-images.githubusercontent.com/92804006/144571218-f1378074-e038-432f-935e-9218e6912f66.jpg)

Output -

![Layout_op](https://user-images.githubusercontent.com/92804006/144571231-13d90ffa-d86e-4bb6-b620-1975a2659845.jpg)

Next step is to characterise the inverter using ngspice. The command " extraxt all,ext2spice chresh 0 rthresh 0 followed by ext2spice" create spice netlist.

![Extract_done](https://user-images.githubusercontent.com/92804006/144572035-2f5e8cc4-c061-4e01-8a9b-31d2cd410b8f.jpg)

The spice netlist can be observed with the nodes available in the layout:

![spice-file](https://user-images.githubusercontent.com/92804006/144572264-542cff89-b9f6-4c86-9759-c21372378152.jpg)

The spice netlist is incomplete without the VDD, VSS and input pulse. Below is the updated spice netlist consisting of all necessary inputs and power:

![spice_newcode](https://user-images.githubusercontent.com/92804006/144572276-fe4081eb-b1c0-4bf7-b8c7-968fff3735d2.jpg)

Result after issuing the ngspice command ngspice sky130_inv.spice :

![ngsp_command](https://user-images.githubusercontent.com/92804006/144703199-8091a249-2477-4f96-be2d-fe42d0dcd590.jpg)

![Initialtransientvalues](https://user-images.githubusercontent.com/92804006/144703274-dac9db60-30c4-4f4f-899f-97cdc007fa5e.jpg)

![sim1](https://user-images.githubusercontent.com/92804006/144703209-fff988a6-c0cd-4e19-b173-e0dd0a01fd79.jpg)

The inverter waveform after changing C3 to 2fF :

![sim2](https://user-images.githubusercontent.com/92804006/144703349-ee2690f8-4fcc-4b82-8ba7-4947ba9be73a.jpg)

Characterizing:

Rise transition : time taken by the output to transit from 20% of the maximum value to 80% of the maximum value

Fall transition : time taken by the output to transit from 80% of the maximum value to 20% of the maximum value

Fall cell delay : difference between the time when output has fallen to 50% of the maximum value while input is rised to 50% of the maximum value

Rise cell delay : difference between the time when output has rised to 50% of the maximum value while input is falled to 50% of the maximum value

propogation delay = time(out_thr) - time(in_thr)

Extraction of LEF file:

In magic layout, activate the tracks to ensure that A and Y pins are placed on the intersection of tracks.

In tkcon to activate grids

![extlay](https://user-images.githubusercontent.com/92804006/144703616-49f94033-f5dc-4b4f-bd91-df423a9a6ef6.jpg)

For synthesis, a library which has the cell defnition is necessary so copy this lef file into picoprv32a src directory

Edit the config file to include the variables required for synthesis of std cell inverter.

![4](https://user-images.githubusercontent.com/92804006/144704264-a4e9dda2-6d87-4308-9c9b-fec90631501a.jpg)

