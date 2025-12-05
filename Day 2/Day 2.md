# Timing Libraries, Synthesis Styles, and Flip-Flop RTL
In this we learn about the given below topics such as 
- What is this library file and why we use 
- Learning abt the SKY130 timing library file
- Undertanding the Synthesis flow (i.e Hierarchical vs. Flattened Synthesis)
- Using yosys for the clk based (i.e ff based) synthesis

## Sky130_fd_sc_hd__tt_025C_1v80.lib library file

On the name itself we see the what that library file means itself , where these three `tt_025C_1v80` means the what type of cells , temperature, voltage.

On this library file we could find the same logic device as multiple different cell of that same device which they differ in their `power supply` , `delay`, etc charecteristic.  

 Inside the library file we could see the cell and their functions along with that Timing arcs, Pin configuration ,etc.

## Hierarchical vs. Flattened Synthesis

### Hierarchical Synthesis

Definition:
Maintains the original RTL hierarchy, ensuring modules are synthesized with boundaries preserved.

#### How it works:
Each block is processed based on the design hierarchy, retaining the structure as defined in RTL.

#### Pros:

- Faster synthesis for complex, large-scale designs.

- Debugging becomes simpler since module names and boundaries are visible.

- Well-suited for modular development, reuse of IP blocks, and incremental compilation.

#### Cons:

- Optimization is restricted within individual modules.

- Additional setup may be needed to analyze complete chip-level performance easily.

### Flattened Synthesis

#### Definition:
Merges the design into a single level by removing hierarchy to optimize across the entire structure.

#### How it works:
All submodules are flattened, allowing the tool to apply global logic and timing improvements.

#### Pros:

- Enables broad, cross-module optimization for better performance.

- Generates one unified netlist that may simplify certain downstream processes.

#### Cons:

- Requires more time and memory, especially for bigger designs.

- Debugging becomes tougher due to loss of RTL structure.

- Netlist becomes more complex and less readable.

## Implementation 
Here we going to see synthesis for multiple module as well as submodule also
#### Multiple_module
For Hierarchical Synthesis we proceed with general synthesising only,Which are as follows
```sh
yosys> read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```  
```sh
yosys> read_verilog multiple_modules.v  
```
```sh
yosys> synth -top multiple_modules  
```
```sh
yosys> abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib  
```
```sh
yosys> write_verilog -noattr multiple_modules_net.v
```
<img width="1913" height="1076" alt="screenshot-2025-12-05_11-34-02" src="https://github.com/user-attachments/assets/2c6811cc-194a-42fb-b3ba-c01dc157dc59" />


<img width="1920" height="1080" alt="screenshot-2025-12-05_11-34-26" src="https://github.com/user-attachments/assets/c8130649-011c-4e3b-b76e-130d2c5eac9f" />


for flatten we use the command `flatten` which are as follows
```sh
yosys> flatten
```
<img width="1920" height="1079" alt="screenshot-2025-12-05_11-54-56" src="https://github.com/user-attachments/assets/605e81c0-12a0-4fa4-9fe2-6c9de1f6760f" />

```sh
yosys> write_verilog -noattr multiple_modules_flat_net.v
```

<img width="1920" height="1080" alt="screenshot-2025-12-05_11-42-05" src="https://github.com/user-attachments/assets/5ca4bf61-935e-41ec-99b1-0aad972ba5f5" />

<img width="1920" height="1070" alt="screenshot-2025-12-05_11-51-43" src="https://github.com/user-attachments/assets/e4b5d64b-7080-45fe-b108-0eb9d5ad41a6" />


<img width="1920" height="1079" alt="screenshot-2025-12-05_11-52-04" src="https://github.com/user-attachments/assets/bff69494-b47f-4210-84b9-30242949fb07" />

#### Sub_module1
Similarly for `Sub_module1`
```sh
yosys> read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```  
```sh
yosys> read_verilog multiple_modules.v  
```
```sh
yosys> synth -top sub_module1  
```
```sh
yosys> abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib  
```
```sh
yosys> write_verilog -noattr sub_module1_net.v
```
for flatten we use the command `flatten` which are as follows
```sh
yosys> flatten
```
```sh
yosys> write_verilog -noattr sub_module1_flat_net.v
```
<img width="1910" height="1065" alt="screenshot-2025-12-05_11-43-26" src="https://github.com/user-attachments/assets/81edab71-c2c8-4868-b137-c00f4a9785b3" />

<img width="1920" height="1080" alt="screenshot-2025-12-05_11-44-31" src="https://github.com/user-attachments/assets/a1d61943-26bd-416e-a807-f8c36bec96b8" />

<img width="1920" height="1080" alt="screenshot-2025-12-05_11-45-43" src="https://github.com/user-attachments/assets/9621bfd5-e994-4965-bed1-8cedec0cc942" />

<img width="1920" height="1080" alt="screenshot-2025-12-05_11-46-10" src="https://github.com/user-attachments/assets/79ba33d8-2d08-49c9-8899-6bc87b085814" />

<img width="1920" height="1074" alt="screenshot-2025-12-05_11-50-35" src="https://github.com/user-attachments/assets/9c824d4b-5304-47c8-a5eb-997e1d0f2350" />

# Flop coding styles

We use this to avoid glitch , where glitches are nothing but the unessary or unwanted, short, temporary changes in a signals

Understanding the diff btw the synchronous and asynchronous reset  

- Where Synchrounus has the mux based input which is given in direct to d ,where the inuts of tha mux are d and 0 bit,selection bit as synchronous reset pin

- Where asynchrounous reset which we'll have the reset pin as direct, in which in the reset occurs (i.e high) then there will be no pause for the clk to get posedge to change the output instead it changes the output right immedietly once after it goes high.

`dff_asyncres`
```sh
sky130RTLDesignAndSynthesisWorkshop/verilog_files main ❯ iverilog dff_ayncres.v tb_dff_asyncres.v  
```
```sh
sky130RTLDesignAndSynthesisWorkshop/verilog_files main ❯ ./a.out  
```
```sh
sky130RTLDesignAndSynthesisWorkshop/verilog_files main ❯ gtkwave tb_dff_asyncres.vcd  
```

Similarly here `dff_async_set.v`

```sh
sky130RTLDesignAndSynthesisWorkshop/verilog_files main ❯ iverilog dff_aync_set.v tb_dff_async_set.v  
```
```sh
sky130RTLDesignAndSynthesisWorkshop/verilog_files main ❯ ./a.out  
```
```sh
sky130RTLDesignAndSynthesisWorkshop/verilog_files main ❯ gtkwave tb_dff_async_set.vcd  
```
For `synchronous`

```sh
sky130RTLDesignAndSynthesisWorkshop/verilog_files main ❯ iverilog dff_syncres.v tb_dff_syncres.v  
```
```sh
sky130RTLDesignAndSynthesisWorkshop/verilog_files main ❯ ./a.out  
```
```sh
sky130RTLDesignAndSynthesisWorkshop/verilog_files main ❯ gtkwave tb_dff_syncres.vcd  
```
### Yosys synthesis for this flop based coding style

here after synth -top we give `dfflibmap -liberty ..` so that it will select the `d flipflop afterwards` we give standard synthesis step  

 `dff_asyncres.v`

```sh
yosys> read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```  
```sh
yosys> read_verilog dff_asyncres.v  
```
```sh
yosys> synth -top dff_asyncres  
```
```sh
yosys> dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib  
```
```sh
yosys> abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib  
```
```sh
yosys> write_verilog -noattr dff_asyncres_net.v  
```
 `dff_syncres.v`

```sh
yosys> read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```  
```sh
yosys> read_verilog dff_syncres.v  
```
```sh
yosys> synth -top dff_syncres  
```
```sh
yosys> dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib  
```
```sh
yosys> abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib  
```
```sh
yosys> write_verilog -noattr dff_syncres_net.v  
```
  
