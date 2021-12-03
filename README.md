# SoC-Physical-design-using-Sky130
 A 5-day lab-based hands-on workshop that gives knowledge of hierarchical physical design flow on how to build a chip from scratch using multiple opensource tools. This workshop also gives an understanding of all the steps in RTL to GDSII flow in detail.

# Overview of Physical Design flow -

Place and Route (PnR) is the core of any ASIC implementation and Openlane flow integrates into it several key open source tools which perform each of the respective stages of PnR. Below are the stages and the respective tools (in ( )) that are called by openlane for the functionalities as described:

1. Synthesis:
Generating gate-level netlist (yosys).
Performing cell mapping (abc).
Performing pre-layout STA (OpenSTA).
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

OpenLANE (An open source automated flow) - 

OpenLANE is an automated RTL to GDSII flow based on several tools including OpenROAD, Yosys, Magic, Netgen, Fault, OpenPhySyn, SPEF-Extractor and custom methodology scripts for designing and optimization.

Here's a picture of the OpenLANE flow diagram :

