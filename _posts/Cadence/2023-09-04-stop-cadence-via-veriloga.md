---
title: stop cadence simulation via VerilogA code
date: 2023-09-04 11:33:00 +0800
categories: [Tools, Cadence]
tags: [cadence, VerilogA, simulation]
math: true
mermaid: true
---


This VerilogA block/code for Stop the current simulation in Cadence Virtuoso depending on analog conditions.

This block is perfect when you need to perform multiple long simulations, for example, corners or Monte Carlo. You will save a lot of simulation time

The following VerilogA code will stop the simulation after some time when the condition is fulfilled.

```verilog
///////////////////////////////////////////////////////////////////////////
// Engineer: Alberto Lopez
//
// Description: stop the simulation when the input voltage arrives to a minimum value
//
// Change history: feb/2021
//
/////////////////////////////////////////////////////////////////////////////

`include "constants.vams"
`include "disciplines.vams"

module FINISH_run_delay( vin, GND );

inout GND;
input vin;

electrical GND;
electrical  vin;


//Parameters
parameter real tiempo = 1u;            //refreshing time for the Verilog code

parameter real MIN_VOLTAGE = 2;    //Min voltage level to trigger the FINISH
parameter real DELAY_TIME = 1000; // number of "tiempo" steps to execute the stop action
integer flag;
integer cnt;

analog begin

    @(initial_step) begin
        flag = 0;
        cnt = 0;
    end

    @(timer(0,tiempo)) begin
        if( V(vin,GND) > MIN_VOLTAGE ) begin
            flag = 1;    //only for testing purposes

        end
        if (flag == 1) begin
            cnt = cnt +1;
            if (cnt == DELAY_TIME) begin
                //$finish(0);                         //command to stop the whole simulation
                $finish_current_analysis(0);        //command to stop the current simulation
            end
        end

    end    

end //analog
endmodule
```


https://miscircuitos.com/veriloga-code-to-stop-simulation-cadence/
