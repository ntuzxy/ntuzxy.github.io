---
title: Read data from file in Verilog-A code 
date: 2023-11-01 11:33:00 +0800
categories: [Tools, Cadence]
tags: [cadence, Verilog-A, read file]
math: true
mermaid: true
---

This post is to show how to read data from file for simulation.

---

## Read decimal data from file and output directly when clock comes. 
Exmaple code: 

```verilog
`include "constants.vams"
`include "disciplines.vams"

module fileReader_1output(out1, clk);
input clk;
output out1;
electrical out1, clk;

parameter real vlogic_high = 1.0;
parameter real vlogic_low  = 0.0;
parameter real vtrans = 0.5;
parameter real tdel  = 0;
parameter real trise = 0;
parameter real tfall = 0;
parameter fileName = "full_path/data_file.txt";

integer fileHandle;
integer decimal_output;
integer data1;

 analog begin

    @ (initial_step)
        fileHandle = $fopen(fileName, "r");
    @ (final_step)
        $fclose(fileHandle);
    @ (cross (V(clk) - vtrans,+1)) begin
        decimal_output = $fscanf(fileHandle,"%d", data1);
        $display("%d", data1);                   
    end
    
    V(out1) <+ transition( vlogic_high*data1 + vlogic_low*!data1, tdel, trise, tfall );
 end

endmodule
```

The data file should be in the format of a column, like this
```
13
22
8
......
```

## Read decimal data from file and output in binary format when clock comes. 
Exmaple code: 

```verilog
`include "constants.vams"
`include "disciplines.vams"

module fileReader_3output(out1, out2, out3, clk);

`define NumOfBits 8

input clk;
output [`NumOfBits-1:0] out1, out2, out3;
electrical [`NumOfBits-1:0] out1, out2, out3;
electrical clk;

parameter real vlogic_high = 1.0;
parameter real vlogic_low  = 0.0;
parameter real vtrans = 0.5;
parameter real tdel  = 0;
parameter real trise = 0;
parameter real tfall = 0;
parameter fileName = "full_path/data_file.txt";

integer fileHandle;
integer decimal_output;
integer data1, out2, out3;
genvar i;

  analog begin

    @ (initial_step)
        fileHandle = $fopen(fileName, "r");
    @ (final_step)
        $fclose(fileHandle);
    @ (cross (V(clk) - vtrans,+1)) begin
        decimal_output = $fscanf(fileHandle,"%d", data1, out2, out3);
        $display("%d, %d, %d", data1, out2, out3);                   
    end
    
    for (i=0; i<`NumOfBits; i=i+1) begin
      V(out1) <+ transition( ((data1 >> i) % 2) * vlogic_high, tdel, trise, tfall );
      V(out2) <+ transition( ((data2 >> i) % 2) * vlogic_high, tdel, trise, tfall );
      V(out3) <+ transition( ((data3 >> i) % 2) * vlogic_high, tdel, trise, tfall );
  end

endmodule
```

The data file should be in the format of a column, like this
```
133,241,156
223,178,109
218,189,255
......
```
