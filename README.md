# Brainfuck-CPU
Verilog implementation of native Brainfuck CPU

# Description

Brainfuck CPU is a hardware implementation of Brainfuck programing language. It uses simple 2-stage pipelining and Harvard's architecture.

This CPU is very similar to a Touring machine. It operates over a linear memory space. A main difference is that the memory is finite.

Instruction set is fairy simple. Opcodes are only 3-bit wide. This allows lower usage of resources over original coding (7 bit ASCII). Verion using original instruction set is available as well. (brainfuck_cpu_ascii.v)

# Opcodes

Opcodes:
000 == < (move pointer to the left)
001 == > (move pointer to the right)
010 == + (add 1 to the value of a current memory cell)
011 == - (subtract 1 from the value of a current memory cell)
100 == , (read from the I/O port)
101 == . (write to the I/O port)
110 == [ (begining of the loop)
111 == ] (end of the loop)

ASCII opcodes (7-bit):
0x3C == <
0x3E == >
0x2B == +
0x2D == -
0x2C == ,
0x2E == .
0x5B == [
0x5D == ]
other == NOP

# Opcodes timing

At start it is necesary to reset all memory locations to 0. The CPU does that automatically after reset.

Operations '+', '-', ',' and '.' are always completed in one cycle. As it is necessary to load or store a memory cell from/to memory, '>' and '<' takes two cycles to finish. Opcode ']' takes two cycles on a loop finish and one on end, but '[' is a special case. If a loop condition is not met (value of the current cell is not 0) it takes one cycle. But in worst case scenario, a condition is meet at entry. It means that CPU has to find closing bracket ']' right away. This can waste many cycles, but it is a situation that should not occur, if the program is writen correctly.

# Files

CPU consists of only one file: "brainfuck_cpu.v". It is a CPU core and it requires addition of ROM and RAM.

Second file is a testbench - "brainfuck_cpu_tb.v". It demonstrates the execution of a "Hello World!" program writen in Brainfuck and compressed to 3 bits.

# Parameters

brainfuck_cpu.v accepts 3 parameters:

DATA_ADDR_WIDTH - defines the width of data memory address bus and in a consequence the size of this memory.

ROM_ADDR_WIDTH - defines the width of program memory address bus and in a consequence the size of this memory. Note that Brainfuck language requires a lot of program memory. Simple "Hello World!" program uses 115 cells.

STACK_DEPTH - defines a depth of the stack. Stack is used to keep track of the execution of loops. It's depth imposes maximal level of loops nesting supported.
