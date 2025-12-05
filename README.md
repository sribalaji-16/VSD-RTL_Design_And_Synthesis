# VSD-RTL_Design_And_Synthesis
## Day 1 – Introduction, Linux Commands & Basic Simulation

The first session covered essential Linux usage for RTL development, including directory navigation, file handling, and tool execution. I gained a clear understanding of RTL code, testbench roles, and simulation output. Using iverilog for compilation and GTKWave for waveform analysis, I developed basic combinational Verilog modules, created testbenches, and studied signal behavior through .vcd waveform files. This provided the foundational skills required for simulation-based design verification.

## Day 2 – RTL Synthesis with Yosys

This day introduced the concept of RTL synthesis and how hardware is derived from Verilog descriptions. I used Yosys commands to analyze, optimize, and synthesize RTL designs, ensuring the correct top module was specified. From the generated gate-level netlists, I observed how RTL operations are mapped into standard-cell components such as logic gates and flip-flops, strengthening the connection between abstract RTL code and actual hardware logic.

## Day 3 – Gate-Level Simulation (GLS) & Debugging

The focus of this session was Gate-Level Simulation, where synthesized netlists are verified against the same testbench used for RTL simulation. This helped validate that synthesis does not alter the logical behavior. I studied common causes of mismatches such as missing sensitivity lists, uninitialized signals, improper blocking/non-blocking usage, and latch inference. Waveform comparisons between RTL and GLS enabled practical debugging and refinement of RTL design.

## Day 4 – Sequential Logic & Optimization Techniques

This session emphasized the distinction between combinational and sequential hardware elements, including clock-driven registers and flip-flops. I explored how synthesis tools apply optimizations to improve timing and minimize area, such as removing unnecessary registers and refining state machines. By working with sequential circuits, I observed how register placement and logic restructuring can influence overall hardware performance.

## Day 5 – Behavioral Modeling & Latch Avoidance

The final session focused on behavioral constructs in Verilog such as if-else, case statements, and loops, and how they translate into hardware. Key learnings included avoiding unintended latches caused by incomplete conditional assignments and understanding the difference between priority logic and parallel decision structures. Through practical examples, I applied proper default assignments and structured coding practices to produce clean, synthesizable RTL.
