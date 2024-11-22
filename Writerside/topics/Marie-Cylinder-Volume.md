# Marie: Cylinder Volume

## Cylinder Volume Formula $V=\pi r^2 h$

In this program, we will calculate the volume of a cylinder using the formula $V=\pi r^2 h$. We will use the MARIE assembly language to write the program. The program will take the radius and height of the cylinder as input and output the volume of the cylinder.

Order of input:
1. Height
2. Radius

## Inputs

<code-block lang="asm6502">
Input
Store height
Input
Store radius
</code-block>

## First loop: $V = r^2$
During the first loop, we will calculate radius squared and store it in the 'volume' variable. And we will also store radius in the 'temp1' variable to know how many times to add radius to itself. Every loop we subtract one from the 'loop_count' variable until it reaches zero. When it reaches zero, skipcond 400 will make the program skip the next instruction and exit the loop.
<code-block lang="asm6502">
Load radius
Store loop_count
loop1,  Load volume
        Add radius
        Store volume
        Load loop_count
		Subt one
        Store loop_count
        Skipcond 400
        Jump loop1
</code-block>

## Second loop: $V = r^2h$

In the second loop of our app, we will multiply the result of the first loop ($r^2$) by the height
and store it in the 'volume' variable. We will also store the height in the 'temp1' variable to know how many times to add $r^2$ to itself. Since we are loading volume, which now is not empty, we will subtract one from the 'height' variable so the loop is one iteration shorter Every loop we subtract one from the 'loop_count' variable until it reaches zero.

<code-block lang="asm6502">
Load volume
Store temp1
Load height
Subt one
Store loop_count
loop2,	Load volume
		Add temp1
        Store volume
        Load loop_count
        Subt one
        Store loop_count
        Skipcond 400
        Jump loop2
</code-block>

## Final loop: $V = \pi r^2h$

In the final loop, we will multiply the result of the second loop ($r^2h$) by $\pi$ and store it in the 'volume' variable. We will also store $\pi$ in the 'temp1' variable to know how many times to add $r^2h$ to itself. Since we are loading volume, which now is not empty, we will subtract one from the 'loop_count' variable so the loop is one iteration shorter. Every loop we subtract one from the 'loop_count' variable until it reaches zero.

<code-block lang="asm6502">
Load volume
Store temp1
Load pi
Subt one
Store loop_count
loop3,	Load volume
        Add temp1
        Store volume
        Load loop_count
        Subt one
        Store loop_count
        Skipcond 400
        Jump loop3
</code-block>

## Halt Program

Finally, we will output the volume of the cylinder and halt the program.

<code-block lang="asm6502">
halt,   Load volume
        Output
        Halt
</code-block>

## Define Variables

<code-block lang="asm6502">
pi,         DEC 3
one,        DEC 1
radius,     DEC 0
height,     DEC 0
volume,     DEC 0
temp1,      DEC 0
loop_count, DEC 0
</code-block>

## Full Code

<code-block lang="asm6502">
Input
Store height
Input
Store radius

Load radius
Store loop_count
loop1,  Load volume
        Add radius
        Store volume
        Load loop_count
        Subt one
        Store loop_count
        Skipcond 400
        Jump loop1

Load volume
Store temp1
Load height
Subt one
Store loop_count
loop2,	Load volume
        Add temp1
        Store volume
        Load loop_count
        Subt one
        Store loop_count
        Skipcond 400
        Jump loop2

Load volume
Store temp1
Load pi
Subt one
Store loop_count
loop3,	Load volume
        Add temp1
        Store volume
        Load loop_count
        Subt one
        Store loop_count
        Skipcond 400
        Jump loop3

halt,   Load volume
        Output
        Halt

pi,         DEC 3
one,        DEC 1
radius,     DEC 0
height,     DEC 0
volume,     DEC 0
temp1,      DEC 0
loop_count, DEC 0
</code-block>