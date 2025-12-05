# Optimization in synthesis
In this session it covers the major part of the topics such as,
- If , Case constructs
- Labs on "Incomplete If Case"
- Labs on "Incomplete overlapping Case"
- for loop and for generate
- Labs on "for loop" and "for generate"

## If Conditioning
It is a priority logic based conditonal statement, which is similar like Mux where depends upon the selection pin which here it is the condition ,the logic given to that condition is excecuted.

## `incomp_if.v`
```v
module incomp_if (input i0 , input i1 , input i2 , output reg y);
always @ (*)
begin
        if(i0)
                y <= i1;
end
endmodule
```
This code typical depicts an behaviour of an D latch which gives output based on the sel pin of the mux which is condition that we set on the if statement. 
#### Iverilog Simulation
```sh
iverilog incomp_if.v tb_incomp_if.v
```
```sh
./a.out
```
```sh
gtkwave tb_incomp_if.vcd
```
<img width="1920" height="1080" alt="screenshot-2025-12-06_02-48-14" src="https://github.com/user-attachments/assets/90051325-642d-4e30-bf0e-2a88cad606ec" />

<img width="1920" height="1080" alt="screenshot-2025-12-06_02-50-42" src="https://github.com/user-attachments/assets/21045595-4f39-49eb-8101-dedcde262cd5" />

#### Synthesis 

```sh
yosys> read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```  
```sh
yosys> read_verilog incomp_if.v  
```
```sh
yosys> synth -top incomp_if  
```
<img width="1920" height="1080" alt="screenshot-2025-12-06_02-57-28" src="https://github.com/user-attachments/assets/5f75ede6-8cb5-410a-9a12-7604a3e39c2f" />

```sh
yosys> abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib  
```
```sh
yosys> show
```
<img width="1920" height="1080" alt="screenshot-2025-12-06_03-00-37" src="https://github.com/user-attachments/assets/c89b6c44-a417-4dbe-a665-04a8d62f589c" />

<img width="1920" height="1079" alt="screenshot-2025-12-06_03-00-47" src="https://github.com/user-attachments/assets/f71449ce-a11c-43da-b45d-4a7bfec7d433" />



## `incomp_if2.v`
```v
module incomp_if2 (input i0 , input i1 , input i2 , input i3, output reg y);
always @ (*)
begin
        if(i0)
                y <= i1;
        else if (i2)
                y <= i3;

end
endmodule
```
#### Iverilog Simulation
```sh
iverilog incomp_if2.v tb_incomp_if2.v
```
```sh
./a.out
```
```sh
gtkwave tb_incomp_if2.vcd
```
<img width="1920" height="1080" alt="screenshot-2025-12-06_03-07-11" src="https://github.com/user-attachments/assets/55e6c5a4-6180-4e6f-96a4-2666ffb15cf1" />

<img width="1920" height="1080" alt="screenshot-2025-12-06_03-07-41" src="https://github.com/user-attachments/assets/d305aff1-c2a4-43c8-87e1-348e84992a86" />

#### Synthesis 

```sh
yosys> read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```  
```sh
yosys> read_verilog incomp_if2.v  
```
```sh
yosys> synth -top incomp_if2  
```
<img width="1920" height="1080" alt="screenshot-2025-12-06_03-09-14" src="https://github.com/user-attachments/assets/be84a4f3-1a04-406f-9d37-31a05007cf3b" />

```sh
yosys> abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib  
```
```sh
yosys> show
```
<img width="1920" height="1080" alt="screenshot-2025-12-06_03-09-50" src="https://github.com/user-attachments/assets/77fb0bba-cc75-4e82-86fe-c05c6352e3d0" />

<img width="1920" height="1080" alt="screenshot-2025-12-06_03-09-39" src="https://github.com/user-attachments/assets/6f4cd01e-6bb2-44e1-a522-134691e81905" />



## Incomplete case

## `incomp_case.v`
```v
module incomp_case (input i0 , input i1 , input i2 , input [1:0] sel, output reg y);
always @ (*)
begin
        case(sel)
                2'b00 : y = i0;
                2'b01 : y = i1;
        endcase
end
endmodule
```
In this we have a latched input in the 4 input Mux 

#### Iverilog Simulation
```sh
iverilog incomp_case.v tb_incomp_case.v
```
```sh
./a.out
```
```sh
gtkwave tb_incomp_case.vcd
```
<img width="1920" height="1080" alt="screenshot-2025-12-06_03-15-57" src="https://github.com/user-attachments/assets/0562961e-2b36-4c92-a9c6-e22daba0390f" />

<img width="1920" height="1080" alt="screenshot-2025-12-06_03-16-34" src="https://github.com/user-attachments/assets/dd6e98c2-9bae-4a6e-b7a1-f73984a1ba77" />


#### Synthesis 

```sh
yosys> read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```  
```sh
yosys> read_verilog incomp_case.v  
```
```sh
yosys> synth -top incomp_case  
```
<img width="1920" height="1080" alt="screenshot-2025-12-06_03-17-49" src="https://github.com/user-attachments/assets/158da923-615f-47fa-9f50-a94e877ddd9c" />

```sh
yosys> abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib  
```
```sh
yosys> show
```
<img width="1920" height="1080" alt="screenshot-2025-12-06_03-18-03" src="https://github.com/user-attachments/assets/5a05257b-c588-4a61-86a3-fca8e348c45b" />

<img width="1920" height="1080" alt="screenshot-2025-12-06_03-18-17" src="https://github.com/user-attachments/assets/cd9f6de6-21f0-4459-a584-0364cea6e358" />


## `comp_case.v`
```v
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
```
The simulation and synthesis commands are as same as the previous one .  
Here we don't have latch in the mux in `comp_case.v`

#### Simulation

<img width="1920" height="1080" alt="screenshot-2025-12-06_03-26-33" src="https://github.com/user-attachments/assets/84e163a7-8293-411b-81f5-bdafe56f47c6" />

<img width="1920" height="1080" alt="screenshot-2025-12-06_03-27-04" src="https://github.com/user-attachments/assets/4562804b-e53c-41c0-9d73-2ff8d118a44a" />

#### Synthesis

<img width="1920" height="1080" alt="screenshot-2025-12-06_03-27-50" src="https://github.com/user-attachments/assets/42bdf6f4-7f7a-4b24-9430-489dc07fb211" />

<img width="1920" height="1080" alt="screenshot-2025-12-06_03-28-08" src="https://github.com/user-attachments/assets/fa7fd091-43d4-41c6-ba35-189776d10708" />

## `partial_case_assign.v`
```v
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
```
`No, Here the boolean function will not matches the RTL Synthesis. Because x is not assigned in the 2'b01 case, the synthesis tool will infer a latch, so the RTL does not represent a pure combinational boolean function.`
Here also the simulation and synthesis commands are as same as the previous one .  

#### Simulation 

<img width="1920" height="1080" alt="screenshot-2025-12-06_03-34-23" src="https://github.com/user-attachments/assets/7f32df41-54d7-46d3-805c-9802395b8930" />

<img width="1920" height="1079" alt="screenshot-2025-12-06_03-35-05" src="https://github.com/user-attachments/assets/56174eea-befb-499b-b8ab-6dce9c24acd3" />

#### Synthesis

<img width="1920" height="1080" alt="screenshot-2025-12-06_03-39-07" src="https://github.com/user-attachments/assets/a55f46df-494b-4e04-b758-b996bdbb8e0e" />

<img width="1920" height="1080" alt="screenshot-2025-12-06_03-39-31" src="https://github.com/user-attachments/assets/2dab3ab3-8379-4a2f-9244-313f319cb1b9" />

# Loping Constructs

Here for using the loop we can follow two method of looping which is
- `for loop`(general for loop given inside the always block)
- `generate for` loop(we use this outside the always block, often used for instantiating hardware)

## `mux_generate.v`
```v
module mux_generate (input i0 , input i1, input i2 , input i3 , input [1:0] sel  , output reg y);
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
```

####  Simulation 
```sh
iverilog mux_generate.v tb_mux_generate.v
```
```sh
./a.out
```
```sh
gtkwave tb_mux_generate.vcd
```
<img width="1920" height="1080" alt="screenshot-2025-12-06_04-01-53" src="https://github.com/user-attachments/assets/04ebed23-7368-4f80-8c0f-82ad6837fac6" />

<img width="1920" height="1080" alt="screenshot-2025-12-06_04-02-20" src="https://github.com/user-attachments/assets/eb39f3bc-1f2f-4e2a-815d-f634118cd8e7" />

#### Synthesis

```sh
yosys> read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```  
```sh
yosys> read_verilog mux_generate.v  
```
```sh
yosys> synth -top mux_generate  
```
<img width="1920" height="1080" alt="screenshot-2025-12-06_04-03-27" src="https://github.com/user-attachments/assets/bc0d8adc-3d12-461f-852b-f1d80da00cbb" />

```sh
yosys> abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib  
```
```sh
yosys> show
```
<img width="1920" height="1080" alt="screenshot-2025-12-06_04-04-08" src="https://github.com/user-attachments/assets/8374822e-ceec-45d8-b386-913e066401c1" />


#### Similary we need to understand the diff btw the demux_case and demux_generate
Where the demux_generate in more efficient and simplecoding to produce dmux algorithm where the code of dmux for case and generate are 
### `demux_case.v`
```v
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
```
### `demux_generate.v`
```v
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
```
### Simulation
Similarly the Simulation also varied such as  

`demux_case.v`

<img width="1920" height="1080" alt="screenshot-2025-12-06_04-16-01" src="https://github.com/user-attachments/assets/f7e7b5d3-d657-4c5d-ba31-b4da11b3f55e" />

<img width="1920" height="1080" alt="screenshot-2025-12-06_04-16-31" src="https://github.com/user-attachments/assets/a1318f0d-a1d1-4bde-9ee4-839e08e22bfc" />

`demux_generate.v`

<img width="1920" height="1080" alt="screenshot-2025-12-06_04-17-14" src="https://github.com/user-attachments/assets/70edbf85-2aa5-4d25-b893-8bf9b7582138" />

<img width="1920" height="1080" alt="screenshot-2025-12-06_04-17-59" src="https://github.com/user-attachments/assets/ca6eb316-3e8c-47ea-ab86-af2471abeca8" />


## Ripple Carry adder 
### `rca.v`
```v
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
```
This is actually `instatiating` the hardware adder in this design using the `generate for` loop

### `fa.v`
```v
module fa (input a , input b , input c, output co , output sum);
        assign {co,sum}  = a + b + c ;
endmodule
```
This is actually an Full Adder.  

### Simulation of RCA
To stimulate this we need to give both the rca.v and its instantiated file fa.v also in iverilog to stimulate

```sh
iverilog fa.v rca.v tb_rca.v
```
```sh
./a.out
```
```sh
gtkwave tb_rca.vcd
```
<img width="1920" height="1080" alt="screenshot-2025-12-06_04-27-18" src="https://github.com/user-attachments/assets/46172bd6-64d0-42bd-9295-4be6f0530beb" />

<img width="1920" height="1080" alt="screenshot-2025-12-06_04-28-13" src="https://github.com/user-attachments/assets/0db4f74e-2de2-4ebf-9aa0-3d3eb0e33bef" />

### Synthesis

```sh
yosys> read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```  
```sh
yosys> read_verilog fa.v rca.v tb_rca.v
```
```sh
yosys> synth -top rca 
```
<img width="1920" height="1080" alt="screenshot-2025-12-06_04-30-35" src="https://github.com/user-attachments/assets/d19ca138-03f3-414a-9fb7-465515526071" />

```sh
yosys> abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib  
```
```sh
yosys> show
```

<img width="1920" height="1080" alt="screenshot-2025-12-06_04-31-36" src="https://github.com/user-attachments/assets/1e418646-8b40-4351-9464-a9e904ece721" />

<img width="1920" height="1080" alt="screenshot-2025-12-06_04-31-22" src="https://github.com/user-attachments/assets/f32fe50e-ab77-4b8f-9af4-5c84119c796d" />
