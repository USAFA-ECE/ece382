# 🔬 Lab 2 Assembly Basic

## 📌 Objectives
- Students should be able to identify the flags tested when conditional instructions in the assembly are executed.
- Students should be able to find values stored in registers and memory space using the CCS debugging tools.
- Students should be able to debug assembly code using debugging techniques like single stepping, breakpoints, and watch windows.

```{note}
Design is where science and art break even.
```


## 📜 Synopsis
In this lab, you will write an Assembly program that finds the integer square root. The equivalent C program is provided in `Lab02_SqrtMain.c` in the `Lab02_SqaureRoot` project.

```{note}
The algorithm used in this lab is Newton's Method.  For an explanation on how Newton's Method works and how we can use it to find the square root, check out [this video](https://youtu.be/q0DyLZyStcg).
```

## 💻 Procedure

### Setup
- Connect the LaunchPad to your computer via the provided USB cable.
- Open Code Composer Studio (CCS) and select your workspace.
- Ensure your Project Explorer is open on the left of the CCS screen. Otherwise, select View > Project Explorer.
- Open the `Lab02_SqaureRoot` project by double-clicking it.


### Run Lab02_SqrtMain.c

- Open `Lab02_SqrtMain.c` in the `Lab02_SquareRoot` project and carefully read the code. 
- Build and debug the project.  
- Ensure the `Variables` tab is visible. If it is not visible, Click View > Variables. 
- While stepping through the code, carefully examine the variables until `t` remains unchanged. Ensure you use the **yellow arrows** to step through the C code, not the green arrows.

```{image} ./figures/HW2_debug.png
:height: 40px
:align: center
```
<br>

```{Attention}
The yellow arrows are for stepping through the **C** code and the green arrows are for the **Assembly** code.
```

- If you click on `Resume` (F8) followed by `Suspend` (Alt+F8), it will stop at `while(1);`.  Observe the variables. 

### Complete `Lab02_Sqrt.asm`

- Right-click `Lab02_SqrtMain.c` and click **Exclude from Build**.
- Right-click `Lab02_Sqrt.asm` and uncheck **Exclude from Build**.
- Write your code. 
- Build and debug the project.  
- Ensure the `Registers` tab is visible. If it is not visible, Click View > Registers. 
- While stepping through the code, carefully examine the register values until R2 (`t`) remains unchanged. Ensure you use the **green arrows** to step through the assembly code, not the yellow arrows. You can change the number format of a value from the default of Hex to Decimal by right-clicking the value followed by `Number Format` > `Decimal` as shown below.

```{image} ./figures/Lab02_NumberFormat.png
:width: 460px
:align: center
```

- If you click on `Resume` (F8) followed by `Suspend` (Alt+F8), it will stop at `stall B stall`.  Observe the registers. 
- Use this program to find $\sqrt{29583}$.
- Take a screenshot of the registers: R0 - R4.

```{warning}
Please take a screenshot of the `region of interest` and ensure that it is legible. If you take a screenshot of the entire display, for example, and your instructor has trouble reading it, you might not get full credit. An example is provided below.
```

```{image} ./figures/Lab02_BadScreenShot.png
:width: 560px
:align: center
```

```{attention}
You will receive a grade of **-10 points** if you submit a picture of a computer screen taken by your phone or mobile device.
```

## 🚚 Deliverables

```{warning}
Your code **must be compilable**.  If your code throws any compile errors, you will get **a grade of 0** for the coding part.
```

### Deliverable 1
- **[10 Points]** Complete `Lab02_Sqrt.asm` and push your code to your repository. 

### Deliverable 2
- **[8 Points]** Find $\sqrt{29583}$ and provide a screenshot of the register that stores
$\sqrt{29583}$.  _Change the number format to Decimal before taking the screenshot._

### Deliverable 3
- **[1.5 Points]** Briefly explain why your answer keeps changing between two values.

### Deliverable 4
- **[0.5 Points]** Time log.

