# pes_pd
VLSI PHYSICAL DESIGN

# OPENLANE Design Stages

OpenLANE flow consists of several stages. By default all flow steps are run in sequence. Each stage may consist of multiple sub-stages. OpenLANE can also be run interactively as shown [here][25].

1. *Synthesis*
    1. `yosys` - Performs RTL synthesis
    2. `abc` - Performs technology mapping
    3. `OpenSTA` - Pefroms static timing analysis on the resulting netlist to generate timing reports
2. *Floorplan and PDN*
    1. `init_fp` - Defines the core area for the macro as well as the rows (used for placement) and the tracks (used for routing)
    2. `ioplacer` - Places the macro input and output ports
    3. `pdn` - Generates the power distribution network
    4. `tapcell` - Inserts welltap and decap cells in the floorplan
3. *Placement*
    1. `RePLace` - Performs global placement
    2. `Resizer` - Performs optional optimizations on the design
    3. `OpenPhySyn` - Performs timing optimizations on the design
    4. `OpenDP` - Perfroms detailed placement to legalize the globally placed components
4. *CTS*
    1. `TritonCTS` - Synthesizes the clock distribution network (the clock tree)
5. *Routing*
    1. `FastRoute` - Performs global routing to generate a guide file for the detailed router
    2. `CU-GR` - Another option for performing global routing.
    3. `TritonRoute` - Performs detailed routing
    4. `SPEF-Extractor` - Performs SPEF extraction
6. *GDSII Generation*
    1. `Magic` - Streams out the final GDSII layout file from the routed def
    2. `Klayout` - Streams out the final GDSII layout file from the routed def as a back-up
7. *Checks*
    1. `Magic` - Performs DRC Checks & Antenna Checks
    2. `Klayout` - Performs DRC Checks
    3. `Netgen` - Performs LVS Checks
    4. `CVC` - Performs Circuit Validity Checks

OpenLane can be operated at 2 different modes ie., Automated flow and Interactive mode.




# CourseWork

<details>
  <summary>Day 1 : Inception of open-source EDA, OpenLANE and Sky130 PDK
</summary>

</br>

**OpeLANE directory structure**

![1](https://github.com/saneeaman9/pes_asic_class/assets/75088597/bbd00f12-51f5-45bc-81fe-18c7b16ca72c)


**Config.tcl**

![3](https://github.com/saneeaman9/pes_asic_class/assets/75088597/746ddc16-31b7-4d4f-944f-1dc0bbe9439f)


**Design preparation**

```bash
docker
pwd
/openLANE_flow
ls -ltr
./flow.tcl -interactive
package require openlane 0.9
prep -design picorv32a

```

![Screenshot from 2023-09-11 03-34-32](https://github.com/saneeaman9/pes_asic_class/assets/75088597/335b7bd7-eef3-4c23-8aaf-090fc7920a50)

* Now synthesize the design using command 

```bash
  run_synthesis
   ```

![2](https://github.com/saneeaman9/pes_asic_class/assets/75088597/04f94d6d-af37-40a9-b70d-3ef5be9e896c)

**Synthesis Result**

![synthesis result](https://github.com/saneeaman9/pes_asic_class/assets/75088597/ccc0ee82-5d12-4063-a978-5373520f2013)


**Report**

![report](https://github.com/saneeaman9/pes_asic_class/assets/75088597/605f0c34-1eb2-4cde-aa39-8ca457169ce5)


**D FF Ratio Calculation**

![dff ratio](https://github.com/saneeaman9/pes_asic_class/assets/75088597/bfd197ff-5ace-4764-a164-efdc6f74b24d)


</details>


<details>
  <summary>Day 2 : Good floorplan vs bad floorplan and introduction to library cells</summary>

  ### Chip Floorplanning considerations

**Steps to run the FloorPlan**

* To run the floorplan use this command after completing synthesis.
  ```bash
    run_floorplan
  ```
![1](https://github.com/saneeaman9/pes_pd/assets/75088597/229894f8-25a5-42a0-a889-ccac51b0018c)

  

* Checking logs(ioplacer.log)

![2](https://github.com/saneeaman9/pes_pd/assets/75088597/eff73f99-afc9-4806-b635-36ed54126404)


* Floorplan(.def file)
  ![floorplan](https://github.com/saneeaman9/pes_pd/assets/75088597/d63310f0-305d-446c-b868-c6d311a48cd9)
![core util](https://github.com/saneeaman9/pes_pd/assets/75088597/f06e9747-abcb-4b40-bc46-31b80632c335)


### Library Binding and Placement

* Run the placement 

```bash
  run_placement
```

![runplace](https://github.com/saneeaman9/pes_pd/assets/75088597/8ea796f3-d7fb-4c72-ad48-1411c1bbd16b)



* To check the floorplan using magic run this command in the floorplan folder of the desired run.

```bash
magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.floorplan.def &
```

![Screenshot from 2023-09-17 20-08-42](https://github.com/saneeaman9/pes_pd/assets/75088597/52008e44-8745-4214-8f08-8f12af18c797)



 
</details>


<details>
  <summary>Day 3 : Design library cell using Magic Layout and ngspice characterization</summary>

</br>

* Clone this repository into the openlane folder
  
  ```bash
  
    cd /Desktop/work/tools/openlane_working_dir/openlane

    git clone https://github.com/nickson-jose/vsdstdcelldesign.git

  ```

### Inverter layout using Magic

```bash
cd Desktop/work/tools/openlane_working_dir/openlane/vsdstdcelldesign

magic -T sky130A.tech sky130_inv.mag


```

[img]:magic  

</br>

### Exploring the layout using Magic

</br>

* To select a region hover over the area and press ```s``` .
 
* After selecting the area type ```what``` in the console to display information on the selected area.


![what](https://github.com/saneeaman9/pes_pd/assets/75088597/28a4451b-d681-429c-9d48-6e6058f05a14)

</br>

### DRC Check

* To check for DRC Errors, select a region (left click for starting point, right click at end point) and see the DRC column at the top that shows how many DRC errors are present.The Details of DRC Errors will be printed on the console.

![DRC check](https://github.com/saneeaman9/pes_pd/assets/75088597/3f26fcc4-d108-4618-8502-61d487179ab7)


### Extracting PEX to SPICE with Magic

Select the entire inverter layout.

![ext2spice](https://github.com/saneeaman9/pes_pd/assets/75088597/8ff4e4dc-a15c-4303-a116-9b3bd3a9b1b5)

![spice](https://github.com/saneeaman9/pes_pd/assets/75088597/e4816874-6c0d-49ad-97a3-488829331047)

### Grid Size

![grid size](https://github.com/saneeaman9/pes_pd/assets/75088597/5115f020-cbc6-41c8-a5cd-db6356129231)

![gridsize2](https://github.com/saneeaman9/pes_pd/assets/75088597/7b8199b1-ce31-43f0-9d66-074de0bbecbc)

### Modified SPICE netlist

[img]:modifiedspice

* To run the SPICE netlist
  ```bash
  ngspice sky130_inv.spice
  plot y vs time a
  ```

![sim](https://github.com/saneeaman9/pes_pd/assets/75088597/0b9aa0f1-66a8-4095-9b52-0008f8d2cfe6)

**Results from the graph**

* Rise Transition : 0.0395ns
* Fall transition : 0.0282ns
* Cell Rise delay : 0.03598ns
* Cell fall delay : 0.0483ns





</details>



<details>
  <summary>Day 4 : Pre-layout timing analysis and importance of good clock tree
</summary>

### LEF Info

1. Technology LEF: This file contains critical information about metal layers, via configurations, and restricted Design Rule Check (DRC) rules. It defines the foundational aspects of the chip's technology.

2. Cell LEF: The Cell LEF file provides an abstract representation of standard cells, encapsulating their characteristics. It is essential for accurate placement and routing of standard cells.

3. Standard Cell Dimensions - The width of standard cells should be an odd multiple of the track pitch. This ensures proper alignment with horizontal tracks.Similarly, the height of standard cells should be an odd multiple of the vertical track pitch. This alignment is crucial for efficient routing.

4. PnR Tool Integration - PnR tools utilize the abstract view information from the Cell LEF to guide the placement and interconnection of standard cells.

5. PnR Tool Integration - PnR tools utilize the abstract view information from the Cell LEF to guide the placement and interconnection of standard cells.

6. PnR Tool Integration - PnR tools utilize the abstract view information from the Cell LEF to guide the placement and interconnection of standard cells.


### Extraction of LEF

* The ```tracks.info``` file can be found in:
  ```~/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/openlane/sky130_fd_sc_hd```


* Open the file 
  
```bash
less tracks.info
```

![tracks](https://github.com/saneeaman9/pes_pd/assets/75088597/acb7521b-254a-4a77-a4f1-7555935115b6)

From above image we can say that 1st value indicates the offset and 2nd value indicates the pitch along provided direction.

### Setting grid values using above file info


![gridafter](https://github.com/saneeaman9/pes_pd/assets/75088597/0fb23cd3-ac36-4165-8b3b-ed0d8e3c0828)

From the above pic, its confirmed that the pins A and Y are at the intersection of X and Y tracks. So both conditions (The width of standard cells should be an odd multiple of the track pitch & the height of standard cells should be an odd multiple of the vertical track pitch) are met.


### LEF Generation

Step 1 : Enter these commands in console

```bash
save sky130_vsdinv.mag
```

Step 2 : Open the file and extract LEF by using this command.

```bash
magic -T sky130A.tch sky130_vsdinv.mag
```

* Now enter this command in the new console.

```bash
lef write
```
![lefwrite](https://github.com/saneeaman9/pes_pd/assets/75088597/c64ce9ce-2786-4a90-936a-9a5eade2837d)


Step 3 : Plug the generated lef file into PICORV32a

* You can see the lef file in the vsdstdcell folder

![viewlef](https://github.com/saneeaman9/pes_pd/assets/75088597/26eafec2-7f17-424a-8ab5-fb18385b3e93)

* Now copy this lef file into the following directory
  
 ```bash
cp sky130_vsdinv.lef /home/vsduser/Desktop/work/tools/openlane_working_dir/designs/picorv32a/src/
  ```

![copylef](https://github.com/saneeaman9/pes_pd/assets/75088597/1834c38a-a3dd-4558-b2d7-cd344c07ff54)

* Now we need to copy the ```sky130_fd_sc_hd__*``` which is present in this ```Desktop/work/tools/openlane_working_dir/openlane/vsdstdcelldesign/libs``` in this directory into this ```Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/src``` directory by using the command given below.

```bash
cp sky130_fd_sc_hd__* /home/vsduser/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/src/

```

![cpy2nd](https://github.com/saneeaman9/pes_pd/assets/75088597/6b92a417-ddf4-4730-9aea-130643d84626)

*  Modify the ```config.tcl``` file to specify the usage of these libraries and the LEF file.

![configtcl](https://github.com/saneeaman9/pes_pd/assets/75088597/47fe649a-5aac-40e0-b43f-e01b4dfb232a)

Step 4 : Make sure the lef file is added

* Invoke OPENLANE 

```bash
cd Desktop/work/tools/openlane_working_dir/openlane/
docker
./flow.tcl -interactive
```

__Make sure the lef file is added and enter these set of commands given below.__

**Note : change the timestamp according to your runs.**

```bash
package require openlane 0.9
prep -design picorv32a -17-09_10-34 -overwrite
set lefs [glob $::env(DESIGN_DIR)/src/*.lef]
add_lefs -src $lefs
run_synthesis
```

![edit openlane](https://github.com/saneeaman9/pes_pd/assets/75088597/44b68417-1a01-449b-808c-29d403896438)

![synthprcss](https://github.com/saneeaman9/pes_pd/assets/75088597/d990cb65-5f6c-4735-839e-e55856948b40)

The above figure shows that our vsdinv cell has been used in synthesis process

**Managing slack in Very Large Scale Integration (VLSI) design, particularly in the context of Static Timing Analysis (STA). Let me provide some additional information and clarification on the points you mentioned:**

1. Obtaining System Specifications: In the architecture design phase of VLSI, engineers gather system specifications that include various parameters, one of which is the required frequency of operation. This frequency is crucial for determining the overall performance of the integrated circuit.
2. Static Timing Analysis (STA): STA is a critical step in VLSI design to ensure that the designed circuit meets its timing requirements. It involves analyzing the timing behavior of the circuit to ensure that signals meet setup and hold time requirements.
3. Setup Timing: When referring to "pre clock tree synthesis STA analysis" and setup timing, you're concerned with ensuring that signals reach their destination registers reliably before the clock edge (setup time) to avoid setup violations.
4. Worst Negative Slack (WNS) and Total Negative Slack (TNS): WNS represents the worst-case delay violation in the design, while TNS is the sum of all negative slack values across the design. These metrics help identify critical paths and areas where timing violations are occurring.
5. Fixing Slack Violations: To address timing violations, designers often use STA analysis tools like OpenSTA, which is integrated into the OpenLANE tool. These tools help identify the specific violations and allow for debugging and optimization to meet timing constraints.

**Two Steps for Correct Operation of Tools:**

1. Design Configuration Files (.conf): These files contain tool-specific configuration settings for the design. They specify how the tools should analyze and optimize the design.
2. Design Synopsys Design Constraint (.sdc) Files: SDC files provide industry-standard timing constraints for the design. They specify the timing requirements for various elements of the design, such as setup and hold times, clock definitions, and input/output delays.
  
By providing these configuration files and constraints to tools, you ensure that the tools operate correctly and optimize the design based on the specified requirements. In summary, managing slack and ensuring proper timing constraints are essential steps in VLSI design to guarantee that the integrated circuit operates correctly and meets its performance specifications. The use of STA tools and well-defined configuration files and constraints plays a crucial role in achieving this goal.

Step 5 : We need to run run_synthesis again

* Enable STNTH_SIZING and set SYNTH_STRATEGY "DELAY 1," carefully monitor the synthesis results for improvements in critical path delay. Adjust these settings iteratively as needed to meet your design's performance goals.

![1](https://github.com/saneeaman9/pes_pd/assets/75088597/71ca3ecc-8744-4f82-b1eb-6332bfb07d2a)

![2](https://github.com/saneeaman9/pes_pd/assets/75088597/1d84d875-a059-4c0c-b4be-fd45c4d7702d)

Despite a significant reduction in slack, the timing requirements have not yet been met. This issue could be related to the constraints defined in the "my_base.sdc" file, which is specified in the "pre_sta.conf" configuration file. To address this, consider revising the constraints within "my_base.sdc" and then rerun the STA analysis using the command "sta pre_sta.conf" for further optimization.

![3](https://github.com/saneeaman9/pes_pd/assets/75088597/2bad9960-5db8-42ff-8913-01e667ba4f6b)

High fanout can lead to increased delay in digital circuits. To address this, you can enhance synthesis results by adjusting the SYNTH_MAX_FANOUT variable and then rerunning the synthesis process to optimize the fanout and reduce delay.

Step 2: Enable cell buffering & performing manual cell replacement on our WNS path with the OpenSTA tool

To improve timing on the worst negative slack (WNS) path, enable cell buffering to enhance signal propagation. Additionally, identify the primary net responsible for driving multiple outputs and consider replacing the driving cell with a larger version of the same cell type for potential performance gains.

![4](https://github.com/saneeaman9/pes_pd/assets/75088597/40044050-8107-424f-8bc2-02c3d9255750)

* Optimize the fanout value with OpenLANE tool

Since we successfully synthesized the core using our VSDINV cell, it should be reflected in the layout after the ```run_placement``` stage, which follows the ```run_floorplan``` stage.

![5](https://github.com/saneeaman9/pes_pd/assets/75088597/c18c5cdf-cade-4ea1-91b0-02542403b131)



Step 6 : Clock Tree Synthesis 

After addressing slack violations using the "pre_sta.conf" configuration, generate a netlist with the corrected design using "write_verilog." Subsequently, replace the original OpenLANE-generated "picorv32a.synthesis.v" file with this modified netlist to ensure the design incorporates the necessary fixes.

In the OpenLANE flow, proceed with the "run_floorplan," "run_placement," and "run_cts" stages to further refine the design and ensure that the corrections made to the netlist are incorporated into the physical layout.

Step 7 :  Post CTS- STA Analysis

* Within OpenROAD, perform timing analysis by generating a ```.db``` database file.

1) Launch OpenRoad.
2) Load the LEF file from the "tmp" folder in your OpenLANE runs.
3) Load the DEF file from the CTS results.
4) Create and save the .db database file.
5) Load the generated .db file.
6) Read the CTS-generated Verilog file.
7) Import both the minimum and maximum liberty files.
8) Define clock domains.
9) Generate timing reports to analyze the design further.

![6](https://github.com/saneeaman9/pes_pd/assets/75088597/60e12995-ed9c-41bc-9d0b-d4f807b7cf9c)

![7](https://github.com/saneeaman9/pes_pd/assets/75088597/2ad66bc9-d8a0-433a-9ca1-e20457ec32c3)

![8](https://github.com/saneeaman9/pes_pd/assets/75088597/f0ce56b6-dd99-4ad0-b2dc-67bf5509e022)


The timing results are unlikely to meet our expectations due to the utilization of min and max library files, which OpenRoad does not currently support for multi-corner optimization. To address this, we will solely employ the typical corner library for optimization purposes.


![9](https://github.com/saneeaman9/pes_pd/assets/75088597/f240738a-6c07-44ea-bdc9-ddad50770f5d)
![10](https://github.com/saneeaman9/pes_pd/assets/75088597/0ad78c43-d686-4936-b518-d9d722f9f5fd)
![11](https://github.com/saneeaman9/pes_pd/assets/75088597/af966cb8-b47d-4ea9-98af-bc6ba75dec24)
![12](https://github.com/saneeaman9/pes_pd/assets/75088597/63a46f5b-a26a-491f-873c-312e0c6201d7)





</details>


<details>
  <summary>Day 5 :</summary>
</details>
