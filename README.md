# fibonacci
##Abstract
In this project we are going to design and implement the first five numbers produced by Fibonacci sequence. The designed circuit will be synthesized and tested by ISE XILINX using VHDL programing language. Finally, the aforementioned circuit will be implemented using Spartan family xc3s400-4pq208 model.

##Introduction
In the first step, we will design and simulate an asynchronous Fibonacci sequence producer using VHDL programing language. The relation bellow depicts the function for producing the sentences of Fibonacci sequence.
The output of the circuit contains 8 bits and this way it can produce the corresponding sequence with the request number (n=Reqnumber+1) in the range of 1 to 28-1. The Fibonacci sequence producer contains some latches, the control circuit and an adder. Fig. 1 illustrates the employed blocks, the connections and the handshake signals between different parts. The wider arrows shows the data flow structure of the circuit. 
