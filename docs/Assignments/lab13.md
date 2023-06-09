# 🔬 Lab 13 Timers

## 📌 Objectives

- Students should be able to write software module that generates periodic interrupts to run user-specific tasks.
- Students should be able to generate PWM signals using a timer module.
- Students should be able to use PWM outputs to adjust the motor speeds.


```{note}
Make it work, make it right, make it fast. – Kent Beck
```

## 📜 Synopsis

This lab aims to write software that can adjust the applied power to the two motors. You will create PWM outputs on the P2.6 and P2.7 pins connected to the EN pins of the two TI DRV8838 motor drivers. The period of both PWM outputs should be fixed at 20 ms (50 Hz). However, the software should be able to independently set the duty cycle of the EN pin to each motor from 0 to 9,999 (0 to 99.99%). At 50 Hz, the motor will not respond to individual highs and lows; instead, the motors will respond to the average level. More specifically, the delivered power will be:

$$
P = VI \frac{d}{100}
$$
where $V$ is the voltage, $I$ is the current, and $d \in [0, 99.99]$ is the duty cycle in percent (%).



## 💻 Procedure

### Setup
- Go to Teams > General > Files > Class Materials > SourceFiles.
- Download `SPIA3.c` into your workspace/inc folder using Windows File Explorer (not Code Composer Studio). 
- You need to overwrite the existing `SPIA3.c`.

```{Warning}
It must be in the inc folder, not in your project folder.
```

Before starting the next, check the solution to `TimerA1_Init` posted in Gradescope to ensure you have the correct code. If the solution is not published in Gradescope, go through your code with your instructor.


### Write Timer functions in `TimerA1.c`.

- Delete line 62 in Lab13_Timersmain.c.  We don't use SysTick this year.
- Open `TimerA1.h` and `TimerA1.c` and read them thoroughly.
- Carefully examine `Program13_1` to understand (i) how TimerA1 is initialized and (ii) how the semaphore is used between the foreground and background threads. 
- Write `TimerA1_Stop()`, and `TA1_0_IRQHandler()`.  These functions were covered in Lecture 13 and Valvano's textbook. 
- Demo `Program13_1` as shown in the video below.  You should demonstrate the red LED blinking at 5 Hz and the blue LED blinking at 2.5 Hz. The LCD should update the elapsed time at 5 Hz.

<center>
<iframe width="560" height="315" src="https://www.youtube.com/embed/ySVa26xwUzA" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
<br>
Video Credit: C24 Chanon Mallanoo
</center>
<br>


:::{important}
Why do we use a function pointer? We want the interrupt service routines to perform various user-defined tasks, but we do not want to rewrite them every time we have a new task.  For example, we have different tasks for `TimerA1` to run, and we don't have to rewrite `TimerA1.c`, but we can simply write functions performing the tasks and pass their addresses to `TimerA1`.  
:::


### Write functions in `PWM.c` and `Motor.c`.

- Carefully read the documentation in `PWM.h` and `PWM.c` for the functions you need to implement.  
- The PWM period for both motors is 20 ms (50 Hz). When `PWM_Init34` is called by `Motor.c`, you must pass 15,000 for the period corresponding to the _PWM period_ of 20 ms. It is not the Timer period (refer to Lecture 13 Slide 17). 
- The duty cycles passed into `PWM_DutyRight` and `PWM_DutyLeft` are in permyriad, which we need to convert to the Timer period (it is already done in the code).  Otherwise, we always need to calculate the duty cycles when you call the functions in `Motor.c`, e.g., you need to pass $(duty \%)\times 100\times(3/2)$. However, the two duty cycles passed into `PWM_Init34` are in a 0-14,999 range, and you don't need a unit conversion between permyriad and the Timer period.  
- Read `Motor.h` and `Motor.c` thoroughly.
- We will replace the suite of software functions built in Lab 12 with functions that use `PWM.c` to create the two PWM outputs. This suite of functions will control the two wheels on the robot. When active, the PWM to both motors will be 50 Hz (20 ms), but have independent duty cycles. 
- Demo `Program13_2` as shown in the video below. Use motor functions defined in Motor.c to move the robot. Use a bump switch to start the next motor function. You must run at least one iteration in the while loop. Note: The bump switches cannot stop the motors.

<center>
<iframe width="560" height="315" src="https://www.youtube.com/embed/jMpPHZ5NVKg" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
</center>
<br>



### Write functions in Lab13_Timersmain.c

- Write `CheckBumper()` and `Program13_3()`.
- Combine the `PWM` and `Timer_A1` periodic interrupt functionality into one software system that runs the robot like `Program 13.2`, but uses the periodic interrupt to check the bump switches, stopping the robot on a collision.  The robot will run for 3 seconds unless a bump switch is pressed, and the robot will not skip the next functions in the while loop as it did in Lab 12.
- As shown in the component block diagram below, `PWM.c` is not exposed to `Program13_3`.  It is hidden under `Motor.c` and `Program13_3` will only access the functions in Motor.c to control the motors.

```{image} ./figures/Lab13_ComponentBlockDiagram.png
:width: 460
:align: center
```
<br>

- Demo `Program13_3` as shown in the video below.  Your robot must stop on a collision.

<center>
<iframe width="560" height="315" src="https://www.youtube.com/embed/DqtfwLTfbmc" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
</center>
<br>



## 🚚 Deliverables

### Deliverable 1 
- **[6 Points]**  Demo `Program13_1()`. Use `TimerA1` to blink the red LED at 5 Hz while the blue LED blinks at 2.5 Hz and the LCD updates the elapsed time at 5 Hz. 

### Deliverable 2 
- **[7 Points]**  Demo `Program13_2()`. Use motor functions defined in `Motor.c` to move the robot. Use a bump switch  to start the next motor function. You must run at least one iteration in the while loop. Note: The bump switches cannot stop the motors.

### Deliverable 3 
- **[7 Points]**  Demo `Program13_3()`. Use PWM to move the robot like `Program13_2`, but use `TimerA1` to periodically check the bump switches, stopping the robot on a collision. You must run the robot on the ground for at least one iteration in the while loop without hitting any bump switch. You should show how the robot responds when you hit a bumper switch in each motor function.

### Deliverable 4 
- **[9.5 Points]**  Push your code to your repository using git. Write comments in your code.


This lab has been adapted from the TI RSLK: [The Solderless Maze Edition curriculum](https://university.ti.com/en/faculty/ti-robotics-system-learning-kit/ti-rslk-max-edition-curriculum)
