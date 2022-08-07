# RTL-Design-using-Verilog-with-sky130-Technology
![Verilog-flyer (1)](https://user-images.githubusercontent.com/104474928/165444531-7100dd83-27a8-4cf3-a2f8-f574f72e82fd.png)

This repo is the report for 5-day workshop on RTL design using Verilog with SKY130 Technology by VLSI sytem design.

## *Contents*
---------------
* [Introduction to tools](#introduction-to-tools)
* [Day 1](#day-1)
  * [Simulation Flow](#simulation-flow)
  * [Synthesis Flow](#synthesis-flow)
* [Day 2](#day-2)
  * [Library files](#library-files)
  * [Hierarchial and Flat Synthesis](#hierarchial-and-flat-synthesis)
  * [Flipflop Coding and Synthesis](#flipflop-coding-and-synthesis)
* [Day 3](#day-3)
  * [Combinational Logic Optimisations](#combinational-logic-optimisations)
  * [Sequential Logic Optimisations](#sequential-logic-optimisations)
* [Day 4](#day-4)
  * [GLS and Synthesis Simulation Mismatch](#gls-and-synthesis-simulation-mismatch)
  * [Blocking and Non Blocking Statements](#blocking-and-non-blocking-statements) 
* [Day 5](#day-5)
  * [IF and CASE Statements](#if-and-case-statements)
  * [FOR loop and FOR GENERATE](#for-loop-and-for-generate)
* [Acknowledgement](#acknowledgement)

---------
## Introduction to tools
* **_SKY130_** - Sky130 process node and pdk(process design kit) are an open-source packages provided by Google and skywater for 130nm technology.
* **_iverilog_** - Iverilog stands for Icarus verilog, is an open source verilog simulator.
* **_GTKWave_** - GTKWave is an open-source vcd(value change dump) waveform viewer.
* **_Yosys_** - Yosys is an open-source synthesis tool.
These are the _open-source_ tools used in the labs for the workshop.
---------

## Day 1 -  Introduction to Verilog RTL Design and Synthesis:

The first day covers the basics of RTL Design, Testbench, Simulation and Synthesis. Open-Source softwares like iverilog (simulator) and YOSYS (Synthesis) are provided through remote access in the portal to practice labs.

```RTL Design``` -  It consists of an actual verilog code / a set of verilog codes that have the functionality to meet the required design specifications of the circuit.

```TestBench``` - Testbench is a setup that one uses to apply a set of stimuli (test-case vector) to check the functional working of the design file.

![image](https://user-images.githubusercontent.com/110079770/183237263-c0a11a8a-eb1c-459f-a25e-a8dd25edfad3.png)

## NOTE:
```
Design may have 1 or more Primary inputs,1 or more primary outputs.
Testbench doesnot have primary inputs or primary outputs.
```
## iverilog simulator flow:

![image](https://user-images.githubusercontent.com/110079770/183237394-4d77ca95-61ed-4145-bc08-4dc3afbfecd4.png)

We do the above processes using a simulator software. The simulator is loaded with the design and its respective testbench file after which it looks for changes in the input signals and depending on the change, the output is evaluated. These changes in input and corresponding output values are dumped in a special format file called "value change dump" (.vcd) file. This file can be pictorially represented in waveforms using a waveform tool like gtkwave.

## Labs using gtkwave:

## Part 1 -  Setup the lab instance with libraries and verilog files:

Firstly, we have to clone 2 separate repositories namely [vsdflow](https://github.com/kunalg123/vsdflow) and [sky130RTLDesignAndSynthesisWorkshop](https://github.com/kunalg123/sky130RTLDesignAndSynthesisWorkshop) which contain the required library files and verilog design files to perform the simulations and logic synthesis parts of the workshop. It can be done using basic linux command gitclone ex: git clone https://github.com/kunalg123/vsdflow.git .
We are given a default set of files and libraries shown below to work on using the practical lab instance.

## verilog models:

![image](https://user-images.githubusercontent.com/110079770/183274639-a47cdc25-c0e3-40b7-a5ea-5aee282ec39c.png)

## Part 2 - Simulation using iverilog simulator - 2:1 multiplexer rtl design:

After cloning the respective repositories in our lab instance, we perform a simulation run of 2:1 multiplexer rtl file namely good_mux.v and its corresponding testbench file tb_good_mux.v to obtain .vcd files and analyze the waveform in gtkwave to see the change in output instances with respect to change in input values.

### verilog code for 2:1 mux:
![image](https://user-images.githubusercontent.com/110079770/183237816-018d7e3a-45b4-46c8-ba1a-6091756e60c3.png)

### Testbench for 2:1 mux:
![image](https://user-images.githubusercontent.com/110079770/183237845-297b85bb-e150-4b6b-ab2b-e0c12e101f66.png)

##### Commands for simulation
Use the below commands for simulation and view the waveform with iverilog and GTKWave respectively.

        iverilog file_name.v testbench_name.v           //creates an executable file
        ./a.out                                         //generates vcd file
        gtkwave testbench_name.vcd                      //view vcd file in gtkwave viewer

### Simulation of the above waveform by using iverilog and gtkwave:
![image](https://user-images.githubusercontent.com/110079770/183237970-b246ccda-d324-4be4-a146-11cb0049f394.png)

## Part-3 Introduction to the Yosys and Logic Synthesis.

#### Introduction to Yosys:

```Synthesizer``` : Tool is used for converting the RTL to netlist.

```Yosys``` : It is the tool used to convert the RTL to Gate level netlist. 

### Synthesis Flow
The synthesis tool being used is Yosys. The inputs for Yosys are the RTL design written in HDL and the libraries required. Here the libraries by sky130 is used. Libraries exist as ".lib" files. These libraries contain different flavours of the most common logical blocks(like AND, NOT gate, MUX, Flip-flops, etc). The circuit is synthesised with these logical blocks.  
##### Why do we need different flavours?
The different flavours of same logic blocks(standard cells) allows these to be used in different applications. These flavours may work on different speeds. The faster the cell the more area and power required. The cell used depends on which parameter(s) is to be optimised.  
Additionally different cells are required to meet the timing requirements. More about that is discussed in [Day 2](day-2).  

For the below circuit the three factors are
- Clock to Q of flipflop A
- Propagation delay of combinational circuit
- Setuptime of flipflop B
![Timedelay circuit](https://user-images.githubusercontent.com/104454253/166098730-33bf0734-abec-466f-abe2-a2ac6813b5e0.JPG)

The equation is as follows

![Time](https://user-images.githubusercontent.com/104454253/166097710-2c1099e3-6323-496c-8eb7-12ee04c12096.JPG)

As per the above equation, for a smaller propagation delay, we need faster cells.
But again, why do we have faster cells? This is to ensure that there are no HOLD time violations at B flipflop.
**This complete collection forms .lib**

**Faster Cells vs Slower Cells**: 
Load in digital circuit is of **Capacitence**. Faster the charging or dicharging of capacitance, lesser is the celll delay. However, for a quick charge/ discharge of capacitor, we need transistors capable of sourcing more current i.e, we need WIDE TRANSISTORS. 

Wider transistors have lesser delay but consume more area and power. Narrow transistors are other way around. Faster cells come with a cost of area and power.

**Selection of the Cells**: We'll need to guide the Synthesizer to choose the flavour of cells that is optimum for implementation of logic circuit. Keeping in view of previous observations of faster vs slower cells,to avoid hold time violations, larger circuits, sluggish circuits, we offer guidance to synthesizer in the form of **Constraints**.

Below is an illustration of Synthesis.
![image](https://user-images.githubusercontent.com/110079770/183248437-0bf2e4e9-9868-4919-a311-2de1a9d1de6b.png)

First of all, Yosys tool is invoked in the terminal.

                $ yosys         

Now inside the yosys, type the following commands for synthesis

                yosys> read_liberty -lib ../path_to_library             //reads library file
                yosys> read_verilog file_name.v                         //reads RTL file to be synthesized
                yosys> synth -top file_name
                yosys> abc -liberty ../path_to_library                  //actual synthesis
                yosys> write_verilog -noattr netlist_name.v             //write created netlist as verilog file
                yosys> show                                             //shows netlist as circuit diagram with cells

Finally use "exit" command when you want to exit from yosys.  
*Note: The present working directory should contain RTL design before invoking yosys.

### Labs on Yosys introduction

Invoking Yosys:

![image](https://user-images.githubusercontent.com/110079770/183258775-8b868edc-0597-45b5-8a0b-16246f971da7.png)

![image](https://user-images.githubusercontent.com/110079770/183274919-f9352765-8624-4f5d-a118-650f2009cf48.png)

### ```Netlist code```
![netlistcode](https://user-images.githubusercontent.com/104474928/165451249-091978a1-f314-489e-8f89-e8eb1845a0d5.png)


---------

## Day 2
### Library files
The library file used is *_sky130_fd_sc_hd_tt_025C_1v80.lib_*. The nomenclature for library is not random and shows important parameters most importantly the *Process*,*Temperature* and *Voltage*. These parameters are show in the end of the name "tt_025C_1v80".  
* tt means the process is "typical"
* 025C shows the temperature - 25 degree celsius
* 1v80 shows the voltage - 1.8V

![image](https://user-images.githubusercontent.com/110079770/183258200-37385d07-9bfb-4158-b7c1-da0b0fd10a80.png)



_SKY130 library file_
  
##### Flavours of standard cells
As mentioned before, the library contains different flavours of same logic gates. This is done mainly to meet timing constraints.  
* *Faster cells* increases the clock frequency.
  T(clk) > T(cq_launch_flop) + T(combi) +T(setup_capture_flop)
* *Slower cells* are required to prevent hold violations.
  T(hold_capture_flop) < T(cq_launch_flop) + T(combi)
  
These different flavours are named different within the library. For example, the different flavours of and gate present are:
- based on delay:      _sky130_fd_sc_hd_and2_0 > sky130_fd_sc_hd_and2_2 > sky130_fd_sc_hd_and2_4_
- based on power/area: _sky130_fd_sc_hd_and2_0 < sky130_fd_sc_hd_and2_2 < sky130_fd_sc_hd_and2_4_

### Hierarchial and Flat synthesis
Many times in complex systems, different parts of system are designed seperately as RTL blocks(sub-modules) which can later be combined to form the whole system(top module). In such cases the synthesis can be done in two ways:
* *Hierarchial synthesis* - Here the top module is synthesised in terms of sub-modules. Only the higher level abstraction is required.
* *Flat Synthesis* - Here all sub-modules are also expanded and the top module is sysnthesised in terms of standard cells. It has lower abstraction compared to hierarchial synthesis.
Consider the example

image

If this logic is synthesised noramlly it will do hierarchial synthesis. The netlist made following hierarchial synthesis is.

image

For flat synthesis an additional command needs to be added to the normal synthesis flow.

              yosys> synth -top file_name
              yosys> flatten
              yosys> abc -liberty ../path_to_library
              yosys> write_verilog flat_netlist_name

This will give us a flattened netlist like shown below.  

### Flipflop Coding and Synthesis
When it comes to flipflops, the set/reset logic is very important. They can be either synchronous or asynchronous. For flops with synchronous set/reset, the set/reset is dependant on clock and therefore this logic gets realised on the input (D- input).  
In the HDL code the asynchronous and  synchronous reset can be distinguished by the sensistivity list. A flipflop with asynchronous reset should have both clock and reset on the sensitiivity list. A flipflop with synchronous reset on the other hand would have only clock on the sensitivity list as the reset is also dependant on the clock.  


D flipflop with synchronous reset.


D flipflop with asynchronous reset. 


The netlist clearly shows that the synchronous reset is applied at D input as expected.  
---------
 ## Day 3
 The synthesis tool comes with many features. One of such features which has a huge impact on design is _optimisation_. The tool does optimisation on the logic (RTL design). Usually these optimisation are done to obtain least hardware and omit unwanted components.  
 ### Combinational Logic Optimisations
 In designs containing complex combinational logic, most of the time certain components are not reflected in the output and hence are considered _useless_. Such parts are removed by the tool. Sometimes sub-modules within a top module which does not affect the output may also be removed.    
 Consider the example:  
 
 
 
 From the boolean logic for this RTL, it is clear that "b" input is not reflected on the output. Hence the synthesised netlist should optimisations. This becomes clear from the netlist given below.
 
 
 Now consider a combinational circuit written as sub-modules.
 
 
 The optimised netlist obtained is:  
 
 
 ### Sequential Logic Optimisations
The most common optimisations in sequential circuits are optimisations for constant and optimising unused outputs.
  
For optimisation of constants in sequential circuits, consider below example.  


After simulating with the help og testbench the waveform obtained is:



From the waveform it is clear the output remains constant for all cases and hence can be optimised. The synthesis report obtained is given below.


Finally the optimised netlist given by the tools is:  


Now consider the next example for optimisation of unused outputs in sequential circuits. 


_Waveform_  
  
  
From the waveform it is clear that output is only dependant single bit of the counter. Hence the other flops can be optimised.




The synthesised output contains only one flipflop as other unused flops are optimised off.

---------
## Day 4
### GLS and Synthesis Simulation Mismatch
While using an HDL to write the RTL logic, if not careful, synthesis simulation mismatch may occur.
Gate-level simulation(GLS) is the simulation of gate level netlist of a design unlike the normal functional simulation where the behavioural RTL is simulated. When the RTL simulation is different from GLS the circuit is said to exhibit synthesis simulation mismatch.  
This can occur due to a variety of reasons including:
* missing sensitivity list
* wrong use of blocking/non-blocking statements
* non-standard verilog coding  

Lets see an example of such sythesis simulation mismatch



This is the RTL code for a mux, but the sensitivity list shows only "sel". This means the output will only change when there is a change in "sel" signal. This is obviously not the desired logic and will cause synthesis simulation mismatch due to the missing elements in sensitivity list.  
The waveform obtained from RTL simulation is :


compared to the waveform from GLS


_Note: The verilog models of standard cells must also be called on the iverilog for GLS._
### Blocking and Non Blocking Statements
In verilog HDL, there are two types of assignment operators:
* <= this assignment creates a non-blocking statement
* =  this assignment creates a blocking statement
  
For non-blocking statements all the RHS are first found out before it is assigned. Since all such statements are assigned at the same time the order of statements are irrelevant. In the case of blocking statements, values are assigned instantly and therefore the order of statements are extremely important.  
While using blocking statements if care is not taken it could result in synthesis simulation mismatch. For example consider:


_RTL simulation waveform_



_GLS waveform_




## Day 5
### IF and CASE statements
The _IF_ and _CASE_ statements are the conditional statements present in verilog HDL.  
If statements-    
              
              if (condition1)
                  ......
              else if (condition2)
                  ......
              else
                  ......
Case statements-

             case(sel)
                condition1 : ......
                condition2 : ......
                default    : ......
                
The main difference between  case and if statements is the priority level. If statements has priority level and case statements do not. Although these statements are required in almost all high level design it also brings complexities. An incomplete if or case statements may create additional problems. For example consider:



The incomplete if statement in this example with infer unwanted latches. This is evident in simulation and synthesis results given below.  
_Simulation waveform_


_Synthesis report_

_Synthesised circuit_


Similarly unwanted latches are inferred for incomplete case statements as well.


### FOR loop and FOR GENERATE
For loops are always written inside "always" statements. The syntax for "for" loop is similar to that in C. For loops are used where multiple evaluating statements need to be run.  
For example:  
_Demux using for loop_


For generate statements are written outside "always" statement. It is used for instantiating a module multiple times within an RTL.  
For example:  
_Ripple carry adder using for generate_





  
## Acknowledgement
* [Kunal Ghosh](https://github.com/kunalg123)





