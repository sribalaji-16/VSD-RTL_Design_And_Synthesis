# Combinational and Sequential Optimization

In logic optimization we will look the two major types of optimizations which are 
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

## Combinational Logic Optimization
### Direct Optimization
Optimization code of `opt_check.v` which is like an 2x1 mux
```v
module opt_check (input a , input b , output y);
        assign y = a?b:0;
endmodule
```
This code is nothing just outputs based on a when value of a , if a is 1 then it assigns value of b else 0 to y  
This is nothing but an y=a.b 

#### Synthesis of Opt_check.v

```sh
yosys> read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```  
```sh
yosys> read_verilog opt_check.v  
```
```sh
yosys> synth -top opt_check  
```
```sh
yosys> opt_clean -purge
```
```sh
yosys> abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib  
```
```sh
yosys> show
```
<img width="1920" height="1079" alt="screenshot-2025-12-05_13-43-36" src="https://github.com/user-attachments/assets/509491dd-8342-4aaf-8099-b91e5a127ee6" />

<img width="1920" height="1080" alt="screenshot-2025-12-05_15-56-18" src="https://github.com/user-attachments/assets/8d78a7c9-8f54-4db3-9caa-e2678fc7914e" />

<img width="1920" height="1080" alt="screenshot-2025-12-05_15-56-18" src="https://github.com/user-attachments/assets/24e41b08-a2eb-4d78-aff0-a7a6cb91572a" />


`Opt_check2.v` is a 2x1 mux
```v
module opt_check2 (input a , input b , output y);
        assign y = a?1:b;
endmodule
```
This assigns y to value of b when a is low else it assign one to y  
This is nothing but an Y=a+b
#### Synthesis of Opt_check2.v


```sh
yosys> read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```  
```sh
yosys> read_verilog opt_check2.v  
```
```sh
yosys> synth -top opt_check2  
```
```sh
yosys> opt_clean -purge
```
```sh
yosys> abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib  
```
```sh
yosys> show
```
<img width="1920" height="1080" alt="screenshot-2025-12-05_15-56-18" src="https://github.com/user-attachments/assets/6d3e6567-8382-4d36-b26c-4eecedb59b62" />

<img width="1920" height="1080" alt="screenshot-2025-12-05_15-56-18" src="https://github.com/user-attachments/assets/ba4172d3-10c1-413a-afe6-19589283026e" />

<img width="1920" height="1080" alt="screenshot-2025-12-05_15-56-18" src="https://github.com/user-attachments/assets/b83d1709-8824-45c1-8c5d-4bd3561a490b" />


`Opt_check3.v` 
```v
module opt_check3 (input a , input b, input c , output y);
        assign y = a?(c?b:0):0;
endmodule
```
This is nothing but an Y=a.b.c
#### Synthesis of Opt_check3.v


```sh
yosys> read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```  
```sh
yosys> read_verilog opt_check3.v  
```
```sh
yosys> synth -top opt_check3  
```
```sh
yosys> opt_clean -purge
```
```sh
yosys> abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib  
```
```sh
yosys> show
```
<img width="1920" height="1080" alt="screenshot-2025-12-05_15-56-18" src="https://github.com/user-attachments/assets/4aa8066d-c5d2-4ae5-9633-468b61aa2be2" />

<img width="1920" height="1080" alt="screenshot-2025-12-05_15-56-18" src="https://github.com/user-attachments/assets/6dc5a3d1-d931-4a51-9f68-f9e29434cc43" />




`Opt_check4.v` 
```v
module opt_check4 (input a , input b , input c , output y);
 assign y = a?(b?(a & c ):c):(!c);
 endmodule
```
This is nothing but an Y=a^c
#### Synthesis of Opt_check4.v


```sh
yosys> read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```  
```sh
yosys> read_verilog opt_check4.v  
```
```sh
yosys> synth -top opt_check4  
```
```sh
yosys> opt_clean -purge
```
```sh
yosys> abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib  
```
```sh
yosys> show
```
<img width="1920" height="1080" alt="screenshot-2025-12-05_15-56-18" src="https://github.com/user-attachments/assets/29dbc0ce-75a9-4bbd-8fcb-ef04e65ff377" />

<img width="1920" height="1080" alt="screenshot-2025-12-05_15-56-18" src="https://github.com/user-attachments/assets/450abb19-29d1-4fff-9978-aa6bc5fbd0e4" />


the similar steps follows for Multiple module opt also 
`multiple_module_opt.v`
```v 
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
```
<img width="1920" height="1080" alt="screenshot-2025-12-05_15-56-18" src="https://github.com/user-attachments/assets/01ea11c5-586a-4484-9d98-71b049b7e266" />

<img width="1920" height="1080" alt="screenshot-2025-12-05_15-56-18" src="https://github.com/user-attachments/assets/b7279be5-41e7-47b9-a256-e66ea6a9ea3f" />

<img width="1920" height="1080" alt="screenshot-2025-12-05_15-56-18" src="https://github.com/user-attachments/assets/622719a6-d015-412e-85f0-fa0d2354761b" />


`multiple_module_opt2.v`
```v
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

sky130RTLDesignAndSynthesisWorkshop/verilog_files main  ? ❯ cat ../verilog_files/multiple_module_opt2.v

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
```
<img width="1920" height="1080" alt="screenshot-2025-12-05_15-56-18" src="https://github.com/user-attachments/assets/9ddd1045-79a2-4607-9ddb-50171c58cdef" />

## Sequential logic optimization
Here We now going to see the `sequential constant propagation`

Here we see the `dff_const1.v`
```v
module dff_const1(input clk, input reset, output reg q);
always @(posedge clk, posedge reset)
begin
        if(reset)
                q <= 1'b0;
        else
                q <= 1'b1;
end

endmodule
```
`Synthesis` of this ff is 

```sh
yosys> read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```  
```sh
yosys> read_verilog dff_count1.v  
```
```sh
yosys> synth -top dff_count1  
```
```sh
yosys> dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib  
```
```sh
yosys> abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib  
```
```sh
yosys> show
```
<img width="1920" height="1080" alt="screenshot-2025-12-05_15-56-18" src="https://github.com/user-attachments/assets/4ac4d89e-bbdb-4a1e-bed7-dc800d294e02" />

<img width="1920" height="1080" alt="screenshot-2025-12-05_15-56-18" src="https://github.com/user-attachments/assets/f1a9f220-25e2-41d1-8374-8a4545687eee" />

<img width="1920" height="1080" alt="screenshot-2025-12-05_15-56-18" src="https://github.com/user-attachments/assets/db01ea83-d56e-45db-832c-6e2abc7ff0c5" />



Similarly this implies for other dff_const verilog files as well  
`Dff_const2.v`
```v
module dff_const2(input clk, input reset, output reg q);
always @(posedge clk, posedge reset)
begin
        if(reset)
                q <= 1'b1;
        else
                q <= 1'b1;
end

endmodule
```
`dff_count3.v`
```v
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
```
<img width="1920" height="1080" alt="screenshot-2025-12-05_15-56-18" src="https://github.com/user-attachments/assets/1fd1ef76-62f8-41ea-aa93-75c6d3cb9c25" />

`dff_count4.v`
```v
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
```
`dff_count5.v`
```v
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
```
<img width="1920" height="1080" alt="screenshot-2025-12-05_15-56-18" src="https://github.com/user-attachments/assets/6b27256c-173d-4852-b50e-1b6b45093fd1" />

## Counter Optimization
Similarly we have optimization for counter cell also, where it is nothing but an `state optimization`

`counter_opt.v`
```v
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
```
Synthesis for this is also same like given before

<img width="1920" height="1080" alt="screenshot-2025-12-05_15-56-18" src="https://github.com/user-attachments/assets/c20c2d05-b4b7-442c-a16c-d1f228b8351b" />

<img width="1920" height="1080" alt="screenshot-2025-12-05_15-56-18" src="https://github.com/user-attachments/assets/5e90db85-e0b1-46fa-ac12-9b7277bd8149" />

<img width="1920" height="1080" alt="screenshot-2025-12-05_15-56-18" src="https://github.com/user-attachments/assets/dc8fc0ad-4195-4c0d-8175-3b618e87fe71" />

`counter_opt2.v`
```v
module counter_opt (input clk , input reset , output q);
reg [2:0] count;
assign q = (count[2:0] == 3'b100);

always @(posedge clk ,posedge reset)
begin
        if(reset)
                count <= 3'b000;
        else
                count <= count + 1;
end

endmodule
```
Synthesis of this is also similar like before what we did

<img width="1920" height="1080" alt="screenshot-2025-12-05_15-56-18" src="https://github.com/user-attachments/assets/2482a625-c578-451e-9678-25f2a1afecee" />

<img width="1920" height="1080" alt="screenshot-2025-12-05_15-56-18" src="https://github.com/user-attachments/assets/780719db-0fcf-4c21-b08d-2ec9021e95eb" />

<img width="1920" height="1080" alt="screenshot-2025-12-05_15-56-18" src="https://github.com/user-attachments/assets/d593fdee-435c-458a-8b8e-1292496f0be6" />
