# Gate-Level Simulation (GLS)

Gate-Level Simulation is performed on the synthesized netlist, meaning the RTL design has already been converted into actual gate structures. The goal is to ensure that what the synthesis tool produced still behaves exactly like the original RTL, now including real hardware timing.

### Why We Perform GLS

- To ensure synthesis has not altered the logical behavior of the design.

- To confirm that timing constraints are satisfied by applying real gate propagation delays through Standard Delay Format (SDF).

- To verify that additional test hardware inserted during synthesis, such as scan logic or BIST circuitry, functions correctly.

### Stages When GLS Is Commonly Used

- Right after synthesis, to do a quick functional verification before further implementation.

- At the final sign-off stage, after static timing closure, to validate the design with accurate timing information.

### Synthesis vs. Simulation Mismatch

Occasionally, the behavior observed in gate-level simulation or real hardware does not match RTL simulation. These mismatches arise because some RTL constructs do not translate cleanly into hardware.

#### Typical Sources of Mismatch

-Using simulation-only constructs such as delays or initial blocks in the synthesizable code.

- Incomplete sensitivity lists in combinational processes, which may hide dependencies during simulation.

- Poorly defined logic (like missing else clauses or asynchronous races) that leads to unpredictable synthesis outcomes.

- Different handling of logic by simulators and synthesis engines, causing subtle interpretation differences.

### Preventive Measure:
Follow synthesizable coding guidelines and make sure all behavior is explicitly defined in the RTL.








ternary_operator_mux.v