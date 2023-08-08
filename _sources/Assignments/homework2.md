# ✏️ HW 2 Assembly Basic

## 📜 Agenda
- Build and debug `Example02_Addressing` project.
- Build and debug `Homework02_AssemblyBasic` project.
- Learn debugging techniques like single stepping, breakpoints, and watch windows.

```{note} 
Be better programmer by mastering debugging!
```

## 💻 Procedure


```{warning} 
The GIF animations provided on this page are intended to complement the main resources, rather than serve as the main source of information. It's important to thoroughly read the text instructions in order to understand and follow the guidance. If the instructions are unclear, you can refer to the supplementary GIF animations for further clarity. However, solely relying on the animations without reading the accompanying text may make it difficult to accurately follow the instructions.
```


### Setup
- Connect the LaunchPad to your computer via the provided USB cable.
- Open Code Composer Studio (CCS) and select your workspace if prompted.
- Ensure your `Project Explorer` is open on the left of the CCS screen. If not, select View > Project Explorer.

```{note} 
You may be worried about multiple-choice questions where you don’t get partial credit if you make a mistake. However, that’s why there are bonus points in homework, and you can get a grade greater than 16. The points above 16 can be used for other homework assignments. 
```

<br>

### Build and debug `Example02_Addressing` project


- Double-click the `Example02_Addressing` project in the Project Explorer to open it.
- The project that is currently selected will list **[Active - Debug]** next to it.
- Double-click the `main.asm` file under the `Example02_Addressing` project to open it. Before you compile your project, always ensure it is selected and has **[Active - Debug]** next to it.
- Observe the main toolbar at the top of the screen. The toolbar shown below presents before entering the debugger.  If you don't have the toolbar, go to the `Window` menu and then click `Appearance` > `Show Toolbar`.  

```{image} ./figures/HW2_toolbar.png
:width: 540
:align: center
```

- The two main symbols we are concerned with are the `Debug` symbol (bug) and the `Build` symbol (hammer). The `Debug` tool will first build your code and then open the debugger. The `Build` tool will build your code but not open the debugger.  If your code is already built, the `Debug` tool will open the debugger without rebuilding the code.

```{tip}
Always run `Build` first to ensure you have no compile errors.  Then, you can run `Debug`.  Once your code is built with no errors, CCS will just open Debugger without rebuilding the code. If there are compile errors, you cannot run Debugger anyway.
```


- Activate the `Debug` tool by pressing the bug and selecting the `Texas Instruments XDS110 USB Debug Probe/CORTEX_M4_0` tool if prompted. If it requests to update the firmware, select `Update`. Ensure the LaunchPad is connected during this process.
- After the debugger is launched, you should notice a new layout for your screen. This layout is customizable. Different views can be added using the `View` menu at the top.
- The following gif animation displays `HW02_Addressing` for the project title.  It should read `Example02_Addressing`.

```{image} ./figures/HW2_BuildProject.gif
:width: 760
:align: center
```
<br>


- **Breakpoints**: A breakpoint is used to stop the debugger at a specific line of code and allow you to observe the values of variables or expressions. The debugger will then wait to process any more code until the user tells it. When we start the debugger, it automatically stops at the first line of code in the `main` function. You can add breakpoints by double-clicking the vertical bar next to the line of code you are interested in or right-clicking the line and selecting `Breakpoint` > `Breakpoint`. You can observe, turn off, or remove breakpoints using the breakpoint menu.

```{image} ./figures/HW2_Breakpoints.gif
:width: 720
:align: center
```
<br>

- Add a breakpoint at line 78: `ADD R2, R0, R1`. You should see a bubble in the left vertical bar at line 78.


- You should also notice a new toolbar at the top used to debug the code. These tools are used to step through the program and navigate through breakpoints. From left to right they do the following:
    - **Resume**: continue processing code until the next breakpoint or until the code finishes (or loops).
    - **Suspend**: halts processing the program at the current line being processed.
    - **Halt**: stops all processing and exits the debugger.
    - **Step Into (C)**: enter a function call if it is the current line to be processed.
    - **Step Over (C)**: does not enter a function call.
    - **Step Return**: if in a function call, will process the rest of the function and then halt after returning from the function call.
    - **Step Into (ASM)**: enter a function call if it is the current line to be processed.
    - **Step Over (ASM)**: does not enter a function call.


```{image} ./figures/HW2_debug.png
:height: 40px
:align: center
```
<br>

```{attention}
The yellow arrows are for stepping through the **C** code and the green arrows are for the **Assembly** code.
````

<br>

- You can add `Expressions` to observe the values of registers, variables, etc by right-clicking on a register or variable and selecting `Add Watch Expression`. You can also add an expression to the `Expressions` menu by selecting `+ Add new expression`. In the `Expressions` menu, the `Continuous Refresh` button (yellow pause button with two arrows) allows you to continuously update expressions while the program is running.



- Register values can be viewed using the `Registers` tab. The main registers used in assembly code are under the `Core Registers` menu. The values can be set to update continuously using the pause button with two arrows.


```{image} ./figures/HW2_SteppingThrough.gif
:width: 720
:align: center
```

<br>

- You can also add `Memory Browser` in CCS by exploring `View` > `Memory Browser`. Step over until line 83 to find the address of `msg`, which is stored in R0. Enter the value of R0 into Memory Browser to inspect the memory.

- Next, step over until line 102 to find the address of `str`, which is stored in R0. Enter the value of R0 into Memory Browser to inspect the memory.


```{image} ./figures/HW2_MemoryBrowser.gif
:width: 700
:align: center
```

- Click the `Bug` button to build and debug the project.
- Ensure that the `Registers` and `Memory Browser` tabs are visible.
- While stepping through the code, carefully examine the `Program Counter (PC)` value and the addresses in `Memory Browser` as shown below.

```{note} 
In the debug mode, the assembly code highlighted (with the blue arrow next to the line number) is the **next** instruction to execute. The instruction has **NOT** been executed yet! For example, if `AND R2, R0, R1` (line 47) is highlighted, it is the **next** instruction to execute. Currently, The PC value is 0x00000544, which is the address of the instruction, `MOV R2, #0`. Remember PC always holds the address of the **next** instruction to execute.   
```

```{image} ./figures/HW3_ProgramCounter.gif
:width: 760
:align: center
```
<br>

### Build and debug `HW02_AssemblyBasic` project

- Open `HW02_AsmBasic.asm` under the `HW02_AssemblyBasic` project in the Project Explorer.
- Ensure the project that is currently selected - Check **[Active - Debug]** next to it.
- Build and debug the project.  
- Ensure the `Registers` and `Memory Browser` tabs are visible.
- While stepping through the code, carefully examine the `Program Counter (PC)` value and the addresses in `Memory Browser`.

## 🚚 Deliverables
Use the debugger and step through the program to answer the questions in Gradescope.

```{Important} 
Go to Gradescope to answer homework questions.
```







