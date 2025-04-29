# Counter_4bit_up-down

## Aim:

To write a verilog code for 4bit up/down counter and verify the functionality using Test bench. 

## Tools used for ASIC Flow:

1.	nclaunch- Used for Functional Simulation
   
## Design Information and Bock Diagram:

	An up/down counter is a digital counter which can be set to count either from 0 to
MAX_VALUE or MAX_VALUE to 0.

	The direction of the count(mode) is selected using a single bit input. The module has 3 inputs - clk, reset which is active high and a Up Or Down mode input. 
The output is Counter which is 4 bit in size.

	When Up mode is selected, counter counts from 0 to 15 and then again from 0 to 15.

	When Down mode is selected, counter counts from 15 to 0 and then again from 15 to 0.

	Changing mode doesn't reset the Count value to zero.

	You have to apply high value to reset, to reset the Counter output.
 
![image](https://github.com/user-attachments/assets/efe1095e-989e-4005-b53b-e9dc50d4025c)

## Fig 1: 4 Bit Up/Down Counter

## Creating a Work space :

	Create a folder in your name (Note: Give folder name without any space) and Create a new sub-Directory name it as Exp2 or counter_design for the Design and open a terminal from the Sub-Directory.
Functional Simulation: 

	Invoke the cadence environment by type the below commands 

	tcsh (Invokes C-Shell) 

	source /cadence/install/cshrc (mention the path of the tools) 
      (The path of cshrc could vary depending on the installation destination)
      
	After this you can see the window like below 

![Screenshot 2025-04-29 145942](https://github.com/user-attachments/assets/1cee3db4-4e52-44a0-b696-a481ea4ee0e1)

## Fig 2: Invoke the Cadence Environment


## Creating Source Code:

	In the Terminal, type gedit <filename>.v or <filename>.vhdl depending on the HDL Language you are to use (ex: 4b_up_downCount.v).

	A Blank Document opens up into which the following source code can be typed down.

(Note : File name should be with HDL Extension)

### Verilog code for 4-Bit Up-Down Counter:
```
module up_down_counter (
    input wire clk,        // Clock input
    input wire reset,      // Asynchronous reset
    input wire up_down,    // Control signal: 1 = count up, 0 = count down
    output reg [3:0] count // 4-bit counter output
);

always @(posedge clk or posedge reset) begin
    if (reset) begin
        count <= 4'b0000; // Reset the counter
    end else begin
        if (up_down) begin
            count <= count + 1; // Count up
        end else begin
            count <= count - 1; // Count down
        end
    end
end

endmodule
```

	Use Save option or Ctrl+S to save the code or click on the save option from the top most right corner and close the text file.

## Creating Test bench:

	Similarly, create your test bench using gedit <filename_tb>.v or <filename_tb>.vhdl to open a new blank document (4bitup_down_count_tb.v).

### Test-bench code for 4-Bit Up-Down Counter:

```
`timescale 1ns / 1ps

module tb_up_down_counter;

    // Testbench signals
    reg clk;
    reg reset;
    reg up_down;
    wire [3:0] count;

    // Instantiate the counter
    up_down_counter uut (
        .clk(clk),
        .reset(reset),
        .up_down(up_down),
        .count(count)
    );

    // Clock generation: 10ns period
    always #5 clk = ~clk;

    initial begin
        // Initialize signals
        clk = 0;
        reset = 1;
        up_down = 1; // Start with count up

        // Hold reset for a short time
        #10;
        reset = 0;

        // Count up for 10 clock cycles
        #100;

        // Change to count down
        up_down = 0;

        // Count down for 10 clock cycles
        #100;

        // Trigger reset
        reset = 1;
        #10;
        reset = 0;

        // Count up again
        up_down = 1;
        #50;

        // Finish simulation
        $finish;
    end

    // Monitor count value
    initial begin
        $monitor("Time = %0t | Reset = %b | Up_Down = %b | Count = %b", 
                  $time, reset, up_down, count);
    end

endmodule

```

### To Launch Simulation tool
	linux:/> nclaunch -new&            // “-new” option is used for invoking NCVERILOG for the first time for any design

	linux:/> nclaunch&                 // On subsequent calls to NCVERILOG

It will invoke the nclaunch window for functional simulation we can compile,elaborate and simulate it using Multiple step

![Screenshot 2025-04-29 145951](https://github.com/user-attachments/assets/0053eda3-8ddd-4f5c-a1d3-a2230602b6a5)

## Fig 3: Setting Multi-step simulation

Select Multiple Step and then select “Create cds.lib File” as shown in below figure

Click the cds.lib file and save the file by clicking on Save option

![Screenshot 2025-04-29 150005](https://github.com/user-attachments/assets/46440d44-55aa-4a1e-9050-1046481e653c)

## Fig 4: cds.lib file Creation

	Save cds.lib file and select the correct option for cds.lib file format based on the  HDL Language and Libraries used.

	Select “Don’t include any libraries (verilog design)” from “New cds.lib file” and click on “OK” as in below figure

	We are simulating verilog design without using any libraries

![Screenshot 2025-04-29 150015](https://github.com/user-attachments/assets/1d0dbe5b-d09a-413e-bf7f-a6e037aaf3ac)

## Fig 5: Selection of Don’t include any libraries

	A Click “OK” in the “nclaunch: Open Design Directory” window

	A ‘NCLaunch window’ appears as shown in figure below

	Left side you can see the HDL files. Right side of the window has worklib and snapshots directories listed.

	Worklib is the directory where all the compiled codes are stored while Snapshot will have output of elaboration which in turn goes for simulation

![Screenshot 2025-04-29 150035](https://github.com/user-attachments/assets/ef3e9d32-7694-4165-8097-768d7433d4b7)

## Fig 6: Nclaunch Window

To perform the function simulation, the following three steps are involved Compilation, Elaboration and Simulation.

## Step 1: Compilation:– Process to check the correct Verilog language syntax and usage 

	Inputs: Supplied are Verilog design and test bench codes 

	Outputs: Compiled database created in mapped library if successful, generates report else error reported in log file 

## Steps for compilation:

1. Create work/library directory (most of the latest simulation tools creates automatically)
  
2. Map the work to library created (most of the latest simulation tools creates automatically)
  
3. Run the compile command with compile options

i.e Cadence IES command for compile: ncverilog +access+rwc -compile fa.v

Left side select the file and in Tools : launch verilog compiler with current selection will get enable. Click it to compile the code 

Worklib is the directory where all the compiled codes are stored while Snapshot will have output of elaboration which in turn goes for simulation 

![Screenshot 2025-04-29 150130](https://github.com/user-attachments/assets/c2fffdb6-4f4b-4a84-bbf4-974ee814aa4c)

## Fig 7: Compiled database in worklib

	After compilation it will come under worklib you can see in right side window

	Select the test bench and compile it. It will come under worklib. Under Worklib you can see the module and test-bench. 

	The cds.lib file is an ASCII text file. It defines which libraries are accessible and where they are located.
It contains statements that map logical library names to their physical directory paths. For this Design, you will define a library called “worklib”

## Step 2: Elaboration:– To check the port connections in hierarchical design 

	Inputs: Top level design / test bench Verilog codes

	Outputs: Elaborate database updated in mapped library if successful, generates report else error reported in log file 

	Steps for elaboration – Run the elaboration command with elaborate options 

1.	It builds the module hierarchy
	
3.	Binds modules to module instances
  
5.	Computes parameter values
  
7.	Checks for hierarchical names conflicts
  
9.	It also establishes net connectivity and prepares all of this for simulation
    
	After elaboration the file will come under snapshot. Select the test bench and simulate it. 

![Screenshot 2025-04-29 150632](https://github.com/user-attachments/assets/f34e897b-a543-4d9b-bd4c-cae1b8e4d78b)

## Fig 8: Elaboration Launch Option

### Step 3: Simulation: – Simulate with the given test vectors over a period of time to observe the output behaviour. 

	Inputs: Compiled and Elaborated top level module name 

	Outputs: Simulation log file, waveforms for debugging 

	Simulation allow to dump design and test bench signals into a waveform 

	Steps for simulation – Run the simulation command with simulator options

![Screenshot 2025-04-29 150705 - Copy](https://github.com/user-attachments/assets/54640d9a-431b-4041-9e88-a7d5c212fc22)

## Fig 9: Design Browser window for simulation

![Screenshot 2025-04-29 150722](https://github.com/user-attachments/assets/bd96a8a3-dd80-4080-8ee1-de20f968d9b2)

## Fig 10: Simulation Waveform Window

![Screenshot 2025-04-29 150821](https://github.com/user-attachments/assets/73f88f6e-7ba6-4c9b-b645-83b682d0204d)

## Fig 11: Simulation Waveform Window

### Result

The functionality of a 4bit_up-down asynchronous reset Counter was successfully verified using a test bench and simulated with the nclaunch tool.

