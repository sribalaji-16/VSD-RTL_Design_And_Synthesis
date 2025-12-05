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

`ternary_operator_mux.v`

<img width="1920" height="1080" alt="screenshot-2025-12-05_19-40-55" src="https://github.com/user-attachments/assets/f47e9ddb-b5f1-415e-8286-d1796e4c982c" />

<img width="1920" height="1080" alt="screenshot-2025-12-05_19-45-12" src="https://github.com/user-attachments/assets/f76ba827-3743-4e38-8d93-7d92be1b5af3" />

<img width="1920" height="1080" alt="screenshot-2025-12-05_19-45-12" src="https://github.com/user-attachments/assets/a0d0d44c-39f0-4294-9ff1-1d582d2d259b" />

<img width="1920" height="1080" alt="screenshot-2025-12-05_19-45-12" src="https://github.com/user-attachments/assets/cc884208-c0ac-4b03-a547-29bff94ca4c0" />

![WhatsApp Image 2025-12-05 at 23 32 04](https://github.com/user-attachments/assets/45539269-93e7-48bf-8111-60a0e00c8021)



`bad_mux.v`

<img width="1920" height="1080" alt="screenshot-2025-12-05_19-45-12" src="https://github.com/user-attachments/assets/460e5431-0455-437c-92fb-d1b5fb4892a3" />

<img width="1920" height="1080" alt="screenshot-2025-12-05_19-45-12" src="https://github.com/user-attachments/assets/90cdc6d4-21fb-40eb-8d14-36efe4324180" />

![WhatsApp Image 2025-12-05 at 23 33 32](https://github.com/user-attachments/assets/ebd1b3b7-23a8-4acb-9c75-bfe2aade0d2f)

