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

  Various instructions in RISC-V are listed below:

  1.)Pseudo instructions (for e.g- mv rd, rs1)

  2.)Base integer instructions(RV64I)(RV32I) (for e.g- addi, lui)

  3.)Multiply extension(RV64M)(RV32M) (for e.g- divw, mulw)

  4.)Single(RV64F) & double(RV64D) precision floating point extension (for e.g- flw, fadd)

  5.)Application binary interface

  6.)Memory allocation & stack pointer (for e.g- a1, sp, 8)
</details>

<details>
  <summary> Software toolchain </summary>

  To start with 1st lab, we write a simple C program in Ubuntu v18.04 text editor. 

  Following is the C program:

  ```
#include <stdio.h>
int main()
{
  int n=5, y=0, i;

  for (i=0; i<=n; i++)
  {
    y = y + i;
  }
  printf("Required sum is: ", y);
  return 0;
}
  ```
On executing the program, we get the desired result.

![program-1](https://github.com/Vivekchoudhary2/somaiya-riscv/assets/154996509/156b68c6-cf2b-4e82-8383-f11c86921470)

In the above exapmle we ran the code through Windows compiler. 

Now we try to run the same program through RISC-V compiler and try to dive deep till assembly language of RISC-V. 

We do that by using the following command:

```
riscv64-unknown-elf-gcc -o1 -mabi=lp64 -march=rv64 -o <filename.o> <filename.c>
```
On executing the above command, the OS generates an object file(.o).

![program-2](https://github.com/Vivekchoudhary2/somaiya-riscv/assets/154996509/90888fe3-081c-4e66-b63e-deb366f661f0)

The next job is of the linker which combines all the various object files and outputs a single executable file.

The input to linker in an object file.

The output is an single executable file.

To know the details of the file we use the following command:

```
ls -ltr <filename.o>
```

To finally look at the assembly level we use the following command:

```
riscv64-unknown-elf-objdump -d <filename.o>
```

The '-d' stands for disassemble the object file suffixed afterwards.

Here is the behind the scenes of the computer executing the provided C program with 'main' function.

The command for that is:

```
riscv64-unknown-elf-objdump <object file> -d <object filename.o> | less
/main
n
```

![program-3](https://github.com/Vivekchoudhary2/somaiya-riscv/assets/154996509/003ca669-55af-4998-9f66-56a2b4c1309e)

If we were try to figure out number of instructions, it turns out to be
```
(10204 - 10184)/4 = 20 instructions
```
### Spike simulation and debug




  
</details>


