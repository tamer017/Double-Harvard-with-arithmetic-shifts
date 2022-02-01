# Double-Harvard-with-arithmetic-shifts
simulate a fictional processor design and architecture using Java.


Double Harvard with arithmetic shifts


    1 Memory Architecture 
      a) Architecture: Harvard
        • Harvard Architecture is the digital computer architecture whose design is based on the concept
        where there are separate storage and separate buses (signal path) for instruction and
        data. It was basically developed to overcome the bottleneck of Von Neumann Architecture.

      b) Instruction Memory Size: 1024 * 16
      
  ![1](https://user-images.githubusercontent.com/83555471/151936981-f49a4e78-a68e-42e0-8675-cf656ad6a8df.PNG)

        • The instruction memory addresses are from 0 to 2 10 − 1 (0 to 1023).
        • Each memory block (row) contains 1 word which is 16 bits (2 bytes).
        • The instruction memory is word addressable.
        • The program instructions are stored in the instruction memory
     
     c) Data Memory Size: 2048 * 8 
     
 ![2](https://user-images.githubusercontent.com/83555471/151936984-476e5b81-57c0-4a13-a6c3-49cc23dc5738.PNG)

        • The data memory addresses are from 0 to 2^11 − 1 (0 to 2047).
        • Each memory block (row) contains 1 word which is 8 bits (1 byte).
        • The data memory is word/byte addressable (1 word = 1 byte).
        • The data is stored in the data memory.

      d) Registers: 66
        • Size: 8 bits
        • 64 General-Purpose Registers (GPRS)
          – Names: R0 to R63
        • 1 Status Register
        
   ![3](https://user-images.githubusercontent.com/83555471/151936986-56409704-0d22-439d-83b3-85bd4ca73bf1.PNG)

          – Name: SREG
          – A status register, flag register, or condition code register (CCR) is a collection of statusflag bits for a processor.
          – The status register has 5 flags updated after the execution of specific instructions:
        ∗ Carry Flag (C): Indicates when an arithmetic carry or borrow has been generated outof the most significant bit position.
          · C = 1 if result > Byte.MAX_VALUE
          · C = 0 if result <= Byte.MAX_VALUE
          · Full cases and scenarios explanation: https://piazza.com/class/kmoutjsotl76h5?cid=295
        ∗ Two’s Complement Overflow Flag (V): Indicates when the result of a signed numberoperation is too large, causing the high-order bit to overflow into the sign bit.
          · If 2 numbers are added, and they both have the same sign (both positive or bothnegative), then overflow occurs (V = 1) if and only if the result has the  oppositesign. Overflow never occurs when adding operands with different signs.
          · If 2 numbers are subtracted, and their signs are different, then overflow occurs (V= 1) if and only if the result has the same sign as the subtrahend.
          · The difference between carry and overflow is explained in: https://piazza.com/class/kmoutjsotl76h5?cid=79
        ∗ Negative Flag (N): Indicates a negative result in an arithmetic or logic operation.
          · N = 1 if result is negative.
          · N = 0 if result is positive or zero.
        ∗ Sign Flag (S): Indicates the expected sign of the result (not the actual sign).
          · S = N ⊕ V (XORing the negative and overflow flags will calculate the sign flag).
        ∗ Zero Flag (Z): Indicates that the result of an arithmetic or logical operation was zero.
          · Z = 1 if result is 0.
          · Z = 0 if result is not 0.
        ∗ Since all registers are 8 bits, and we are only using 5 bits in the Status Register forthe flags, you are required to keep Bits7:5 cleared “0” at all times in the register.
        • 1 Program Counter
          – Name: PC
          – Type: Special-purpose register with a size of 16 bits (not 8 bits).
          – A program counter is a register in a computer processor that contains the address (location) of the instruction being executed at the current time.
          – As each instruction gets fetched, the program counter is incremented to point to the next instruction to be executed.



    2 Instruction Set Architecture
      a) Instruction Size: 16 bits
      
      b) Instruction Types: 2
      
   ![4](https://user-images.githubusercontent.com/83555471/151936972-fd47fe00-0e1a-493d-80d9-c53bf8d391da.PNG)

      c) Instruction Count: 12
        • The opcodes are from 0 to 11 according to the instructions order in the following table:
        
   ![5](https://user-images.githubusercontent.com/83555471/151936975-11fec273-d517-4467-83c9-a70aa8299464.PNG)

        “||” symbol indicates concatenation (0100 || 1100 = 01001100).
        ∗ 0 is repeated IMM times before right concatenating it to R1[7-IMM:0].
        ∗∗ R1[7] is repeated IMM times before left concatenating it to R1[7:IMM].
      d) The Status Register (SREG) flags are affected by the following instructions:
        • The Carry flag (C) is updated every ADD, SUB, and MUL instruction.
        • The Overflow flag (V) is updated every ADD and SUB instruction.
        • The Negative flag (N) is updated every ADD, SUB, MUL, ANDI, EOR, SAL, and SAR instruction.
        • The Sign flag (S) is updated every ADD and SUB instruction.
        • The Zero flag (Z) is updated every ADD, SUB, MUL, ANDI, EOR, SAL, and SAR instruction.
        • A flag value can only be updated by the instructions related to it.
        
        
    3 Datapath
      a) Stages: 3
        • All instructions regardless of their type must pass through all 3 stages.
        • Instruction Fetch (IF): Fetches the next instruction from the main memory using the address in the PC (Program Counter).
        • Instruction Decode (ID): Decodes the instruction and reads any operands required from the register file.
        • Execute (EX): Executes the instruction. In fact, all ALU operations are done in this stage.
        Moreover, it performs any memory access required by the current instruction. For loads, it
        would load an operand from the main memory, while for stores, it would store an operand into
        the main memory. Finally, for instructions that have a result (a destination register), it writes this result back to the register file.
      b) Pipeline: 3 instructions (maximum) running in parallel
        • Number of clock cycles: 3 + ((n − 1) ∗ 1), where n = number of instructions
          – Imagine a program with 7 instructions:
            ∗ 3 + (6 ∗ 1) = 9 clock cycles
          – You are required to understand the pattern in the example and implement it.

   ![6](https://user-images.githubusercontent.com/83555471/151936978-cd53c8b1-8dd9-4097-b15a-f23023d3355f.PNG)
   
   
   How to run the code 
   
    a)  write your program in assembly language in a text file.
    b) run the main class
  
    The following items is printed in the console after each Clock Cycle:
        a) The Clock Cycle number.
        b) The Pipeline stages:
            • Which instruction is being executed at each stage?
            • What are the input parameters/values for each stage?
        c) The updates occurring to the registers in case a register value was changed.
        d) The updates occurring in the memory (data segment of main memory or data memory ) in case a value was stored or updated in the memory.
        e) The content of all registers after the last clock cycle.
        f) The full content of the memory (main memory or instruction and data memories ) after the last clock cycle

