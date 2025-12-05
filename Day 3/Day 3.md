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

