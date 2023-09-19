
# 🔬 Lab 10 Multithreading

## 📌 Objectives

- Students should be able to flash the RGB LED using a background thread.
- Students should be able to use Timers to implement multithreading.
- Students should be able to implement an atomic operation to avoid causal dependency.

```{note}
Testing leads to failure, and failure leads to understanding. -Burt Rutan
```

## 📜 Synopsis

The primary objective of this lab is to develop a program that utilizes Timer interrupts to flash RGB LED. When the red and blue LEDs flash slowly, we can observe the two distinct colors alternately. However, if they flash very fast, we will see only one color. The resulting color will be magenta if there is an equal amount of red and blue (50% each) and violet if there is 33.3% red and 66.6% blue. We can use a _foreground_ thread with a delay function to flash red and blue. In this case, the CPU performs no useful operation but wastes CPU power to run a simple loop. Alternatively, we can use a _background_ thread that utilizes a Timer module. In this case, the LED changes its color in the background while the CPU can perform many other operations.

In the next step, you will investigate _causal dependency_ (or race condition) that causes unexpected results when two or more threads access the same data or memory. For example, incrementing a value with a finite number of iterations concurrently can yield random results.

## 💻 Procedure

### Setup
- Open Code Composer Studio (CCS) and select your workspace.
- Ensure your Project Explorer is open on the left of the CCS screen. Otherwise, select View > Project Explorer.
- Open the `Lab10_Multithreading` project by double-clicking it.
- Open Code Composer Studio (CCS) and select your workspace.
- Ensure your Project Explorer is open on the left of the CCS screen.

<!--
### Copy object files
- Go to Teams > General > Files > Class Materials > ObjectFiles.
- Download `SPIA3.obj` and `TimerA2.obj` to your computer.
- Select the two object files and copy them to the `Lab10_Multithreading` project in CCS as shown below.
- Delete `SPIA3.c` and `TimerA2.c` under the `Lab10_Multithreading` project in CCS.

```{image} ./figures/Lab10_ObjectFiles.gif
:width: 640
:align: center
```
<br>
-->

### Blink the RGB LED using a background thread.

- Open the `Lab10_MultithreadingMain.c` file by double-clicking it.
- Thoroughly read `Program10_1` .  
- How does the first while-loop work? What is the delay inside the first while loop? What is the delay inside the second while loop?
- Run `Program10_1`. The RGB LED will flash red and blue.
- Press `Switch 2` on the left side of your LaunchPad.
- The RGB LED will light magenta.
- If you read `Program10_1` carefully, you can find that the RGB LED blinks red and blue before and after you press `Switch 2`.  Why does it show magenta when it blinks at high speed?
- In `Program10_1`, there is only one thread (which is the foreground thread) that flashes the RGB LED. 
- In `Program10_2`, you will flash the RGB LED red and blue in the background thread.
- Read `Program10_2` and find `DisableInterrupts()` and `EnableInterrupts()` inside `Program10_2`. What do they do?  
- In `Program10_2`, we use TimerA2 to execute the `Flash` function periodically. We will learn about Timers in Lecture 13. All we need to know in Lab10 is that the `Flash` function will be executed every millisecond by the _interrupt handler_.   
    - Do not use a loop (while-loop or for-loop) inside the `Flash` function.  Use `Time_1ms` to keep the LED light red for 5 ms and blue for 5 ms. 
    - Remember that the function is executed every 1 ms, and you need to use `Time_1ms` to keep track of the time elapsed.
    - Every time the ISR (`Flash` function) is executed, increment `Time_1ms`.
    - Turn the red LED on if `Time_1ms == 0`, and turn the blue LED on if `Time_1ms == 5`. 
    - If `Time_1ms == 10`, roll over to 0. 

```{important}
The `Flash` function is not to be called by you.  The _interrupt handler_ automatically calls the function every 1 ms and requests the CPU to execute the function.  
```
<br>

```{note}
You should be very familiar with this background thread because we will use it over and over for the rest of the semester.
```

- Demo `Program10_2` showing the RGB LED lights magenta.

<center>
<iframe width="560" height="315" src="https://www.youtube.com/embed/4aZwsF1P0d4" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
<br>
Video Credit: C24 Chanon Mallanoo
</center>
<br>


### Examine causal dependency and fix it using an atomic operation


- Examining the Foreground Thread
    - Start by thoroughly reviewing the `incrementer.asm` code.
    - Ensure that the `Enable_Interrupt()` function inside `Program10_3` is commented out. Uncomment the line `count = Increment();` inside the for-loop to run the Increment function exclusively in the foreground.
    - Run `Program10_3` and take note of the `count` value displayed on the LCD.
    - Does the displayed value match your expectations? If not, explain in Gradescope.

- Examining the Background Thread
    - Ensure that the `Enable_Interrupt()` function inside `Program10_3` is uncommented, and the line `count = Increment();` inside the for-loop is commented out to execute the `Increment` function exclusively in the background.
    - Keep in mind that the background thread operates through `TimerA2`, and the Increment function is invoked every 1 ms.
    - Run `Program10_3` and record the `count` value displayed on the LCD.
    - Does the displayed value align with your expectations? If not, provide an explanation in Gradescope.

- Examining Multithreading
    - Make sure both `Enable_Interrupt()` and `count = Increment();` are uncommented to enable concurrent execution of the `Increment` function in both foreground and background threads.
    - Run `Program10_3` and document the `count` value displayed on the LCD.
    - Does the displayed value match your expectations? If not, explain in Gradescope.

- Fixing Race Condition
    - Make adjustments to `Program10_3` to resolve the issue encountered in the previous step, which was attributed to a race condition.
    - Execute `Program10_3` once more and take note of the count value displayed on the LCD.
    - Does the displayed value now align with your expectations? Explain how your modifications resolved the issue.
    - Demonstrate `Program10_3` displaying the correct value on the LCD while utilizing both foreground and background threads concurrently.


## 🚚 Deliverables

### Deliverable 1 
- **[5 Points]** Demo `Program10_2()` showing that the RGB LED lights magenta using a background thread. 

### Deliverable 2 
- **[5 Points]** Demo `Program10_3()` displaying the correct `count` value on the LCD. You must use both foreground and background threads concurrently.

### Deliverable 3 
- **[9.5 Points]** Push your code to your repository using git. Write comments in your code.


