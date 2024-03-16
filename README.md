# RISCV

- [Day 1-Introduction to RISC-V ISA And GNU compiler toolchain ](#Day1--Introduction-to-RISC-V-ISA-And-GNU-compiler-toolchain)

- [Day 2-Introduction to Application Binary Interface And Basic Error Flow](#Day2--Introduction-to-Application-Binary-Interface-And-Basic-error-flow)

- [Day 3-Setting up Platform IDE](#Day3--Setting-up-Platform-IDE)

- [Day 4-Implementing basic functions of input & output](#Day4--Implementing-basic-functions-of-input-and-output)

- [Day 5-Implementing 2-bit array multiplier](#Day5--Implementing-2-bit-array-multiplier)


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

Steps to install Risc-tools (linux)

```
cd /home/vsduser/Desktop
sudo apt install libboost-all-dev
git clone https://github.com/kunalg123/riscv_workshop_collaterals.git
cd riscv_workshop_collaterals
chmod +x run.sh
./run.sh

```

 Once you run it you will get make error. ignore it  and type the following command

 ```

cd ~/riscv_toolchain/iverilog/
git checkout --track -b v10-branch origin/v10-branch
git pull 
chmod 777 autoconf.sh 
./autoconf.sh 
./configure 
make
sudo make install

```

- To set the PATH variable

```
sudo nano .bashrc
#Type the below line at very last and change the username accordingly

export PATH="/home/Alwin/riscv_toolchain/riscv64-unknown-elf-gcc-8.3.0-2019.08.0-x86_64-linux-ubuntu14/bin:$PATH"

# close the bashrc while saving the changes implemented
```

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
  int n=10, y=0, i;

  for (i=0; i<=n; i++)
  {
    y = y + i;
  }
  printf("Required sum is: %d\n", y);
  return 0;
}
  ```
On executing the program, we get the desired result.

![program-1](https://github.com/Vivekchoudhary2/somaiya-riscv/assets/154996509/156b68c6-cf2b-4e82-8383-f11c86921470)

In the above exapmle we ran the code through Windows compiler. 

Now we try to run the same program through RISC-V compiler and try to dive deep till assembly language of RISC-V. 

We do that by using the following command:

```
riscv64-unknown-elf-gcc -o1 -mabi=lp64 -march=rv64i -o <filename.o> <filename.c>
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
Now, to compile and get result of any program implemented in C; the commands used are:

```
gcc filename.c
./a.out
```
We try to do the same process, but this time using RISC-V compiler. 

The commands that help us achieve that are:

```
# For compilation
riscv64-unknown-elf-gcc -ofast -mabi=lp64 -march=rv64i -o <filename.o> <filename.c>
# For output
spike pk <filename.o>
```

![program-13](https://github.com/Vivekchoudhary2/somaiya-riscv/assets/154996509/db5f7997-ce91-4ad7-9408-709d8ffc7422)

For interactive debugging, command used is:

```
spike -d pk <filename.o>
```
As an example shown below, the real-time execution and data updation can be monitored

The command ``` until pc 0 10194``` points the program counter to given address(in this case: 10194). The instruction at that address is still to be executed; it is only executed after we press 'Enter' key.
The data is accordingly udpated.

Now to view the data, the following command used is ``` reg 0 a5 ``` where 'a5' is the ABI name of the register we want to monitor.


In the exapmple shown below, the decimal 10 is loaded into 'a5' register as hexadecimal value(000000000000000a) as soon as the command ``` li a5, 10 ``` is executed.

![program-14](https://github.com/Vivekchoudhary2/somaiya-riscv/assets/154996509/056af35f-8823-46c1-90db-feee7e3b7fef)



</details>

<details>
  <summary> Number system in RISC-V </summary>

  RISC-V operates on binary as well as hexadecimal number system.

  Binary number system: It allows only 2 symbols(0 and 1) to convey information. 
  
  For e.g- 1001 are 4 bits.

  Decimal equivalent = 1*(2^3) + 0*(2^2) + 0*(2^1) + 1*(2^0) = 9

  Hexadecimal number system: It allows 16 symbols to convey information. 4 bits together represent a single symbol.


  For e.g- 110110101111 is same as DAF.
  
  Information content is same but representaion changes.

* Some key terms which will be helpful to know:

  * bit - A bit is the smallest unit of data in computing. It can represent one of two values: 0 or 1

  * byte -  A byte is a group of 8 bits

  * word - A word is group of 32 bits

  * double word - A double word is group of 64 bits

![program-8](https://github.com/Vivekchoudhary2/somaiya-riscv/assets/154996509/6c722045-069f-46e1-a762-052dc973e297)

  
</details>
<details> 
  <summary>Lab on signed and unsigned integers</summary>

  Signed and unsigned integers are two ways of representing whole numbers (integers) in computer programming. The key difference between them lies in how they handle the representation of positive and negative values

  ### Signed Integers:
  *  Range: Signed integers can represent both positive and negative values.

  *  Representation: In a signed integer representation, most significant bit is used to indicate the sign (positive or negative), and the remaining bits represent the magnitude of the number using two's complement or sign-magnitude representation

  *  For e.g- In a 32-bit signed integer, you might have values ranging from -2,147,483,648 to 2,147,483,647

To find the negative number: We do so through 2's complement method

*  Find binary equivalent of given number

*  Find 1's complement(invert individual bits)

*  Then we add 1 to the LSB of the bit sequence to get result

![download](https://github.com/Vivekchoudhary2/somaiya-riscv/assets/154996509/55ea4bfd-bdfc-46e4-a328-89e5d3c2c71c)

### Unsigned Integers:
  *  Range: Unsigned integers represent only non-negative values (zero and positive)

  *  Representation: All bits are used to represent the magnitude of the number

  *  For e.g- In a 32-bit unsigned integer, you might have values ranging from 0 to 4,294,967,295

The highest unsigned integer number in 64-bit OS that can be obtained is 1,84,46,74,40,73,70,95,51,615.
*  How?
    *  The highest number is obtained when all the bits in 64-bit sequence are set to 1. The decimal equivalent turns out to be (2^(64)-1) = 1,84,46,74,40,73,70,95,51,615
 
The following exapmle demonstrates the idea:

We implement the following C code:

```
#inlcude <stdio.h>
#include <math.h>
int main()
{
  unsigned long long int max = (unsigned long long int) (pow(2,64)-1);
  printf("Highest number represented by unsigned long long int is %llu\n", max);
  return 0;
}
```     

We compile the code using RISC-V compiler. The command for that is:

``` riscv64-unknown-elf-gcc -ofast -mabi=lp64 -march=rv64i -o <filename.o> <filename.c> ```

And to get the result using Spike simulator:

``` spike pk <filename.o> ```

![program-15](https://github.com/Vivekchoudhary2/somaiya-riscv/assets/154996509/fe8bbc5a-8d36-4344-b562-e3174ac0cd3e)



On increasing the exponent value (number of bit), we get the following result:


![program-16](https://github.com/Vivekchoudhary2/somaiya-riscv/assets/154996509/7bda31b3-6991-4f95-ba4b-15ac6416479a)

The result is same as the is OS is 64-bit.

Hence, 1,84,46,74,40,73,70,95,51,615 is the maximum unsigned integer value that can be expressed using 64-bit system.

*  For maximum signed integers
    
    *    To get maximum and minimum signed integers, we implement following C code:
 
      ```
      #include <stdio.h>
      #include <math.h>
      int main()
      {
          long long int max = (long long int) (pow(2,63)-1);
          long long int min = (long long int) (pow(2,63)*-1);
          printf("Highest signed number: %lld\n", max);
          printf("Highest signed number: %lld\n", max);
          return 0;
      }
      ```
      *  Results:
 
  ![program 17](https://github.com/Vivekchoudhary2/somaiya-riscv/assets/154996509/41f46020-ea0b-48ff-bc97-72291f88559f)

  Some useful datatypes:

  ![program-20](https://github.com/Vivekchoudhary2/somaiya-riscv/assets/154996509/4aeb8d46-34cc-450c-b19e-6946cecaeb2d)


</details>

<details>
  <summary>Quiz on signed integers</summary>
  For the code as shown below, we won't get the actual maximum and minimum value of signed integers because 'int' is 31 bit and therefore its range is '-2,14,74,83,648 to 2,14,74,83,647' even though the container is 64-bit long.(long long int is 64-bit data)


```     
      #include <stdio.h>
      #include <math.h>
      int main()
      {
          long long int max = (int) (pow(2,63)-1);
          long long int min = (int) (pow(2,63)*-1);
          printf("Highest signed number: %lld\n", max);
          printf("Highest signed number: %lld\n", max);
          return 0;
      }
```  
    


![program 18](https://github.com/Vivekchoudhary2/somaiya-riscv/assets/154996509/c7ea6243-2aeb-476b-ad1e-7be771e23b78)

When made the appropiate corrections, the code works as expected.

```
#include <stdio.h>
      #include <math.h>
      int main()
      {
          long long int max = (long long int) (pow(2,63)-1);
          long long int min = (long long int) (pow(2,63)*-1);
          printf("Highest signed number: %lld\n", max);
          printf("Highest signed number: %lld\n", max);
          return 0;
      }
```

![program 19](https://github.com/Vivekchoudhary2/somaiya-riscv/assets/154996509/af04788f-ffbf-401f-9bae-d0735dadf3f0)

</details>


# Day 2- Introduction to Application Binary Interface And Basic error flow
<details>
  <summary> What is Application binary interface? </summary>
  When the application program accesses the hardware resources through system call process; the way all this works is called the application binary interface.

  What is system call?

  System call is an application trying to directly interact with hardware system.

  This is called the application binary interface.

  One interesting feature of the system call is called the "Kernel mode", wherein the program has access to all system resources, including hardware, memory

  For certain application, if the user wants to access the hardware resources; it does so through registers.

  For that purpose one must understand the architecture of the registers.(For e.g- length of register= 4bit, 8bit and so on)

  * In RISC V architecture, the width of the register is defined as XLEN. For RV64 and RV32, the widths are 64 bits and 32 bits, respectively.

  * RISC V belongs to the little endian memory addressing system, which means that the least significant byte of a word is stored in the smallest memory address.

  ### Registers in RISC-V

  Registers are a type of memory built directly into the processor or CPU that is used to store and manipulate data during the execution of instructions. A register may hold an instruction, a storage address, or any kind of data (such as a bit sequence or individual characters).

  In RISC-V, the width of register is 64-bit for 64-bit architecture(RV-64) and 32-bit for 32- bit architecture(RV-32)

For a certain 64-bit data, the data can either be directly loaded into 64-bit register or it can be loaded through the memory matrix.

When in the case of loading data through memory, memory addressing system is to be known first hand for orderly extraction and storing of data.

* There are 2 types of memory addressing system:
    -  Little endian - least significant byte of a word is stored in the smallest memory address.
    -  Big endian - most significant byte of a word is stored in the smallest memory address
    
* How do we use ABI to access the hardware resources?

  * We make use of certain ISA RISC-V instruction set to perform operations on data bits.
 
# Load, Add and Store Instructions with examples
```
ld x8 16(x23)
```
Here 'ld' stands for load doubleword,x8 shows destination register (rd),16 is offset,x23 is source register. This is I type Instructions: 

![program-4](https://github.com/Vivekchoudhary2/somaiya-riscv/assets/154996509/151c641e-b4dc-4aad-b12a-fc04c92245a6)

The offset value is difference value required to reach the desired address.

```
 add x8,x29,x8
```
Here add is function,x8 is destination register (rd),x29 & x8 is source register. This is R type Instructions: 

![program-5](https://github.com/Vivekchoudhary2/somaiya-riscv/assets/154996509/1f0c8668-709e-4a68-9412-eaa81606a581)

``` 
sd x8,8(x23)
```
Here store is store doubleword,x8 is data registers,8 tell offset(immediate) ,x23 is source register. This is S type Instructions: 

![program-6](https://github.com/Vivekchoudhary2/somaiya-riscv/assets/154996509/2f858320-229a-4d80-aca9-8b46f6cb2f5b)

The ABI names of the registersand their respective functionalities which can accessed by the user through system call are listed below:

![program-7](https://github.com/Vivekchoudhary2/somaiya-riscv/assets/154996509/052e93e4-ce7a-4844-b38e-46222b0b3b89)

</details>
<details>
  <summary> Labwork using ABI function calls </summary>
  In this lab, we take aid of the ABI interface to implement a simple algorithm of adding numbers from 1 to n.

  An interesting thing about this lab is that we, will be explicitly interacting with the hardware.

  The algorithm for the program is as follows:

  
![progrma-8](https://github.com/Vivekchoudhary2/somaiya-riscv/assets/154996509/7452e235-ea7e-465a-b196-f6341a633714)

    
We start by implementing the following C and assembly code with risc-v compiler.

We execute the code using RISC-V Spike simulator.

For compilation we use the following command:

```
riscv64-unknown-elf-gcc -o1 -mabi=lp64 -march=rv64i -o <filename.o> <filename.c> <assembly_filename.S>
```

To get output using RISC-V Spike Simulator, the command used is:

```
spike pk <filename.o>
```
```
#include <stdio.h>

extern int load(int x, int y)

int main(){
  int result = 0;
  int count = 9;
  result = load(0x0, count+1);
  printf("Sum of numbers 0 to %d is %d ", count, result);
}
```
```
.section .text
.global load
.type load, @function

load:
      add a4, a0, zero
      add a2, a0, a1
      add a3, a3, zero
loop: add a3, a3, a4
      adddi a3, a3, 1
      blt a3, a2, loop
      add a0, a4, zero
      ret
```


![program-11](https://github.com/Vivekchoudhary2/somaiya-riscv/assets/154996509/61b3ac31-1d2d-4b10-8cc4-ee1cc07995da)

The 'main' function being executed in RISC-V assembly language.


![program-12](https://github.com/Vivekchoudhary2/somaiya-riscv/assets/154996509/de08fce9-21a4-4f87-a4fe-ebb09c373539)

  
</details>

# Day-3 Setting up PlatformIO IDE
<details>
 <summary> VSDSquadron Development Environment Installation Guide </summary>

  ## Install VSCode

  Download and install VSCode from [https://code.visualstudio.com/](https://code.visualstudio.com/).

  ## Install PlatformIO

  1. Open the "Extensions" sidebar in VSCode.
  2. Search for "PlatformIO" and click "install".

  ![program-21](https://github.com/Vivekchoudhary2/somaiya-riscv/assets/154996509/380e61b4-a63b-4926-92cb-bc1a63d72fde)

  ## Install CH32V Platform

1. Expand the PlatformIO sidebar (ant icon) and click "PIO Home".


![program-22](https://github.com/Vivekchoudhary2/somaiya-riscv/assets/154996509/07325c6e-8acb-4ad8-93ab-820b4777820c)


2.In the PIO Home window, click on the "Platforms" sidebar and choose “Advanced Installation”.

![program-23](https://github.com/Vivekchoudhary2/somaiya-riscv/assets/154996509/95fef7e2-00e4-4782-a109-621969e785e7)


Now, to install files through github on VSCode, it must first have the git extension; otherwise it gives warning of absence of git-extension
The git extension can be found with help of following link

> https://git-scm.com/downloads

Run the installer with default settings.

3.Enter the following repository URL when prompted and press "Install".

> [https://github.com/yathAg/vsdsquadron_pio.git](https://github.com/yathAg/vsdsquadron_pio.git)

The VSDSQUADRON platform should now be successfully installed.

## Uploading blink example

- Click on platforms
- click on vsdsquadron


![program-24](https://github.com/Vivekchoudhary2/somaiya-riscv/assets/154996509/fa34d549-8cc1-4685-94c0-bad400753d37)

- click on Examples
- click on import



![program-25](https://github.com/Vivekchoudhary2/somaiya-riscv/assets/154996509/7124c60c-337e-424e-9315-d8c7660477c4)

The platform is now ready to be used.

</details>

# Day-4 Implementing basic functions of input & output
<details>
 <summary>How to write a Basic code </summary>

 1. Include Header Files:
	
```
#include <ch32v00x.h>
#include <debug.h>
```
2. Pin Configurations:

```
void GPIO_Config(void)
{
GPIO_InitTypeDef GPIO_InitStructure = {0}; //structure variable used for the GPIO configuration
RCC_APB2PeriphClockCmd(RCC_APB2Periph_GPIOD, ENABLE); // to Enable the clock for Port D
}
```

Input Pin Definition:
```
GPIO_InitStructure.GPIO_Pin = GPIO_Pin_X | GPIO_Pin_Y | GPIO_Pin_Z ; // Defines which Pin to
configure
GPIO_InitStructure.GPIO_Mode = GPIO_Mode_IPU;
GPIO_Init(GPIOD, &GPIO_InitStructure);
```

Output Pin Definition:
```
GPIO_InitStructure.GPIO_Pin = GPIO_Pin_X | GPIO_Pin_Y | GPIO_Pin_Z ; // Defines which Pin to
configure
GPIO_InitStructure.GPIO_Mode = GPIO_Mode_Out_PP; // Defines Output Type
GPIO_InitStructure.GPIO_Speed = GPIO_Speed_50MHz; // Defines speed
GPIO_Init(GPIOD, &GPIO_InitStructure);
```
Pin Description According to the Header File ch32v00x.h
```
PD0 => GPIO_Pin_0
PD1 => GPIO_Pin_1
PD2 => GPIO_Pin_2
PD3 => GPIO_Pin_3
PD4 => GPIO_Pin_4
PD5 => GPIO_Pin_5
PD6 => GPIO_Pin_6
PD7 => GPIO_Pin_7
```
3. Main Function:
```
int main(void)
{
uint8_t b0, b1, b2, g0 , g1, g2 = 0;
NVIC_PriorityGroupConfig(NVIC_PriorityGroup_2);
SystemCoreClockUpdate();
Delay_Init();
GPIO_Config();
while(1)
 {

 }
}
```
5. Input / Output Statements:
```
b0 = GPIO_ReadInputDataBit(GPIOD, GPIO_Pin_4); //Input Statement
GPIO_WriteBit(GPIOD, GPIO_Pin_0, RESET); //Reset Output Pin
GPIO_WriteBit(GPIOD, GPIO_Pin_0, SET); //Set Output Pin
```
</details>

# Day 5- Implementing 2-bit array multiplier
<details>
	<summary>Project report</summary>
		(https://drive.google.com/file/d/1FeMuWhDoe-iyqMA1rNgYxU5zoSzB0ILD/view?usp=drive_link)
</details>
<details>
	<summary>Implementation video</summary>
		(https://drive.google.com/file/d/1JeMlfQFkYJ_VlXxBGRq7TP8nSrtVv1xa/view?usp=drive_link)
</details>

# References
<summary>(https://vsdiat.com/)</summary>

<summary>(https://github.com/alwinshaju08/RISCV/blob/main/README.md)</summary>

<summary>(https://pallavaggarwal.in/2023/10/01/ch32v003-low-cost-mcu-programming/)</summary>

<summary>(https://pallavaggarwal.in/2022/11/09/wch-riscv-microcontroller-ch32v003/)</summary>

<summary>(https://pallavaggarwal.in/2023/09/21/ch32v003-gpio-input-polling-interrupt/)</summary>

<summary>(https://pallavaggarwal.in/2023/09/21/ch32v003-programming-gpio-as-output/)</summary>

<summary>(http://www.mounriver.com/)</summary>
