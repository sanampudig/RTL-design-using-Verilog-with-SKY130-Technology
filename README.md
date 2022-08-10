# RTL-design-using-Verilog-with-SKY130-Technology
![sd](https://user-images.githubusercontent.com/110079648/183350533-eb98d97d-6514-4e4b-b9bb-8540d872cc21.png)
# Table of contents
 - [1. Introduction](#1-Introduction)
 - [2. Day-1- Introduction to Verilog RTL design and Synthesis](#2-Day-1--Introduction-to-Verilog-RTL-design-and-Synthesis)
    - [2.1 Various aspects of frontend design](#21-Various-aspects-of-frontend-design)
    - [2.2 Introduction to open source simulator iverilog and gtkwave](#22-Introduction-to-open-source-simulator-iverilog-and-gtkwave)
     	- [2.2.1 Lab examples using iverilog and gtkwave](#221-Lab-examples-using-iverilog-and-gtkwave)
    - [2.3 Introduction to Yosys synthesizer](#23-Introduction-to-Yosys-synthesizer)
        - [2.3.1 Labs on Yosys introduction](#231-Labs-on-Yosys-introduction)
 - [3. DAY2-Timing libs, hierarchical, flat synthesis, efficient flop coding styles](#3-DAY2-Timing-libs-hierarchical-flat-synthesis-efficient-flop-coding-styles)
    - [3.1 Introduction to timing .libs](#31-Introduction-to-timing-libs)
        - [3.1.1 LAB- Introduction to dot Lib](#311-LAB--Introduction-to-dot-Lib)
    - [3.2 LAB- Hierarchical synthesis and flat synthesis](#32-LAB-Hierarchical-synthesis-and-flat-synthesis)
    - [3.3 Various Flop coding styles and optimization](#33-Various-Flop-coding-styles-and-optimization)
         - [3.3.1 Lab- flop synthesis simulations](#331-Lab-flop-synthesis-simulations)
         - [3.3.2 Interesting optimisations](#332-Interesting-optimisations)
 - [4. Day3- Combinational and sequential optmizations](#4-Day3--Combinational-and-sequential-optmizations)
    - [4.1 Combinational logic optimization with examples](#41-Combinational-logic-optimization-with-examples)
    - [4.2 Sequential logic optimization with examples](#42-Sequential-logic-optimization-with-examples)
        - [4.2.1 Basic](#421-Basic)
        - [4.2.2 Advanced](#422-Advanced)
        - [4.2.3 Sequential optimisation of unused outputs](#423-Sequential-optimisation-of-unused-outputs)
 - [5. DAY4- GLS, blocking vs non-blocking and Synthesis-Simulation mismatch](#5-DAY4--GLS-blocking-vs-non-blocking-and-Synthesis-Simulation-mismatch)
    - [5.1 GLS, Synthesis-Simulation mismatch and Blocking, Non-blocking statements](#51-GLS-Synthesis-Simulation-mismatch-and-Blocking-Non-blocking-statements)
        - [5.1.1 GLS Concepts And Flow Using Iverilog](#511-GLS-Concepts-And-Flow-Using-Iverilog)
        - [5.1.2 Synthesis Simulation Mismatch](#512-Synthesis-Simulation-Mismatch)
    - [5.2 Lab- GLS Synth Sim Mismatch](#52-Lab--GLS-Synth-Sim-Mismatch)
    - [5.3 Lab- Synthesis simulation mismatch blocking statement](#53-Lab--Synthesis-simulation-mismatch-blocking-statement)
 - [6. DAY5- if, case, for loop and for generate](#6-DAY5--if-case-for-loop-and-for-generate)
    - [6.1 If and Case constructs](#61-If-and-Case-constructs)
       - [6.1.1 If construct](#611-If-construct)
       - [6.1.2 Case construct](#612-Case-construct)
    - [6.2 Lab- Incomplete IF](#62-Lab--Incomplete-IF)
    - [6.3 Lab- incomplete overlapping Case](#63-Lab--incomplete-overlapping-Case)
    - [6.4 For Loop and For Generate](#64-For-Loop-and-For-Generate)
    - [6.5 Lab- For and For Generate](#65-Lab--For-and-For-Generate)
 - [7. Word of Thanks](#7-Word-of-Thanks)
# 1. Introduction
This report is a final submission of 5-day workshop from [VLSI Sytem Design-IAT](https://www.vlsisystemdesign.com/) on RTL design and synthesis using open source tools, in particular iVerilog, GTKWave, Yosy and Skywater 130nm Standard Cell Libraries  
# 2. Day-1- Introduction to Verilog RTL design and Synthesis
## 2.1 Various aspects of frontend design
**RTL Design**: In simple terms RTL design or Register Transfer Level design is a method in which we can transfer data from one register to another. In RTL design we write code for Combinational and Sequential circuits in HDL(Hardware Description Language) like Verilog or VerilogHDL which can model logical and hardware operation. RTL design can be one code or set of verilog codes. **One key note is that we need to write RTL design with optimized and synthesizable (realizable as physical gates)**.

**Sample RTL design outline:**

	module module_name (port list);
		//declarations;
		//initializations;
		//continuos concurrent assigments;
		//procedural blocks;
	endmodule

**Test Bench**: Using Verilog we can write a test bench to apply stimulus to the RTL design and verify the results of the design by instantiating design with in test bench. Up-front verification becomes very important as design size increases in size and complexity while any project progresses. This ensures simulation results matches with post synthesis results. A test bench can have two parts, the one generates input signals for the model to be tested while the other part checks the output signals from the design under test. It can be represented as follows.
![Capture2](https://user-images.githubusercontent.com/104454253/166088950-634be5a4-7d5a-4b43-9990-711f8f660aaf.JPG)

**Simulation**: RTL design is checked for adherence to its design specification using simulation by giving sample inputs. This helps finding and fixing bugs in the RTL design in the early stages of design development. 

**Simulator**: Simulator is the tool used for this process. It looks for changes on input signals to evaluate outputs. No change in output if there is no change in input signals
Here is the flow of frondend design:

![Capture1](https://user-images.githubusercontent.com/104454253/166088866-80a4e792-7db7-4bf2-b3b5-b4b9b92452a8.JPG)

## 2.2 Introduction to open source simulator iverilog and gtkwave
**iverilog**: iverilog stands for Icarus Verilog. Icarus Verilog is an implementation of the Verilog hardware description language. It supports the 1995, 2001 and 2005 versions of the standard, portions of SystemVerilog, and some extensions.

**Gtkwave**: GTKWave is a fully featured GTK+ based wave viewer for Unix, Win32, and Mac OSX which reads LXT, LXT2, VZT, FST, and GHW files as well as standard Verilog VCD/EVCD files and allows their viewing. 

### 2.2.1 Lab examples using iverilog and gtkwave

We were introducted to Linux operating system and were made aware of the basic commands. Using **git clone** command we've cloned library files like standard cell library, primitives which are used for synthesis and few verilog codes for practice.

![image](https://user-images.githubusercontent.com/110079648/183275544-f11fb53a-1e66-4271-9867-2a28534fdce0.png)
![image](https://user-images.githubusercontent.com/110079648/183275547-c003b56d-5ba7-420f-a5d0-aacf58e20846.png)

<img width="1045" alt="image" src="https://user-images.githubusercontent.com/110079648/183275617-4cebce4a-d1b4-4f06-b14a-61121f6c8c87.png">

In this session, I've performed simulation of multiplexer. I've added both the RTL design code and test bench code in iverilog to generate vcd file which I used in gtkwave generator to get the output waveformes after simulation. The output was generated by taking the inputs from the testbench code. 


Here is the code :<br />

	module good_mux (input i0 , input i1 , input sel , output reg y); 
		always @ (*)
		begin
			if(sel)
			y <= i1;
			else 
			y <= i0;
		end
	endmodule**


	`timescale 1ns / 1ps
	module tb_good_mux;
	// Inputs
	reg i0,i1,sel;
	// Outputs
	wire y;
      		// Instantiate the Unit Under Test (UUT), name based instantiation
		good_mux uut (.sel(sel),.i0(i0),.i1(i1),.y(y));
		//good_mux uut (sel,i0,i1,y);  //order based instantiation
	initial begin
		$dumpfile("tb_good_mux.vcd");
		$dumpvars(0,tb_good_mux);
		// Initialize Inputs
		sel = 0;
		i0 = 0;
		i1 = 0;
		#300 $finish;
	end
	always #75 sel = ~sel;
	always #10 i0 = ~i0;
	always #55 i1 = ~i1;
	endmodule**



## 2.3 Introduction to Yosys synthesizer

**Synthesis**: Synthesis transforms the simple RTL design into a gate-level netlist with all the constraints as specified by the designer. In simple language, Synthesis is a process that converts the abstract form of design to a properly implemented chip in terms of logic gates.

Synthesis takes place in multiple steps:
- Converting RTL into simple logic gates.
- Mapping those gates to actual technology-dependent logic gates available in the technology libraries.
- Optimizing the mapped netlist keeping the constraints set by the designer intact.

**Synthesizer**: It is a tool we use to convert out RTL design code to netlist. Yosys is the tool I've used in this workshop.
Here is the flow of above processess.

![rtl-netlist](https://user-images.githubusercontent.com/104454253/166097298-41d913ee-640d-4e1e-9e70-5bf427f35ef4.JPG)

**Yosys**:Yosys is a framework for RTL synthesis and more. It currently has extensive Verilog-2005 support and provides a basic set of synthesis algorithms for various application domains. Yosys is the core component of most our implementation and verification flows.

I was given an overview of the operation of the tool and the files we'll need to provide the tool to give the required netlist. We give RTL design code, .lib file which has all the building blocks of the netlist. Using these two files, Yosys synthesizer generates a netlist file. .lib basically is a collection of logical modules like, And, Or, Not etc.... These are equivalent gate level representation of the RTL code. 

Below are the commands to perform above synthesis.

- RTL Design  - read_verilog
- .lib        - read_liberty
- netlist file- write_verilog

**Operational flow of Yosys Synthesizer**

![Synthesizer](https://user-images.githubusercontent.com/104454253/166094901-27c70c0d-8ef2-4a34-a4b2-7307af492698.JPG)

**Verification of Synthesized design**: In order to make sure that there are no errors in the netlist, we'll have to verify the synthesized circuit. The netlist verification flow can be seen in the below image:

![Synthesisgtkwave](https://user-images.githubusercontent.com/104454253/166095185-f82dbbe0-afb4-43ac-8ec6-6b75491d6b58.JPG)

The gtkwave output for the netlist should match the output waveform for the RTL design file. As netlist and design code have same set of inputs and outputs, we can use the same testbench and compare the waveforms.

**Introduction to loigc synthesis**: Below is the snippet RTL code and equivalent digital circuit:

![sample rtl](https://user-images.githubusercontent.com/104454253/166097112-0fb5685c-fe88-4ca0-8ecf-bc014de46088.JPG)

In the above image, mapping of code and digital circuit is done using Synthesis.

**.lib**: It is a collection of logical modules like, And, Or, Not etc...It has different flvors of same gate like 2 input AND gate, 3 input AND gate etc... with different performace speed.

**Need for different flavours of gate**: In order to make a faster circuit, the clock frequency should be high. For that the time period of the clock should be as low as possible. However, in a sequential circuit, clock period depends on three factors so that data is not lost or to be glitch free.

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

![Screenshot (44)](https://user-images.githubusercontent.com/104454253/166099264-e3842e91-1a27-44ae-830c-0757dc5b1a5e.png)

### 2.3.1 Labs on Yosys introduction

Invoking Yosys:

<img width="748" alt="image" src="https://user-images.githubusercontent.com/110079648/183275821-3148562c-7e88-45c3-92d7-d4c84d200c79.png">

Snippet below illustrates reading .lib, design and choosing the module to synthesize:
<img width="628" alt="image" src="https://user-images.githubusercontent.com/110079648/183276206-b5c93a58-1579-49cc-932b-696e90f64e7f.png">

**Generating Netlist**: The logic of good_mux will be realizable using gates in the sky130_fd_sc_hd__tt_025C_1v80.lib file.

<img width="841" alt="image" src="https://user-images.githubusercontent.com/110079648/183276271-65d1fb99-1e31-4fda-bc97-6b282448e0ac.png">

Below is the snippet showing the synthesis results and synthesized circuit for multiplexer.

<img width="720" alt="image" src="https://user-images.githubusercontent.com/110079648/183276334-94c3d0b7-eeee-4b57-88c4-861e0986111d.png">

<img width="637" alt="image" src="https://user-images.githubusercontent.com/110079648/183276359-d8aa7711-2e34-4581-a975-31b911458dff.png">


**Netlist Code**

<img width="1294" alt="image" src="https://user-images.githubusercontent.com/110079648/183299489-ca1014c2-40c8-460a-8877-189e71141c8b.png">

**Simplified netlist code**: This code consisits of additional switch. To further simplify, we use below command
<img width="1359" alt="image" src="https://user-images.githubusercontent.com/110079648/183277276-3fd1c8a7-59f8-4997-ab14-82a359ebfe40.png">

```
$ yosys
yosys> read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
yosys> read_verilog good_mux.v 
yosys> synth -top good_mux 
yosys> abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
yosys> show

yosys> write_verilog good_mux_netlist.v 
yosys> !gvim good_mux_netlist.v 

yosys> write_verilog -noattr good_mux_netlist.v
yosys> !gvim good_mux_netlist.v 

```

# 3. DAY2-Timing libs, hierarchical, flat synthesis, efficient flop coding styles

## 3.1 Introduction to timing .libs
### 3.1.1 LAB- Introduction to dot Lib

This lab guides us through the .lib files where we have all the gates coded in. According to the below parameters the libraries will be characterized to model the variations.

![lib1](https://user-images.githubusercontent.com/104454253/166105787-19a638a3-fe01-4fcf-828d-0b56a6acb8f7.JPG)

With in the lib file, the gates are delared as follows to meet the variations due to process, temperatures and voltages.

<img width="1397" alt="image" src="https://user-images.githubusercontent.com/110079648/183277476-a3cdd2aa-cefa-4704-bbc0-31ac17ebc14f.png">

For the above example, for all the 32 cominations i.e 2^5 (5 is no.of variables), the delay, power and all the related parameters for each gate are mentioned.

<img width="1387" alt="image" src="https://user-images.githubusercontent.com/110079648/183277556-84c6cd9e-de6d-49c7-ae1a-47db7ec355a0.png">

This image displays the power consumtion comparision.

![lib5](https://user-images.githubusercontent.com/104454253/166107259-6fa398a4-2099-4da3-9b93-818c2c3f2404.JPG)

Below image is the delay order for the different flavor of gates.

![delay_libraries](https://user-images.githubusercontent.com/104454253/166187423-d21465e1-abc3-4ad0-a534-60f8e706ab6f.JPG)

## 3.2 LAB- Hierarchical synthesis and flat synthesis

**multiple_module**<br />

	module sub_module2 (input a, input b, output y);
		assign y = a | b;
	endmodule
	
	module sub_module1 (input a, input b, output y);
		assign y = a&b;
	endmodule


	module multiple_modules (input a, input b, input c , output y);
	wire net1;
	sub_module1 u1(.a(a),.b(b),.y(net1));  //net1 = a&b
	sub_module2 u2(.a(net1),.b(c),.y(y));  //y = net1|c ,ie y = a&b + c;
	endmodule

This is the schematic as per the connections in the above module.

![telegram-cloud-photo-size-5-6314223892675276833-y](https://user-images.githubusercontent.com/110079648/183962587-1dde8169-5e72-43b6-920b-f488ee000e44.jpg)

However, the yosys synthesizer generates the following schematic instead of the above one and with in the submodules, the connections are made
```
$ yosys
yosys> read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
yosys> read_verilog multiple_modules.v
yosys> synth -top multiple_modules
yosys> show multiple_modules 

```

<img width="802" alt="image" src="https://user-images.githubusercontent.com/110079648/183289083-9b6f9bec-f3aa-4a73-bc97-41d01244382d.png">

The synthesizer considers the module hierarcy and does the mapping accordting to instantiation. Here is the hierarchical netlist code for the  multiple_modules:

	module multiple_modules(a, b, c, y);
		  input a;
 		 input b;
 		 input c;
		  wire net1;
 		 output y;
 	  sub_module1 u1 (.a(a),.b(b),.y(net1) );
	  sub_module2 u2 (.a(net1),.b(c),.y(y));
	endmodule
	
	module sub_module1(a, b, y);
 	 wire _0_;
 	 wire _1_;
 	 wire _2_;
 	 input a;
 	 input b;
 	 output y;
 	 sky130_fd_sc_hd__and2_0 _3_ (.A(_1_),.B(_0_),.X(_2_));
 	 assign _1_ = b;
 	 assign _0_ = a;
 	 assign y = _2_;
	endmodule

	module sub_module2(a, b, y);
  	wire _0_;
 	 wire _1_;
 	 wire _2_;
  	input a;
  	input b;
 	 output y;
 	 sky130_fd_sc_hd__lpflow_inputiso1p_1 _3_ (.A(_1_),.SLEEP(_0_),.X(_2_) );
 	 assign _1_ = b;
 	 assign _0_ = a;
 	 assign y = _2_;
	endmodule

Flattened netlist:

In flattened netlist, the hierarcies are flattend out and there is single module i.e, gates are instantiated directly instead of sub_modules. Here is the flattened netlist code for the  multiple_modules:

	module multiple_modules(a, b, c, y);
 		 wire _0_;
  		 wire _1_;
 		 wire _2_;
 		 wire _3_;
		 wire _4_;
		 wire _5_;
 		 input a;
 		 input b;
 		 input c;
 		 wire net1;
 		 wire \u1.a ;
		 wire \u1.b ;
		 wire \u1.y ;
		 wire \u2.a ;
		 wire \u2.b ;
 		 wire \u2.y ;
  		output y;
 		 sky130_fd_sc_hd__and2_0 _6_ (
  		  .A(_1_),
  		 .B(_0_),
   		 .X(_2_)
  		);
 		 sky130_fd_sc_hd__lpflow_inputiso1p_1 _7_ (
  		  .A(_4_),
 		  .SLEEP(_3_),
  		  .X(_5_)
 		 );
 		 assign _4_ = \u2.b ;
 		 assign _3_ = \u2.a ;
 		 assign \u2.y  = _5_;
 		 assign \u2.a  = net1;
		 assign \u2.b  = c;
 		 assign y = \u2.y ;
		 assign _1_ = \u1.b ;
		 assign _0_ = \u1.a ;
		 assign \u1.y  = _2_;
		 assign \u1.a  = a;
		 assign \u1.b  = b;
 		 assign net1 = \u1.y ;
		endmodule

The commands to get the hierarchical and flattened netlists is shown below:

**yosys> write_verilog -noattr multiple_modules_hier.v**

8. Executing Verilog backend.
Dumping module `\multiple_modules'.
Dumping module `\sub_module1'.
Dumping module `\sub_module2'.

**yosys> !gvim multiple_modules_hier.v**

11. Shell command: gvim multiple_modules_hier.v

**yosys> flatten**

12. Executing FLATTEN pass (flatten design).
Deleting now unused module sub_module1.
Deleting now unused module sub_module2.
<suppressed ~2 debug messages>

**yosys> write_verilog -noattr multiple_modules_flat.v**

13. Executing Verilog backend.
Dumping module `\multiple_modules'.

**yosys> !gvim multiple_modules_flat.v**

14. Shell command: gvim multiple_modules_flat.v

This is the synthyesized circuit for a flattened netlist. Here u1 and u2 are flattened and directly or gates are realized.

![lab51](https://user-images.githubusercontent.com/104454253/166112988-1b02eea6-a6e8-4a7e-8eee-0772182f914f.JPG)

Here is the synthesized circuit of sub_module1. We are also generating module level synthesis so that if there is a top module with multiple and same sub_modules, we can synthesize it once and can use and connect the same netlist multiple times in the top module netlist.

Another reason to generate module level synthesis and then stictch them together is to avoid errors in a top module if its massive and consists of several sub modules. Generating netlist for synthesis and then stiching it together in top level becomes easier and reduces risk of output mismatch.

We control this synthesis using **synth -top <module_name>** command

![lab52](https://user-images.githubusercontent.com/104454253/166113791-5c245c1c-727a-4f15-aaec-9fef1b817aec.JPG)

### 3.3 Various Flop coding styles and optimization

**Why Flops and Flop coding styles part1**

In this session, the discussion was about how to code various types of flops and various styles of coding a flop.

**Why a Flop?**

 In a combinational circuit, the output changes after the propagation delay of the circuit once inputs are changed. During the propagation of data, if there are different paths with different propagation delays, there might be a chance of getting a glitch at the output.<br />
 If there are multiple combinational circuits in the design, the occurances of glitches are more thereby making the output unstable.<br />
To curb this drawback, we are going for flops to store the data from the cominational circuits. When a flop is used, the output of combinational circuit is stored in     it and it is propagated only at the posedge or negedge of the clock so that the next combinational circuit gets a glitch free input thereby stabilising the output.
 
 We use initialize signals or control pins called **set** and **reset** on a flop to initialize the flop, other wise a garbage value to sent out to the next combinational circuit. These control pins can be synchronous or asynchronous.
 
### 3.3.1 Lab- flop synthesis simulations
 
 **d-flipflop with asynchronous reset**- Here the output **q** goes low whenever reset is high and will not wait for the clock's posedge, i.e irrespective of clock, the output is changed to low.
 
![telegram-cloud-photo-size-5-6314223892675276834-y](https://user-images.githubusercontent.com/110079648/183966438-05a31060-6a35-44dc-8717-1a0f76fe4c3e.jpg)
 
	 module dff_asyncres ( input clk ,  input async_reset , input d , output reg q );
		always @ (posedge clk , posedge async_reset)
		begin
			if(async_reset)
				q <= 1'b0;
			else	
				q <= d;
		end
	endmodule

**Simulation**:

<img width="1399" alt="image" src="https://user-images.githubusercontent.com/110079648/183290396-885a6242-d63f-4065-b6e7-c27d5cc6ed58.png">

**Synthesized circuit**:

<img width="614" alt="image" src="https://user-images.githubusercontent.com/110079648/183290710-e4d5ddb0-eda1-4858-8f1f-0e8a80a6cb60.png">

 **d-flipflop with asynchronous set**- Here the output **q** goes high whenever set is high and will not wait for the clock's posedge, i.e irrespective of clock, the output is changed to high.
 

	module dff_async_set ( input clk ,  input async_set , input d , output reg q );
		always @ (posedge clk , posedge async_set)
		begin
			if(async_set)
				q <= 1'b1;
			else
				q <= d;
		end
	endmodule

**Simulation**:
<img width="1064" alt="image" src="https://user-images.githubusercontent.com/110079648/183290941-cb504489-0341-44a6-af7e-f7df80e0c7ec.png">

**Synthesized circuit**:

<img width="1121" alt="image" src="https://user-images.githubusercontent.com/110079648/183291077-9da7eba5-f056-4915-92f1-4058324fc11e.png">

**d-flipflop with synchronous reset**- Here the output **q** goes low whenever reset is high and at the positive edge of the clock. Here the reset of the output depends on the clock.



	module dff_syncres ( input clk , input async_reset , input sync_reset , input d , output reg q );
		always @ (posedge clk )
		begin
			if (sync_reset)
				q <= 1'b0;
			else	
				q <= d;
		end
	endmodule

**Simulation**:

<img width="1029" alt="image" src="https://user-images.githubusercontent.com/110079648/183291190-b8905649-4656-494f-a8c1-d3b54982e4c1.png">

**Synthesized circuit**:

<img width="993" alt="image" src="https://user-images.githubusercontent.com/110079648/183291417-c913ac11-7469-4b7b-b2cb-c19bdd6c0713.png">
**d-flipflop with synchronous and asynchronbous reset**- Here the output **q** goes low whenever asynchronous reset is high where output doesn't depend on clock and also when synchronous reset is high and posedge of clock occurs.

![telegram-cloud-photo-size-5-6314223892675276906-y](https://user-images.githubusercontent.com/110079648/183970239-3f159d5c-5802-43df-849a-0d6d2daa901a.jpg)

	module dff_asyncres_syncres ( input clk , input async_reset , input sync_reset , input d , output reg q );
		always @ (posedge clk , posedge async_reset)
		begin
			if(async_reset)
				q <= 1'b0;
			else if (sync_reset)
				q <= 1'b0;
			else	
				q <= d;
		end
	endmodule

**Simulation**:

<img width="1173" alt="image" src="https://user-images.githubusercontent.com/110079648/183291508-57a5a2c2-993e-4245-9b38-0cb111ccebad.png">

**Synthesized circuit**:

<img width="993" alt="image" src="https://user-images.githubusercontent.com/110079648/183291710-1ab1ee7f-511a-4ac2-8ca5-28fa07013262.png">

### 3.3.2 Interesting optimisations

This lab session deals with some automatic and interesting optimisations of the circuits based on logic. In the below example, multiplying a number with 2 doesn't need any additional hardeware and only needs connecting the bits from **a** to **y** and grounding the LSB bit of y is enough and the same is realized by Yosys.

	module mul2 (input [2:0] a, output [3:0] y);
		assign y = a * 2;
	endmodule

![telegram-cloud-photo-size-5-6314223892675276908-y](https://user-images.githubusercontent.com/110079648/183971707-8f04b9d0-2ef9-4160-ad44-12981cc97add.jpg)

**Synthesized circuit**:

<img width="943" alt="image" src="https://user-images.githubusercontent.com/110079648/183298732-209b9f9e-26f1-4cd7-aed5-2b553c8aafd8.png">

When it comes to multiplying with powers of 2, it just needs shifting as shown in the below image:

![telegram-cloud-photo-size-5-6314223892675276910-y](https://user-images.githubusercontent.com/110079648/183974153-5c183b58-2eba-4ea1-8bd5-6a792f994d17.jpg)

**Netlist for the above schematic**

<img width="896" alt="image" src="https://user-images.githubusercontent.com/110079648/183298792-63868767-65e4-432c-b92a-01bc5f95473f.png">

Special case of multiplying **a** with **9**. The result is shown in the below image:


The schematic for the same is shown below:

<img width="1141" alt="image" src="https://user-images.githubusercontent.com/110079648/183298941-6fea62bb-a835-4eec-b830-ed2f90ac509e.png">

**Netlist for the above schematic**

<img width="755" alt="image" src="https://user-images.githubusercontent.com/110079648/183299298-ae04036f-9c51-4958-9406-0557510a7f0c.png">

# 4. Day3- Combinational and sequential optmizations

## 4.1 Combinational logic optimization with examples

Optimising the combinational logic circuit is squeezing the logic to get the most optimized digital design so that the circuit finally is area and power efficient. This is achieved by the synthesis tool using various techniques and gives us the most optimized circuit.

**Techniques for optimization**:
- Constant propagation which is Direct optimizxation technique
- Boolean logic optimization using K-map or Quine McKluskey

Here is an example for **Constant Propagation**

![optimizations1](https://user-images.githubusercontent.com/104454253/166127772-9ff3dc8e-c5e2-4621-8070-d300df31667e.JPG)

In the above example, if we considor the trasnsistor level circuit of output Y, it has 6 MOS trasistors and when it comes to invertor, only 2 transistors will be sufficient. This is achieved by making A as contstant and propagating the same to output.

Example for **Boolean logic optimization**:

Let's consider an example concurrent statement **assign y=a?(b?c:(c?a:0)):(!c)**

The above expression is using a ternary operator which realizes a series of multiplexers, however, when we write the boolean expression at outputs of each mux and simplify them further using boolean reduction techniques, the outout **y** turns out be just **~(a^c)**

Command to optimize the circuit by yosys is **yosys> opt_clean -purge**

**Example-1**

![62695d29-f76d-426c-ab99-af6fbb2abda0](https://user-images.githubusercontent.com/104454253/166292324-f3243d68-55c1-4829-a836-8177edc79613.jpg)

	module opt_check (input a , input b , output y);
		assign y = a?b:0;
	endmodule

**Optimized circuit**

<img width="1136" alt="image" src="https://user-images.githubusercontent.com/110079648/183299741-343e0f72-abf2-45b1-b31c-2159eab3744f.png">

**Example-2**



	module opt_check2 (input a , input b , output y);
		assign y = a?1:b;
	endmodule

<img width="1126" alt="image" src="https://user-images.githubusercontent.com/110079648/183299819-375d3424-07b1-46c6-b316-9bdf3f52893e.png">

**Example-3**



	module opt_check3 (input a , input b, input c , output y);
		assign y = a?(c?b:0):0;
	endmodule

<img width="1189" alt="image" src="https://user-images.githubusercontent.com/110079648/183299877-a5460680-5619-4234-9fa2-6c3953517d4d.png">

**Example-4**



	module opt_check4 (input a , input b , input c , output y);
		assign y = a?(b?(a & c ):c):(!c);
	endmodule
 
<img width="1254" alt="image" src="https://user-images.githubusercontent.com/110079648/183299977-cd7b1bf7-47c2-497e-ad64-65f81ce0984d.png">

**Example- 5**



	module sub_module(input a , input b , output y);
		assign y = a & b;
	endmodule

	module multiple_module_opt2(input a , input b , input c , input d , output y);
		wire n1,n2,n3;
		sub_module U1 (.a(a) , .b(1'b0) , .y(n1));
		sub_module U2 (.a(b), .b(c) , .y(n2));
		sub_module U3 (.a(n2), .b(d) , .y(n3));
		sub_module U4 (.a(n3), .b(n1) , .y(y));
	endmodule

<img width="1293" alt="image" src="https://user-images.githubusercontent.com/110079648/183300152-abd895c7-2b74-49d2-8b53-252bc1a3956c.png">

**Example-6**



		module sub_module1(input a , input b , output y);
		 assign y = a & b;
		endmodule

		module sub_module2(input a , input b , output y);
		 assign y = a^b;
		endmodule

		module multiple_module_opt(input a , input b , input c , input d , output y);
		wire n1,n2,n3;
		sub_module1 U1 (.a(a) , .b(1'b1) , .y(n1));
		sub_module2 U2 (.a(n1), .b(1'b0) , .y(n2));
		sub_module2 U3 (.a(b), .b(d) , .y(n3));

		assign y = c | (b & n1); 
		endmodule

<img width="1332" alt="image" src="https://user-images.githubusercontent.com/110079648/183300208-1cffdafa-cffe-4293-913a-ed5ac7eb0401.png">

### 4.2 Sequential Logic Optimization with examples

Below are the various techniques used for sequential logic optimisations:<br />
-Basic
  - Sequential contant propagation
- Advanced
  - State optimisation
  - Retiming
  - Sequential Logic Cloning (Floor Plan Aware Synthesis)
 
#### 4.2.1 Basic

**Sequential contant propagation**- Here only the first logic can be optimized as the output of flop is always zero. However for the second flop, the output changes continuously, therefor it cannot be used for contant propagation.

![telegram-cloud-photo-size-5-6314223892675276922-y](https://user-images.githubusercontent.com/110079648/183978891-36f71300-ce08-4447-912d-ff23b5a9259a.jpg)



#### 4.2.2. Advanced
**State Optimisation**: This is optimisation of unused state. Using this technique we can come up with most optimised state machine.

**Cloning**: This is done when performing PHYSICAL AWARE SYNTHESIS. Lets consider a flop A which is connected to flop B and flop C through a combination logic. If B and C are placed far from A in the flooerplan, there is a routing path delay. To avoid this, we connect A to two intermediate flops and then from these flops the output is sent to B and C thereby decreasing the delay. This process is called cloning since we are generating two new flops with same functionality as A.

**Retiming**: Retiming is a powerful sequential optimization technique used to move registers across the combinational logic or to optimize the number of registers to improve performance via power-delay trade-off, without changing the input-output behavior of the circuit. 

**Example-1**<br />
Here flop will be inferred as the output is not constant. <br />

	module dff_const1(input clk, input reset, output reg q);
		always @(posedge clk, posedge reset)
		begin
			if(reset)
				q <= 1'b0;
			else
				q <= 1'b1;
		end
	endmodule



**Simulation**

<img width="1219" alt="image" src="https://user-images.githubusercontent.com/110079648/183300412-7431eec1-87db-4ff7-8a51-117984a56e12.png">

**Synthesis**<br />
In the synthesis report, we'll see that a Dflop was inferred in this example.

<img width="1304" alt="image" src="https://user-images.githubusercontent.com/110079648/183300468-f13b826e-61a2-46af-8880-f8c292544fda.png">

<img width="393" alt="image" src="https://user-images.githubusercontent.com/110079648/183300511-b5c34845-071c-4e2a-b9f7-2ac0d0eaba52.png">

**Example-2**<br />
Here flop will not be inferred as the output is always high. <br />

	module dff_const2(input clk, input reset, output reg q);
		always @(posedge clk, posedge reset)
		begin
			if(reset)
				q <= 1'b1;
			else
				q <= 1'b1;
		end
	endmodule



**Simulation**

<img width="986" alt="image" src="https://user-images.githubusercontent.com/110079648/183300599-09f4f07a-62b6-4c28-9605-20647112bd3a.png">

**Synthesis**

<img width="1258" alt="image" src="https://user-images.githubusercontent.com/110079648/183300671-05de59d9-d12d-4b3a-b34c-9ac331664000.png">

<img width="413" alt="image" src="https://user-images.githubusercontent.com/110079648/183300676-8aaefb62-6c4c-4e67-8bc5-5453676e1074.png">

**Example-3**

		module dff_const3(input clk, input reset, output reg q);
		reg q1;

		always @(posedge clk, posedge reset)
		begin
			if(reset)
			begin
				q <= 1'b1;
				q1 <= 1'b0;
			end
			else
			begin
				q1 <= 1'b1;
				q <= q1;
			end
		end
		endmodule


**Simulation***

<img width="1112" alt="image" src="https://user-images.githubusercontent.com/110079648/183301443-d86d29e5-03fc-467a-8793-1b80551a6c68.png">

**Synthesis**

<img width="1329" alt="image" src="https://user-images.githubusercontent.com/110079648/183301501-089afedc-f858-4319-8ac7-d2922e58841f.png">

**Example4**

		module dff_const4(input clk, input reset, output reg q);
		reg q1;

		always @(posedge clk, posedge reset)
		begin
			if(reset)
			begin
				q <= 1'b1;
				q1 <= 1'b1;
			end
		else
			begin
				q1 <= 1'b1;
				q <= q1;
			end
		end
		endmodule

**Simulation***

<img width="1097" alt="image" src="https://user-images.githubusercontent.com/110079648/183301555-3dabd0c9-0649-404c-a77d-ea454e35fd21.png">

**Synthesis**

<img width="1310" alt="image" src="https://user-images.githubusercontent.com/110079648/183301602-b0c47a98-b828-45e1-a0b8-80d1974a37fc.png">

**Example5**

		module dff_const5(input clk, input reset, output reg q);
		reg q1;
		always @(posedge clk, posedge reset)
			begin
				if(reset)
				begin
					q <= 1'b0;
					q1 <= 1'b0;
				end
			else
				begin
					q1 <= 1'b1;
					q <= q1;
				end
			end
		endmodule

**Simulation***

<img width="1118" alt="image" src="https://user-images.githubusercontent.com/110079648/183301643-739ba512-9a9d-4651-a247-d46ea9cea706.png">

**Synthesis**

<img width="1329" alt="image" src="https://user-images.githubusercontent.com/110079648/183301680-6a1528e8-c966-4f81-b676-a986bcd97e0e.png">

### 4.2.3 Sequential optimisation of unused outputs
**Example1**

		module counter_opt (input clk , input reset , output q);
		reg [2:0] count;
		assign q = count[0];
		always @(posedge clk ,posedge reset)
		begin
			if(reset)
				count <= 3'b000;
			else
				count <= count + 1;
		end
		endmodule
		


**Synthesis**

<img width="1266" alt="image" src="https://user-images.githubusercontent.com/110079648/183301750-6716f80c-32a5-449a-9ed5-584df5526eab.png">

**Updated counter logic-** 

	module counter_opt (input clk , input reset , output q);
		reg [2:0] count;
		assign q = {count[2:0]==3'b100};
		always @(posedge clk ,posedge reset)
		begin
		if(reset)
			count <= 3'b000;
		else
			count <= count + 1;
		end
	endmodule



**Synthesis**

All the other blocks in synthesizer are for incrementing the counter but the output is only from the three input NOR gate.

<img width="1176" alt="image" src="https://user-images.githubusercontent.com/110079648/183301840-f09fcbd5-3e75-428a-b734-d4d27c086fd4.png">

# 5. DAY4- GLS, blocking vs non-blocking and Synthesis-Simulation mismatch

# 5.1 GLS, Synthesis-Simulation mismatch and Blocking, Non-blocking statements
### 5.1.1 GLS Concepts And Flow Using Iverilog

**What is GLS- Gate Level Simulation?**:<br />
GLS is generating the simulation output by running test bench with netlist file generated from synthesis as design under test. Netlist is logically same as RTL code, therefore, same test bench can be used for it.

**Why GLS?**:<br />
We perform this to verify logical correctness of the design after synthesizing it. Also ensuring the timing of the design is met.

Below picture gives an insight of the procedure. Here while using iverilog, we also include gate level verilog models to generate GLS simulation.

![Screenshot (49)](https://user-images.githubusercontent.com/104454253/166256679-1ac9167a-1358-4c60-bbdb-0f6423f0faa3.png)

### 5.1.2 Synthesis Simulation Mismatch

There are three main reasons for Synthesis Simulation Mismatch:<br />
- Missing sensitivity list in always block
- Blocking vs Non-Blocking Assignments
- Non standard Verilog coding

**Missing sensitivity list in always block:**<br />

If the consider - Example-2, we can see the only **sel** is mentioned in the sensitivity list. During the simulation, the waveforms will resemble a latched output but the simulation of netlist will not infer this as the synthesizer will only look at the statements with in the procedural block and not the sensitivity list.

As the synthesizer doen't look for sensitivity list and it looks only for the statements in procedural block, it infers correct  circuit  and if we simulate the netlist code, there will be a synthesis simulation mismatch.

To avoid the synthesis and simulation mismatch. It is very important to check the behaviour of the circuit first and then match it with the expected output seen in simulation and make sure there are no synthesis and simulation mismatches. This is why we use GLS.

**Blocking vs Non-Blocking Assignments**:

Blocking statements execute the statemetns in the order they are written inside the always block. Non-Blocking statements execute all the RHS and once always block is entered, the values are assigned to LHS. This will give mismatch as sometimes, improper use of blocking statements can create latches. Please click here to go to example - Example4

## 5.2 Lab- GLS Synth Sim Mismatch

**Example-1**

	module ternary_operator_mux (input i0 , input i1 , input sel , output y);
		assign y = sel?i1:i0;
	endmodule
	
**Simulation**

<img width="1210" alt="image" src="https://user-images.githubusercontent.com/110079648/183302033-64b5795e-c317-4315-ae29-578529b0d526.png">

**Synthesis**

<img width="1318" alt="image" src="https://user-images.githubusercontent.com/110079648/183302083-a1f9ee6b-8434-4f93-b27a-c0903a38202a.png">

**Netlist Simulation**

<img width="1401" alt="image" src="https://user-images.githubusercontent.com/110079648/183302375-e77c79d7-1eb0-44d3-ba3e-b6b508e3735f.png">

# Example-2

	module bad_mux (input i0 , input i1 , input sel , output reg y);
		always @ (sel)
		begin
			if(sel)
				y <= i1;
			else 
				y <= i0;
		end
	endmodule

**Simulation**

<img width="1370" alt="image" src="https://user-images.githubusercontent.com/110079648/183302435-957b52f3-e26c-46cf-842d-4865a6c56789.png">

**Synthesis**

<img width="1378" alt="image" src="https://user-images.githubusercontent.com/110079648/183302481-332d58bc-4ddc-4444-874f-cbf541054cc8.png">

**Netlist Simulation**

<img width="1389" alt="image" src="https://user-images.githubusercontent.com/110079648/183302582-b8f61e11-717a-4ebb-a679-525381e1b578.png">

**MISMATCH**<br />

![Capture](https://user-images.githubusercontent.com/104454253/166259062-c3cb7fb2-e28a-4e12-b2d6-83812c5f2582.JPG)

**Example-3**

	module good_mux (input i0 , input i1 , input sel , output reg y);
		always @ (*)
		begin
			if(sel)
				y <= i1;
			else 
				y <= i0;
		end
	endmodule
	
**Simulation**

<img width="1383" alt="image" src="https://user-images.githubusercontent.com/110079648/183302664-efa38fa4-647a-4b56-be9c-40caf9d45785.png">

**Synthesis**

<img width="1402" alt="image" src="https://user-images.githubusercontent.com/110079648/183302727-ce33a8c4-97af-4a21-97ac-fac2eb898919.png">

**Netlist Simulation**

<img width="1396" alt="image" src="https://user-images.githubusercontent.com/110079648/183302799-4f0a3402-e9f7-4b06-ad18-41af34f301ac.png">

## 5.3 Lab- Synthesis simulation mismatch blocking statement

Here the output is depending on the past value of x which is dependednt on a and b and it appears like a flop.

# Example4

	module blocking_caveat (input a , input b , input  c, output reg d); 
	reg x;
	always @ (*)
		begin
		d = x & c;
		x = a | b;
	end
	endmodule

**Simulation**

!<img width="1386" alt="image" src="https://user-images.githubusercontent.com/110079648/183302865-3c28005c-fd9c-45e6-a3cc-62ba220622fd.png">

**Synthesis**

<img width="1398" alt="image" src="https://user-images.githubusercontent.com/110079648/183302910-fe4cc852-56bb-4666-9ec6-0a2a4523f14a.png">

**Netlist Simulation**

<img width="1397" alt="image" src="https://user-images.githubusercontent.com/110079648/183303006-ac1cb710-6660-4a26-9c81-17f412fca5d6.png">

**MISMATCH**

![Capture2](https://user-images.githubusercontent.com/104454253/166260812-1832a4ba-8f95-4de3-8129-6b6c39ed17ce.JPG)

# 6. DAY5- if, case, for loop and for generate

## 6.1 If and Case constructs

### 6.1.1 If construct
The construct **if** is mainly used to create priority logic. In a nested if else construct, the conditions are given priority from top to bottom. Only if the condition is satisfied, if statement is executed and the compiler comes out of the block. If condition fails, it checks for next condition and so on as shown below.

**Syntax for nested if else**

	if (<condition 1>)
	begin
	-----------
	-----------
	end
	else if (<condition 2>)
	begin
	-----------
	-----------
	end
	else if (<condition 3>)
	.
	.
	.
	
**Dangers with IF**:

If use a bad coding style i.e, using incomplete if else constructs will infer a latch. We definetly don't require an unwanted latch in a combinational circuit.
When an incomplete construct is used, if all the conditions are failed, the input is latched to the output and hence we don't get desired output unless we need a latch.

This can be shown in below example:

![telegram-cloud-photo-size-5-6314223892675276920-y](https://user-images.githubusercontent.com/110079648/183977733-7c117d11-c336-451b-831e-1d09b3ced053.jpg)

### 6.1.2 Case construct

**Syntax**

	case(statement)
	  case1: begin
  	       --------
		 --------
		 end
 	 case2: begin
    	     --------
		 --------
		 end
 	 default:
	 endcase
 
 In case construct, the execution checks for all the case statements and whichever satisfies the statement, that particular statement is executed.If there is no match, the default statement is executed. But here unlike if construct, the execution doesn't stop once statement is satisfied, but it continues further.
 
**Caveats in Case**<br />
Caveats in case occur due to two reasons. One is **incomplete case statements** and the other is **partial assignments in case statements.**

## 6.2 Lab- Incomplete IF

This incomplete if construct forms a connection between i0 and output y i.e, D-latch with input as i1 and i0 will be the enable for it.<br />
**Example-1**

	module incomp_if (input i0 , input i1 , input i2 , output reg y);
	always @ (*)
	begin
		if(i0)
			y <= i1;
	end
	endmodule

**Simulation**

<img width="1385" alt="image" src="https://user-images.githubusercontent.com/110079648/183303149-dc9fc4f9-7bf5-4896-9519-f4c83ed8d0e5.png">

**Synthesis**

<img width="1394" alt="image" src="https://user-images.githubusercontent.com/110079648/183303227-f71bfd6f-c5eb-4058-9283-87b429e8bd9d.png">

**Example-2**<br />
The below code is equivalent to two 2:1 mux with i0 and i2 as select lines with i1 and i3 as inputs respectively. Here as well, the output is connected back to input in the form of a latch with an enable input of **OR** of i0 and i2.

	module incomp_if2 (input i0 , input i1 , input i2 , input i3, output reg y);
		always @ (*)
		begin
			if(i0)
				y <= i1;
			else if (i2)
				y <= i3;
		end
	endmodule

**Simulation**

<img width="1380" alt="image" src="https://user-images.githubusercontent.com/110079648/183303275-fecafb40-7a62-4f86-8454-98ff6f849c36.png">

**Synthesis**

<img width="1387" alt="image" src="https://user-images.githubusercontent.com/110079648/183303313-224b2748-24c0-41b6-baa6-e419fa7a9354.png">

## 6.3 Lab- incomplete overlapping Case

**Example-1**<br />
Thie is an example of incomplete case where other two combinations 10 and 11 were not included. This is infer a latch for the multiplexer and connect i2 and i3 with the output.

	module incomp_case (input i0 , input i1 , input i2 , input [1:0] sel, output reg y);
		always @ (*)
		begin
		case(sel)
			2'b00 : y = i0;
			2'b01 : y = i1;
		endcase
		end
	endmodule

**Simulator**

<img width="1395" alt="image" src="https://user-images.githubusercontent.com/110079648/183303376-5d940333-017c-4103-a2d9-d396e067bd00.png">

**Synthesis**

<img width="1399" alt="image" src="https://user-images.githubusercontent.com/110079648/183303438-58278163-7b65-459b-82fb-402fbe1ad0d0.png">

**Example-2- Complete case**

This is the case of complete case statements as the default case is given. If the actual case statements don't execute, the compiler directly executes the default statements and a latch is not inferred.

	module comp_case (input i0 , input i1 , input i2 , input [1:0] sel, output reg y);
	always @ (*)
	begin
		case(sel)
			2'b00 : y = i0;
			2'b01 : y = i1;
			default : y = i2;
		endcase
	end
	endmodule

**Simulation**

<img width="1397" alt="image" src="https://user-images.githubusercontent.com/110079648/183303481-2efa5580-ee52-489d-a95a-10173590c1b1.png">

**Synthesis**

<img width="1404" alt="image" src="https://user-images.githubusercontent.com/110079648/183303516-405881a4-15a5-440f-8edc-bd224a6eba2f.png">

**Example-3**<br />
In the below example, y is present in all the case statements and it had particular outut for all cases. There no latch is inferred in case of y. 
When it comes to x, it is not assigned for the input 01, therefore a latch is inferred here.

	module partial_case_assign (input i0 , input i1 , input i2 , input [1:0] sel, output reg y , output reg x);
	always @ (*)
	begin
		case(sel)
			2'b00 : begin
				y = i0;
				x = i2;
				end
			2'b01 : y = i1;
			default : begin
		         	  x = i1;
				  y = i2;
			 	 end
		endcase
	end
	endmodule

**Simulation**

<img width="1348" alt="image" src="https://user-images.githubusercontent.com/110079648/183303554-40076c39-8ff8-47ab-816e-bfe2beacd549.png">

**Synthesis**

<img width="1384" alt="image" src="https://user-images.githubusercontent.com/110079648/183304002-883bbfd3-338a-45ab-9c7b-051e0a879a91.png">

**Example-4-Bad case construct**

	module bad_case (input i0 , input i1, input i2, input i3 , input [1:0] sel, output reg y);
	always @(*)
	begin
		case(sel)
			2'b00: y = i0;
			2'b01: y = i1;
			2'b10: y = i2;
			2'b1?: y = i3;
			//2'b11: y = i3;
		endcase
	end
	endmodule
	
**Simulation**

<img width="1360" alt="image" src="https://user-images.githubusercontent.com/110079648/183304053-250d43fc-749f-44c3-bd5f-104ada1046a6.png">

**Synthesis**

<img width="1364" alt="image" src="https://user-images.githubusercontent.com/110079648/183304095-2f92a1d8-a79f-4f78-9b0c-8772df3be2a0.png">

**Netlist simulation**

<img width="1390" alt="image" src="https://user-images.githubusercontent.com/110079648/183304172-0766b521-2808-434e-b6f4-afb066ef99b3.png">

## 6.4 For Loop and For Generate

**For Loop**<br />
- For look is used in always block
- It is used for excecuting expressions alone

**Generate For loop**<br />
- Generate for loop is used for instantaing hardware
- It should be used only outside always block

For loop can be used to generate larger circuits like 256:1 multiplexer or 1-256 demultiplexer where the coding style of smaller mux is not feesible and can have human errors since we would need to include huge number of combinations.

FOR Generate can be used to instantiate any number of sub modules with in a top module. For example, if we need a 32 bit ripple carry adder, instead of instantiating 32 full adders, we can write a generate for loop and connect the full adders appropriately.

## 6.5 Lab- For and For Generate

**Example-1- Mux using generate**<br />
Here for loop is used to design a 4:1 mux. This can also be written using case or if else block, however, for a large size mux, only for loop model is feasible.

	module mux_for (input i0 , input i1, input i2 , input i3 , input [1:0] sel  , output reg y);
		wire [3:0] i_int;
		assign i_int = {i3,i2,i1,i0};
		integer k;
	always @ (*)
		begin
		for(k = 0; k < 4; k=k+1) begin
			if(k == sel)
			y = i_int[k];
			end
		end
	endmodule

**Simulation**

!<img width="1378" alt="image" src="https://user-images.githubusercontent.com/110079648/183304233-a13a8370-53b7-4994-bc44-e41795c6f2ec.png">

**Synthesis**

<img width="1239" alt="image" src="https://user-images.githubusercontent.com/110079648/183304295-b8f3c404-805a-41c2-8399-f54ca919dad9.png">

**Netlist Simulation**

<img width="1381" alt="image" src="https://user-images.githubusercontent.com/110079648/183304437-6909c1a4-1afc-41e2-aeba-46d3d4a40c9c.png">

**Example-2-Demux using Case**

	module demux_case (output o0 , output o1, output o2 , output o3, output o4, output o5, output o6 , output o7 , input [2:0] sel  , input i);
	reg [7:0]y_int;
	assign {o7,o6,o5,o4,o3,o2,o1,o0} = y_int;
	integer k;
	always @ (*)
	begin
	y_int = 8'b0;
	case(sel)
		3'b000 : y_int[0] = i;
		3'b001 : y_int[1] = i;
		3'b010 : y_int[2] = i;
		3'b011 : y_int[3] = i;
		3'b100 : y_int[4] = i;
		3'b101 : y_int[5] = i;
		3'b110 : y_int[6] = i;
		3'b111 : y_int[7] = i;
	endcase
	end
	endmodule

**Simulation**

![sim_demux_case](https://user-images.githubusercontent.com/104454253/166215304-63d9a1ef-6840-4d13-a096-7f728a706d76.JPG)

**Synthesis**

![synthdemux_case](https://user-images.githubusercontent.com/104454253/166215350-db45ff4f-6346-45ed-8d0d-40db10039446.JPG)

**Netlist Simulation**

<img width="1397" alt="image" src="https://user-images.githubusercontent.com/110079648/183326337-5414c1e9-fe58-4f62-8ac6-16951d8903d4.png">


**Example-3-Demux using Generate**

The code in above example is big and also there is a chance of human error wile writing the code. However, using for loop as shown below, this drawback can be elimiated to a great extent.

	module demux_generate (output o0 , output o1, output o2 , output o3, output o4, output o5, output o6 , output o7 , input [2:0] sel  , input i);
	reg [7:0]y_int;
	assign {o7,o6,o5,o4,o3,o2,o1,o0} = y_int;
	integer k;
	always @ (*)
	begin
		y_int = 8'b0;
		for(k = 0; k < 8; k++) begin
			if(k == sel)
			y_int[k] = i;
		end
	end
	endmodule

**Simulation**

<img width="1393" alt="image" src="https://user-images.githubusercontent.com/110079648/183326508-ce2b8e9f-b897-40b7-9d83-ecc37527035e.png">

**Synthesis**

<img width="1355" alt="image" src="https://user-images.githubusercontent.com/110079648/183326640-05500b7f-984f-4289-bab7-41442981ed1c.png">

**Netlist Simulation**

<img width="1404" alt="image" src="https://user-images.githubusercontent.com/110079648/183326840-e8e0a413-b846-4296-b4e1-728d88d78be6.png">


**Example-4- Ripple carry adder using fulladder**

In this Ripple carry adder example, unlike instantiating fulladder for 8 times, generate for loop is used to instantiate the fulladder for 7 times and only for first full adder, it is instantiated seperately. Using the same code, just by changing bus sizes and condition of for loop, we can design any required size of ripple carry adder.

	module rca (input [7:0] num1 , input [7:0] num2 , output [8:0] sum);
	wire [7:0] int_sum;
	wire [7:0]int_co;

	genvar i;
	generate
		for (i = 1 ; i < 8; i=i+1) begin
			fa u_fa_1 (.a(num1[i]),.b(num2[i]),.c(int_co[i-1]),.co(int_co[i]),.sum(int_sum[i]));
		end

	endgenerate
	fa u_fa_0 (.a(num1[0]),.b(num2[0]),.c(1'b0),.co(int_co[0]),.sum(int_sum[0]));


	assign sum[7:0] = int_sum;
	assign sum[8] = int_co[7];
	endmodule

	module fa (input a , input b , input c, output co , output sum);
	endmodule

**Simulation**

<img width="1399" alt="image" src="https://user-images.githubusercontent.com/110079648/183327027-71ee2742-c1d3-4131-b3f6-7bbeba082eab.png">

**Synthesis**

<img width="1323" alt="image" src="https://user-images.githubusercontent.com/110079648/183327385-9bde3a73-6c32-48f7-9408-73cc7f7296df.png">

**Netlist Simulation**

<img width="1394" alt="image" src="https://user-images.githubusercontent.com/110079648/183327762-bfedcb42-0d4a-4839-9556-8b8e39357dc2.png">

# 7. Word of Thanks
I sciencerly thank **Mr. Kunal Gosh**(Founder/**VSD**) for helping me out to complete this flow smoothly.

