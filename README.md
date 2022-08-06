# Sky130-PD-WorkShop



<aside>
⚙️Report from Jhon Pinto for the SkyWater130 Physical Design Workshop held in Aug 2 - 6

</aside>

## Day 1

### Starting OpenLANE

For calling the tool, and put all the packages required for the flow, we run **inside the openlane folder** the following commands:

```python
docker
./flow.tcl -interactive 
package require openlane 0.9
```

![Untitled](Day%201%20649c37faeca741a4aa844421c98653ef/Untitled.png)

### Basic folders and files

For this example, there will be used the design picorv32a. It can be found in the route openlane/designs/picorv32a

![Untitled](Day%201%20649c37faeca741a4aa844421c98653ef/Untitled%201.png)

- The src file is where all the Verilog files of our circuit will be stored

![Untitled](Day%201%20649c37faeca741a4aa844421c98653ef/Untitled%202.png)

- The config.tcl bypasses any configurations that has been already done

![Untitled](Day%201%20649c37faeca741a4aa844421c98653ef/Untitled%203.png)

<aside>
📌 The other .tcl files are the more detailed configuration files and has higher priority. We can’t run the flow without config.tcl

</aside>

### Preparing the design

→ Now, we need to set a design to the flow in openlane, in this case, picorv32a

```python
prep -design picorv32a
```

![Untitled](Day%201%20649c37faeca741a4aa844421c98653ef/Untitled%204.png)

- After that, a folder of the synthesized design will be created in the runs folder

![Untitled](Day%201%20649c37faeca741a4aa844421c98653ef/Untitled%205.png)

![Untitled](Day%201%20649c37faeca741a4aa844421c98653ef/Untitled%206.png)

<aside>
📌 the merged.lef file in tmp contains the needed information about the .tech and .ref. Also the parameters being taken by the run can be found in the config.tcl file in the synthesis/reports folder.

</aside>

### Running Synthesis & ABC

```python
run_synthesis
```

At the end of the process, we should get the message:

![Untitled](Day%201%20649c37faeca741a4aa844421c98653ef/Untitled%207.png)

### Characterize synthesis results

After synthesizing, we can get the results as for example, the area occupied by the chip:

![Untitled](Day%201%20649c37faeca741a4aa844421c98653ef/Untitled%208.png)

Also, we can find the total number of gates that the desing will use

![Untitled](Day%201%20649c37faeca741a4aa844421c98653ef/Untitled%209.png)

<aside>
📌 This report can be later be found in the .stat.rpt file located in reports/synthesis

![Untitled](Day%201%20649c37faeca741a4aa844421c98653ef/Untitled%2010.png)

</aside>

### FF Ratio

The **Flop Ratio** is the portions of D flip flops respect to the total number of cells. It can be calculated as:

$$
ffRatio = \frac{NumberOfFlipFlops}{NumberOfCells}
$$

For this example, taking the total number of cells (14876) and the total number of d flip flops (1613), we calculate the flop ratio as:

$$
ffRatio = \frac{1613}{14876} = 0,1084
$$


## Day 2

<aside>
📌 After making floorplanning, we first need to do the synthesis

</aside>

### Setting the “switches”

The first thing to do is to set the “switches”. Those switches are located in openlane/configuration. 

- You can find in the README file all the information about the variables needed for each step

![Untitled](Day%202%20ec8488d7a7634fb4b1164e6d91da0f45/Untitled.png)

- For setting those switches we to to the .tcl files in openlane/configuration

![Untitled](Day%202%20ec8488d7a7634fb4b1164e6d91da0f45/Untitled%201.png)

![floorplan.tcl file](Day%202%20ec8488d7a7634fb4b1164e6d91da0f45/Untitled%202.png)

floorplan.tcl file

<aside>
📌 in the OpenLANE flow, the vertical metal and the horizontal metal is one more than what is  specified. E.g If specified metal 4 it will be metal 5

</aside>

Those .tcl config files have parameters in common with other .tcl files. For that, OpenLANE has a series of priorities for each file. This order consists of (From higher to lower):

1. The file sky130A….._config.tcl inside of any design
2. the config.tcl of any design
3. System defaults (floorplan.tcl)

### Running floorplan

```python
run_floorplan
```

At the end of the process, you should get the following messages:

![Untitled](Day%202%20ec8488d7a7634fb4b1164e6d91da0f45/Untitled%203.png)

### Review floorplan files

- If we go to runs/X config.tcl we can find **all the parameters** that openlane used in the flow (including the “switches” mentioned before)

![Untitled](Day%202%20ec8488d7a7634fb4b1164e6d91da0f45/Untitled%204.png)

- To see relevant information about the floorplanning we go to results/floorplan and then open the .def file. In this file, we can obtain the chip area
    
    ![Untitled](Day%202%20ec8488d7a7634fb4b1164e6d91da0f45/Untitled%205.png)
    

<aside>
📌 The units for the area used aren’t micros. It is necessary to perform a conversion from units to microns.

</aside>

### Floorplan in Magic

To open magic we use the following command in the /results/floorplan folder:

```python
magic -T /home/jhonstevenpintoh/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech leaf read ../../tmp/merged.lef def read picorv32a.floorplan.def &
```

Then we can see the resulting floorplanning in magic:

![Untitled](Day%202%20ec8488d7a7634fb4b1164e6d91da0f45/Untitled%206.png)

### Placement using RePlAce

<aside>
📌 OpenLane uses HPWL to do Placement.

</aside>

<aside>
📌 Placement goes in two stages: the global placement and the detailed placement. Legalization occurs in Detail placement. Legality is the rules used to place STD cells. For example, there should be no overlaps.

</aside>

- After obtaining the floorplan we run placement:

```python
run_placement
```

After the run, a placement analysis is obtained and we can see that the placement is legalized.

![Untitled](Day%202%20ec8488d7a7634fb4b1164e6d91da0f45/Untitled%207.png)

- Now, to see how our design is after placement, we go to the folder results/placement and run the following command:

```python
magic -T /home/jhonstevenpintoh/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech leaf read ../../tmp/merged.lef def read picorv32a.placement.def &
```

Here we can see that the power network was made in this step, not in the floorplan stage

![Untitled](Day%202%20ec8488d7a7634fb4b1164e6d91da0f45/Untitled%208.png)


## Day 3

### Making changes

There is an option to make changes on some steps on the flow. For this example, we are going to change the I/O pins distribution in the floorplan.

For doing that we go to openlane/configuration and we open the floorplan.tcl to check the variables involved in the config. After selecting the variable we are going to set, we run:

```python
set ::env(FP_IO_MODE) 2
run_floorplan
```

### Cloning vsd std cell design

For cloning the repo for designing our std cell, we run in the openlane folder:

```python
git clone https://github.com/nickson-jose/vsdstdcelldesign
```

Now, we need to paste the tech file from magic. That file is located in openlane_working_dir/pdks/sky130A/libs.tech/magic. We need to copy the [sky130A.tech](http://sky130A.tech) file

```python
cp sky130A.tech [Location]
```

![Untitled](Day%203%203ed60a259c514fdebb0441860a52f150/Untitled.png)

Now, to see the std cell in magic, we launch the following command:

```python
magic -T sky130A.tech sky130_inv.mag &
```

![Untitled](Day%203%203ed60a259c514fdebb0441860a52f150/Untitled%201.png)

<aside>
📌 A std cell in LEF format contains information about available metal layer.

</aside>

### Layers in Magic

The layers are distributed at the right side. If we select a portion and write “and” on the terminal, we can see the material of a determined layer

![Untitled](Day%203%203ed60a259c514fdebb0441860a52f150/Untitled%202.png)

### DRC errors

When violating the DRC errors from the pdk, the layer will be highlighted in white

![Untitled](Day%203%203ed60a259c514fdebb0441860a52f150/Untitled%203.png)

If we would want to know what DRC rule was violate, go to the option DRC → Find next error and check de terminal for the reason of the error

![Untitled](Day%203%203ed60a259c514fdebb0441860a52f150/Untitled%204.png)

### Extracting the Spice circuit

<aside>
📌 All the commands will be executed on the magic terminal

</aside>

First, we need to create an extraction file (EHD file)

```python
extract all
```

Now, we need to take that extracted file an take it into spice. Run the command to obtain parasitic capacitances:

```python
ext2spice cthresh 0 rthresh 0
ext2spice
```

Now, inside this folder we open the spice file with VIM

![Untitled](Day%203%203ed60a259c514fdebb0441860a52f150/Untitled%205.png)

![Untitled](Day%203%203ed60a259c514fdebb0441860a52f150/Untitled%206.png)

### Check the scale

With the minimum distance set by the magic layout, check the option of scale. If its not the same, change it

![Untitled](Day%203%203ed60a259c514fdebb0441860a52f150/Untitled%207.png)

![Untitled](Day%203%203ed60a259c514fdebb0441860a52f150/Untitled%208.png)

### Defining Va and Vdd + Fixing errors

<aside>
📌 Esc+:wq! for getting out of VIM

</aside>

![Untitled](Day%203%203ed60a259c514fdebb0441860a52f150/Untitled%209.png)

### Simulating in ngspice

In the vds…design folder, execute:

```python
ngspice sky130_inv.spice
```

Obtaining the following results:

![Untitled](Day%203%203ed60a259c514fdebb0441860a52f150/Untitled%2010.png)

### Plot in ngspice

We run into ngspice: (where a means the input)

```python
plot y vs time a
```

![Untitled](Day%203%203ed60a259c514fdebb0441860a52f150/Untitled%2011.png)

### Characterizing the cell

For characterizing, it means to find 4 parameters:

- Rise transition / Fall transition
- Rise cell delay / Fall cell delay

![Untitled](Day%203%203ed60a259c514fdebb0441860a52f150/Untitled%2012.png)

$$
RiseCellDelay = 4.0528ns-4.05ns = 2.82ps
$$


## Day 4
