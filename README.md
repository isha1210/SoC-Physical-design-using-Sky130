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

![floorplansuccess](https://user-images.githubusercontent.com/92804006/144569134-f9033bda-e7d9-4709-b39c-007b9172c494.jpg)

Floorplan using Magic tool -

Command -

![Layout_command](https://user-images.githubusercontent.com/92804006/144569766-a427601d-f430-4622-b177-05ff9cea8b25.jpg)

Output- 

![Layout_op](https://user-images.githubusercontent.com/92804006/144569890-58abc422-3ff0-4aab-b9d4-9c3bb976cc00.jpg)

