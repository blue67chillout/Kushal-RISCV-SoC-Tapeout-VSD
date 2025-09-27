# Introduction to Digital Design & Simulation Basics

This README explains the common terminologies in digital hardware design and verification, and shows how to use **Icarus Verilog (iverilog)** to run simulations.

---

## 🔹 Key Terminologies

### 1. **Design (DUT – Device Under Test)**
- The **hardware logic** you want to implement and verify.
- Written in **HDL** (Verilog, SystemVerilog, VHDL).
- Example: a 4-bit adder, a multiplier, or an FSM.

**Example: `adder.v`**
```verilog
module adder (
    input  [3:0] a, b,
    output [4:0] sum
);
    assign sum = a + b;
endmodule

```
### 2. **Testbench**
	•	A Verilog module that tests your design.
	•	Contains no inputs/outputs (top-level module).
	•	It instantiates the DUT, generates inputs (stimuli), and observes outputs.

⸻

### 3. **Testbench Setup**

A testbench usually includes:
	1.	Stimuli Generator
Provides test inputs to the DUT. Examples:
	•	Hardcoded signals,
	•	File-driven ($readmemb, $readmemh),
	•	Randomized.

```
initial begin
   a = 4'b0011; b = 4'b0101;
   #10 a = 4'b1111; b = 4'b0001;
end

```
	2.	Observer (Checker/Monitor)
Captures DUT outputs and either prints them or checks against expected values.

```
always @(*) begin
   $display("Time=%0t: a=%b, b=%b, sum=%b", $time, a, b, sum);
end
```
### 4. **Simulator**
	•	Hardware description languages don’t run directly on CPUs.
	•	A simulator compiles the HDL into a model and executes it step-by-step in simulated time.

Icarus Verilog (iverilog):
	•	Open-source Verilog simulator.
	•	Two main commands:
	•	iverilog → compiles Verilog into a simulation executable.
	•	vvp → runs the executable (executes the simulation).

⸻

### 🔹 Example Flow with Icarus Verilog

Files:
	•	adder.v → design (DUT).
	•	tb_adder.v → testbench.

tb_adder.v

```
`timescale 1ns/1ps
module tb_adder;
    reg [3:0] a, b;
    wire [4:0] sum;

    // Instantiate DUT
    adder uut (
        .a(a),
        .b(b),
        .sum(sum)
    );

    initial begin
        // Apply test vectors
        a = 4'b0011; b = 4'b0101; #10;
        a = 4'b1111; b = 4'b0001; #10;
        $finish;
    end

    initial begin
        // Monitor output
        $monitor("Time=%0t | a=%b b=%b sum=%b", $time, a, b, sum);
    end
endmodule

```

### Compile & Run Simulation

```
# Compile design + testbench
iverilog -o sim.out adder.v tb_adder.v

# Run simulation
vvp sim.out

```
### Generate Waveforms (for GTKWave)

Add in testbench

```
initial begin
    $dumpfile("wave.vcd");   // output file
    $dumpvars(0, tb_adder); // dump all signals in tb_adder
end

```
Then run,

```
vvp sim.out
gtkwave wave.vcd

```




