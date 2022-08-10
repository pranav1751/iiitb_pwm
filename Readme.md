
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
$   iverilog iiitb_pwm.v iiitb_pwm_tb.v
$   ./a.out
$   gtkwave pwm.vcd
```  
## Functional Characteristics
The simulation results when increasing duty cycle
![image](https://user-images.githubusercontent.com/110840360/183490276-91787469-5c75-4f2f-8465-82a4288d1807.png)
The simulation results when decreasing duty cycle
![image](https://user-images.githubusercontent.com/110840360/183490364-4f771446-0e22-4747-8c1b-be7f25e4a3f9.png)

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
