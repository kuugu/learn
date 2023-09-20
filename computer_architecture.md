### Computer Architecture 

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