# 8x1-mux-using-2x1-mux-
# EXPERIMENT 1: Design and Implementation of 8:1 Multiplexer using 2:1 Multiplexers
## AIM
To design, implement, and verify an 8-to-1 multiplexer using 2-to-1 multiplexers in Verilog HDL and simulate it using Vivado.

## TOOLS / SOFTWARE REQUIRED
Vivado
PC with Windows OS

## THEORY
A multiplexer (MUX) is a combinational circuit that selects one input from multiple inputs and routes it to a single output line based on selection lines.
An 8:1 multiplexer has:
8 input lines (I₀–I₇)
3 selection lines (S₂, S₁, S₀)
1 output (Y)
Using 2:1 multiplexers as building blocks:
A 2:1 MUX selects one of two inputs based on one select line.
To construct an 8:1 MUX, we need seven 2:1 MUXes arranged hierarchically.

## Hierarchy:
First stage: 4 MUXes combine 8 inputs → 4 outputs
Second stage: 2 MUXes combine 4 outputs → 2 outputs
Final stage: 1 MUX selects between 2 outputs → final output

## PROCEDURE

1.Open Vivado → Create a New Project.
2.Create a new Verilog HDL file for the 8:1 MUX circuit.
3.Write and save the code as mux8x1_using_2x1.v.
4.Create another Verilog file for the testbench and save it as tb_mux8x1_using_2x1.v.
5.Compile both files and check for errors.
6.Run Simulation 
7.Add signals (d, sel, and y)
8.Run simulation for sufficient time (e.g., 100 ns)
9.Observe the waveform and verify that output matches the expected input for the given select lines.

## Verilog code :
```
// 2x1 Multiplexer module
module mux2x1 (
    input d0, d1,   // data inputs
    input sel,      // select input
    output y        // output
);
    assign y = sel ? d1 : d0;
endmodule
// 8x1 Multiplexer using 2x1 MUX
module mux8x1 (
    input [7:0] d,      // 8 data inputs
    input [2:0] sel,    // 3 select inputs
    output y            // output
);
    wire [3:0] w1;  // outputs of first stage
    wire [1:0] w2;  // outputs of second stage

    // First stage (4 muxes)
    mux2x1 m1 (d[0], d[1], sel[2], w1[0]);
    mux2x1 m2 (d[2], d[3], sel[2], w1[1]);
    mux2x1 m3 (d[4], d[5], sel[2], w1[2]);
    mux2x1 m4 (d[6], d[7], sel[2], w1[3]);

    // Second stage (2 muxes)
    mux2x1 m5 (w1[0], w1[1], sel[1], w2[0]);
    mux2x1 m6 (w1[2], w1[3], sel[1], w2[1]);

    // Final stage (1 mux)
    mux2x1 m7 (w2[0], w2[1], sel[0], y);
endmodule
```

## Testbench
```
timescale 1ns/1ps
module tb_mux8x1;
    reg [7:0] d;
    reg [2:0] sel;
    wire y;
    // Instantiate DUT (Device Under Test)
    mux8x1 uut (
        .d(d),
        .sel(sel),
        .y(y)
    );
    initial begin
        // Monitor signals
        $monitor("Time=%0t | d=%b | sel=%b | y=%b", $time, d, sel, y);
        // Test sequence
        d = 8'b10101010; sel = 3'b000; #10;
        sel = 3'b001; #10;
        sel = 3'b010; #10;
        sel = 3'b011; #10;
        sel = 3'b100; #10;
        sel = 3'b101; #10;
        sel = 3'b110; #10;
        sel = 3'b111; #10;
        // Another input pattern
        d = 8'b11001100; sel = 3'b000; #10;
        sel = 3'b111; #10;
        $finish;
    end
endmodule
```

## OUTPUT

## RESULT
The 8:1 Multiplexer was successfully designed and simulated using 2:1 MUXes in Verilog HDL.
The simulation results and waveforms verified the correct functionality of the design — the output corresponds to the input selected by the 3-bit select line.
