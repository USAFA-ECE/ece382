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

Our first objective in this lab is to develop `Timer_A` software capable of measuring periods from two encoders. The counter of `Timer_A` is 16 bits wide, which means our period measurements will have a precision of 16 bits, allowing us to measure 65,536 different periods.

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

### Setup
1. If you haven't already, download `TimerA2.c` from Teams > General > Files > Class Materials > SourceFiles. Move it to the `inc` folder to replace the existing file with the same name.
1. Download `Lab16_Tachmain.c` from the same location. Move it to the `Lab16_Tach` folder to replace the existing file.
1. Download `TA3InputCapture.[c,h]` and `Tachometer.[c,h]` from the same location. Move them to the `inc` folder to replace the existing file.

### Complete functions in `TA3InputCapture.c`.

- **This is part of Homework 16** 
- Complete `TimerA3Capture_Init()`, `TA3_0_IRQHandler()`, and `TA3_N_IRQHandler()` in the `TA3InputCapture.c` file.
- Carefully follow the provided instructions within `TA3InputCapture.c` to ensure your code is accurate.
- After completing the tasks, submit your Homework 16 by copying and pasting your code into Gradescope.

Before starting this lab, review the solutions available on Gradescope to confirm that you have the correct code.  If the solutions are not published on Gradescope, go through your code with your instructor.

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

1. `Program16_1()` stores the speed of each motor and their corresponding duty cycles in the following buffers. 

    ```C
    uint16_t SpeedBufferL[BUFFER_SIZE];     // Buffer to store left motor speed data
    uint16_t SpeedBufferR[BUFFER_SIZE];     // Buffer to store right motor speed data
    uint16_t DistanceBufferL[BUFFER_SIZE];  // Buffer to store left motor distance data
    uint16_t DistanceBufferR[BUFFER_SIZE];  // Buffer to store right motor distance data
    uint16_t DutyBuffer[BUFFER_SIZE];       // Buffer to store duty cycle values
    ```

1. Upon completion of of `Program16_1`, the LCD will display an option to transmit the data stored in the buffers, as illustrated below.

    ```{image} ./figures/Lab16_TxBuffer.png
    :width: 300
    :align: center
    ```
    <br>

1. Press the left switch to select "Y" (yes), but do not press any bump switch yet.
1. Connect the USB cable to the robot and open a serial terminal, following the steps outlined in [Lab 11](lab11-uart).
1. When you're ready to transmit data to the serial terminal, press one of the bump switches. The data will be transmitted as shown below. 

    ```{image} ./figures/Lab16_SerialTerminal.gif
    :width: 700
    :align: center
    ```
    <br>

1. Once the data transmission is finished, the LCD will indicate `TX is Done`.
1. In the serial terminal, select the entire data, excluding the header, which reads ***Receiving buffer data***.
1. Copy the selected data and save it as a text file with a .txt extension or as a comma-separated value (CSV) file with a .csv extension.
1. Use a software tool of your choice to create plots for the duty cycle in percent, timer periods, and actual speeds in rpm, using data from the `PeriodBuffer`, `SpeedBuffer`, and `DutyBuffer` arrays. Recommended software tools include, but are not limited to, MATLAB, Excel, Python with matplotlib, and Jupyter notebook. You can also utilize the Jupyter notebook in Colab <a target="_blank" rel="noopener noreferrer" href="https://colab.research.google.com/github/USAFA-ECE/ece382/blob/master/docs/Assignments/lab15_calibration.ipynb">![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)</a> used for Lab 15 

<br>

It is crucial to emphasize that data-driven analyses are fundamental in engineering and scientific endeavors. While videos and images can be helpful for showcasing work, they should never replace the core elements of engineering experiments. Collecting accurate measurements, creating informative plots, and conducting comprehensive analyses will be essential to your final presentations and reports.

```{caution}
**_Full credit will only be awarded if the figures include correct labels and units_**. You must use standard units. Units such as permille or 10 ms are not considered standard, whereas RPM, percent, and milliseconds are acceptable.
```

1. Ensure that the first column of the data represents time in **10-millisecond intervals**; "1" signifies 10ms, and "2" signifies 20ms.
1. Label your axes with appropriate units, e.g., `time (seconds)`. Convert the time data from 10ms to seconds or milliseconds. You have to use standard units, such as seconds (sec), milliseconds (ms), and microseconds (us). Note that 10ms is not a standard unit of time. Therefore, it is essential to convert the time data into seconds, and you should be familiar with the process of converting from 10ms to seconds.
1. Represent the duty cycle in `percentage` rather than `permille`.
1. Express the speed in rpm, radians per second (RPM) and include the relevant units in your plots.
1. Examples of incorrectly labeled plots include, but are not limited to, the following.

    ```{image} ./figures/Lab16_plot_duty_incorrect.png
    :width: 360
    :align: center
    ```
    <br>

1. The y-axis in this plot is incorrect; it lacks units, and the correct standard unit for the duty cycle is percent.

    ```{image} ./figures/Lab16_plot_time_incorrect.png
    :width: 360
    :align: center
    ```
    <br>
1. The x-axis in this plot is incorrect; it should span from 0 to 5 seconds. Additionally, the entire transition cannot be completed within just 50 milliseconds. This suggests that the plot was not carefully reviewed before submission, which is unacceptable in the field of engineering. 
1. Submit the following plots on Gradescope.
    - Periods vs. time
    - Speeds vs. time
    - Duty cycle vs. time


### Finite State Machine

```{note}
This part is the foundation of Deliverable 1 of the final project.
```

1. Complete `Controller3` to implement the finite state machine as shown below.

    ```{image} ./figures/Lab16_FSM.png
    :width: 380
    :align: center
    ```
    <br>
    
    - FRWD_DIST = 700 mm.
    - BKWD_DIST = 90 mm.
    - TR_DIST refers to the displacement for a 30$^\circ$, 60$^\circ$, or 90$^\circ$ turn depending on the bump switch triggered. 

1. Start in the **Forward** state. 
1. If **Bump3** or **Bump4** is triggered, move backward 90 mm, make a 90$^\circ$ left turn, and then move forward. 
1. If **Bump1** is triggered, make a 30° left turn, then move forward. 
1. If **Bump2** is triggered, make a 60° left turn, then move forward. 
1. If **Bump5** is triggered, make a 60° right turn, then move forward. 
1. If **Bump6** is triggered, make a 30° right turn, then move forward. 
1. If no bump switch is triggered while in the **Forward** state, move forward for 700 mm and then stop. 
1. Ignore any bump triggers in states other than **Forward**, 
1. Demo `Program16_3` to showcase the finite state machine by running your robot on the floor. 

<br>
<center>
<iframe width="560" height="315" src="https://www.youtube.com/embed/9AU9-E0leFA?si=xeDwPTWQhSXg2xSZ" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>
</center>
<br>

## 🚚 Deliverables

### Deliverable 1 
- **[9 Points]** Demo `Program16_1()` running at the speeds of 25\%, 50\%, 75\%, 100\%, and then return to 25\%.   

### Deliverable 2 
- **[8 points]** Use a software tool of your choice to create plots for the `duty cycle in percent`, `timer periods in microsecond`, and `actual speeds in rpm`. Submit your three plots in Gradescope.

### Deliverable 3 
- **[9 Points]** Demo `Program16_3()` to showcase the finite state machine by running your robot on the floor. 

### Deliverable 4 
- **[8.5 Points]** Ensure that you provide comments in your code for clarity and push your code to your repository using git.

This lab was originally adapted from the [TI-RSLK MAX Solderless Maze Edition Curriculum](https://university.ti.com/en/faculty/ti-robotics-system-learning-kit/ti-rslk-max-edition-curriculum) and has since been significantly modified.




