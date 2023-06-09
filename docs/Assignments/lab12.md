# 🔬 Lab 12 Motors

## 📌 Objectives

- Students should be able to interface the motors with the MSP432 microcontroller.
- Students should be able to implement PWM signals to set the speed of each motor independently.

```{note}
One man’s crappy software is another man’s full-time job. -Jessica Gaston
```

## 📜 Synopsis

The objective of this lab is to develop a set of software functions that control the two wheels on the robot. You will generate PWM outputs on the P2.6 and P2.7 pins, which are connected to the EN pins of the two TI DRV8838 motor drivers. The period of both PWM outputs should be fixed at 10 ms (100 Hz). However, the software should be capable of independently setting the duty cycle of the EN pin to each motor from 0.0% to 99.99%. At 100 Hz, the motor will not respond to individual highs and lows; instead, it will respond to the average level.

Duty cycles are typically expressed as a percentage of `ON` time, which is a real number between 0.0% and 100.0%. However, in microcontrollers, it is preferable to avoid floating point numbers and their computations. Therefore, we will use a 16-bit integer between 0 and 10,000 to represent a duty cycle between 0.00% and 100.00%. We will use the unit permyriad, which is rarely used and means one out of every ten thousand (myriad), or one percent of one percent. Another advantage of using permyriad is that we can express 10,000 different values using a 16-bit integer, whereas we can only have 100 different integer values if we use a percentage. 

```{important}
Permyriad (literally, per 10,000) represents a portion of a whole in parts per 10,000, just as permille represents parts per 1,000 and percent represents parts per 100. Permyriad means one out of every ten thousand (myriad), or one percent of one percent.”
```
 
    
## 💻 Procedure

### Setup
- Connect the LaunchPad to your computer via the provided USB cable.
- Open Code Composer Studio (CCS) and select your starter workspace.
- Ensure your Project Explorer is open on the left of the CCS screen. Otherwise, select View > Project Explorer.
- Open the `Lab12_Motors` project by double-clicking it.

### Write motor functions in `MotorSimple.c`

- Open `MotorSimple.h` and read it thoroughly.
- Open `MotorSimple.c` and read the comments thoroughly.
- Write `MotorSimple_Forward()`, `MotorSimple_Backward()`, `MotorSimple_Left()`, and `MotorSimple_Right()`.
- Write `MotorSimple_Brake()` and `MotorSimple_Coast()`.
- Run `Program12_1()` to test `MotorSimple.c`
- Press one of the bump switches to test the following motor functions.
- Demo `Program12_1()` proving both motors work and the bump switches stop the motors. For a video demo, **add your voice** to clearly state the motor functions. 
- You must run at least one iteration of the while loop without hitting any bump switches.
- You also need to run at least one iteration of the while loop and stop the motor for each motor function by hitting a bump switch. Note that `MotorSimple_Backward()` should **not** respond to the bump switches - should keep moving backward when a bump switch is pressed.
- An example video demo (with voice) is linked below.

<center>
<iframe width="560" height="315" src="https://www.youtube.com/embed/GnwVuP6Y6JU" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
<br>
Video Credit: C24 Chanon Mallanoo
</center>

- Push your code to your repository using git. Ensure you write comments in your code.


### Demo `Program12_2()`

- Demo `Program12_2()` proving both motors work and the bump sensors stop the motors.  
- You must run the robot **on the ground** for at least one iteration in the while loop without hitting any bump switches.  
- You also need to show how the robot responds when you hit a bump switch for each motor function, as shown in the video below.
- When you hit a bump switch while the robot turns, it usually skips the following one or two function calls and immediately moves backward.  Explain why it happens.

<center>
<iframe width="560" height="315" src="https://www.youtube.com/embed/8WJe7KLqGQA" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
</center>

<br>


## 🚚 Deliverables

### Deliverable 1 
- **[6 Points]** Demo `Program12_1()` proving both motors work and the bump sensors stop the motors. You must run at least one iteration of the while loop without hitting any bump switches. You also need to run at least one iteration of the while loop and stop the motor for each motor function by hitting a bump switch. For a video demo, **add your voice** to clearly state the motor functions. 

### Deliverable 2 
- **[4 Points]** Demo `Program12_2()` proving both motors work and the bump sensors stop the motors.  You must run the robot **on the ground** for at least one iteration in the while loop without hitting any bump switches.  You need to show how the robot responds when you hit a bump switch for each motor function.

### Deliverable 3 
- **[9.5 Points]** Push your code to your repository using git. Ensure you write comments in your code.

### Bonus Points 
- **[1 Points]**  When you hit a bump switch while the robot turn, it usually skips the next one or two function calls and immediately moves backward.  Explain why it happens. Explain why it happens. Hint: How fast can you press a bump switch and release it? What would happen if a motor function is called while you are pressing a switch?


<br>

This lab has been adapted from [TI-RSLK MAX Solderless
Maze Edition Curriculum](https://university.ti.com/en/faculty/ti-robotics-system-learning-kit/ti-rslk-max-edition-curriculum)
