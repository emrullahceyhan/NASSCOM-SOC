# NASSCOM-SOC

## Day 1 - Design Preparation and Synthesis 

This section explains how to synthesize the picorv32a project using the OpenLane flow and interprets some basic parameters from the synthesis results. In this step of OpenLane flow, Yosys open source synthesizer is used.

```sh
# Change directory to openlane flow directory
> cd Desktop/work/tools/openlane_working_dir/openlane
```
```sh
# The OpenLane flow is in docker so type docker to invoke the flow
> docker
```
We have entered the docker subsystem as shown in below. 

![](https://github.com/emrullahceyhan/NASSCOM-SOC/blob/development/Pictures/1.png?raw=true)

```sh
# To invoke the OpenLANE flow in the Interactive mode use below command
> ./flow.tcl -interactive
```
The OpenLane flow has now been opened, and the terminal output is as follows.

![](https://github.com/emrullahceyhan/NASSCOM-SOC/blob/development/Pictures/2.png?raw=true)

```sh
# For the OpenLane flow to function properly, the appropriate package needs to be provided.
> package require openlane 0.9
```
![](https://github.com/emrullahceyhan/NASSCOM-SOC/blob/development/Pictures/3.png?raw=true)

```sh
# In this study, the picorv32a project will be synthesized, but before that, some necessary files and directories need to be created. Therefore, the design must first be prepared using the prep design step.
> prep -design picorv32a
```
![](https://github.com/emrullahceyhan/NASSCOM-SOC/blob/development/Pictures/4.png?raw=true)

```sh
# Then run synthesis
> run_synthesis
```
![](https://github.com/emrullahceyhan/NASSCOM-SOC/blob/development/Pictures/5.png?raw=true)

The synthesis was completed with negative TNS(Total Negative Slack) and WNS(Worse Negative Slack) values.

The synthesis results can be reviewed under the `Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/Day-Month_Hour-Minute` path, specifically in the **reports** and **results** sections.

Within the `reports/synthesis` directory under `Day-Month_Hour-Minute`, many timing details can be accessed. Timing results from all paths can be analyzed.

As a result of the synthesis, resource utilization outputs can be accessed either from the synthesis screen or from the `1-yosys_4.stat.rpt` file located under `reports/synthesis`.

![](https://github.com/emrullahceyhan/NASSCOM-SOC/blob/development/Pictures/6.png?raw=true)


Above, the resource usage resulting from the synthesis of the project can be seen. From this, a percentage-based utilization calculation can be derived.

For example there are 14876 cells are available in the system and 1613 D type Flip-Flops are used.

Then D Flip-Flop usage percentage = (#D Flip-Flop) / (#Total Cells available)

So about %10.84 of cells are used by D Flip-Flop


## Day 2 - Floorplanning, Placement and Introduction to Library Cells
### Section 1 - Floorplanning


```sh
# Change directory to openlane flow directory
> cd Desktop/work/tools/openlane_working_dir/openlane
```
```sh
# The OpenLane flow is in docker so type docker to invoke the flow
> docker
```
```sh
# To invoke the OpenLANE flow in the Interactive mode use below command
> ./flow.tcl -interactive
```

```sh
# For the OpenLane flow to function properly, the appropriate package needs to be provided.
> package require openlane 0.9
```

```sh
# In this study, the picorv32a project will be synthesized, but before that, some necessary files and directories need to be created. Therefore, the design must first be prepared using the prep design step.
> prep -design picorv32a
```

```sh
# Then run synthesis
> run_synthesis
```

```sh
# Then run floorplan
> run_floorplan
```
Below is the example terminal output of floorplannig.
![](https://github.com/emrullahceyhan/NASSCOM-SOC/blob/development/Pictures/7.png?raw=true)

```sh
Note : There are several files that contain the configurations used during the flow. The priority order among them is as follows:
1. Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/sky130A_sky130A_fd_sc_hd_config.tcl
2. Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/config.tcl
3. Desktop/work/tools/openlane_working_dir/openlane/*.tcl ( Includes defaults of every step in the flow)

Configuration files with a higher priority level always overwrite the ones that come after them.
```

Additionally, the values set for the `FP_IO_VMETAL` and `FP_IO_HMETAL` parameters are always processed as one more than the specified value. For example, if `FP_IO_VMETAL` is set to 3 and `FP_IO_HMETAL` to 4, you will see in the floorplan log files that these values are taken as 4 for `FP_IO_VMETAL` and 5 for `FP_IO_HMETAL`.


If you're curious about how your floorplan result looks, you can check the following file.
`Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/Day-Month_Hour-Minute/results/floorplan/picorv32a.floorplan.def`

Below is the example terminal output of floorplannig.
![](https://github.com/emrullahceyhan/NASSCOM-SOC/blob/development/Pictures/8.png?raw=true)

In the terminal, the values in the DIEAREA ( 0 0 )( 660685 671405 ) line represent the following: the first 0 corresponds to the lower-left x, the second 0 corresponds to the lower-left y, 660685 represents the upper-right x, and 671405 represents the upper-right y. These values are referred to as the unit distance. Using these values, you can calculate the area of the chip. As shown in the previous line, the unit distance is in microns, and 1000 unit distances are equal to 1 micron.

> Die width in unit distance = 660685 - 0 = 660685

> Die higth in unit distance = 671405 - 0 = 671405

> Die width in microns = 660685/1000 = 660.685 microns

> Die higth in microns = 671405/1000 = 671.405 microns

> Die area = die width*die higth = 443587.212425 square microns

To see how the layout looks after the floorplan;

```sh
# First go to floorplan directory
> cd Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/Day-Month_Hour-Minute/results/floorplan/
# Then by using magic tool open layout
> magic -T ~/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.floorplan.def &
``` 
You can see the floorplan layout below. To center the design, first press the 's' key, then the 'v' key.


![](https://github.com/emrullahceyhan/NASSCOM-SOC/blob/development/Pictures/9.png?raw=true)


'z' for zoomin 'Z' for zoomout. One of the pinouts is shown below.

![](https://github.com/emrullahceyhan/NASSCOM-SOC/blob/development/Pictures/10.png?raw=true)

If you hover the mouse over the pin and press 's', you will have selected it. Then, if you type 'what' in the `tkcon` window, you can see the layout and label information of the selected unit.

![](https://github.com/emrullahceyhan/NASSCOM-SOC/blob/development/Pictures/11.png?raw=true)

### Section 2 - Placement

```sh
# Global Placement by defaults
> run_placement
``` 
The terminal output of the placement result is shown below.

![](https://github.com/emrullahceyhan/NASSCOM-SOC/blob/development/Pictures/12.png?raw=true)

To view the placement result using the Magic tool, enter the following command:

```sh
# First go to placement directory
> cd Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/Day-Month_Hour-Minute/results/placement/

# Then by using magic tool open layout
> magic -T ~/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.placement.def &
``` 
The placement result will appear like this.

![](https://github.com/emrullahceyhan/NASSCOM-SOC/blob/development/Pictures/13.png?raw=true)

When you zoom in on a specific area, the cells can be seen in their legacy placed state.

![](https://github.com/emrullahceyhan/NASSCOM-SOC/blob/development/Pictures/14.png?raw=true)

## Day 3 -  Design library cell using Magic Layout and ngspice characterization

In the first step, we need to download a cell repository from GitHub.

```sh
# go to openlane directory
> cd Desktop/work/tools/openlane_working_dir/openlane

# clone the repo
> git clone https://github.com/nickson-jose/vsdstdcelldesig

# enter in the'vsdstdcelldesign'
> cd vsdstdcelldesign

# To easily access the sky130A.tec file, copy it into the repository you downloaded using the following command:
> cp ~/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech .

# Then, use the Magic tool to open the inverter layout you downloaded.
> magic -T sky130A.tech sky130_inv.mag &
``` 
You can see the custom inverter layout in the image below.

![](https://github.com/emrullahceyhan/NASSCOM-SOC/blob/development/Pictures/15.png?raw=true)

Move the cursor to the area where the polysilicon intersects at the bottom of the layout, press 's' to select it, and then type 'what' in the tkcon window. You will see that it is an N-MOS.

![](https://github.com/emrullahceyhan/NASSCOM-SOC/blob/development/Pictures/16.png?raw=true)

When you apply the same process to the top section, you will see that it is a P-MOS.

![](https://github.com/emrullahceyhan/NASSCOM-SOC/blob/development/Pictures/17.png?raw=true)

If you double-click on 's', the connection of the layer selected by the cursor will be highlighted. Below, you can see the connection of the output.

![](https://github.com/emrullahceyhan/NASSCOM-SOC/blob/development/Pictures/18.png?raw=true)

When there is an error in the layout, you can see it in the DRC section at the top of the Magic tool. Below, we deleted part of the polysilicon, and DRC=3, meaning there are 3 errors.

![](https://github.com/emrullahceyhan/NASSCOM-SOC/blob/development/Pictures/19.png?raw=true)

To observe the functionality of the designed layout, a simulation can be performed using ngspice. To do this, we first need to convert the design into a format that ngspice can run. For this, execute the following commands in the tkcon window:

```sh
# Extract the design in .ext format
> extract all

# Activate parasitic extractions before converting ext to spice
> ext2spice cthresh 0 rthresh 0

# Then convert the ext format to spice format
> ext2spice
``` 
After these steps, a sky130_inv.spice file will be generated in your current directory.

Some changes need to be made in this file. First, we need to include the pshort.lib and nshort.lib files under the libs folder. Then, the model definitions need to be changed as shown below. In this file, only definitions for pmos and nmos are available. For simulation, we need to connect input Vdd and Vss. You can do this as shown below. Finally, a pulse generator should be connected to the input, i.e., the Va section, as shown in the code below.

![](https://github.com/emrullahceyhan/NASSCOM-SOC/blob/development/Pictures/20.png?raw=true)

You can perform the ngspice simulation using the following command:
```sh
# Run following for ngspice simulation
> ngspice sky130_inv.spice
``` 

The terminal output for this command will look like the one below.

![](https://github.com/emrullahceyhan/NASSCOM-SOC/blob/development/Pictures/21.png?raw=true)

Later, to see the input vs output graph within ngspice, you can run the following command.

```sh
# Too see out vs in graph with respected to the time
> plot y vs time a
``` 
The graph output will look like this:

![](https://github.com/emrullahceyhan/NASSCOM-SOC/blob/development/Pictures/22.png?raw=true)

You can measure some parameters from this graph.

To measure the rise time, we need to measure the time between 20% and 80% of the power. 20% of the power corresponds to 0.66V.

![](https://github.com/emrullahceyhan/NASSCOM-SOC/blob/development/Pictures/23.png?raw=true)

In the image above, the time measurement at 20% power is 2.17984e-09s.

When the output power is 80%, i.e., at 2.64V, the image is as follows:

![](https://github.com/emrullahceyhan/NASSCOM-SOC/blob/development/Pictures/24.png?raw=true)

As shown in the image, the time measurement at 2.64V is 2.23949e-09s.

The difference between the two measurements gives the rise time value.

> 2.23949e-09s - 2.17984e-09s = 0.05965e-09s rise time değeridir.


We can measure the fall time using the same method. This time, we need to take the measurement at the falling edge, where the output value falls from logic 1 to logic 0. Below, the time values at 80% and 20% power at the falling edge are shown in the terminal.

![](https://github.com/emrullahceyhan/NASSCOM-SOC/blob/development/Pictures/25.png?raw=true)


In the terminal, as seen, the 80% power value is:
> x0 = 4.05054e-09s, y0 = 2.64V

%20 
> x0 = 4.09254e-09s, y0 = 0.6V

> Fall time 4.09254e-09s - 4.05054e-09s =  0.042e-09s

For measuring the cell rise delay time, i.e., propagation delay, we need to observe the input to output delay at 50% of the output rise edge, i.e., at 1.65V. You can see these two values in the terminal screen.

![](https://github.com/emrullahceyhan/NASSCOM-SOC/blob/development/Pictures/26.png?raw=true)


The difference between these two values gives the propagation delay.

> 2.20699e-09s - 2.15e-09s = 0.05699e-09s propagation delay.

If we use the same method and take the measurements at the output fall edge, we can find the cell fall delay value.

![](https://github.com/emrullahceyhan/NASSCOM-SOC/blob/development/Pictures/27.png?raw=true)

We can calculate it using the values in the terminal output above.

> 4.07512e-09s - 4.05e-09s = 0.02512e-09s cell fall delay.

### SkyWater DRC Rules

Link for SkyWater DRC rules : https://skywater-pdk.readthedocs.io/en/main/rules/periphery.html


```sh
# Go to home directory
> cd

# Download SkyWater lab files
> wget http://opencircuitdesign.com/open_pdks/archive/drc_tests.tgz

# Extract the files
> tar xfz drc_tests.tgz

# go to inside labs
> cd drc_tests

# Open any of mag file to see layout
> magic -d XR & 
``` 
Below, you can see example labs.

![](https://github.com/emrullahceyhan/NASSCOM-SOC/blob/development/Pictures/28.png?raw=true)

After opening the Magic tool, you can open any layout by going to File > Open. Below is a screenshot of Magic for Metal3. Each design shows which rule corresponds to the layout. You can find the explanations for each rule in the link I added above.

![](https://github.com/emrullahceyhan/NASSCOM-SOC/blob/development/Pictures/29.png?raw=true)


#### Fixing Poly DRC

First, open the poly.mag layout using the load poly command in the tkcon window. Below is the relevant layout.

![](https://github.com/emrullahceyhan/NASSCOM-SOC/blob/development/Pictures/30.png?raw=true)

The selected box dimensions are shown below in the tkcon screen.

![](https://github.com/emrullahceyhan/NASSCOM-SOC/blob/development/Pictures/32.png?raw=true)

Looking at the poly.9 rule from the SkyWater DRC rules, the rule is:

Poly resistor spacing to poly or spacing (no overlap) to diff/tap = 0.480 µm.

![](https://github.com/emrullahceyhan/NASSCOM-SOC/blob/development/Pictures/31.png?raw=true)

To fix this error, we need to make some modifications on the sky130A.tech file. The changes made are shown in the images below.

![](https://github.com/emrullahceyhan/NASSCOM-SOC/blob/development/Pictures/33.png?raw=true)

![](https://github.com/emrullahceyhan/NASSCOM-SOC/blob/development/Pictures/34.png?raw=true)

Then, by following the commands below, we can observe the effect of the changes we made. Enter these commands in the tkcon screen.

```sh
# load sky130.tech file
> tech load sky130A.tech
> drc check
> drc why
``` 
As shown, the DRC issue in the related part has been resolved.

![](https://github.com/emrullahceyhan/NASSCOM-SOC/blob/development/Pictures/35.png?raw=true)

#### Fixing nwell.4 DRC

Since no tap presents in nwell it is implemented incorrectly.

![](https://github.com/emrullahceyhan/NASSCOM-SOC/blob/development/Pictures/36.png?raw=true)

As shown below, we add a new rule to the sky130A.tech file.

![](https://github.com/emrullahceyhan/NASSCOM-SOC/blob/development/Pictures/37.png?raw=true)

![](https://github.com/emrullahceyhan/NASSCOM-SOC/blob/development/Pictures/38.png?raw=true)

By following the commands below, check if the error has been fixed:
```sh
# load sky130.tech file
> tech load sky130A.tech
# drc full check modunu aktifleştir
> drc style drc(full)
# check drc
> drc check
> drc why
``` 
![](https://github.com/emrullahceyhan/NASSCOM-SOC/blob/development/Pictures/39.png?raw=true)

## Day 4 - Prelayout Timing Analaysis and Importance of Good Clock Tree

#### Verifying Custom Inverter Layout Design

First check whether IO's are located on the intersecton of vertical and horizontal tracks. There is an `tracks.info` file under `/home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/openlane/sky130_fd_sc_hd`. In this file width and pitch information of the all metals both in x and y direction. 

This is what tracks.info includes.

![](https://github.com/emrullahceyhan/NASSCOM-SOC/blob/development/Pictures/40.png?raw=true)

To make it easier to check the intersection for li1, we can change the grid sizes in the Magic tool to the values written for li1.

```sh
# First open inverter design
> magic -T sky130A.tech sky130_inv.mag &

# Then change grid size by using tckon screen
> grid 0.46um 0.34um 0.23um 0.17um
```
We can see that input and output intersect with vertical and horizontal tracks.

![](https://github.com/emrullahceyhan/NASSCOM-SOC/blob/development/Pictures/41.png?raw=true)

Second rule we need to check is that the width of transistor must be odd multiple of x pitch. As we know from tracks.info file the x witch is 0.46um.

![](https://github.com/emrullahceyhan/NASSCOM-SOC/blob/development/Pictures/42.png?raw=true)

As we can see the above picture the width of transistor is 1.38um so;
> widht = 1.38um = 0.46um * 3 

This proves the second condition.

Same condition is for heigth of standart cell.

![](https://github.com/emrullahceyhan/NASSCOM-SOC/blob/development/Pictures/43.png?raw=true)

> Heigth of cell 2.72um = 0.34um * 8

Now that the checks are complete, we can say that our design is ready. Let's first save the design from the tkcon screen.

```sh
# save design with different name
> save sky130_vsdinv.mag

# Then exit from design
> exit

# Open new design
> magic -T sky130A.tech sky130_vsdinv.mag &

# Write lef file
> lef write
```
Below is the new design.

![](https://github.com/emrullahceyhan/NASSCOM-SOC/blob/development/Pictures/44.png?raw=true)


This is new generated lef file.

![](https://github.com/emrullahceyhan/NASSCOM-SOC/blob/development/Pictures/45.png?raw=true)

To integrate the inverter design into the picorv32a, first follow the steps below.

```sh
# Copy lef file under src of picorv32a
> cp sky130_vsdinv.lef ~/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/src/
# Copy typical, slow and fast libs under src
> cp libs/sky130_fd_sc_hd__* ~/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/src/
```
After copying the necessary files, add the following code to the config.tcl file located under /home/vsduser/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a.

```sh
set ::env(LIB_SYNTH) "$::env(OPENLANE_ROOT)/designs/picorv32a/src/sky130_fd_sc_hd__typical.lib"
set ::env(LIB_FASTEST) "$::env(OPENLANE_ROOT)/designs/picorv32a/src/sky130_fd_sc_hd__fast.lib"
set ::env(LIB_SLOWEST) "$::env(OPENLANE_ROOT)/designs/picorv32a/src/sky130_fd_sc_hd__slow.lib"
set ::env(LIB_TYPICAL) "$::env(OPENLANE_ROOT)/designs/picorv32a/src/sky130_fd_sc_hd__typical.lib"

set ::env(EXTRA_LEFS) [glob $::env(OPENLANE_ROOT)/designs/$::env(DESIGN_NAME)/src/*.lef]
```
Below is the screenshot of the config.tcl file.

![](https://github.com/emrullahceyhan/NASSCOM-SOC/blob/development/Pictures/46.png?raw=true)

To add the custom inverter design to the OpenLane flow, follow the steps below.

```sh
# Go to this directory
> cd Desktop/work/tools/openlane_working_dir/openlane

# open docker
> docker

# Enter OpenLane flow with interactive mode
> ./flow.tcl -interactive

# Enter require package
> package require openlane 0.9

# Initialize design
> prep -design picorv32a

# Use this commands for include new added lef file
> set lefs [glob $::env(DESIGN_DIR)/src/*.lef]
> add_lefs -src $lefs

# run synthesis
> run_synthesis
```

This is the synthesis result of design.

![](https://github.com/emrullahceyhan/NASSCOM-SOC/blob/development/Pictures/47.png?raw=true)

As seen in the image, there are timing violations. To reduce them, we will use the following strategies:

```sh
# prep design again and overwrite you last work
> prep -design picorv32a 19-05_20-01 -overwrite

# Use this commands for include new added lef file
> set lefs [glob $::env(DESIGN_DIR)/src/*.lef]
> add_lefs -src $lefs

# synth strateji more tends to improve time. if it is 3, it will be more tend to reduce area. 
> set ::env(SYNTH_STRATEGY) "DELAY 3"
> set ::env(SYNTH_SIZING) 1
# then run synthesis again
> run_synthesis
```

Now as we see WNS and TNS is 0s.

![](https://github.com/emrullahceyhan/NASSCOM-SOC/blob/development/Pictures/48.png?raw=true)

Then run floorplan.

```sh
# Now we can run floorplan
> run_floorplan
```
After run the command an error is occured.

![](https://github.com/emrullahceyhan/NASSCOM-SOC/blob/development/Pictures/49.png?raw=true)

To fix this, apply the following steps:

```sh
> init_floorplan
> place_io
> tap_decap_or
```
Then run placement
```sh
> run_placement
```
Below is the terminal output for placement results.

![](https://github.com/emrullahceyhan/NASSCOM-SOC/blob/development/Pictures/50.png?raw=true)

To view the placement results, follow these steps:

```sh 
# Go to placement results directory
> cd /home/vsduser/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/19-05_21-10/results/placement

# Open layout with magic tool
> magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.placement.def &
```

The placement result is shown below. As seen in the image, our custom inverter block has been placed.

![](https://github.com/emrullahceyhan/NASSCOM-SOC/blob/development/Pictures/51.png?raw=true)

If you add the following command to the tckon screen, the metal layer layout will be visible:

```sh
# metal layer view
> expand
```
![](https://github.com/emrullahceyhan/NASSCOM-SOC/blob/development/Pictures/52.png?raw=true)


#### Post Synthesis Timing
In this step, we will make changes to the base.sdc file to improve WNS and TNS. Since we previously had 0s WNS, let’s revert our strategies and introduce negative slack again.

```sh
# Go to this directory
> cd Desktop/work/tools/openlane_working_dir/openlane

# open docker
> docker

# Enter OpenLane flow with interactive mode
> ./flow.tcl -interactive

# Enter require package
> package require openlane 0.9

# Initialize design
> prep -design picorv32a

# Use this commands for include new added lef file
> set lefs [glob $::env(DESIGN_DIR)/src/*.lef]
> add_lefs -src $lefs

# run synthesis
> run_synthesis
```
As seen in the terminal, the negative slack error still exists.
![](https://github.com/emrullahceyhan/NASSCOM-SOC/blob/development/Pictures/53.png?raw=true)

First, we limit the fanout and run synthesis again.

```sh
# Go to this directory
> cd Desktop/work/tools/openlane_working_dir/openlane

# open docker
> docker

# Enter OpenLane flow with interactive mode
> ./flow.tcl -interactive

# Enter require package
> package require openlane 0.9

# Initialize design
> prep -design picorv32a

# Use this commands for include new added lef file
> set lefs [glob $::env(DESIGN_DIR)/src/*.lef]
> add_lefs -src $lefs
> set ::env(SYNTH_SIZING) 1

# Limit fanout
> set ::env(SYNTH_MAX_FANOUT) 4

# run synthesis
> run_synthesis
```
![](https://github.com/emrullahceyhan/NASSCOM-SOC/blob/development/Pictures/54.png?raw=true)

As seen in the output, the timing issue persists.

Let’s check the timing details in a separate terminal:

```sh
> cd Desktop/work/tools/openlane_working_dir/openlane

> sta pre_sta.conf
```
![](https://github.com/emrullahceyhan/NASSCOM-SOC/blob/development/Pictures/55.png?raw=true)

Here, there is an OR gate with 4 fanouts. Let’s increase its drive strength value from 2 to 4 and try again.

```sh
> report_net -connections _10566_
> replace_cell _13165_ sky130_fd_sc_hd__or3_4
> report_checks -fields {net cap slew input_pins} -digits 4
```
With this fix, a decrease in slack can be seen.

![](https://github.com/emrullahceyhan/NASSCOM-SOC/blob/development/Pictures/56.png?raw=true)

We will apply the same fix to another gate.

![](https://github.com/emrullahceyhan/NASSCOM-SOC/blob/development/Pictures/57.png?raw=true)

```sh
> report_net -connections _10534_
> replace_cell _13132_ sky130_fd_sc_hd__or4_4
> report_checks -fields {net cap slew input_pins} -digits 4
```
As seen in the terminal, the slack value has dropped again.

![](https://github.com/emrullahceyhan/NASSCOM-SOC/blob/development/Pictures/58.png?raw=true)

Now, we need to incorporate this updated netlist into our flow. We should take a backup and overwrite it using the write_verilog command.

```sh
# Go to synthesis result directory
> cd /home/vsduser/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/19-05_23-26/results/synthesis

# Take a copy
> cp picorv32a.synthesis.v picorv32a.synthesis_old.v
```

Then, run the following command in STA:
```sh
# Overwriting current synthesis netlist
> write_verilog /home/vsduser/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/19-05_23-26/results/synthesis/picorv32a.synthesis.v
```
![](https://github.com/emrullahceyhan/NASSCOM-SOC/blob/development/Pictures/62.png?raw=true)

```sh
# synthesis
> run_synthesis
```

![](https://github.com/emrullahceyhan/NASSCOM-SOC/blob/development/Pictures/59.png?raw=true)

```sh
# Commands for floorplan
> init_floorplan
> place_io
> tap_decap_or

# placment step
> run_placement
```

![](https://github.com/emrullahceyhan/NASSCOM-SOC/blob/development/Pictures/60.png?raw=true)

```sh
# Then run cts
> run_cts
```

![](https://github.com/emrullahceyhan/NASSCOM-SOC/blob/development/Pictures/61.png?raw=true)

In this step, we will use OpenRoad for post-CTS analysis. Let’s invoke OpenRoad in the OpenLane terminal.

```sh
# Invoke OpenRoad
> openroad

# Reading lef file
> read_lef /openLANE_flow/designs/picorv32a/runs/19-05_23-26/tmp/merged.lef

# Reading def file
> read_def /openLANE_flow/designs/picorv32a/runs/19-05_23-26/results/cts/picorv32a.cts.def

# Create OpenRoad Database
> write_db pico_cts.db

# Read created database
> read_db pico_cts.db

# Read netlist post CTS
> read_verilog /openLANE_flow/designs/picorv32a/runs/19-05_23-26/results/synthesis/picorv32a.synthesis_cts.v

# Read library for design
> read_liberty $::env(LIB_SYNTH_COMPLETE)

# Read in the custom sdc we created
> read_sdc /openLANE_flow/designs/picorv32a/src/my_base.sdc

# Setting all cloks as propagated clocks
> set_propagated_clock [all_clocks]

# Generate timing report
> report_checks -path_delay min_max -format full_clock_expanded -digits 4
```
![](https://github.com/emrullahceyhan/NASSCOM-SOC/blob/development/Pictures/63.png?raw=true)

As seen in the report above, the slack value is 14.1462ns.

In this step, we will remove the 'sky130_fd_sc_hd__clkbuf_1 cell from the clock buffer list and check the post-CTS results.
```sh
# Remove 'sky130_fd_sc_hd__clkbuf_1' from the list
> set ::env(CTS_CLK_BUFFER_LIST) [lreplace $::env(CTS_CLK_BUFFER_LIST) 0 0

# Setting def as placement def
> set ::env(CURRENT_DEF) /openLANE_flow/designs/picorv32a/runs/19-05_23-26/results/placement/picorv32a.placement.def

# Then run_cts
> run_cts

# Run OpenROAD tool
> openroad

# Reading lef file
> read_lef /openLANE_flow/designs/picorv32a/runs/19-05_23-26/tmp/merged.lef

# Reading def file
> read_def /openLANE_flow/designs/picorv32a/runs/19-05_23-26/results/cts/picorv32a.cts.def

# Create database with custom name
> write_db pico_cts1.db

# Loading the created database
> read_db pico_cts1.db

# Read netlist post CTS
> read_verilog /openLANE_flow/designs/picorv32a/runs/19-05_23-26/results/synthesis/picorv32a.synthesis_cts.v

# Read library for design
> read_liberty $::env(LIB_SYNTH_COMPLETE)

# Link design
> link_design picorv32a

# Read custom sdc
> read_sdc /openLANE_flow/designs/picorv32a/src/my_base.sdc

# Setting all cloks as propagated clocks
> set_propagated_clock [all_clocks]

# Generating custom timing report
> report_checks -path_delay min_max -fields {slew trans net cap input_pins} -format full_clock_expanded -digits 4

# Report hold skew
> report_clock_skew -hold

# Report setup skew
> report_clock_skew -setup

# Exit to OpenLANE flow
> exit

# Inserting 'sky130_fd_sc_hd__clkbuf_1' to first index of list
> set ::env(CTS_CLK_BUFFER_LIST) [linsert $::env(CTS_CLK_BUFFER_LIST) 0 sky130_fd_sc_hd__clkbuf_1]
```

# Day 5 - Final steps for RTL2GDS using tritonRoute and openSTA 

Execute Power Distribution Network (PDN) and explore the layout.

```sh
# Go to this directory
> cd Desktop/work/tools/openlane_working_dir/openlane

# open docker
> docker

# Enter OpenLane flow with interactive mode
> ./flow.tcl -interactive

# Enter require package
> package require openlane 0.9

# Initialize design
> prep -design picorv32a

# Use this commands for include new added lef file
> set lefs [glob $::env(DESIGN_DIR)/src/*.lef]
> add_lefs -src $lefs

# Command to set new value for SYNTH_STRATEGY
> set ::env(SYNTH_STRATEGY) "DELAY 3"

# Command to set new value for SYNTH_SIZING
> set ::env(SYNTH_SIZING) 1

# run synthesis
> run_synthesis

# Commands for floorplan
> init_floorplan
> place_io
> tap_decap_or

# placment step
> run_placement

# Then run_cts
> run_cts

# Finally generate pdn
> gen_pdn 
```

After these steps, the terminal output will look like this:

![](https://github.com/emrullahceyhan/NASSCOM-SOC/blob/development/Pictures/64.png?raw=true)

To view the layout after PDN, follow these steps:

```sh
> cd Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/20-05_20-50/tmp/floorplan

> magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read 14-pdn.def &
```
Screenshot of PDN

![](https://github.com/emrullahceyhan/NASSCOM-SOC/blob/development/Pictures/65.png?raw=true)

Then run routing.

```sh
> run_routing
```

You can see the routing result in the terminal:

![](https://github.com/emrullahceyhan/NASSCOM-SOC/blob/development/Pictures/66.png?raw=true)

To view the design after routing, follow these steps:
```sh
> cd /home/vsduser/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/20-05_20-50/results/routing

> magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.def &
```
Routing screenshots are shown below.

![](https://github.com/emrullahceyhan/NASSCOM-SOC/blob/development/Pictures/67.png?raw=true)

![](https://github.com/emrullahceyhan/NASSCOM-SOC/blob/development/Pictures/68.png?raw=true)


Finally, we will perform post-route OpenSTA timing analysis with the extracted parasitics. Follow these steps:

```sh
# openroad
> openroad

# Reading lef file
> read_lef /openLANE_flow/designs/picorv32a/runs/20-05_20-50/tmp/merged.lef

# Reading def file
> read_def /openLANE_flow/designs/picorv32a/runs/20-05_20-50/results/routing/picorv32a.def

# Create database with custom name
> write_db pico_route.db

# Loading the created database
> read_db pico_route.db

# Read netlist
> read_verilog /openLANE_flow/designs/picorv32a/runs/20-05_20-50/results/synthesis/picorv32a.synthesis_preroute.v

# Read library for design
> read_liberty $::env(LIB_SYNTH_COMPLETE)

# Link design and library
> link_design picorv32a

# Read custom sdc
> read_sdc /openLANE_flow/designs/picorv32a/src/my_base.sdc

# Setting all cloks as propagated clocks
> set_propagated_clock [all_clocks]

> read_spef /openLANE_flow/designs/picorv32a/runs/20-05_20-50/results/routing/picorv32a.spef

> report_checks -path_delay min_max -fields {slew trans net cap input_pins} -format full_clock_expanded -digits 4
```

After parasitic extraction, the slack improved from 14.1462ns to 13.9090ns. Below is the terminal output:

![](https://github.com/emrullahceyhan/NASSCOM-SOC/blob/development/Pictures/69.png?raw=true)