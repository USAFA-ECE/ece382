# 🔬 Lab 16 Tachometer

## 📌 Objectives

- Students should be able to configure Timer_A module for input capture measurements.
- Students should be able to develop low-level software drivers to measure distance and speed of the two motors on the robot.

```{note}
Simplicity is the soul of efficiency. – Austin Freeman
```

## 📜 Synopsis

In this lab, we will develop a software module that measures the speeds of motors and the displacement of the robot. The key element for measuring the speed and displacement of our robot is a Hall-effect sensor, a semiconductor device that produces a voltage proportional to the magnetic field. As shown in the figure below, a Hall-effect sensor can measure the magnetic field intensity of two magnets attached to a rotating plate.

```{image} ./figures/Hall_sensor_tach.gif
:width: 400
:align: center
```

Our first objective in this lab is to develop `Timer_A` software capable of measuring periods from two encoders. The counter of `Timer_A`` is 16 bits wide, which means our period measurements will have a precision of 16 bits, allowing us to measure 65,536 different periods.

The term `resolution` here refers to the smallest period that our measurement system can distinguish. When operating in input capture mode, this resolution is determined by the period of the selected clock. For example, if we choose the SMCLK clock running at 12 MHz, with a prescale of 1 and an input divider of 1/8, the period measurement resolution will be 2/3 microseconds. This means that the system can detect periods as small as 2/3 microseconds. With this configuration, the maximum measurable period is determined by the product of precision and resolution, which equals roughly 43 milliseconds (65,536 $\times$ 2/3 $\mu s \approx$ 43 ms).

The second objective of this lab is to utilize the measured periods to determine the wheel speed. Given that there are 360 pulses per rotation, the 43 ms maximum measurement period corresponds to the slowest wheel speed we can accurately measure, which is about 3.81 revolutions per minute (rpm).

We can calculate the angular speed of the wheel, denoted as $\omega$ in rpm, using the following formula:
$$
\omega = \frac{1\text{ pulse}}{2/3\cdot10^{-6}~\text{ sec}} \times\frac{1^\circ}{ n~\text{pulses}} \times \frac{1~\text{rev}}{360^\circ} \times \frac{60\text{ sec}}{1\text{ min}} 
= \frac{250,000}{n} \text{rpm}
$$
where $n$ is the period in units of 2/3 microseconds. This formula allows us to convert the measured periods into angular speed in rpm.

The third objective involves utilizing the second input of the encoder to determine the direction of the motor's rotation. This entails developing software that keeps track of the number of pulses detected on each wheel as the robot advances. As the robot moves forward, these pulses will be added to a counter, and as it moves backward, pulses will be subtracted from the counter. Subsequently, this counter will play a crucial role in determining the distance traveled. 

## 💻 Procedure


### Complete functions in `TA3InputCapture.c`.

- **This is part of Homework 16** 
- Complete `TimerA3Capture_Init()`, `TA3_0_IRQHandler()`, and `TA3_N_IRQHandler()` in the `TA3InputCapture.c` file.
- Carefully follow the provided instructions within `TA3InputCapture.c` to ensure your code is accurate.
- After completing the tasks, submit your Homework 16 by copying and pasting your code into Gradescope.

Before starting this lab, review the solutions available in Gradescope to confirm that you have the correct code.  If the solutions are not published in Gradescope, go through your code with your instructor.

### Demo `Program16_1()`


```{important}
You should understand the code inside the `main()` function in every module. For your final project, you will be required to create your `main.c` file entirely from scratch, as no pre-written code will be provided 
```

- Complete `LCDSpeed()` in the `Lab16_Tachmain.c` file. The LCD should display the left and right tachometer periods and the speeds of both wheels in RPM as shown below.

```{image} ./figures/Lab16_Tachometer.png
:width: 300
:align: center
```
<br>

- Demo `Program16_1()` running at the speeds of 25\%, 50\%, 75\%, 100\%, and then return to 25\% as shown below.  


<center>
<iframe width="560" height="315" src="https://www.youtube.com/embed/-YDAwoAS6Sw" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
</center>
<br>

### Graph duty cycle versus actual speed.

- `Program16_1()` stores the speed of each motor and their corresponding duty cycles in the following buffers. 

```C
uint16_t PeriodBufferR[BUFFER_SIZE];    // to store right period
uint16_t PeriodBufferL[BUFFER_SIZE];    // to store left period
uint16_t SpeedBufferR[BUFFER_SIZE];     // to store right speed
uint16_t SpeedBufferL[BUFFER_SIZE];     // to store left speed
uint16_t DutyBuffer[BUFFER_SIZE];       // to store duty cycles
```

- Upon completion of of `Program16_1`, the LCD will display an option to transmit the data stored in the buffers, as illustrated below.

```{image} ./figures/Lab16_TxBuffer.png
:width: 300
:align: center
```
<br>

- Press the left switch to select "Y" (yes), but do not press any bump switch yet.
- Connect the USB cable to the robot and open a serial terminal, following the steps outlined in [Lab 11](lab11-uart).
- When you're ready to transmit data to the serial terminal, press one of the bump switches. The data will be transmitted as shown below. 

```{image} ./figures/Lab16_SerialTerminal.gif
:width: 700
:align: center
```
<br>

- Once the data transmission is finished, the LCD will indicate `TX is Done`.
- In the serial terminal, select the entire data, excluding the header, which reads ***Receiving buffer data***.
- Copy the selected data and save it as a text file with a .txt extension or as a comma-separated value (CSV) file with a .csv extension.
- Use a software tool of your choice to create plots for the duty cycle, timer periods, and actual speeds in rpm, using data from the `PeriodBuffer`, `SpeedBuffer`, and `DutyBuffer` arrays. Recommended software tools include, but are not limited to, MATLAB, Excel, Python with matplotlib, and Jupyter notebook. You can also utilize the Jupyter notebook in Colab <a target="_blank" rel="noopener noreferrer" href="https://colab.research.google.com/github/USAFA-ECE/ece382/blob/master/docs/Assignments/lab15_calibration.ipynb">![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)</a> used for Lab 15 

<br>

It is crucial to emphasize that data-driven analyses play a vital role in engineering and scientific endeavors. While videos and images may be useful for showcasing, they should not replace essential elements in engineering experiments. Gathering measurements, creating informative plots, and conducting thorough analyses will be integral to your final presentations and reports

```{caution}
Full credit will only be awarded if the figures have **_correct labels and units_**."
```
- Ensure that the first column of the data represents time in **10-millisecond intervals**; "1" signifies 10ms, and "2" signifies 20ms.
- Label your axes with appropriate units, e.g., `time (seconds)`. Convert the time data from 10ms to seconds. You have to use standard units, such as seconds (sec),milliseconds (ms), and microseconds (us). Note that 10ms is not a standard unit of time. Therefore, it is essential to convert the time data into seconds, and you should be familiar with the process of converting from 10ms to seconds.
- Represent the duty cycle in `percentage` rather than `permyriad`.
- Express the speed in rpm, radians per second (RPM), or degrees per second (deg/sec), and include the relevant units in your plots.
<br>




- The component block diagram of this part is shown below. 

```{image} ./figures/Lab16_ComponentBlockDiagram1.png
:width: 760
:align: center
```
<br>

- Submit the following plots in Gradescope.
    - Periods vs. time
    - Speeds vs. time
    - Duty cycle vs. time

### Complete functions for `Program16_2`.

- Read `Tachometer.c` and `Tachometer.h` thoroughly.  
- Read the following functions thoroughly.  You need to understand how the semaphore, `IsControllerEnabled`, is used in multiple functions.

- `average()`
    - You will average a specific number of the latest tachometer readings stored in an array. 
    - The `average()` function will be used to calculate the average over an array.
    - This function is already implemented for you.
    
- Complete the `UpdateParameters()` function in `Program16_2`.
    - Use the two switches to increment the desired speed of each wheel by 10 rpm (sw1 - right and sw2 - left) and roll over if the maximum value is reached. 
    - Use the comments to complete the code inside the `while` loop.
    
- Complete the `Controller()` function in `Program16_2`.
    - Create a basic controller that will increase/decrease the speed of the wheels accordingly to get close to the desired speed.
    - Use the comments to complete the code.
        
- Demo `Program16_2()` that you can increase the desired speed of the wheels with the switches and display the actual and desired speeds and the distance traveled on the LCD. Show that both wheels are running at 50 rpm followed by 100 rpm. Slightly hold a wheel to add friction to the wheel and show that the speed controller maintains the desired speed. You also need to show that the desired speeds roll over if you reach the maximum value.

The component block diagram of this part is shown below.

```{image} ./figures/Lab16_ComponentBlockDiagram2.png
:width: 760
:align: center
```
<br>

<center>
<iframe width="560" height="315" src="https://www.youtube.com/embed/RfeW7urILWM" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
</center>
<br>

### Make your robot drive in a square.

- Complete `Program16_3` to drive the robot in the maze.
- You may need to adjust your drive-straight wheel speed values if one wheel is going faster/slower than the other.
- The robot must start anywhere behind the start line shown in the figure below.
- You are not allowed to intentionally bump into the walls to decide when to turn (cannot do the bump-n-turn algorithm).  
- You have to base your decision to turn on the distance traveled only. 
- Your robot must stop before hitting the wall at the goal.
- Demo `Program16_3` driving the robot in a maze.

```{image} ./figures/Lab14_DriveInMaze.png
:width: 560
:align: center
```
<br>

<center>
<iframe width="560" height="315" src="https://www.youtube.com/embed/9sA1o86J1-w" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
</center>
<br>



## 🚚 Deliverables

### Deliverable 1 
- **[10 Points]** Demo `Program16_1()` running increasing speed from 25\%, 50\%, 75\%, 100\%, and back to 25\%  **Comment your code** and push it to your repository. 

### Deliverable 2 
- **[10 points]** Submit your three plots in Gradescope.

### Deliverable 3 
- **[15 Points]** Demo `Program16_2()` with operational switches and LCD. The actual wheel speeds must reach the desired wheel speeds.  **Comment your code** and push it to your repository.

### Deliverable 4 
- **[15 Points]**  Demo `Program16_3()` driving the robot in the maze. **Comment your code** and push it to your repository.

This lab has been adapted from the TI RSLK: [The Solderless Maze Edition curriculum](https://university.ti.com/en/faculty/ti-robotics-system-learning-kit/ti-rslk-max-edition-curriculum)








