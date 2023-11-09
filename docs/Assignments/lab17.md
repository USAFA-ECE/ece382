# 🔬 Lab 17 Control Systems

## 📌 Objectives

- Students should be able to integrate input capture measurements from Timer A3 and PWM outputs using Timer A0.
- Students should be able to design a speed control system for the two motors.
- Students should be able to assess the effectiveness of the control system's performance.
- Students should be able to implement proportional-integral (PI) controllers that control a wall-following robot.

```{note}
If debugging is the process of removing software bugs, then programming must be the process of putting them in. -Edsger Dijkstra
```

## 📜 Synopsis

Equipped with the knowledge gained from previous labs, we now have the tools to both drive the robot and gather data on various parameters, including the robot's speed, distance traveled, and the distance to nearby walls. In this lab, our focus will be on implementing automatic controllers that enable the robot to perform specific tasks, such as driving between walls. These control methods belong to the category known as **proportional-integral-derivative (PID) control** systems.

If the controller does not utilize feedback from the system's output to compute the control action, it is referred to as an open-loop control system. Conversely, when the system's output is measured and used as feedback in the controller's computation, it is termed a closed-loop or feedback control system. The diagram below illustrates both open-loop control (top) and closed-loop control (bottom) systems.

```{image} ./figures/Lab17_Feedback_Control.png
:width: 500
:align: center
```
<br>

```{note}
Control systems are a rich and complex field within engineering spanning: electrical engineering, robotics engineering, aerospace engineering, mechanical engineering, and computer engineering. This module provides a brief introduction.
```

## 💻 Procedure

### Implement a proportional-integral controller in `Program17_1`

- **This is part of Homework 17** 
- Read the entire code in `Program17_1.c`. 
- Complete the `Controller()` function in `Program17_1.c` according to the provided instructions.
- For this homework, implement a proportional-integral controller to drive both motors at the desired speed. 
- Copy and paste your `Controller()` code into Gradescope for Homework 17 submission.

### Demo `Program17_1`

- Experimentally determine the best $K_p$ and $K_i$ in order to establish a stable system.
- You can change $K_p$ and $K_i$ at line 80 in the `Program17_1` file.  You can also fine-tune these values in real-time using the bump switches.
- Record the $K_p$ and $K_i$ values and report them in the following section.
- Demonstrate the functionality of `Program17_1()` which showcases a proportional-integral controller responsible for regulating wheel speeds at 50 rpm and 100 rpm in the presence of external disturbance. Make sure to apply the disturbance only after the desired speed has settled.


<center>
<iframe width="560" height="315" src="https://www.youtube.com/embed/OveTuwviGjA" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
</center>
<br>


### Discuss the performance of your controller in `Program17_1()`

- Gather data on wheel speed at 100 rpm through the USB/UART port and evaluate the performance of your controller in `Program17_1()`.
- You may find it necessary to adjust the Serial Terminal buffer size, which defaults to 1000 lines. If you require more than 1000 lines, refer to the instructions {ref}`here <FAQ:CCS:TerminalBuffer>`.
- Provide the step response of the speed controller with an external disturbance, ensuring that disturbance is applied **after the speed has settled at the desired speed**.
- Provide the input signals (PWM duty cycles) applied to the robot. 
- Feel free to use any software of your choice, such as MATLAB or MS Excel, for generating figures. Ensure that your plots include labels, units, and legends as needed. Make sure to focus on the region of interest and utilize axis limit functions like `plt.ylim` or `plt.xlim`.
- Discuss the controller's performance, taking into consideration factors like response time, overshoot, and steady-state error.  Report the $K_p$ and $K_i$ values you used for these plots.
- Execute `Program16_2()` and after the speed is settled, gently holding one of the wheels to introduce a disturbance. Then, run `Program17_1()` with the PI controller you have implemented and repeat the same procedure. Determine which one performs better and provide an explanation for your findings.

```{warning} 
Take this part very seriously. The critical component of your final report will involve analyzing the system's performance. Scientific analysis predominantly relies on the data gathered, frequently presented through figures.
```

**The points allocated to this analysis section are twice as many as those assigned to the demonstration.**  If you have any doubts about the plots you've created, don't hesitate to seek guidance from one of the instructors.

- Examples of plots that may not receive full credit include, but are not limited to, the following.

```{image} ./figures/Lab17_BadPlotExample1.png
:width: 420
:align: center
```
<br>

- This plot has no step response.  The initial motor speeds must be 0. 

```{image} ./figures/Lab17_BadPlotExample2.png
:width: 420
:align: center
```
<br>

- The x-axis is off by at least an order of magnitude. The whees cannot react at the speed shown in the plot. 

```{image} ./figures/Lab17_BadPlotExample3.png
:width: 420
:align: center
```
<br>

- The x-axis unit should be in seconds (s) or milliseconds (ms), not 10 ms or 20 ms. The standard time units in engineering are s, ms, us, ns, and so on.

### Implement a Wall-Following controller in `Program17_3`


- Go to ECE382 Teams > General > Files > Class Materials > SourceFiles.
- Download `TimerA2.c` into your workspace/inc folder using Windows File Explorer (not Code Composer Studio). You should overwrite the existing `TimerA2.c`. **Caution**: It must be in the `inc` folder not in the `Lab17_Control` folder.
- Complete the `Controller()` function in Program17_3.c.
- Develop a proportional controller that guides the robot between two walls using IR sensors.
- Empirically determine the optimal proportional constant and swing values to establish a stable system.
- Determine three distinct $K_p$ values: one for a slow response, another for a fast response with a slight overshoot (or no overshoot), and the third for a very fast response with significant overshoot.
- Document the $K_p$ values and provide them in the subsequent section.
- Demo `Program17_3` using the three different $K_p$ values. **Important Note**: Ensure that your robot is initially positioned at least 3 inches (or 7.5 cm) away from the center line.


<center>
<iframe width="560" height="315" src="https://www.youtube.com/embed/7S8QNVCjVyo" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
</center>
<br>

### Evaluate controller performance in `Program17_3()`

- Collect data via the USB/UART port to assess the controller's performance.
- Provide the $K_p$ values recorded in the previous section.
- Provide the best step response from Deliverable 3, along with the input signal (PWM duty cycles) applied to the robot. Report the corresponding $K_p$ value. 
- Utilize software of your choice, such as MATLAB or MS Excel, for generating figures. Ensure that your plots feature labels, units, and legends when necessary. - Assess the controller's performance, considering factors like response time, overshoot, and steady-state error.

## 🚚 Deliverables

### Deliverable 1 **[4 Points]**  
- Demonstrate the functionality of `Program17_1()` which showcases a proportional-integral controller responsible for regulating wheel speeds at 50 rpm and 100 rpm in the presence of external disturbance. Make sure to apply the disturbance only after the desired speed has settled.


### Deliverable 2  **[8 Points]**  
- Provide a step response of the speed controller with disturbance, which is applied after the speed settles at the desired speed. Your plots must include labels, units, and legends whenever required. 
- Discuss the controller's performance, taking into consideration factors like response time, overshoot, and steady-state error.  Report the $K_p$ and $K_i$ values you used for these plots.
- Execute `Program16_2()` and after the speed is settled, gently holding one of the wheels to introduce a disturbance. Then, run `Program17_1()` with the PI controller you have implemented and repeat the same procedure. Determine which one performs better and provide an explanation for your findings.

### Deliverable 3 **[4 Points]**  
- Demo `Program17_3` using the three different $K_p$ values. **Important Note**: Ensure that your robot is initially positioned at least 3 inches (or 7.5 cm) away from the center line. Your demo must include three step responses - one for a slow response, another for a fast response with a slight overshoot (or no overshoot), and the third for a very fast response with significant overshoot. Report their $K_p$ values. 

### Deliverable 4 **[8 Points]**  
- Collect data via the USB/UART port to assess the controller's performance. Provide the $K_p$ values recorded in the previous section. Provide the best step response from Deliverable 3, along with the input signal (PWM duty cycles) applied to the robot. Report the corresponding $K_p$ value. Assess the controller's performance, considering factors like response time, overshoot, and steady-state error.

### Deliverable 5 
- **[5.5 Points]** Ensure that you provide comments in your code for clarity and push your code to your repository using git.