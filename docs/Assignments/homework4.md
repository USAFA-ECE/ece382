# ✏️ HW 4 Subroutines

## 📌 Objectives
- Students should be able to write assembly procedures (also called subroutines or functions) that comply with the AAPCS.
- Students should be able to write assembly conditional instructions and arithmetic operations.

```{note}
In order to understand recursion, one must first understand recursion.
```


## 📜 Synopsis

This homework is an opportunity to learn about subroutines (functions or procedures) in Assembly and to apply that knowledge to two subroutines for calculating factorials.

The first part of this homework is to build and debug the `Example05_Power` project and answer questions in Gradescope. Please read the code and comments carefully as they will help you write the factorial functions in the second part of this homework.

For the second part of this homework, you will write two subroutines calculating **factorials** of _non-negative_ integers. One of them is an iterative function that loops to repeat a block of the code and the other is a recursive function that calls itself to repeat the code.

As all of you are already familiar with, the factorial $n$ is the product of $n$ with the next smaller factorial, $(n-1)!$, i.e.,

\begin{eqnarray*}
  n! &=& n \times (n-1)! \\
     &=& n \times (n-1) \times (n-2) ! \\
     &=& n \times (n-1) \times (n-2) \times \cdots \times 1 
\end{eqnarray*}

```{tip} 
Why is $0!=1$? From the definition of factorial, we can find $(n-1)! = n! / n$.  When $n=1$, we have $0! = 1! / 1 = 1$. 
```

### ARM Architecture Procedure Call Standard (AAPCS)

The Arm architecture places few restrictions on how _general_ purpose registers are used. If you want your code to interact with code that is written by someone else, or with code that is produced by a compiler, then you need to agree rules for register usage. For the Arm architecture, these rules are called the Procedure Call Standard (PCS).

The PCS specifies:

- Which registers are used to pass arguments into the function.
- Which registers are used to return a value to the function doing the calling, known as the caller.
- Which registers the function being called, which is known as the callee, can corrupt.
- Which registers the callee cannot corrupt.

reference: https://developer.arm.com/documentation/102374/0100/Procedure-Call-Standard

To elaborate on the standard:

- If there is one input parameter, it is passed in R0; two, R0-R1; three, R0-R2; four, R0-R3
- If there is an output parameter, it is returned in R0
- Functions can modify R0-R3, and R12 freely
- If a function wishes to use R4-R11 it must save and restore them using the stack.
- If a function calls another, then it must save and restore LR
- Functions must balance the stack

The entire AAPCS can be found <a href="https://github.com/ARM-software/abi-aa/releases/download/2022Q1/aapcs32.pdf" target="_blank">here</a>


```{important} 
It’s important to ensure that your assembly code complies with the AAPCS (ARM Architecture Procedure Call Standard). The AAPCS defines the standard for how subroutines are called and how registers are used in ARM assembly language.

The AAPCS is important because it ensures that code written by different people can be used together without any issues. It also makes it easier to write code that can be reused in different projects.

If you’re not sure whether your code complies with the AAPCS, you can check the ARM documentation or ask instructors for help.
```

## 💻 Procedure

### Setup
- Connect the LaunchPad to your computer via the provided USB cable.
- Open Code Composer Studio (CCS) and select your workspace.
- Ensure your Project Explorer is open on the left of the CCS screen. Otherwise, select View > Project Explorer.

### Execute `Power.asm`.

- Open `PowerMain.c` in the the `Example05_Power` project.
- Uncomment lines 88-89 if they are commented out and ensure lines 90-91 are commented out. They should look like the following.
```C
    Program1();
    Program2();
    // Program3();
    // Program4();
```
- Please read carefully `power1` and `power2` functions. While not required, you can step through the code to understand what each line does.
- Comment out lines 88-89 and uncomment lines 90-91. They should look like the following.
```C
    // Program1();
    // Program2();
    Program3();
    Program4();
```
- `Program3()` and `Program4()` call assembly functions `PowerASM1` and `PowerASM2`, respectively. 
- Build the code and run the debugger.
- Ensure the `Expression`, `Registers`, and `Memory Browser` tabs are visible. 
- As you step through the code, **CAREFULLY READ THE CODE AND ENSURE YOU UNDERSTAND WHAT EACH LINE DOES** as it will help you write two factorial functions in the second part of this homework.
- Step through the program to answer the questions in Gradescope.

### Execute `HW04_FactorialMain.c`

- Open the `HW04_Factorial` project by double-clicking it.
- Open `HW04_FactorialMain.c` in the `HW04_Factorial` project
- Please read the code and comments carefully.  You need to understand the code before writing the equivalent assembly code.
- Execute the code and observe the contents of `result1` and `results2` in the `Expressions` tab.
- Provide a screenshot showing the contents of `result1` and `result2` in the Expressions tab. You must **expand the array to show the individual values** in the array. 

```{attention}
The yellow arrows are for stepping through the C code and the green arrows are for the Assembly code. 
```

### Complete `HW04_Factorial.asm`

- Exclude `HW04_FactorialMain.c` from Build and include `HW04_Factorial.asm`.
- The `main` function is already written for you, but it does not mean you don't have to understand the code. Please read carefully the code and comments before writing your code in `fact_iter` and `fact_rec`.
- Complete `fact_iter` and `fact_rec` in `HW04_Factorial.asm`.
- Use `MUL` instruction for multiplication. For example, Use `MUL R0, R1, R2` for ` R0 <= R1 * R2` or `MUL R0, R1` for ` R0 <= R0 * R1`. 

```{attention} 
Your assembly code must comply with the AAPCS.  
```

## 🚚 Deliverables

### Deliverable 1 

- Build and debug the `Example05_Power` project.  
- Step through the program to answer the questions in Gradescope.

### Deliverable 2 
- Execute `HW04_FactorialMain.c` and provide a screenshot showing the contents of `result1` and `result2` in the Expressions tab. You must **expand the array to show the individual values** in the array. 

- Complete the `fact_iter` subroutine in `HW04_Factorial.asm`.  Provide a screenshot showing the contents of `Res1` after execution of the code. Use Memory Browser and change the encoding style to the **correct format** that everyone can easily understand your result. Your result should be identical to the one from `HW04_FactorialMain.c`. 

- Complete the `fact_rec` subroutine in `HW04_Factorial.asm`. Provide a screenshot showing the contents of `Res2` after execution of the code. Use Memory Browser and change the encoding style to the **correct format** that everyone can easily understand your result.
Your result should be identical to the one from `HW04_FactorialMain.c`. 

- Push your code to Bitbucket.

```{warning}
Your code **must** be compilable.  If your code throws any compile errors, you will get **a grade of 0** for the coding part.
```

