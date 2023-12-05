---
title: cadence AMS Simulation 
date: 2023-12-05 11:33:00 +0800
categories: [Tools, Cadence]
tags: [cadence, AMS, Co-Simulation]
math: true
mermaid: true
---

Analog and Digital Co-Simulation in Cadence Environment

---


## Firstly, generate functional view and symbol for digital design
1.	Create a new cell in your design library with view “functional”, type “Verilog”;
 
2.	Delete everything in the newly created view, open the digital design file and copy all the codes to the new view;

3.	Save the new view, the system should prompt you to create a symbol automatically. 

Note: if the system prompt something like “not able to find the compilier”, which means the ncvlog is not set up properly. Open you C shell file, and set up xcelium tools.
 

## Secondly, set up your AMS test bench
4.	Create a new “config” cell, choose AMS as template;

5.	Edit you schematic, include your analog blocks and digital blocks, and simulation setup;

6.	Launch ADE, select simulator as “ams”;

7.	Set up your variables and simulation analysis as usual;

8.	Setup connect rules from setup menu of ADE;
 
9.	If you do not have any existing connect rule, then include cadence default connect rule library in your cds.lib:
 
10.	Choose an existing connect rule and modify from it. For example, choose the 1.8V connect rule and change the supply to 1.1V and adjust the threshold.

## Thirdly, prepare to run simulation
11.	Before starting the simulation, delete all “*.pak” files in the design folder and sub-folders. It will remove all prior library information if the design library is copied from an existing one. Otherwise, the AMS compiler will prompt you to include the prior library;

12.	In the output option, save all the nets that you want to monitor; The default setting is only to save those selected nets;

13.	If you want to interactively watch digital signals, select simulation mode to “Interactive” mode from the simulation menu of ADE: (I prefer to use “batch” mode then open simVision manually)
 
Note: OSS Netlister is preferred. 
  

14.	Start to run a simulation. You can plot any analog signals as usual as in spectre simulation;

15.	To monitor digital signal, open “SimVision waves…” from Tool Menu of ADE.

16.	In SimVision, you can load the simulation database and monitor digital signals. 
 
