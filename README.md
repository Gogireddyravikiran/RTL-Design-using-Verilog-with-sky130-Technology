# RTL-Design-using-Verilog-with-sky130-Technology
![Verilog-flyer (1)](https://user-images.githubusercontent.com/104474928/165444531-7100dd83-27a8-4cf3-a2f8-f574f72e82fd.png)
## Table Of Contents
* ### [Getting Started]
* ### [Introduction to Verilog RTL Design & Synthesis]
* ### [Introduction to Verilog RTL Design & Synthesis]
* #### [Setting up libraries & lab instances]















## Day 1 -  Introduction to Verilog RTL Design and Synthesis

The first day covers the basics of RTL Design, Testbench, Simulation and Synthesis. Open-Source softwares like iverilog (simulator) and YOSYS (Synthesis) are provided through remote access in the portal to practice labs.

RTL Design -  It consists of an actual verilog code / a set of verilog codes that have the functionality to meet the required design specifications of the circuit.

TestBench - Testbench is a setup that one uses to apply a set of stimuli (test-case vector) to check the functional working of the design file.

![image](https://user-images.githubusercontent.com/110079770/183237263-c0a11a8a-eb1c-459f-a25e-a8dd25edfad3.png)
## NOTE
```
Design may have 1 or more Primary inputs,1 or more primary outputs.
Testbench doesnot have primary inputs or primary outputs.
```
## iverilog simulator flow

![image](https://user-images.githubusercontent.com/110079770/183237394-4d77ca95-61ed-4145-bc08-4dc3afbfecd4.png)

We do the above processes using a simulator software. The simulator is loaded with the design and its respective testbench file after which it looks for changes in the input signals and depending on the change, the output is evaluated. These changes in input and corresponding output values are dumped in a special format file called "value change dump" (.vcd) file. This file can be pictorially represented in waveforms using a waveform tool like gtkwave.

##Labs using gtkwave

### Part 1 -  Setup the lab instance with libraries and verilog files

Firstly, we have to clone 2 separate repositories namely [vsdflow](https://github.com/kunalg123/vsdflow) and [sky130RTLDesignAndSynthesisWorkshop](https://github.com/kunalg123/sky130RTLDesignAndSynthesisWorkshop) which contain the required library files and verilog design files to perform the simulations and logic synthesis parts of the workshop. It can be done using basic linux command gitclone ex: git clone https://github.com/kunalg123/vsdflow.git .
We are given a default set of files and libraries shown below to work on using the practical lab instance.

### verilog models used for this workshop.

![image](https://user-images.githubusercontent.com/110079770/183237675-dff34548-9937-4549-91e0-eaf509cdfd07.png)

### Part 2 - Simulation using iverilog simulator - 2:1 multiplexer rtl design

After cloning the respective repositories in our lab instance, we perform a simulation run of 2:1 multiplexer rtl file namely good_mux.v and its corresponding testbench file tb_good_mux.v to obtain .vcd files and analyze the waveform in gtkwave to see the change in output instances with respect to change in input values.

 verilog code for 2:1 mux:
![image](https://user-images.githubusercontent.com/110079770/183237816-018d7e3a-45b4-46c8-ba1a-6091756e60c3.png)

 Testbench for 2:1 mux
![image](https://user-images.githubusercontent.com/110079770/183237845-297b85bb-e150-4b6b-ab2b-e0c12e101f66.png)

### Simulation of the above waveform by using iverilog and gtkwave.
![image](https://user-images.githubusercontent.com/110079770/183237970-b246ccda-d324-4be4-a146-11cb0049f394.png)















