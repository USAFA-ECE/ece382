
# 🔬 Lab 7 FSM

## 📌 Objectives

- Students should be able to implement FSMs using a switch statement.
- Students should be able to implement a simple line-following algorithm with an FSM.



## 📜 Synopsis

This lab aims to develop and test a Finite State Machine (FSM) that could be used in a robot to follow a line. The robot shown below has **two** sensors that detect the line. 


```{image} ./figures/Lab07_TwoLineSensorRobot.png
:width: 640
:align: center
```

If the robot is correctly positioned on the line, both sensors will read 1. If the robot is a little off to the left or right, one sensor reads 1, and the other reads 0. If the robot is entirely off the line, both sensors will read 0. The robot has two motors. The two motors in the back and a passive caster in the front allow the robot to operate in a differential drive fashion. The robot moves forward in a straight line if the software outputs high to both motors. If the software outputs high to just one motor, it will turn. If the software outputs low to both motors, it will stop.

Shown below are the state transition diagram and state transition table for the line following robot. 

```{image} ./figures/Lab07_FSM.png
:width: 640
:align: center
```

## 💻 Procedure

### Lab Materials
- TI RSLK-MAX
- 6 AA batteries
- Black tape
- 8-Channel QTRX Sensor Array documentation

### Complete `Reflectance_Center()`.

- **This is part of Homework 7.** 
- Open `Reflectance.h` under the `inc` folder and read the documentation for `Reflectance_Center` thoroughly.
- Open `Reflectance.c` under the `Lab07_FSM` project or the `inc` folder read `Reflectance_Center`.
- Follow the instructions inside `Reflectance_Center` to complete it.
- Copy the code and paste it in Gradescope to submit your Homework 7.
- Your code **must be compilable**. 

:::{tip}
Do not write the same code you already have in `Reflectance_Read`.  Instead, call the `Reflectance_Read` function inside `Reflectance_Center`.  Do not reinvent the wheel. 
:::
<br>

Before starting the next, check the solution posted in Gradescope to ensure you have the correct code. If the solution is not published in Gradescope, go through your code with your instructor.

### Implement the finite state machine.

- Read `Motor.h` and `Bump.h`.
- Read the entire `Lab07_FSMMain.c`.
- Read the entire `Lab07_FSMMain.c` from the beginning to the end.
- **Read the entire `Lab07_FSMMain.c` thoroughly before writing any code**

:::{tip}
Do you know you can open the function declaration in the source file by right-clicking a function followed by selecting `Open Declaration` or F3?
:::


Implement the state transition algorithm in the `main()` function.

- From the Center state:
    - If the robot is on the line, it will stay straight for 50 milliseconds.
    - If the robot is off to the left, it will enter state Left and turn right for 50 milliseconds.
    - If the robot is off to the right or entirely off the line, it will enter Right and turn left for 50 milliseconds.

- From the Left state:
    - If the robot has gone too far left and is off the line entirely, it will stay Left and continue to turn right for 50 milliseconds.
    - If the robot overcorrects and ends up off to the right, it should enter the Right state and turn left for 50 milliseconds.
    - If the robot is on the line or still off to the left, it will enter the Center state and go straight for 50 milliseconds. 

- From the Right state:
    - If the robot is entirely off the line, it will stay in the Right state and continue to turn left for 50 milliseconds.
    - If the robot overcorrects and ends up off to the left, it will enter the Left state and turn right for 50 milliseconds.
    - If the robot is on the line or still off to the right, it will enter the Center state and go straight for 50 milliseconds. 


Implement the output of the state machine.

- Read `Motor.h`.
- The motor functions take duty cycles as function arguments. 
- In general, duty cycles are expressed as a percentage of `ON` time, which is a real number between 0.0% and 100.0%.  
- To avoid floating point numbers and their computations in microcontrollers, we will use a 16-bit integer between 0 and 10,000 to present a duty cycle. We will use a rarely used unit, _permyriad_, which means one out of every ten thousand (myriad); one percent of one percent. 

:::{tip}
Permyriad (literally, per 10,000) represents a portion of a whole in parts per 10,000, just as permille represents parts per 1,000 and percent parts per 100.
:::

- The robot moves forward in a straight line if the software outputs high to both motors (11 in the state transition diagram). 
- The robot makes a left turn if the software outputs low to the left motor and high to the left motor (01 in the state transition diagram).
- The robot makes a right turn if the software outputs high to the left motor and low to the left motor (10 in the state transition diagram).

:::{hint}
For this lab, run both wheels at the same speed to move forward. To make a turn, run one wheel and stop the other one. Do not run any wheels backward.
:::

For this demo, set the delay to 500ms or greater and set the motor speed (`MOTOR_SPEED`) to 0.  A live demo is preferred, but a video demo is also acceptable. For a video demo, **add your voice** to clearly state six transitions between three states.  

<br>
<center> 
<iframe width="560" height="315" src="https://www.youtube.com/embed/_BXfcoIDvag" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
</center>
<br>



### Implement a line-following algorithm

- Set the delay to 50ms.
- Set the motor speed (`MOTOR_SPEED`) to about 2500. You may need a slower speed.  
- Demo your robot following the line shown below.

<br>

```{image} ./figures/Lab07_BlackLineOnWhiteBackground.png
:width: 360
:align: center
```

<br>

## 🚚 Deliverables

```{warning}
Your code **must be compilable**.  If your code throws any compile errors, you will get **a grade of 0** for the coding part.
```

### Deliverable 1 
- **[30 Points]** Push your code to your repository using git.  Comment your code.

### Deliverable 2 
- **[20 Points]** Demo `Lab07_FSMmain.c`. For this demo, set the delay to 500ms or greater and the motor speed to 0.  A live demo is preferred, but a video demo is also acceptable. For a video demo, add your voice to clearly state six transitions between three states.  

- **[5 Bonus Points]** Demo your robot following the line.


This lab has been adapted from [TI-RSLK MAX Solderless
Maze Edition Curriculum](https://university.ti.com/en/faculty/ti-robotics-system-learning-kit/ti-rslk-max-edition-curriculum)
