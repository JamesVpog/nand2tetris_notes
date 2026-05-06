# Boolean Arithmetic

## Background

### binary numbers 

let x = x_n * x_(n-1) * ... * x_0 be a string of digits. The value of x in base b denoted (x)_b, is defined as follows:

x_i * b_i where i goes from 0 to n

therefore (10011)_2 is actual 19 in base 10 
- so the number 19 is actually stored as 10111 in the computer 

### binary addition

the lsb (least significant bit) is the rightmost digit 
the msb (most significant bit) is the leftmost digit 

when we add 2 binary numbers, it gets added from right to left making sure to add the resulting carry bit 
- we finish wehn we get to the msb, and if the msb has a carry bit, we report overflow

  1011 
+ 1011 would result in overflow

  0010
+ 0110 would not result in overflow

### Signed binary numbers

a binary number with n digits can have 2^n different bit patterns or representations

2's complement is how we can represent signed binary numbers 

the 2's complement of a number follows this formula

nX = 2^n - x if x != 0, else 0, where nX is 2's complement of x

-2 in 4 bit land would be 2^4 - 2 == 16 - 2 == 14 == 1110

following rules of 2's complement:
- 2^(n-1)-1 is the max positive number and -2^(n-1) is the minimal negative number
- all positive numbers start with 0 and all negative numbers start with 1
- to get -x from x: flip all the bits of x and add 1 to the result

also adding two negative numbers works as expected, 4 bit representation

  -2   1110
+ -3   1101
= -5 = 1011 which is correct! (we throw the overflow bit away) 

subtracting also works, which is wehre we flip all bits of x and add 1 to result
- 3 == 0011 == 1100 + 1 == 1101
      5     0101
     -3     1101
    = 2 =   0010 which is correct!

this means a single chip can do all of the arithmetic and logic called the ALU (arithmetic logic unit)
- this starts with the adder chip

## Specification

### half adder

used to add 2 bits

Chip name: HalfAdder
Inputs: a, b
Outputs: sum, carry
Function: sum = LSB of a + b
carry = MSB of a + b

// File name: projects/2/HalfAdder.hdl
/**
 * Computes the sum of two bits.
 */
CHIP HalfAdder {
    IN a, b;    // 1-bit inputs
    OUT sum,    // Right bit of a + b 
        carry;  // Left bit of a + b

    PARTS:
    //// Replace this comment with your code.
    // uses the XOR and AND gate 
    Xor(a=a, b=b, out=sum);
    And(a=a, b=b, out=carry);
}

### Full adder

used to add 3 bits 

Chip name: FullAdder
Inputs: a, b, c
Outputs: sum, carry
Function: sum = LSB of a + b + c
carry = MSB of a + b + c

a   b   c   carry   sum
0   0   0     0      0 
0   0   1     0      1 
0   1   0     0      1 
0   1   1     1      0
1   0   0     0      1
1   0   1     1      0
1   1   0     1      0
1   1   1     1      1

// File name: projects/2/FullAdder.hdl
/**
 * Computes the sum of three bits.
 */
CHIP FullAdder {
    IN a, b, c;  // 1-bit inputs
    OUT sum,     // Right bit of a + b + c
        carry;   // Left bit of a + b + c

    PARTS:
    //// Replace this comment with your code.
    // using 2 halfadders and OR gate
    HalfAdder(a=a, b=b, sum=s1, carry=c1);
    HalfAdder(a=s1, b=c, sum=sum, carry=c2);
    Or(a=c1, b=c2, out=carry);
}

### Add (16 bit)

Chip name: Add16
Inputs: a[16], b[16]
Outputs: out[16]
Function: out = a + b
Comment: Integer 2's complement addition.
Overflow is neither detected nor handled.


basically from right to left in step 0 to step n-1,
- the lsb from each a and b is added, and the carry bit is fed to next addition
- at step n - 1, the msb from each a and b is added, and carry goes nowhere!

sooo an n-bit adder can be implemented by creating an array of n full-adder
chips and propagating the carry bits up the significance ladder.

// File name: projects/2/Add16.hdl
/**
 * 16-bit adder: Adds two 16-bit two's complement values.
 * The most significant carry bit is ignored.
 */
CHIP Add16 {
    IN a[16], b[16];
    OUT out[16];

    PARTS:
    //// Replace this comment with your code.
    HalfAdder(a=a[0], b=b[0], sum=out[0], carry=c1);
    FullAdder(a=a[1], b=b[1], c=c1, sum=out[1], carry=c2);
    FullAdder(a=a[2], b=b[2], c=c2, sum=out[2], carry=c3);
    FullAdder(a=a[3], b=b[3], c=c3, sum=out[3], carry=c4);
    FullAdder(a=a[4], b=b[4], c=c4, sum=out[4], carry=c5);
    FullAdder(a=a[5], b=b[5], c=c5, sum=out[5], carry=c6);
    FullAdder(a=a[6], b=b[6], c=c6, sum=out[6], carry=c7);
    FullAdder(a=a[7], b=b[7], c=c7, sum=out[7], carry=c8);
    FullAdder(a=a[8], b=b[8], c=c8, sum=out[8], carry=c9);
    FullAdder(a=a[9], b=b[9], c=c9, sum=out[9], carry=c10);
    FullAdder(a=a[10], b=b[10], c=c10, sum=out[10], carry=c11);
    FullAdder(a=a[11], b=b[11], c=c11, sum=out[11], carry=c12);
    FullAdder(a=a[12], b=b[12], c=c12, sum=out[12], carry=c13);
    FullAdder(a=a[13], b=b[13], c=c13, sum=out[13], carry=c14);
    FullAdder(a=a[14], b=b[14], c=c14, sum=out[14], carry=c15);
    FullAdder(a=a[15], b=b[15], c=c15, sum=out[15], carry=c16);

}

### Inc16
// File name: projects/2/Inc16.hdl
/**
 * 16-bit incrementer:
 * out = in + 1
 */
CHIP Inc16 {
    IN in[16];
    OUT out[16];

    PARTS:
    //// Replace this comment with your code.
    Add16(a=in, b[0]=true, out=out);
}

### ALU 

the big alu!
- takes in 2 inputs, x and y which are 16 bits 
- 1 output is 16 bits
- six input bits called the control bits decide which function to compute 

Chip name: ALU
Inputs: x[16], y[16], // Two 16-bit data inputs

the following are the 6 control bits:
    zx, // Zero the x input
    nx, // Negate the x input
    zy, // Zero the y input
    ny, // Negate the y input
    f, // Function code: 1 for Add, 0 for And
    no // Negate the out output

Outputs: out[16], // 16-bit output
    zr, // True iff out=0
    ng // True iff out<0

Function: 
    if zx then x = 0 // 16-bit zero constant
    if nx then x = !x // Bit-wise negation
    if zy then y = 0 // 16-bit zero constant
    if ny then y = !y // Bit-wise negation
    if f then out = x + y // Integer 2's complement addition
    else out = x & y // Bit-wise And
    if no then out = !out // Bit-wise negation
    if out=0 then zr = 1 else zr = 0 // 16-bit eq. comparison
    if out<0 then ng = 1 else ng = 0 // 16-bit neg. comparison

Comment: Overflow is neither detected nor handled.

**every if in the spec is a MUX** choosing between original and transformed value

steps in order
1. preprocess x and y 
2. compute f 
3. postprocess and other flags 

// File name: projects/2/ALU.hdl
/**
 * ALU (Arithmetic Logic Unit):
 * Computes out = one of the following functions:
 *                0, 1, -1,
 *                x, y, !x, !y, -x, -y,
 *                x + 1, y + 1, x - 1, y - 1,
 *                x + y, x - y, y - x,
 *                x & y, x | y
 * on the 16-bit inputs x, y,
 * according to the input bits zx, nx, zy, ny, f, no.
 * In addition, computes the two output bits:
 * if (out == 0) zr = 1, else zr = 0
 * if (out < 0)  ng = 1, else ng = 0
 */
// Implementation: Manipulates the x and y inputs
// and operates on the resulting values, as follows:
// if (zx == 1) sets x = 0        // 16-bit constant
// if (nx == 1) sets x = !x       // bitwise not
// if (zy == 1) sets y = 0        // 16-bit constant
// if (ny == 1) sets y = !y       // bitwise not
// if (f == 1)  sets out = x + y  // integer 2's complement addition
// if (f == 0)  sets out = x & y  // bitwise and
// if (no == 1) sets out = !out   // bitwise not

CHIP ALU {
    IN  
        x[16], y[16],  // 16-bit inputs        
        zx, // zero the x input?
        nx, // negate the x input?
        zy, // zero the y input?
        ny, // negate the y input?
        f,  // compute (out = x + y) or (out = x & y)?
        no; // negate the out output?
    OUT 
        out[16], // 16-bit output
        zr,      // if (out == 0) equals 1, else 0
        ng;      // if (out < 0)  equals 1, else 0

    PARTS:
    //// Replace this comment with your code.
        // remember,
        // mux: if sel == 1, out = b, else out = a
        // so mux is an if statement
    
    // preprocess x
        // if zx == 1, x = 0
    Mux16(a=x, b=false, sel=zx, out=x1);

        // if nx == 1, x = !x
    Not16(in=x1, out=x2);
    Mux16(a=x1, b=x2, sel=nx, out= x3);

    // preprocess y
        // if zy == 1, then y = 0
    Mux16(a=y, b=false, sel=zy, out=y1);

        // if ny == 1, then y = !y
    Not16(in=y1, out=y2);
    Mux16(a=y1, b=y2, sel=ny, out=y3);

    // compute f 
    Add16(a=x3, b=y3, out=SumXY);
    And16(a=x3, b=y3, out=AndXY);

        // if f == 1, then x+y, else x&y
    Mux16(a=AndXY, b=SumXY, sel=f, out=f1);

    // negate output
    Not16(in=f1, out=f2);

        // if no == 1, then out = !out
        // if out < 0, then ng = 1 else ng = 0
        // this depends on msb of out which is out[15]
    Mux16(a=f1, b=f2, sel=no, out=out, out[15]=ng, out[0..7]=lo, out[8..15]=hi);

        // if out == 0, then zr = 1 else zr = 0
        // check if all 16 bits are 0
    Or8Way(in=hi, out=hiOut);
    Or8Way(in=lo, out=loOut);

    Or(a=hiOut, b= loOut, out= z); // 1 if at least one 1
    Not(in=z, out=zr);              // negate result
}
