# Introduction to Nand2Tetris

a program is just a bunch of plain text characters
- parse this string of chars into low-level code using machine language of the target computer
- this is called compilation, where an executable is created!

the machine language is also an abstraction for the hardware architecture 
which is implemented by a set of chips
then the chips are implemented by elementary logic gtes 

we will build a complete computer from the ground up!

Hardware roadmap
- all computers architecture have cpu (processor) and ram (memory)
- all cpu and ram devices are made of logic gates 
- all logic gates can be made from NAND gate

software roadmap
- all high level language are either compiled or interpreted to machine code
- java has a vm that does JIT compilation for example

in part 1: we focus on building hardware for a computer platform called Hack 
in part 2: we focus on building high level language called Jack


## Abstraction vs Implementation

abstraction describes "what the module does", and implementation
describes "how it does it."

whenever your implementation uses lower-level modules, dont worry about it and treat it as off-the-shelf
- All you need is the docs on the modules' interfaces, and what it does and use it as is

Modularity helps tie it all together so it can work independently and reusable etc

build the Hack computer in about 30 logic gates/chips using HDL (hardware description language) to produce a HDL program
- then test it in a hardware simulator
- finally gets sent to chip fabrication company to be created!


create an OS in the high level language (assembler, virutal machine, and compiler)

each chapter, learn its Abstraction/background and its implementation details
1. logic gates starting from NAND 
2 and 3. alu built from logic gates 
4. learn about low-level machine language in symbolic and binary
5. build a CPU/RAM with the alu and memory units
6. build an assembler to translate machine code into binary (that we learned in ch4)


