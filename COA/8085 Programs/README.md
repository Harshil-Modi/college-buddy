# 8085 Microprocessor Asssembly Programs

Programs can be run in 8085 simulators like: [8085 Simulator *(by Jubin Mitra)*](https://github.com/8085simulator/8085simulator.github.io), and [sim8085](https://www.sim8085.com/).

## Index

- [Store 8-bit data in memory](#store-8-bit-data-in-memory)
- [Exchange data of 2 registers](#exchange-data-of-2-registers)
- [Exchange the contents of memory location](#exchange-the-contents-of-memory-location)
- [Addition of 2 Decimal Numbers](#addition-of-2-decimal-numbers)
- [Find 1's complement and 2's complement of a given number](#find-1s-complement-and-2s-complement-of-a-given-number)
- [Add, Subtract 2 16 bit numbers](#add-subtract-2-16-bit-numbers)
- [Shift 8 bit data to 4 bit right](#shift-8-bit-data-to-4-bit-right)
- [Shift 16 bit data to 1 bit left](#shift-16-bit-data-to-1-bit-left)
- [Find a greater number between two numbers](#find-a-greater-number-between-two-numbers)
- [Find Even or Odd numbers in given 2 numbers](#find-even-or-odd-numbers-in-given-2-numbers)
- [Copy Array](#copy-array)
- [Copy Array Reverse](#copy-array-reverse)
- [Insert data in array on nth position](#insert-data-in-array-on-nth-position)
- [Sum of array of N elements](#sum-of-array-of-n-elements)
- [Dividing two 8 bit numbers](#dividing-two-8-bit-numbers)
- [Find negative numbers from the array](#find-negative-numbers-from-the-array)
- [Array Sorting](#array-sorting)
- [Counting 0s, negative numbers, and positive numbers in the array](#counting-0s-negative-numbers-and-positive-numbers-in-the-array)
- [Even parity generator and checker](#even-parity-generator-and-checker-8-bits)

## Store 8-bit data in memory

```Assembly
LXI H,C020H
MVI M,FFH
HLT
```

## Exchange Data of 2 Registers

```Assembly
MVI B,12H
MVI C,34H
MOV A,B
MOV B,C
MOV C,A
HLT
```

## Exchange the contents of memory location

```Assembly
LDA C010H
MOV B,A
LDA C020H
STA C010H
MOV A,B
STA C020H
HLT
```

## Addition of 2 Decimal Numbers

```Assembly
MVI A,55H
CMA
ADI 19H
CMA
HLT
```

## Find 1's complement and 2's complement of a given number

*Assume that given number is in register A.*

```Assembly
LDA C000H
CMA
STA C010H
INR A
STA C020H
HLT
```

## Add, Subtract 2 16 bit numbers

*Add, Subtract 2 16 bit numbers stored at C030 and C032. Store result from C035 onwards.*

```Assembly
LHLD C030H
XCHG
LHLD C032H
DAD D
SHLD C035H
LHLD C030H
XCHG
LHLD C032H
MOV A,E
SUB L
STA C037
MOV A,D
SBB H
STA C038H
HLT
```

## Shift 8 bit data to 4 bit right

*Shift 8 bit data to 4 bit right. Assume data in the C register.*

```Assembly
MVI C,01H
MOV A,C
RAR
RAR
RAR
RAR
MOV C,A
HLT
```

## Shift 16 bit data to 1 bit left

*Shift 16 bit data to 1 bit left. Assume data in BC register.*

```Assembly
LXI B,0203H
MOV A,B
RAR
MOV B,A
MOB A,C
RAR
MOV C,A
HLT
```

## Find a greater number between two numbers

*Assume numbers are in memory and result stored in memory.*

```Assembly
LDA C030H
MOV B,A
LDA C031H
CMP B
JNC Label
MOV A,B
Label:STA C032H
HLT
```

## Find Even or Odd numbers in given 2 numbers

*Assume numbers are stored in C001 and C002.If numbers are Odd store in C003 and C005. If numbers are even stored in C004 and C006.*

```Assembly
LXI H,C030H
MOV A,M
RAR
JC L1
RAL
STA C034H
L2:INX H
MOV A,M
RAR
JC L3
RAL
STA C036
HLT
L1:RAL
STA C033H
JMP L2
L3:RAL
STA C035H
HLT
```

## Copy Array

*16 data bytes are stored at memory locations starting from C500H. Write an 8085 assembly level program to copy these data bytes in the same order but starting from memory location C507H.*

```Assembly
LXI B,C508H
LXI D,C520H
MVI H,08H
L1:LDAX B
STAX D
INX B
INX D
DCR H
JNZ L1
HLT
```

## Copy Array Reverse

*16 data bytes are stored at memory locations starting from C500H. Write an 8085 assembly level program to copy these data bytes in the reverse order but starting from memory location C500H.*

```Assembly
MVI B,08H
LXI H,C507H
LXI D,C530H
L2:MOV A,M
STAX D
INX D
DCX H
DCR B
JNZ L2
HLT
```

## Insert data in array on nth position

*The block starts from location C300H onwards. It consists of 10 data bytes. The data byte to be inserted is stored at C200H and the position where it is to be inserted is stored at C201.*

```Assembly
LXI H,C200H
MOV C,M
LXI H,C201H
MOV D,M
LXI H,C2FFH
lbl:INX H
DCR D
JNZ lbl
MVI B,0AH

L1:INX H
DCR B
JZ L1
MOV A,M
CMP C
JC L1

L2:MOV A,L
ADD B
MOV E,A
MOV D,H
MOV A,B
STA C100H

L3:INX H
DCR B
MOV A,B
CPI 01H
JNZ L3
LDA C1--H
MOV B,A

L4:MOV A,M
STAX D
DCX D
DCX H
DCR B
JNZ L4
MOV A,C
STAX D
HLT
```

## Sum of array of N elements

*`N` is the number of elements in the array and `N` is stored in the memory.*

```Assembly
LXI H,C020H
MOV C,M
XRA A
MOV B,A
L1:ADD C
JNC skp
INR B
skp:DCR C
JNZ L1
LXI H,C030H
MOV M,A
INX H
MOV M,B
HLT
```

## Dividing two 8 bit numbers

*Storing the remainder and quotient in memory.*

```Assembly
LXI H,C030H
MOV B,M
MVI C,00H
INX H
MOV A,M
L2:CMP B
JC L1
SUB B
INR C
JMP L2
L1:STA C040H
MOV A,C
STA C041H
HLT
```

## Find negative numbers from the array

*A block of 10 data bytes is present in the memory. Program to find out the negative numbers from the block.*

```Assembly
LXI H,C050H
LXI D,C05FH
MVI C,00H

beg:MOV A,M
ANI 80H
JNZ neg
JMP last

neg:INX D
MOV A,M
STAX D

last:INX H
INR C
MOV A,C
CPI 0AH
JNZ beg
HLT
```

## Array Sorting

*Program to arrange 10 data bytes in an ascending order and storing the resultant block at the same memory location.*

```Assembly
LXI H,C040H
MVI C,0AH
DCR C

rpt:MOV D,C
LXI H,C040H

loop:MOV A,M
INX H
CMP M
JC skp
MOV B,M
MOV M,A
DCX H
MOV M,B
INX H

skp:DCR D
JNZ loop
DCR C
JNZ rpt
HLT
```

## Counting 0s, negative numbers, and positive numbers in the array

*Program using subroutine to count the number of positive, negative and zeros numbers in a block of 16 data bytes. Storing the result in the memory.*

```Assembly
LXI H,C050H
MVI B,00H
MVI C,00H
MVI D,00H
MVI E,16H

bk:MOV A,M
ADI 00H
JZ nxt
JP frnt
INR C
JMP end

nxt:INR D
JMP end

frnt:INR B

end:DCR E
INX H
JNZ bk
HLT
```

## Even Parity Generator and Checker (8 Bits)

Parity Generator

```Assembly
LXI B,C0A0H ;ADDRESS WHERE INPUT WILL BE STORED
MVI A,04H ;INPUT
STAX B
MVI D,0H
ADD D
JPO ODDPARITY
MVI A,02H
JMP GLBL
ODDPARITY:MVI A,01H
GLBL:INX B
STAX B ;GENERATED PARITY ON LOCATION
C0A1H
HLT
```

Parity Checker

```Assembly
LXI D,C0A0H ;INPUT ADDRESS
LXI B,C0B0H ;OUTPUT ADDRESS

;PARITY GENERATOR LOGIC START
LDAX D ;INPUT
STAX B
MVI H,0H
ADD H
JPO ODDPARITY
MVI A,02H
JMP LBL
ODDPARITY:MVI A,01H
LBL:INX B
STAX B ;GENERATED PARITY ON LOCATION C0A1H
;PARITY GENERATOR LOGIC END

INX D
LDAX D
MOV H,A
LDAX B
CMP H
JNZ ERROR
MVI A,0H
JMP NOERROR
ERROR:MVI A,EEH
NOERROR:INX B
STAX B ;OUTPUT ON LOCATION C0B2H
;0 FOR NO ERROR. 1 FOR ERROR
HLT
```
