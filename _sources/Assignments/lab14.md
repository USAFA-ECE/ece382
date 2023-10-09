# 🔬 Lab 14 Real-Time Systems

## 📌 Objectives

- Students should be able to implement software for edge-triggered interrupts to detect collisions.
- Students should be able to utilize shared global variables as a means of communication between threads.
- Students should be able to utilize interrupt priority to establish the execution order when handling multiple concurrent events.

```{note}
Any fool can write code that a computer can understand. Good programmers write code that humans can understand.
```

## 📜 Synopsis

The aim of this lab is to utilize edge-triggered interrupts for detecting collisions through the bump sensors while the robot is in motion. When a collision event occurs, it should trigger an interrupt, and the reading of the bumper switches should be handled within the interrupt service routine (ISR).

In previous labs, we assigned high priority to periodic interrupts to ensure real-time execution of user tasks. For instance, in Lab 13, we updated PWM duty cycles in real time. However, in Lab 14, the collision task requires an even higher priority than the periodic tasks because collisions represent potentially dangerous conditions that demand immediate attention. In this lab, you'll transition from periodic interrupt-driven sensor servicing to event-driven triggering in the software.

## 💻 Procedure

### Complete functions in `BumpInt.c`
- **This is part of Homework 14** 
- Complete the implementation of `BumpInt_Init()` and `PORT4_IRQHandler()` within the `BumpInt.c` file. Refer to the instructions provided within `BumpInt.c` for guidance on how to accomplish these tasks.
- After completion, copy and paste the code into Gradescope to submit your Homework 14.

Before proceeding with the following sections, review the solutions for `BumpInt_Init()` and `PORT4_IRQHandler()` available on Gradescope. This will help ensure the correctness of your code. In case the solutions are not accessible on Gradescope, go through your code with your instructor.

### Demo `Program14_2()`
- Implement the `LCDOut2()` function. Refer to the video below for guidance on what should be displayed on the LCD.
- Below is the block diagram illustrating the components involved in this section of the lab.
 
```{image} ./figures/Lab14_ComponentBlockDiagram14-2.png
:width: 560
:align: center
```

- Demo `Program14_2()`, showcasing the count of collisions and the bump data identifying the pressed bump switch. Explain why the count of collisions displayed on the LCD is incorrect.

<br>

<center>
<iframe width="560" height="315" src="https://www.youtube.com/embed/NP0i63dhgz8" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
<br>
Video Credit: C24 Chanon Mallanoo
</center>
<br>


### Complete functions for `Program14_3()`

- Write `Controller3()`, `Collision3()`, and `Program14_3()`.
- As shown in the component block diagram below, there are two interrupts: TimerA1.0 and Port4.  
```{image} ./figures/Lab14_ComponentBlockDiagram14-3.png
:width: 760
:align: center
```
<br>


- The `Controller3()` function should be called by `TA1_0_IRQHandler`.
- To make this happen, `TA1_0_IRQHandler` must know the address of the `Controller3()` function (function pointer).
- When you initialize TimerA1 using `TimerA1_Init`, you should pass the function pointer as an argument.  Then, you need to assign the address of the function to the `TimerA1Task` variable defined at the top of `TimerA1.c`.
- Then, you can call `Controller3()` inside `TA1_0_IRQHandler` using the function pointer, `TimerA1Task`.

```{note}
TimerA1Task is the address of `Controller3`, not the function itself. You must add the `*` symbol to call the function, i.e., `(*TimerA1Task)()`.  
```
  
<br>

- Similarly, the `Collision3()` functions will be called by `PORT4_IRQHandler`. 
- To make this happen, you need to pass the address of `Collision3()` as an argument to `BumpInt_Init`. 
- The pointer `BumpTask` inside `BumpInt.c` holds the address of `Collision3`, and therefore `*BumpTask` is the `Collision3` function itself. 
- So, `(*BumpTask)(Bump_Read())` is the same as `Collision3(Bump_Read())`.

:::{tip}
```c
Collision3(Bump_Read());
```
is the same as
```c
uint8_t x = Bump_Read();
Collision3(x);
```
You know $f(g(x))$ is the same as $y=g(x)$ followed by $f(y)$. 
:::


- Inside `Lab14_RealTimeSystemsMain.c`, implement the `Controller3` function. You should **not** have a while- or for-loop inside `Controller3`.  Remember that the `Controller3` function will be called every 1 ms. If you have a loop or a delay inside an ISR, you are most likely doing something terribly wrong. 
- Complete `Program14_3()`.
- Test `Program14_3()` without edge-triggered interrupts from the bump sensors. The robot should run the three commands indefinitely.  
- Implement Collision and test your bump sensors. Hitting a bump switch must stop the robot and restart the three commands from the beginning, which is `Motor_Backward`.
- Demo `Program14_3` running a simple set of commands - `Motor_Backward`, `Motor_TurnRight`, and `Motor_Forward`. On a collision, the robot must stop and restart the set of commands.


<center>
<iframe width="560" height="315" src="https://www.youtube.com/embed/dmsQhTmzbmQ" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
<br>
Video Credit: C24 Chanon Mallanoo
</center>
<br>


## 🚚 Deliverables

### Deliverable 1 
- **[6 Points]** Demo `Program14_2()` proving the edge-triggered interrupts are properly working.  Your LCD will display the number of collisions and the bump data that identifies which bump switch is pressed. Explain why the number of collisions displayed in the LCD is incorrect. 

### Deliverable 2 
- **[6 Points]** Demo `Program14_3()` running a simple set of commands - `Motor_Backward()`, `Motor_TurnRight()`, and `Motor_Forward()`. On a collision, the robot must stop and restart the set of commands. 

### Deliverable 3 
- **[7.5 Points]** Push your code to your repository using git. Write comments in your code.


This lab has been adapted from the TI RSLK: [The Solderless Maze Edition curriculum](https://university.ti.com/en/faculty/ti-robotics-system-learning-kit/ti-rslk-max-edition-curriculum)
