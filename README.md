# VSD-RTL_Design_And_Synthesis
## Day 1 – Introduction to Simulator and Synthesizer  

This session outlines the standard workflow for digital logic design using open-source tools. It covers the role of the simulator (`Icarus Verilog`) and the waveform viewer (`GTKWave`) in the verification of Register Transfer Level (`RTL`) designs.

## Day 2: Timing Libraries, Synthesis Styles, and Flip-Flop RTL

In this we learn about the given below topics such as
- What is this library file and why we use 
- Learning abt the SKY130 timing library file
- Undertanding the Synthesis flow (i.e Hierarchical vs. Flattened Synthesis)
- Using yosys for the clk based (i.e ff based) synthesis


## Day 3: Combinational and Sequential Optimization

This session emphasized the distinction between combinational and sequential logic optimizations,major types of optimizations which are
- `Combinational Optimization`
   - Contant Propagation 
     - Direct Optimization
   - Boolean Logic OPtimization
     - K-Map
     - Quine McKluskey
- `Sequential Optimization`
   - Basic
     - Sequential Constant propagation
   - Advanced
     - State optimization(optimization of unused states)
     - Retiming
     - Sequential Logic Cloning (Physical Floor Plan Aware Synthesis)
I applied proper coding practices to produce clean, synthesizable RTL.
       
## Day 4: Gate-Level Simulation (GLS), Blocking vs Non-Blocking, Synthesis Mismatches

The focus of this session was `Gate-Level Simulation`, where synthesized netlists are verified against the same testbench used for RTL simulation. This helped validate that synthesis does not alter the logical behavior. I studied common causes of mismatches such as missing `sensitivity lists`, `uninitialized signals`, improper `blocking/non-blocking `usage, and `latch inference`. Waveform comparisons between RTL and GLS enabled practical debugging and refinement of RTL design.

## Day 5: Verilog Synthesis - Avoiding Latches, Loops & Scalable Hardware

The final session focused on the major part of the topics such as,
- `If , Case constructs`
- Labs on `"Incomplete If Case"`
- Labs on `"Incomplete overlapping Case"`
- `for loop` and `for generate`
- Labs on `"for loop"` and `"for generate"`
