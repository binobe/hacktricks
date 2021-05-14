# Buffer Overflows

### Anatomy of Memory 

| Kernel  |
| :--- |
| Stack  |
| Heap  |
| Data  |
| Text  |

### Anatomy of Stack 

| ESP\(Extended Stack Pointer\)  |
| :--- |
| Buffer Space  |
| EBP \(Extended Base Pointer\)  |
| EIP \(Extended Instruction Pointer\)/ Return Address  |

### Steps to conduct a buffer overflow 

1. Spiking: Find a vulnerable part of program 
2. Fuzzing: Send characters to a program and check if we can break in 
3. Finding the Offset: Finding The point we break of our break-in 
4. Overwriting the EIP 
5. Finding bad characters: 
6. Finding the Right Module 
7. Generating Shellcode 
8. ROOT!! 

