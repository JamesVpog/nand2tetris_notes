# Elementary chips

some sections contain a truth table and its canonical boolean representation to help solve. Key:
- Not(a)    = nA
- And(a, b) = a * b
- Or(a, b)  = a + b 

## Not
// File name: projects/1/Not.hdl
/**
 * Not gate:
 * if (in) out = 0, else out = 1
 */
CHIP Not {
    IN in;
    OUT out;

    PARTS:
    //// Replace this comment with your code.
    // NAND(in, in) == NOT(in)
    Nand(a=in, b=in, out=out); 
}

## And
// File name: projects/1/And.hdl
/**
 * And gate:
 * if (a and b) out = 1, else out = 0 
 */
CHIP And {
    IN a, b;
    OUT out;
    
    PARTS:
    //// Replace this comment with your code.
    // NOT( NAND(a,b) == AND(a,b)

    Nand(a= a, b= b, out= n1);
    Not(in= n1, out= out);
}

## Or
// File name: projects/1/Or.hdl
/**
 * Or gate:
 * if (a or b) out = 1, else out = 0 
 */
CHIP Or {
    IN a, b;
    OUT out;
    
    //// Replace this comment with your code.
    //   OR(a, b) == NOT( AND ( (NOT A) , NOT(B) )  
    PARTS:
    Not(in= a, out= na);
    Not(in= b, out= nb);
    And(a= na, b= nb, out= naAndnb);
    Not(in= naAndnb, out= out);
}

## XOr

a b xor
0 0  0
0 1  1
1 0  1
1 1  0

canonical representation
(nA * b) + (a * nB) 

// File name: projects/1/Xor.hdl
/**
 * Exclusive-or gate:
 * if ((a and Not(b)) or (Not(a) and b)) out = 1, else out = 0
 */
CHIP Xor {
    IN a, b;
    OUT out;

    PARTS:
    //// Replace this comment with your code.
    // OR( AND( a, NOT(b) ), AND( NOT(a), b) )
    Not(in= a, out= na);
    Not(in= b, out= nb);
    And(a= a, b= nb, out= aAndnb);
    And(a= na, b= b, out= naAndb);
    Or(a= aAndnb, b= naAndb, out= out);
}

## Multiplexor
sel a b out
0   0 0  0
0   0 1  0
0   1 0  1
0   1 1  1
1   0 0  0
1   0 1  1
1   1 0  0
1   1 1  1

canonical representation:

(notSel * a * notB) + (notSel * a * b) + (sel * notA * b) + (sel * a * b) 
= (notSel * a ( notB + b)) + (sel * notA * b) + (sel * a * b)     //factor out notSel and a
= (notSel * a * 1)  + (sel * notA * b) + (sel * a * b)            // (b or notB) is just 1
= (notSel * a) + (sel * b ( notA or a))                           // factor out sel and b
= (notSel * a) + (sel * b)

actually even easier  lol

sel out
0    a 
1    b 

(nSel * a) + (sel * b)

// File name: projects/1/Mux.hdl
/** 
 * Multiplexor:
 * if (sel = 0) out = a, else out = b
 */
CHIP Mux {
    IN a, b, sel;
    OUT out;

    PARTS:
    //// Replace this comment with your code.
    // OR( AND (a, NOT(sel)), AND(b, sel)) == Mux(a, b, sel)
    Not(in= sel, out= nsel);
    And(a= a, b= nsel, out= aAndnsel);
    And(a= sel, b= b, out= bAndsel);
    Or(a= aAndnsel, b= bAndsel, out= out);
}

## Demultiplexor

in  sel   out1   out2
0   0     0         0
0   1     0         0
1   0     1         0
1   1     0         1

canoncial SOP:

in * notSel = out1
in * sel    = out2


// File name: projects/1/DMux.hdl
/**
 * Demultiplexor:
 * [a, b] = [in, 0] if sel = 0
 *          [0, in] if sel = 1
 */
CHIP DMux {
    IN in, sel;
    OUT a, b;

    PARTS:
    //// Replace this comment with your code.
    Not(in=sel, out=nSel);
    And(a=nSel, b= in, out=a);
    And(a=in, b=sel, out=b);
    
}

# 16 bit versions

## Not16
// File name: projects/01/Not16.hdl
/**
 * 16-bit Not gate:
 * for i = 0, ..., 15:
 * out[i] = Not(a[i])
 */
CHIP Not16 {
    IN in[16];
    OUT out[16];

    PARTS:
    //// Replace this comment with your code.
    Not(in= in[0], out= out[0]);
    Not(in= in[1], out= out[1]);
    Not(in= in[2], out= out[2]);
    Not(in= in[3], out= out[3]);
    Not(in= in[4], out= out[4]);
    Not(in= in[5], out= out[5]);
    Not(in= in[6], out= out[6]);
    Not(in= in[7], out= out[7]);
    Not(in= in[8], out= out[8]);
    Not(in= in[9], out= out[9]);
    Not(in= in[10], out= out[10]);
    Not(in= in[11], out= out[11]);
    Not(in= in[12], out= out[12]);
    Not(in= in[13], out= out[13]);
    Not(in= in[14], out= out[14]);
    Not(in= in[15], out= out[15]);

}


## And16
// File name: projects/1/And16.hdl
/**
 * 16-bit And gate:
 * for i = 0, ..., 15:
 * out[i] = a[i] And b[i] 
 */
CHIP And16 {
    IN a[16], b[16];
    OUT out[16];

    PARTS:
    //// Replace this comment with your code.
    And(a= a[0], b= b[0], out= out[0]);
    And(a= a[1], b= b[1], out= out[1]);
    And(a= a[2], b= b[2], out= out[2]);
    And(a= a[3], b= b[3], out= out[3]);
    And(a= a[4], b= b[4], out= out[4]);
    And(a= a[5], b= b[5], out= out[5]);
    And(a= a[6], b= b[6], out= out[6]);
    And(a= a[7], b= b[7], out= out[7]);
    And(a= a[8], b= b[8], out= out[8]);
    And(a= a[9], b= b[9], out= out[9]);
    And(a= a[10], b= b[10], out= out[10]);
    And(a= a[11], b= b[11], out= out[11]);
    And(a= a[12], b= b[12], out= out[12]);
    And(a= a[13], b= b[13], out= out[13]);
    And(a= a[14], b= b[14], out= out[14]);
    And(a= a[15], b= b[15], out= out[15]);
}

## Or16

// File name: projects/1/Or16.hdl
/**
 * 16-bit Or gate:
 * for i = 0, ..., 15:
 * out[i] = a[i] Or b[i] 
 */
CHIP Or16 {
    IN a[16], b[16];
    OUT out[16];

    PARTS:
    //// Replace this comment with you r code.
    Or(a= a[0], b= b[0], out= out[0]);
    Or(a= a[1], b= b[1], out= out[1]);
    Or(a= a[2], b= b[2], out= out[2]);
    Or(a= a[3], b= b[3], out= out[3]);
    Or(a= a[4], b= b[4], out= out[4]);
    Or(a= a[5], b= b[5], out= out[5]);
    Or(a= a[6], b= b[6], out= out[6]);
    Or(a= a[7], b= b[7], out= out[7]);
    Or(a= a[8], b= b[8], out= out[8]);
    Or(a= a[9], b= b[9], out= out[9]);
    Or(a= a[10], b= b[10], out= out[10]);
    Or(a= a[11], b= b[11], out= out[11]);
    Or(a= a[12], b= b[12], out= out[12]);
    Or(a= a[13], b= b[13], out= out[13]);
    Or(a= a[14], b= b[14], out= out[14]);
    Or(a= a[15], b= b[15], out= out[15]);
}

## Mux16

// File name: projects/1/Mux16.hdl
/**
 * 16-bit multiplexor: 
 * for i = 0, ..., 15:
 * if (sel = 0) out[i] = a[i], else out[i] = b[i]
 */
CHIP Mux16 {
    IN a[16], b[16], sel;
    OUT out[16];

    PARTS:
    //// Replace this comment with your code.
    Mux(a=a[0], b=b[0], sel=sel, out= out[0]);
    Mux(a=a[1], b=b[1], sel=sel, out= out[1]);
    Mux(a=a[2], b=b[2], sel=sel, out= out[2]);
    Mux(a=a[3], b=b[3], sel=sel, out= out[3]);
    Mux(a=a[4], b=b[4], sel=sel, out= out[4]);
    Mux(a=a[5], b=b[5], sel=sel, out= out[5]);
    Mux(a=a[6], b=b[6], sel=sel, out= out[6]);
    Mux(a=a[7], b=b[7], sel=sel, out= out[7]);
    Mux(a=a[8], b=b[8], sel=sel, out= out[8]);
    Mux(a=a[9], b=b[9], sel=sel, out= out[9]);
    Mux(a=a[10], b=b[10], sel=sel, out= out[10]);
    Mux(a=a[11], b=b[11], sel=sel, out= out[11]);
    Mux(a=a[12], b=b[12], sel=sel, out= out[12]);
    Mux(a=a[13], b=b[13], sel=sel, out= out[13]);
    Mux(a=a[14], b=b[14], sel=sel, out= out[14]);
    Mux(a=a[15], b=b[15], sel=sel, out= out[15]);
}

# n-way versions


## 8-way Or

// File name: projects/1/Or8Way.hdl
/**
 * 8-way Or gate: 
 * out = in[0] Or in[1] Or ... Or in[7]
 */
CHIP Or8Way {
    IN in[8];
    OUT out;

    PARTS:
    //// Replace this comment with your code.
    Or(a=in[0], b=in[1], out=or1);
    Or(a=or1, b=in[2], out=or2);
    Or(a=or2, b=in[3], out=or3);
    Or(a=or3, b=in[4], out=or4);
    Or(a=or4, b=in[5], out=or5);
    Or(a=or5, b=in[6], out=or6);
    Or(a=or6, b=in[7], out=out);
}

## 4-way, 16 bit mux

If sel=00 then out=a else if sel=01 then out=b
else if sel=10 then out=c else if sel=11 then out=d

Comment: The assignment operations mentioned above are all
16-bit. For example, "out=a" means "for i=0..15
out[i]=a[i]".


sel[0] sel[1] out
0        0     a
0        1     b 
1        0     c
1        1     d 

out = (nsel0 * nsel1 * a)  +  (nsel0 * sel1 * b) + (sel0 * nsel1 * c) + (sel0 * sel1 * d)

**looks very similar to 2-1 mux**

so we can think of it was asking 2 questions
1. sel[0] or not which maps to choosing between (a, b) or (c, d)
2. sel[1] or not which maps to choosing between (a, c) or (b, d)

in 2 dimensions, think of sel[0] as top or bottom, and sel[1] is left or right

3 2-1 muxes 
- 2 for sel[0]
- 1 for sel[1]


// File name: projects/1/Mux4Way16.hdl
/**
 * 4-way 16-bit multiplexor:
 * out = a if sel = 00
 *       b if sel = 01
 *       c if sel = 10
 *       d if sel = 11
 */
CHIP Mux4Way16 {
    IN a[16], b[16], c[16], d[16], sel[2];
    OUT out[16];
    
    PARTS:
    //// Replace this comment with your code.
    // the proof 
    Mux16(a=a, b=b, sel=sel[0], out= ab);
    Mux16(a=c, b=d, sel=sel[0], out= cd);
    Mux16(a=ab, b=cd, sel=sel[1], out=out);
}


## 8-way 16 bit mux


sel[0] sel[1] sel [2] out
0        0      0      a
0        0      1      b 
0        1      0      c
0        1      1      d 
1        0      0      e
1        0      1      f
1        1      0      g
1        1      1      h 

this would take 7 2-1 muxes!!!
- 4 for sel0
- 2 for sel1
- 1 for sel2

// File name: projects/1/Mux8Way16.hdl
/**
 * 8-way 16-bit multiplexor:
 * out = a if sel = 000
 *       b if sel = 001
 *       c if sel = 010
 *       d if sel = 011
 *       e if sel = 100
 *       f if sel = 101
 *       g if sel = 110
 *       h if sel = 111
 */
CHIP Mux8Way16 {
    IN a[16], b[16], c[16], d[16],
       e[16], f[16], g[16], h[16],
       sel[3];
    OUT out[16];

    PARTS:
    //// Replace this comment with your code.

    // sel0
    Mux16(a=a, b=b, sel=sel[0], out=ab);
    Mux16(a=c, b=d, sel=sel[0], out=cd);
    Mux16(a=e, b=f, sel=sel[0], out=ef);
    Mux16(a=g, b=h, sel=sel[0], out=gh);

    // sel1
    Mux16(a=ab, b=cd, sel=sel[1], out=abcd);
    Mux16(a=ef, b=gh, sel=sel[1], out=efgh);

    // sel2
    Mux16(a=abcd, b=efgh, sel=sel[2], out=out);  

}

## DMux 4 way, 4 inputs too 1 


sel0   sel1     out1   out2  out3   out4
0      0         in     0       0   0
0      1         0      in      0   0
1      0         0      0       in  0
1      1         0      0       0   in

canoncial SOP:


nSel0 * nSel1 * in = out1 
nSel0 * sel1  * in = out2
Sel0  * nSel1 * in  = out3
Sel0  * sel1  * in  = out4

// File name: projects/1/DMux4Way.hdl
/**
 * 4-way demultiplexor:
 * [a, b, c, d] = [in, 0, 0, 0] if sel = 00
 *                [0, in, 0, 0] if sel = 01
 *                [0, 0, in, 0] if sel = 10
 *                [0, 0, 0, in] if sel = 11
 */
CHIP DMux4Way {
    IN in, sel[2];
    OUT a, b, c, d;

    PARTS:
    //// Replace this comment with your code.
    Not(in=sel[0], out=nSel0);
    Not(in=sel[1], out=nSel1);

    And(a=nSel1, b=nSel0, out=l00);
    And(a=nSel1, b=sel[0], out=l01);
    And(a=sel[1], b=nSel0, out=l10);
    And(a=sel[1], b=sel[0], out=l11);

    And(a=l00, b=in, out=a);
    And(a=l01, b=in, out=b);
    And(a=l10, b=in, out=c);
    And(a=l11, b=in, out=d);
}

## DMux8way

a 8 way deux has 1 input to 8 outputs!
- so we need 2 1-4 demuxes to get 8 outputs but that requires 2 inputs!
- so we need 1 1-2 demux in the first stage to get 2 outputs

i -> 1-2 demux with selbit2 -> 1-4 demux with selbit0 and selbit1  ( 8 outputs
                |
                |----------->  1-4 demux with selbit0 and selbit1              )

// File name: projects/1/DMux8Way.hdl
/**
 * 8-way demultiplexor:
 * [a, b, c, d, e, f, g, h] = [in, 0,  0,  0,  0,  0,  0,  0] if sel = 000
 *                            [0, in,  0,  0,  0,  0,  0,  0] if sel = 001
 *                            [0,  0, in,  0,  0,  0,  0,  0] if sel = 010
 *                            [0,  0,  0, in,  0,  0,  0,  0] if sel = 011
 *                            [0,  0,  0,  0, in,  0,  0,  0] if sel = 100
 *                            [0,  0,  0,  0,  0, in,  0,  0] if sel = 101
 *                            [0,  0,  0,  0,  0,  0, in,  0] if sel = 110
 *                            [0,  0,  0,  0,  0,  0,  0, in] if sel = 111
 *//
CHIP DMux8Way {
    IN in, sel[3];
    OUT a, b, c, d, e, f, g, h;

    PARTS:
    //// Replace this comment with your code.
    DMux(in=in, sel=sel[2], a= in1, b=in2);
    DMux4Way(in= in1, sel=sel[0..1], a=a, b=b, c=c, d=d);
    DMux4Way(in= in2, sel=sel[0..1], a=e, b=f, c=g, d=h);

}
