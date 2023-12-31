# RISCV

- [Day 1-Introduction to RISC-V ISA And GNU compiler toolchain ](#Day1--Introduction-to-RISC-V-ISA-And-GNU-compiler-toolchain)

- [Day 2-Introduction to Application Binary Interface And Basic Error Flow](#Day2--Introduction-to-Application-Binary-Interface-And-Basic-error-flow)

# Day 1- Introduction to RISC-V ISA And GNU compiler toolchain
<details>
<summary> Installation </summary>

1). Install virtual machine through given link
```
https://www.virtualbox.org/wiki/Downloads
```

2). Create a new OS with type as Linux and version as Ubuntu 18.04 LTS (Bionic Beaver) (64-bit)


3). Allocate adequate memory and use existing virtual disk file (add path to provided 26GB .vdi file)

4). Finish the process and start the OS by clicking on the start arrow

![setup_1](https://github.com/Vivekchoudhary2/somaiya-riscv/assets/154996509/3e7c71ad-2df4-4869-b790-23a8582c1084)

</details>

<details>
  <summary> Introduction to RISC-V ISA </summary>

  RISC-V Instruction Set Architecture (ISA) is assembly level language which only RISC-V hardware layout understands. It is designed to communicate instructions with the computer. Since every layout is custom designed one can definetly expect unique instruction set (for e.g- To add two data values the command 'addi rd, rs1, rs2 is used; whereas 8051 microcontroller uses 'add a, b' to add the same two data values.)

  Machine does not understand human language; it only understands 1's and 0's. And humans definitely cannot communicate in 1's and 0's.
  
  How humans take aid of machines to do the same task again and again?

  There are 3 main components:
  
  1.) Source file
  
  2.) Compiler
  
  3.) Assembler

  ## 1.) Source file 
  It is set of instructions in human readable format.
  
  for e.g- addition of two variables of certain data type is
          c = a + b

  Examples of source files are: C++, C 

  Machine does not understand that!

  ## 2.) Compiler
  Compiler converts the source file instructions to mnemonic version of machine code.
  
  This mnemonic version of machine code is also known as assembly language.
  
  The compiler produces .exe file which consists of all instructions close to machine language.

  ## 3.) Assembler
  

  







  
</details>
