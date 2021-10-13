# OpenLane Projects
This repo consists of my projects related to OpenLane

## Running OpenLane flow on new designs

### Pre-requisites
The OpenLane flow setup must be completed before following these steps.
The steps can be found [here](https://github.com/The-OpenROAD-Project/OpenLane).

### Steps to run the flow on new designs

**1. Start the docker**
```console
sudo make mount
```
**2. Create a new design folder**
```console
./flow.tcl -design my_design -init_design_config
```

#### Expected Output
```console
[INFO]: 
	 ___   ____   ___  ____   _       ____  ____     ___
	/   \ |    \ /  _]|    \ | |     /    ||    \   /  _]
	|   | |  o  )  [_ |  _  || |    |  o  ||  _  | /  [_
	| O | |   _/    _]|  |  || |___ |     ||  |  ||    _]
	|   | |  | |   [_ |  |  ||     ||  _  ||  |  ||   [_
	\___/ |__| |_____||__|__||_____||__|__||__|__||_____|


[INFO]: Version: 2021.09.30_02.12.16
[INFO]: Running non-interactively
[INFO]: Creating design src directory /openLANE_flow/designs/my_design/src
[INFO]: Populating /openLANE_flow/designs/my_design/config.tcl..
[INFO]: Finished populating:
/openLANE_flow/designs/my_design/config.tcl 
Please modify CLOCK_PORT, CLOCK_PERIOD and add your advanced settings to /openLANE_flow/designs/my_design/config.tcl
[SUCCESS]: Done...
```

**3. Add verilog source**

Open another terminal and navigate to `OpenLane/designs/<design_name>/src` path. 
Add the verilog source code here. The method I use is as follows:
```console
sudo vim design.v
```
To start editing the file, press i key in vim. After editing is complete, hit esc and then :wq to save and exit.
The newly opened terminal can now be closed. 

Confirm the verilog code by using the following command in the previous terminal with docker running. Navigate to `designs/<design_name>/src`, then run the cmd:
```console
cat <design_name>.v
```
**Note: The new design name and the module name in verilog code must match.**

**4. Running the OpenLane flow**

The next step is to run the OpenLane flow on the new design by running the following command:

**Note: Before running this, ensure that the path is `/openLANE_flow`. This can be verified with `pwd` command.**

```console
./flow.tcl -design <design_name>
```

**5. Edit the config.tcl file if required.**

This step is needed only if the flow exits with errors. One such example is inverter. If the verilog code is only for an inverter, the generic config.tcl will result in errors. Please check the existing examples to understand how to edit the config.tcl based on project requirements.

**6. Looking at the standard cells used for the design.**

The standard cells used for the design can be viewed by navigating to `designs/<design_name>/runs/<run_name>/tmp/synthesis`.
Use xdot to view the post_techmap.dot file.
```console
xdot post_techmap.dot
```
The synthesized verilog code can be found in this path: `designs/<design_name>/runs/<run_name>/results/synthesis/<name>.synthesis.v`.
This file also shows the standard cells mapped to the design.
