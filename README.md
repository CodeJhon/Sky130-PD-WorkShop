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

config.tcl bypasses any configurations that has been already done 

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



