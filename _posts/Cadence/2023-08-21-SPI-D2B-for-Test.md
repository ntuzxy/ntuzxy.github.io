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
可以通过一组理想源实现多位转换，第`i`位的DC voltage设为`VDD*(fmod(vin>>i,2))`。其中，`VDD`是电源电压（参数），`vin`是待转换的十进制值（参数），`i`是相应bit的具体数字。

当然，也可以通过pPar (parent parameters)将参数以CDF parameter显示在symbol属性上，只需将第`i`位的DC voltage设为`pPar("VDD")*(fmod(pPar("vin")>>i,2))`。

## 用veriloga实现

```
`include "constants.vams"
`include "discipline.vams"

module DEC2BIN_16b(out)

`define NumOfBits 16

output [`NumOfBits-1:0] out;
electrical [`NumOfBits-1:0] out;
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

output [`NumOfBits-1:0] out;
electrical [`NumOfBits-1:0] out;
parameter integer vin = 0;
parameter real VDD = 1;

genvar i;

analog begin
  for (i=0; i<`NumOfBits; i=i+1) begin
    V(out[i]) <+ (floor(vin/pow(2,i)) % 2) * VDD;
  end
end

endmodule
```


or

```
//VerilogA Module for 6bit Decimal to Binary Decoder

`include "constants.vams"
`include "disciplines.vams"

module busset6_enhanced(d_out,d_out_b,v_high,v_low)

`define NumOfBits 16

output [`NumOfBits-1:0] d_out;
electrical [`NumOfBits-1:0] d_out;
output [`NumOfBits-1:0] d_out_b;
electrical [`NumOfBits-1:0] d_out_b;
input v_high;
electrical v_high;
input v_low;
electrical v_low;

parameter integer dec_in = 0;
parameter integer v_high = 1;
parameter integer v_low = 0;

analog begin
	generate i (`NumOfBits-1,0) begin
		V(d_out[i]) <+ (VarInDecimal&(1<<i))? V(v_high): V(v_low);
		V(d_out_b[i]) <+ (VarInDecimal&(1<<i))? V(v_low): V(v_high);

endmodule
```
