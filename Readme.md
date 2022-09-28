
# Pulse Width Modulation signal Generator
This project simulates a variable duty cycle PWM signal generator with verilog. It generates a PWM signal of 10MHz, it has a button to increase the duty cycle by 10% and one to decrease the duty cycle by 10%. The verilog code has been taken from fpga4student.com

## Introduction
A Pulse width modulated signal generator is used widely in the industry in many applications. PWM generator produces a square wave with a fixed time period but a variable duty cycle. Because the PWM can operate at high frequencies, it finds applications as a voltage controller in many devices.

## Applications
PWM generator is used in many applications like:

* DC/DC and DC/AC cconverters
* Inverters
* Controlling brightness of lights
* Controlling loudness in speakers
* Controlling speed of motor

## Block Diagram
<img width="668" alt="Screenshot 2022-08-08 at 11 38 58 PM" src="https://user-images.githubusercontent.com/110840360/183485390-8b13b41f-d19e-4d66-8c4a-bee4367ee150.png">
<img width="738" alt="PWM1" src="https://user-images.githubusercontent.com/110840360/183486763-baf39a12-0f32-46a2-8774-6863e79282e5.png">
In a PWM circuit, the pulse frequency is created using a comparator that resets a pulse-window counter, and the duty cycle is set by a comparator that drives the output waveform low whenever the count value is greater than the duty cycle value. Here, we have two buttons to either increase or decrease the duty cycle by 10% as shown above.

## Tools installation
Install the following tools with commands given (for linux):
* Verilog
* GTKwave
* Yosys
```
$   sudo apt-get update
$   sudo apt-get install iverilog gtkwave
$   sudo apt-get install -y yosys
```

## Functional Simulation
Use the following commands to clone the github repo and run the files:
```
$   sudo apt install -y git
$   git clone https://github.com/pranav1751/iiitb_pwm
$   cd iiitb_pwm
$   iverilog iiitb_pwm_gen.v iiitb_pwm_gen_tb.v
$   ./a.out
$   gtkwave pwm.vcd
```  
## Functional Characteristics
The simulation results when increasing duty cycle
![image](https://user-images.githubusercontent.com/110840360/183490276-91787469-5c75-4f2f-8465-82a4288d1807.png)
The simulation results when decreasing duty cycle
![image](https://user-images.githubusercontent.com/110840360/183490364-4f771446-0e22-4747-8c1b-be7f25e4a3f9.png)

## Generating GLS with yosys
Note: in the yosys_run.sh file, change the path name for the lib file as it is in your directory
```
# read design
# read_liberty -lib /media/psf/Home/Stark/Education/ASIC/iiitb_pwm_gen/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog iiitb_pwm_gen.v

# generic synthesis
synth -top iiitb_pwm_gen

# mapping to mycells.lib

dfflibmap -liberty /home/imt2020544/iiitb_pwm/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
abc -liberty /home/imt2020544/iiitb_pwm/lib/sky130_fd_sc_hd__tt_025C_1v80.lib -script +strash;scorr;ifraig;retime,{D};strash;dch,-f;map,-M,1,{D}
clean
flatten
# write synthesized design
write_verilog -noattr iiitb_pwm_synth.v
# stat
# show
```
Use the following commands to generate the Gate Level Simulation file from the initial iiitb_pwm_gen.v 
```
$ iverilog -DFUNCTIONAL -DUNIT_DELAY=#1 primitives.v sky130_fd_sc_hd.v iiitb_pwm_gen_synth.v iiitb_pwm_gen_tb.v
$ ./a.out
$ gtkwave pwm.vcd
```
note: I extracted the files primitives.v sky130_fd_sc_hd.v out of the verilog_model folder and kept them in the main folder as there was an error accessing them inside the folder for some unknown reason.

## Physical design with OpenLane
OpenLane is an automated RTL to GDSII flow based on several components including OpenROAD, Yosys, Magic, Netgen, CVC, SPEF-Extractor, KLayout and a number of custom scripts for design exploration and optimization. The flow performs all ASIC implementation steps from RTL all the way down to GDSII.\
Read more and installation guide at [https://github.com/The-OpenROAD-Project/OpenLane](https://github.com/The-OpenROAD-Project/OpenLane)\\
Installation
Prerequisites:
* docker installation guide: [https://docs.docker.com/get-docker/](https://docs.docker.com/get-docker/)
## Magic
Magic is a venerable VLSI layout tool, written in the 1980's at Berkeley by John Ousterhout, now famous primarily for writing the scripting interpreter language Tcl. Due largely in part to its liberal Berkeley open-source license, magic has remained popular with universities and small companies. The open-source license has allowed VLSI engineers with a bent toward programming to implement clever ideas and help magic stay abreast of fabrication technology. However, it is the well thought-out core algorithms which lend to magic the greatest part of its popularity. Magic is widely cited as being the easiest tool to use for circuit layout, even for people who ultimately rely on commercial tools for their product design flow.\\
More about magic and installation guide at [http://opencircuitdesign.com/magic/index.html](http://opencircuitdesign.com/magic/index.html)
## Genrating Layout with existing cells in OpenLane
run the following commands on the terminal:
```
$   cd OpenLane/
$   cd designs/
$   mkdir iiitb_pwm_gen
$   cd iiitb_pwm_gen/
$   wget https://raw.githubusercontent.com/pranav1751/iiitb_pwm_gen/main/config.json
$   mkdir src
$   cd src/
$   wget https://raw.githubusercontent.com/pranav1751/iiitb_pwm_gen/main/iiitb_pwm_gen.v
$   cd ../../../
$   sudo make mount
$   ./flow.tcl -design iiitb_pwm_gen!
```
[Screenshot from 2022-09-11 18-22-37](https://user-images.githubusercontent.com/110840360/189714149-468fa4b0-0902-41b6-92e0-8b8d24e28a7d.png)

The layout files for the run are created in iiitb_pwm_gen/runs. Find the most recent run, it has all the files, and the def physical layout files.\\ Using magic to view the .def files.
* Go to the folder where the final def file is located, here it is, /home/ubuntu/OpenLane/designs/iiitb_pwm/runs/RUN_2022.09.11_12.50.55/results/final/def 
* Run the command
```
 magic -T /home/ubuntu/OpenLane/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../../tmp/merged.nom.lef def read iiitb_pwm_gen.def &

```
The following window appears, showing the layout

![Screenshot from 2022-09-11 18-30-45](https://user-images.githubusercontent.com/110840360/189715252-74031e94-6dfe-419d-afca-a7c008f4eaba.png)
## Including a custom cell in the layout
Lets include in library and in the final layout. Clone the vsdcelldesign repo in Openlane/ using the following command
```
$ git clone https://github.com/nickson-jose/vsdstdcelldesign
```
copy sky130A.tech to vsdstdcelldesign directory
Viewing the vsdinv cell
'''
$ magic -T sky130A.tech sky130_inv.mag 
'''
![Screenshot from 2022-09-11 18-35-06](https://user-images.githubusercontent.com/110840360/189715991-78e7a3e9-b757-4c8f-945d-fc88c2c01a11.png)
in tkcon terminal using the following, generate the lef file
```
% lef write sky130_vsdinv
```
![Screenshot from 2022-09-11 18-37-23](https://user-images.githubusercontent.com/110840360/189716703-204aac45-41b2-4009-857f-ee840ccfdd26.png)
Copy the generated lef file to designs/iiit_pwm_gen/src. Also copy lib files from vsdcelldesign/libs to designs/iiit_pwm_gen/src. /
Open the OpenLane directory and run the following commands:
```
$ sudo make mount
$ ./flow.tcl -interactive
% package require openlane 0.9
% prep -design iiitb_pwm_gen
% set lefs [glob $::env(DESIGN_DIR)/src/*.lef]
% add_lefs -src $lefs
```

![Screenshot from 2022-09-11 18-43-37](https://user-images.githubusercontent.com/110840360/189718806-d87b12f7-01ee-47c1-a19c-8e032e091f61.png)
```

% run_synthesis
% run_floorplan
% run_placement
% run_cts
% run_routing
```
## Stats
Pre synthesis stats

![Screenshot from 2022-09-11 18-45-37](https://user-images.githubusercontent.com/110840360/189718924-da80876a-1fbd-4025-bd69-50334954d978.png)

Post synthesis stats
![Screenshot from 2022-09-11 22-17-49](https://user-images.githubusercontent.com/110840360/189718977-c5340c48-32c5-4dc8-b170-cbcafb8f6b48.png)

## Layout including vsdinv
To view the layout in magic, go to results/routing in your runs folder and enter the following command:
```
 magic -T /home/ubuntu/OpenLane/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.nom.lef def read iiitb_pwm_gen.def &
 ```
 Note: Change the directories for your PC.
![Screenshot from 2022-09-11 22-31-32](https://user-images.githubusercontent.com/110840360/189719018-9207f33c-1a87-437a-b641-a933b60e57bd.png)

```
% getcell sky130_vsdinv
```

![Screenshot from 2022-09-11 22-32-50](https://user-images.githubusercontent.com/110840360/189719215-f61d01f3-292b-46db-9d68-2abac5fbea97.png)

This shows that vsdinv cell is included.
![Screenshot from 2022-09-11 22-33-55](https://user-images.githubusercontent.com/110840360/189719104-f16e868a-a2f3-4356-8e00-d493e78ae134.png)
![Screenshot from 2022-09-12 23-40-52](https://user-images.githubusercontent.com/110840360/189725948-89fe990a-68a9-46b4-845e-18fbc4d14f1e.png)

![Screenshot from 2022-09-12 23-49-26](https://user-images.githubusercontent.com/110840360/189727643-0c18b36d-0ee2-4e54-a8e6-d52aa6791c4c.png)

## Reports



![Screenshot from 2022-09-13 00-21-00](https://user-images.githubusercontent.com/110840360/189733063-dff788fc-13d1-4fe6-9b32-e9ef09c062d7.png)












## Contributors
* Pranav Vajreshwari, iMtech2020 IIITB
* Kunal Ghosh, Director, VSD Corp. Pvt. Ltd.


## Acknowledgements
* Kunal Ghosh, Director, VSD Corp. Pvt. Ltd.

## Contact information
* Pranav Vajreshwari, Pranav.Vajreshwari@iitb.ac.in
* Kunal Ghosh, Director, VSD Corp. Pvt. Ltd. kunalghosh@gmail.com

## References
* https://www.fpga4student.com/2017/08/verilog-code-for-pwm-generator.html
* https://www.realdigital.org/doc/6136c69c3acc4bf52bc2653a067e36cc

