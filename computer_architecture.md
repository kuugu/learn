### Computer Architecture 

###### Combinational Logic: 
- Basic unit: Transistor 
    - can be abstracted and thought of as a electronic switch 
    - n-type transistor starts conducting from source to drain, if a high voltage is applied    
    - n-type transistors are good at pulling the output down, p-types are good at pulling output up. 
    - nMOS pass 0s well but 1s poorly, pMOS pass 1s well but 0s poorly 
    - so we cannot connect n-type transistors to voltage sources, this affects gate design as well. 
    - this breaks down the transistor abstraction, and we need to dig deeper 
    - one more example: speed of conduction is slower for series vs parallel connections, so we need to breakdown the abstraction to handle this 
    - can be used to create more complex digital logic components (ex: NOT gate := connecting (3V source) -> (n-type) -> (p-type) -> (0V drain), input (A) is supplied to both transistor gates)
    - short-cicuit (if both circuits pMOS and nMOS are on), floating (if both are off)
    - Power consumption:
        - Dynamic Consumption: C * V^2 * f (capacitance of wire & gates, supply voltage, charging frequency of capacitor)
        - Static Consumption: V * I (I = leakage current, V = supply voltage)
- Combinational Logic:
    - Memoryless 
    - Boolean Algebra can be used to represent as mathematical expressions
    - Various kinds of black boxes like decoders, adders, multiplexer can be created from gates
    - Decoder: n -> 2^n mapper, like a one-shot encoding 
    - Multiplexer: selects one out of N inputs to connect to the output 
    - Tristate buffer: 
        - if enabled, passes signal through, else outputs floating value (useful when two resources want to connect to a single wire) 
        - ex: CPU - memory communication via a shared bus, when CPU wants to load some data to bus, CPU gate if enabled and memory gate is disabled
- Sequential Logic: 
    - Combinational Logic + Memory 


###### floating-point representation:
- goal is to have a representation system that works with numbers of vastly different magnitudes
- cannot be a fixed point for certain number of decimal points, wont be able to work with different magnitudes (ex: 10^9 * (2.452545 10^-13)) 
- two main parts: significand - containing digits of the number, exponent - containing position of decimal point 
- IEEE 754 defines this standard 
- some numbers:
    - single precision: 32 = 1 (sign) + 23 (significand) + 8 (exponent)
    - double precision: 64 = 1 (sign) + 52 (significand) + 11 (exponent) 

##### Resources: 
[What every programmer should know about floating-point arithmetic](https://www.phys.uconn.edu/~rozman/Courses/P2200_15F/downloads/floating-point-guide-2015-10-15.pdf)
[What every programmer should know about Memory](https://people.freebsd.org/~lstewart/articles/cpumemory.pdf)
[ETH - Digital Design & Computer Architecture](https://www.youtube.com/playlist?list=PL5Q2soXY2Zi-EImKxYYY1SZuGiOAOBKaf)

##### TODO: 
[Ben Eater - Chronological](https://www.youtube.com/@BenEater/videos)
[jdh - explainer](https://www.youtube.com/watch?v=znxZBWAO2aU)