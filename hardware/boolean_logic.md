# Boolean Logic

## Background
all computers are made  from chips -> logic gates -> NAND gate

boolean f(x) takes binary input and returns binary output

a truth table representation of boolean f(x) is described as:
- for each one of the 2^n possible tuples v1, ..., vn , the last column is f(v1,..,vn) 

e.g and gate for 2 boolean inputs 
x y f(x,y)
0 0     0
0 1     0 
1 0     0 
1 1     1

boolean expression basics/operators
- x AND y, or xy  , 1 when x and y are both 1
- x OR y, or x + y, 1 when x or y are 1
- NOT x, ~x, when x is exactly 0 

the canonical representation of a bool f(x):
- take the rows where it is 1, 
- 'AND' the inputs into terms into their 1's value
- and 'OR'-togther all the terms

boolean function for AND is simple, xy 

something more complex is like 

x y z f(x, y, z)
0 0 0       0
0 0 1       0
0 1 0       1
0 1 1       0
1 0 0       1
1 0 1       0
1 1 0       1
1 1 1       0

row 3 is x = 0, y = 1, and z = 0 so we should NOT x and NOT y into `~xy~z`

row 5 is therefore `x~y~z`

row 7 == `xy~z`

and so `f(x, y, z) == ~xy~z + x~y~z + xy~z`

the book tells me that every logic gate can be made from AND, OR, and NOT ops

NAND (not and) has a speical property that everything can be made from it 

x y  x NAND y
0 0         1
0 1         1
1 0         1
1 1         0



x OR y == (x NAND x) NAND (y NAND y)

e.g

prove 1 OR 0 == 1 with NAND gates

1 OR 0 == (1 NAND 1) NAND (0 NAND 0) == 0 NAND 1 == 1

## Gate Logic

a gate is just a physical device that implements a boolean f(x)
- gate implements f will have n input pins and m output pins,, the internal lofic will output f(n1,..,nm)

simple gates can make composite gates
- can hvae implementation details (see the gates that make it up) or abstraction/interface for people to use off the shelf

the abstraction/interface is unique, but implementation can be done in many ways 

e.g XOR can be done in 5 gaates, or 4 gates?!

logic design == given a gate spec/interface, implement in as efficiently and with as little gates as possible 

but how to test each complex gate efficiently??

VHDL ==> Virtual Hardware Design Language and a Hardware Simulator

specify chip instr with HDL program, and Hardware simulator takes it as input and builds the modeled chip in memory
- then it can be tested to match the spec and other fun things!

building an XOR gate example
- can be done graphically, gate diagram
- or textually, in HDL program 

HDL program specs:
- header section  -> chip interface which is chip name and input/output pin names
- parts section  -> names and topopogy of all the lower level chips (specify the input and output with unique names)
- the parts build on top of each other, or *connecting internal pins*

e.g  XOR.hdl
```/* Xor (exclusive or) gate: If a<>b out=1 else out=0. */
CHIP Xor {

IN a, b;
OUT out;

PARTS:
    Not(in=a, out=nota);
    Not(in=b, out=notb);
    And(a=a, b=notb, out=w1); /* notice that AND takes in notb from previous not*/
    And(a=nota, b=b, out=w2); /
    Or(a=w1, b=w2, out=out);
}
```

test scripts also exist that are written in the HDL and passed to hardware sim
- it creates a printed outfile that we can verify empirically that our gate is doing what its supposed to

## specs 

just refer to book, it has alll the gates to use for building a modern computer and here are ones i havent heard of

multiplexor is cool, its a 3 input gate where the selecotr input just selects one of the 2 data inputs (a,b)

so basically when c == 0, return a, else return b

sel | out
0       a
1       b

demultiplexor is the opposite of multiplexor
- where youu take a single input and selector bit decides between 2 different output channels 
- so if sel == 0, {a = in, b = 0} and sel == 1, {a = 0, b = in}

sel | a   b
0     in  0
1     0   in

## Multi-bit boolean functions
from basic gates, you can also apply them to multi-bit arrays called buses
- 32 bit computesr operate on 32-bit buses with all the boolean functinos

e.g Multi bit NOT
- an n-bit NOT gate applies boolean NOT to every one of the bits in the n-bit input BUS

Chip name: Not16
Inputs: in[16] // a 16-bit pin
Outputs: out[16]
Function: For i=0..15 out[i]=Not(in[i]).

## Multi-way boolean functinos

meaning n amount of inputs to the boolean function like multi-way OR 

An n-way Or gate outputs 1 when at least one of its n bit inputs is 1,
and 0 otherwise. Here is the 8-way variant of this gate:
Chip name: Or8Way
Inputs: in[8]
Outputs: out
Function: out=Or(in[0],in[1],...,in[7]).

mutli-way/mutli-bit multiplexor 
- the 4-way 16-bit multiplexor and 8-way 16-bit multiplexor are used in the computer implementtaion

the official definition is:
- m-way n-bit multiplexor selects one of the m n-bit input buses and outputs to a single n-bit output bus
-- there are k control bit/selector bits where k = log_2(m), and decide the n-bit output bus

heres a good example of the chip: (basically 4 in -> 1 output depending on sel)

Chip name: Mux4Way16
Inputs: a[16], b[16], c[16], d[16], sel[2]
Outputs: out[16]
Function: If sel=00 then out=a else if sel=01 then out=b
else if sel=10 then out=c else if sel=11 then out=d
Comment: The assignment operations mentioned above are all
16-bit. For example, "out=a" means "for i=0..15
out[i]=a[i]".


mutli-way/mutli-bit demultiplexor
- the 4-way 1-bit demultiplexor and 8-way 1-bit demultiplexor are used in the computer implementtaion

the official definition is:
- the m-way n-bit demultiplexor channels a single n-bit input into one of m possible n-bit outputs. The selection is
specified by a set of k control bits, where k = log_2 m.

heres a good example of the chip: (1 in -> 4 outand only one output carries the input depending on sel, rest are 0)

Chip name: DMux4Way
Inputs: in, sel[2]
Outputs: a, b, c, d
Function: If sel=00 then {a=in, b=c=d=0}
else if sel=01 then {b=in, a=c=d=0}
else if sel=10 then {c=in, a=b=d=0}
else if sel=11 then {d=in, a=b=c=0}.



a, b NAND(a,b)
0  0   1
0  1   1 
1  0   1 
1  1   0


NOT (in, in) ==  NAND (NAND (a,b) , NAND(a,b))

- in = NAND(a,b)




a, b AND(a,b)
0  0  0
0  1  0
1  0  0 
1  1  1
