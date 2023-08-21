---
title: SPI or DEC2BIN implementation for quick simulation
date: 2023-08-21 11:33:00 +0800
categories: [Tools, Cadence, code]
tags: [cadence, dec2bin, SPI, veriloga]
math: true
mermaid: true
---

如果仅用于快速模拟仿真用，SPI和dec2bin本质上是一样的。

# 1. 实现方案
可以通过以下两种方式实现
## 方案1：先移位后取余
先右移i位，再对2取余。

## 方案2：先取余后移位
先对2^(i+1)取余，再右移2^i-1位。

# 2. 实现方式
根据以上分析，显然第一个方案更直接。下面两种实现方式均以第一个方案为例。
## 用analogLib中vdc实现
可以通过一组理想源实现多位转换，第`i`位的DC voltage设为`pPar("VDD")*(fmod(pPar("vin")>i,2))`。
## 用veriloga实现

```
`include "constants.vams"
`include "discipline.vams"

module DEC2BIN_16b(out)

`define NumOfBits 16

output electrical [`NumOfBits-1:0] out;
parameter integer vin = 0;
parameter real VDD = 1;

genvar i;

analog begin
  for (i=0; i<`NumOfBits; i=i+1) begin
    V(out[i]) <+ (vin>>i % 2) * VDD;
  end
end

endmodule
```

或者也可以通过以下方式实现：

```
`include "constants.vams"
`include "discipline.vams"

module DEC2BIN_16b(out)

`define NumOfBits 16

output electrical [`NumOfBits-1:0] out;
parameter real vin = 0;
parameter real VDD = 1;

genvar i;

analog begin
  for (i=0; i<`NumOfBits; i=i+1) begin
    V(out[i]) <+ (floor(vin/pow(2,i)) % 2) * VDD;
  end
end

endmodule
```

