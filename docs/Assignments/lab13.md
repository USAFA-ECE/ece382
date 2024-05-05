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
- Navigate to Teams > General > Files > Class Materials > SourceFiles.
- Download `SPIA3.c` and `SPIA3.h` and move them into your `workspace/inc` folder using **Windows File Explorer** (not Code Composer Studio). 
- Ensure you overwrite the existing `SPIA3.c` and `SPIA3.h`.

```{warning}
It must be in the **inc** folder, NOT in the Lab 13 project folder.
```

Before proceeding, review the solution for `TimerA1_Init` posted on Gradescope to confirm you have the accurate code. If the solution isn't available on Gradescope, consult your instructor to go over your code.

### Write Timer functions in `TimerA1.c`.

- Open `TimerA1.h` and `TimerA1.c` and read them thoroughly.
- Take a close look at `Program13_1` to grasp (i) how `TimerA1` is initialized and (ii) how the semaphore is employed to coordinate between the foreground and background threads. 
- Proceed to write the `TimerA1_Stop()` and `TA1_0_IRQHandler()` functions, which were discussed in Lecture 13 and can be found in Valvano's textbook. 
- Demonstrate `Program13_1` as shown in the video below.  Make sure to showcase the red LED blinking at 5 Hz, the blue LED blinking at 2.5 Hz, and the LCD updating the elapsed time at a rate of 5 Hz.

<center>
<iframe width="560" height="315" src="https://www.youtube.com/embed/ySVa26xwUzA" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
<br>
Video Credit: C24 Chanon Mallanoo
</center>
<br>


```{important}
Why do we employ a function pointer? 

We want to enable interrupt service routines to execute a range of user-defined tasks without the need for rewriting them each time a new task arises. As an illustration, when we have different tasks for `TimerA1` to execute, we don't have to rewrite the `TimerA1.c` file. Instead, we can create functions that carry out these tasks and simply pass their addresses to `TimerA1`.
```


### Write functions in `PWM.c` and `Motor.c`.

- Thoroughly review the documentation provided in `PWM.h` and `PWM.c` to understand the functions you will be implementing.  
- Note that the PWM period for both motors is set at 20 ms (50 Hz). When calling `PWM_Init34` from `Motor.c`, you should use a period value of 15,000, corresponding to the _PWM period_ of 20 ms. It is not the Timer period, as explained in Lecture 13 Slide 17. 
- Keep in mind that the duty cycles passed into `PWM_DutyRight` and `PWM_DutyLeft` are in _permyriad_.  The code handles the conversion to the Timer period, so you don't need to perform this calculation when calling these functions in Motor.c. However, when passing duty cycles to `PWM_Init34`, they should be in the range of 0-14,999.
 - Read `Motor.h` and `Motor.c` thoroughly.
- We will be replacing the suite of software functions developed in Lab 12 with functions that use `PWM.c` to generate the two PWM outputs. These functions will control the robot's two wheels. The PWM signals to both motors will operate at 50 Hz (20 ms) when active with independent duty cycles. 
- Demo `Program13_2` as shown in the video below. Use motor functions defined in `Motor.c` to maneuver the robot.Activate the next motor function using a bump switch, ensuring you run at least one iteration in the while loop. Please note that the bump switches won't halt the motors.

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

- Demo `Program13_3` as shown in the video below.  Your robot should stop on a collision.

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


This lab was originally adapted from the [TI-RSLK MAX Solderless Maze Edition Curriculum](https://university.ti.com/en/faculty/ti-robotics-system-learning-kit/ti-rslk-max-edition-curriculum) and has since been significantly modified.
