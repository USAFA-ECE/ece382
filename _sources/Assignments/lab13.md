# 🔬 Lab 13 Timers

## 📌 Objectives

- Students should be able to write software module that generates periodic interrupts to run user-specific tasks.
- Students should be able to generate PWM signals using a timer module.
- Students should be able to use PWM outputs to adjust the motor speeds.


```{note}
Make it work, make it right, make it fast. – Kent Beck
```

## 📜 Synopsis

This lab aims to write software that can adjust the applied power to the two motors. You will create PWM outputs on the P2.6 and P2.7 pins connected to the EN pins of the two TI DRV8838 motor drivers. The period of both PWM outputs should be fixed at 20 ms (50 Hz). However, the software should be able to independently set the duty cycle of the EN pin to each motor from 0 to 999 (0 to 99.9%). At 50 Hz, the motor will not respond to individual highs and lows; instead, the motors will respond to the average level. More specifically, the delivered power will be:

$$
P = VI \frac{d}{100}
$$
where $V$ is the voltage, $I$ is the current, and $d \in [0, 99.9]$ is the duty cycle in percent (%).

Duty cycles are typically expressed as a percentage of `ON` time, which is a real number between 0.0% and 100.0%. However, in microcontrollers, it is preferable to avoid floating point numbers and their computations. Therefore, we will use a 16-bit integer between 0 and 1,000 to represent a duty cycle between 0.0% and 100.0%. We will use the unit permille, which means one out of every one thousand (mille). Another advantage of using permille is that we can express 1,000 different values using a 16-bit integer, whereas we can only have 100 different integer values if we use a percentage. 


## 💻 Procedure

### Setup
Before proceeding, review the solution for `TimerA1_Init` posted on Gradescope to confirm you have the accurate code. If the solution isn't available on Gradescope, consult your instructor to go over your code.

### Write Timer functions in `TimerA1.c`.

1. Open `TimerA1.h` and `TimerA1.c` and read them thoroughly.
1. Carefully examine `Program13_1` to understand (i) how `TimerA1` is initialized and (ii) how the semaphore is employed to coordinate between the foreground and background threads. 
1. Write the `TimerA1_Stop()` and `TA1_0_IRQHandler()` functions, as discussed in Lecture 13 and referenced in Valvano's textbook. 
1. Demonstrate `Program13_1` as shown in the video below. Ensure that the red LED blins at 5 Hz, the blue LED blinks at 2.5 Hz, and the LCD updates the elapsed time at a rate of 5 Hz. Your demo should also show the timer interrupt being enabled by pressing a switch and disabled by pressing a bump sensor. 

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

1. Thoroughly review the documentation provided in `PWM.h` and `PWM.c` to understand the functions you will implement.  
1. Note that the PWM period for both motors is set at 20 ms (50 Hz). When calling `PWM_Init34` from `Motor.c`, you should use a period value of 15,000, corresponding to the _PWM period_ of 20 ms. It is not the Timer period, as explained in Lecture 13 Slide 17. 
1. Keep in mind that the duty cycles passed into `PWM_DutyRight` and `PWM_DutyLeft` are in _permille_.  The code handles the conversion to the Timer period, so you don't need to perform this calculation when calling these functions in Motor.c. 
1. Read `Motor.h` and `Motor.c` thoroughly.
1. Implement the functions in `PWM.c` to generate two PWM outputs controlling the robot's wheels. Both PWM signals operate at 50 Hz (20 ms). 
1. Demo `Program13_2` as shown in the video below. Use motor functions defined in `Motor.c` to maneuver the robot and ensure you run at least one iteration in the while loop. 

    <center>
    <iframe width="560" height="315" src="https://www.youtube.com/embed/qjLGzcN1ncY?si=OrHY9CHkix2kSkfF" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>
    <br>
    Video Credit: C26 Peter Choi, Royal Military College of Canada
    </center>
    <br>

### Write functions in Lab13_Timersmain.c

1. Write `Program13_3()`.
1. Combine the `PWM` and `Timer_A1` periodic interrupt functionality into one software system that controls the robot like `Program 13.2`, but uses the periodic interrupt to track the time elapsed.
1. While the Timer interrupt runs in the background, the foreground thread will display the motor functions on the LCD.
1. As shown in the component block diagram below, `PWM.c` is not directly accessible to `Program13_3`; It is encapsulated in `Motor.c`.  `Program13_3` will only use the functions in Motor.c to control the motors.

    ```{image} ./figures/Lab13_ComponentBlockDiagram.png
    :width: 460
    :align: center
    ```
    <br>

1. Demo `Program13_3` as shown in the video below. 

    <center>
    <iframe width="560" height="315" src="https://www.youtube.com/embed/8QVZNDuu69k?si=qY90STCOwS66JcOY" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>
    <br>
    Video Credit: C26 Peter Choi, Royal Military College of Canada
    </center>
    <br>


## 🚚 Deliverables

### Deliverable 1 
- **[6 Points]**  Demo `Program13_1()`. Use `TimerA1` to blink the red LED at 5 Hz while the blue LED blinks at 2.5 Hz and the LCD updates the elapsed time at 5 Hz. 
Your demo should also show the timer interrupt being enabled by pressing a switch and disabled by pressing a bump sensor. During the demo, explain what you are demonstrating. 

### Deliverable 2 
- **[7 Points]**  Demo `Program13_2()`. Use motor functions defined in `Motor.c` to move the robot. You must run **at least two iterations** in the while loop. During the demo, explain what you are demonstrating. 


### Deliverable 3 
- **[7 Points]**  Demo `Program13_2` as shown in the video below. Use motor functions defined in `Motor.c` to maneuver the robot and ensure you run **at least two iterations** in the while loop. During the demo, explain what you are demonstrating. 

### Deliverable 4 
- **[9.5 Points]**  Push your code to your repository using git. Write comments in your code.


This lab was originally adapted from the [TI-RSLK MAX Solderless Maze Edition Curriculum](https://university.ti.com/en/faculty/ti-robotics-system-learning-kit/ti-rslk-max-edition-curriculum) and has since been significantly modified.
