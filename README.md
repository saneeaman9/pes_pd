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

 ![magic](https://github.com/saneeaman9/pes_pd/assets/75088597/adb0cd26-c4ee-4103-be47-31f287e36518)

 
</details>


<details>
  <summary>Day 3 :</summary>
</details>


<details>
  <summary>Day 4 :</summary>
</details>


<details>
  <summary>Day 5 :</summary>
</details>
