# ✏️ HW 3 Memory

## 📜 Agenda
- Build and debug `Example01_StringLength` project.
- Build and debug `HW03_MemoryAccess` project.
- Learn about PC, Data Memory (RAM), Instruction Memory (ROM), and the stack.
- Learn debugging techniques like single stepping, breakpoints, and watch windows.

```{note}
The debugger is a programmer's best friend!  Be very familiar with the debugger. It is the key to success in programming courses.
```

## 💻 Procedure

### Setup
- Connect the LaunchPad to your computer via the provided USB cable.
- Open Code Composer Studio (CCS) and select your workspace if prompted.
- Ensure your `Project Explorer` is open on the left of the CCS screen. If not, select View > Project Explorer.


### Build and debug `Example01_StringLength` project.

- Open the `Example01_StringLength` project by double-clicking on it in the `Project Explorer`. When the project is successfully selected, it will be marked as **[Active - Debug]**. 
- Proceed to build your code and initiate the debugger.
- As you navigate through `strlen.asm`, read each comment very carefully to fully comprehend the purpose of every line.
- Make sure the `Expression`, `Registers`, and `Memory Browser` tabs are visible. It might be helpful to arrange them side by side, as illustrated below.

```{image} ./figures/HW3_ArrangeTabs.gif
:width: 640
:align: center
```
<br>

```{note} 
In the debug mode, the assembly code highlighted (with the blue arrow next to the line number) is the **next** instruction to execute. The instruction has **NOT** been executed yet! 
```
<br>

- Before executing `MOV R2, #0` at line 66, add a new expression **StrAddr** in the `Expressions` tab. In the `Value` column, you should find 0x00000570, which corresponds to the address of **StrAddr**. 
- To find its value, input 0x570 into the `Memory Browser`. This will show 0x00000558, which is the address of the first character of `str`. If you input `0x558` and change the encoding style (number format) to `Character`, you will find the string, "This is a string."  
- Keep in mind that `StrAddr` holds the address of the first character, 'T', within `str`. The equivalent syntax in C can be given by 
```C
// The following lines are equivalent.
char* StrAddr = "This is a string.";
// or
char StrAddr[] = "This is a string.";
// or
char StrAddr[18] = "This is a string.";  // Must include the space of a '\0'.
```

- A string constant like `StrAddr` is stored as an array of characters containing the characters of the string and terminated with a '\0' (null character, first character of the [ASCII Table](Resources:ASCII_Table)) to mark the end of the string as shown below.

```
        {'T','h','i','s',' ','i','s',' ','a',' ','s','t','r','i','n','g','\0'} 
          ^     
StrAddr   |     
 +---+    |     
 |  -|-----
 +---+

```

```{important}
It is inaccurate to say that `StrAddr` is the address of the entire string. In fact, `StrAddr` corresponds to the address of the initial character, 'T'. Following this pattern, `StrAddr+1` designate the address of the second character, while `StrAddr+N` signifies the address of the $(N+1)$-th character within the string. To access the $(N+1)$-th character, you can use either *(StrAddr+N) or StrAddr[N].   
```

- After executing `LDR R0, StrAddr` at line 67, examine R0. It should be 0x00000558, which is the value of `StrAddr`. This value agrees with the value we found in the `Memory Broswer`. 
- Execute `LDRB R1, [R0], #1` at line 68 and examine R0 and R1. You will find R0 has been incremented to 0x00000559. You will also find R1 hold the value, 0x54 (84$_{10}$) at 0x00000558. It is the [ASCII](Resources:ASCII_Table) value for 'T'.  
- Put a breakpoint at line 75, `STR R2, [R0]` and click `Resume` (or F8). It will stop at line 75. We have executed line 74, `LDR R0, LAddr`. You can find R0 holds 0x20000000, which is the address of `length`. Since we put `length` under `.data`, the assembler has reserved a space for `length` in the **Data Memory** block.     
- Click `Resume` (or F8) followed by `Suspend (Alt-F8)`.  It will stay at `Stall B Stall` at line 77.  
- Examine the value at 0x20000000 in memory. Change the number format to `32-Bit Signed Int`. Did you find the length of the string stored in memory? 

### Build and debug `HW03_MemoryAccess` project.

- Open the `HW03_MemoryAccess` project by double-clicking it in `Project Explorer`. The project currently selected will list **[Active - Debug]** next to it. 
- Build your code and run the debugger.
- Ensure the `Expression`, `Registers`, and `Memory Browser` tabs are visible. 
- As you step through the code, **CAREFULLY READ EVERY COMMENT TO ENSURE WHAT EACH LINE IS DOING!**
- Step through the program to answer the questions in Gradescope.


```{important}
A solid understanding of memory access and pointers hold significant importance in the realm of computer programming. If you've ever believed that certain programming languages like Python, Java, or C# render pointers unnecessary, you're mistaken - BIG mistake. I encourage you to read [this](../NoPointer.ipynb).  
```

## 🚚 Deliverables
Step through `HW03_Memory.asm` to answer the questions in Gradescope.
