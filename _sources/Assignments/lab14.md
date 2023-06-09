# 🔬 Lab 14 Real-Time Systems

## 📌 Objectives

- Students should be able to implement software for edge-triggered interrupts to detect collisions.
- Students should be able to use shared global variables to communicate between threads.
- Students should be able to use interrupt priority to define the execution order when servicing multiple concurrent events.

```{note}
Any fool can write code that a computer can understand. Good programmers write code that humans can understand.
```

## 📜 Synopsis

The goal of this lab is to use edge-triggered interrupts to detect collisions by the bump sensors while the robot is moving. A collision event should cause an interrupt, and reading the status of the bumper switches should occur in the interrupt service routine (ISR).

In the previous labs, we made the periodic interrupt a high priority because we wanted to run user tasks in real time.  For example, in Lab 10, we took the line-sensor measurements in real time.  However, in Lab 14, the collision task should have a priority even higher than the periodic measurements because a collision represents a dangerous condition that requires immediate service. In lab 8, you interfaced the bump sensors with the microcontroller. In this lab, you will shift the software servicing of the sensors from a periodic interrupt to an event-driven trigger.


## 💻 Procedure


### Setup

- Go to ECE382 Teams > General > Files > Class Materials > SourceFiles.
- Download `Lab14_RealTimeSystemsMain.c` from the Teams folder into your `/workspace/Lab14_RealTimeSystems` folder using Windows File Explorer (not Code Composer Studio).  You need to overwrite the existing file.  **Caution**: It must be in the `Lab14_RealTimeSystems` folder not in the `inc` folder.


### Complete functions in `BumpInt.c`
- **This is part of Homework 14** 
- Complete `BumpInt_Init()` and `PORT4_IRQHandler()` in `BumpInt.c`.
- Follow the instructions inside `BumpInt.c` to complete it.
- Copy and paste the code in Gradescope to submit your Homework 14.


Before starting the following sections, check the solutions to `BumpInt_Init()` and `PORT4_IRQHandler()` posted in Gradescope to ensure you have the correct code. If the solution is not published in Gradescope, go through your code with your instructor.

### Demo `Program14_2()`

- Write `LCDOut2()`. Watch the video below to find what you need to display on the LCD.
- Shown below is the component block diagram for this part of the lab.
 
```{image} ./figures/Lab14_ComponentBlockDiagram14-2.png
:width: 560
:align: center
```

- Demo `Program14_2()` displaying the number of collisions and the bump data that identifies which bump switch is pressed. Explain why the number of collisions displayed in the LCD is incorrect.

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

```{note}
You will construct component block diagrams for the final project. 
```

- The `Controller3()` function should be called by `TA1_0_IRQHandler`.
- To make this happen, `TA1_0_IRQHandler` must know the address of the `Controller3()` function (function pointer).
- When you initialize TimerA1 using `TimerA1_Init`, you should pass the function pointer as an argument.  Then, you need to assign the address of the function to the `TimerA1Task` variable defined at the top of `TimerA1.c`.
- Then, you can call `Controller3()` inside `TA1_0_IRQHandler` using the function pointer, `TimerA1Task`.

```{note}
TimerA1Task is the address of `Controller3`, not the function itself. You must add the `*` symbol to call the function, i.e., `(*TimerA1Task)()`.  
```
  
<br>

:::{important}
Why do we use a function pointer? We want the interrupt service routines to perform various user-defined tasks, but we do not want to rewrite them every time we have a new task.  For example, we have different tasks for `TimerA1` to run, and we don't have to rewrite `TimerA1.c`, but we can simply write functions performing the tasks and pass their addresses to `TimerA1`.  
:::

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


### Make your robot drive in a square.

- Complete `Program14_4` to drive the robot in the maze.
- You may need to adjust your drive-straight PWM values if one wheel is going faster/slower than the other.
- The robot must start behind the start line shown in the figure below.
- You are not allowed to intentionally bump into the walls to decide when to turn (cannot do the bump-n-turn algorithm).  You have to base your decision to turn on your velocity and time (dead reckoning).
- Demo `Program14_4` driving the robot in a maze.
- **[5 Bonus Points]** After arriving at the goal, turn around, drive the maze in the opposite direction, and pass the start line. 

```{image} ./figures/Lab14_DriveInMaze.png
:width: 560
:align: center
```
<br>

<center>
<iframe width="560" height="315" src="https://www.youtube.com/embed/mehYdqnOhfo" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
<br>
Video Credit: C24 Chanon Mallanoo
</center>
<br>

<br>


```{admonition} Q&A
Q: My robot only goes backward, and I think it is because the line that actually calls the motors to move is only called once. I'm not sure if I need to add it somewhere else in order to get it to call multiple times or if it is supposed to be called somewhere in an interrupt. Does anyone have anything for this?
<br>
A: I had that same problem, It is supposed to be called inside mainly the controller and collision function. Every time collision is called through the bump initializer, it should be running a control[currentstep].motor….etc

```

## 🚚 Deliverables

### Deliverable 1 
- **[15 Points]** Demo `Program14_2()` proving the edge-triggered interrupts are properly working.  Your LCD will display the number of collisions and the bump data that identifies which bump switch is pressed. Explain why the number of collisions displayed in the LCD is incorrect. Push your code to your repository using git. Write comments in your code.

### Deliverable 2 
- **[15 Points]** Demo `Program14_3()` running a simple set of commands - `Motor_Backward()`, `Motor_TurnRight()`, and `Motor_Forward()`. On a collision, the robot must stop and restart the set of commands.  Push your code to your repository using git. Write comments in your code.

### Deliverable 3 
- **[20 Points]**  Demo `Program14_4()` driving the robot in the maze.   
    - 10 points for the first turn.
    - 5 points for the second turn.
    - 3 points for the third turn.
    - 2 points for arriving at the goal.

- **[5 Bonus Points]**  After arriving at the goal, turn around, drive the maze in the opposite direction, and pass the start line. It must turn around, and moving backward does not count.



This lab has been adapted from the TI RSLK: [The Solderless Maze Edition curriculum](https://university.ti.com/en/faculty/ti-robotics-system-learning-kit/ti-rslk-max-edition-curriculum)
