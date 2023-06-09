# 🔬 Lab 6 GPIO

## 📌 Objectives

- Students should be able to write functions, conditionals, loops, and calculations in C.
- Students should be able to write a software interface for GPIO to perform input and output.
- Students should be able to interface a line sensor to the microcontroller and determine the robot’s distance from the line.


:::{note}
Always code as if the guy who ends up maintaining your code will be a violent psychopath who knows where you live.
:::


## 📜 Synopsis

This lab aims to design a sensor system that measures the robot's position relative to a line. The robot has eight sensors (shown below) that detect the line.

```{image} ./figures/Lab06_LineSensor.jpg
:width: 300
:align: center
```

Each sensor is binary, returning one if it sees black and 0 if it sees white. The middle sensors will see the black line if the robot is properly positioned on the line. If the robot is slightly off to the left or right, one or more outer sensors will see the black line. All sensors will see white if the robot is entirely off the line. 

```{image} ./figures/Lab06_RobotOnLine.PNG
:width: 400
:align: center
```


We will use sensor integration to combine the eight binary readings into a single position parameter. We define the `position` of the robot as the distance from the line. The line sensor (QTRX) is about 76.2 mm wide (with about 9.53 mm between sensors), so we should be able to estimate the robot position of -33.4 to +33.4 mm from the center of the line.

The 8-channel line sensor is interfaced to Ports 5, 7, and 9 as shown below. These connections are made on the TI-RSLK chassis board. Sensor 0 (P7.0) is on the right, and sensor 7 (P7.7) is on the left. P5.3 and P9.2 are used to turn on/off the even and odd LEDs, repectively.

```{image} ./figures/Lab06_LineSensorConnection.PNG
:scale: 60%
:align: center
```

:::{important}
You need 6 AA batteries for this lab.
:::



## 🚚 Deliverables

```{warning}
Your code **must be compilable**.  If your code throws any compile errors, you will get **a grade of 0** for the coding part.
```

### Deliverable 1 
- **[15 Points]** Complete the `Reflectance_Read` function. Demo `Program6_2`  showing the values of P7 Input registers.
- **[10 Points]** Push your code to your repository using git. Comment your code.

### Deliverable 2 
- **[15 Points]** Complete the `Reflectance_Position` function. Demo `Program6_2` printing the sensor reading in hex and binary format and the position on the LCD. 
- **[10 Points]** Push your code to your repository using git. Comment your code.


## 💻 Procedure

### Lab Materials
- TI RSLK-MAX
- 6 AA batteries
- Black tape
- 8-Channel QTRX Sensor Array documentation

(lab6-before_you_start)=
### Before you start

There are two switches on your robot as shown below.  Keep the slide switch in the yellow rectangle in the off position and use the button switch in the yellow circle to turn the robot on and off.   

```{image} ../figures/RSLK_Switches.PNG
:width: 760
:align: center
```

### Complete `Reflectance_Init()`.

- **This is part of Homework 6.** 
- `Reflectance.h` and `Reflectance.c` are located under the `inc` folder. 
- Open `Reflectance.h` under the `inc` folder and read it thoroughly.
- Open `Reflectance.c` under the `Lab06_GPIO` project or the `inc` folder and read it.
- Don't worry about `Reflectance_Start` and `Reflectance_End` this time. They are for Lab 10.

:::{tip}
Do you know you can open a header file by right-clicking the `#include` statement in CCS and selecting `Open Declaration` or F3?
:::

- Follow the instructions inside `Reflectance_Init` to complete it.
- Copy the code and paste it in Gradescope to submit your Homework 6.
- Your code **must be compilable**. 


### Check the solution to `Reflectance_Init`

- Before starting this lab, check the solution to `Reflectance_Init` posted in Gradescope to ensure you have the correct code. If the solution is not published in Gradescope, go through your code with the instructor.

:::{important}
Your incorrect answers could be marked as correct by mistake. You are lucky, but it is your responsibility to check the solutions.  Labs are cumulative - if today's code does not work, you will not be able to complete later labs. You wouldn't be happy to spend hours debugging your code to find an error in the code you wrote weeks ago. 
:::


### Complete `Reflectance_Read()`.

- In this section, you need to thoroughly understand the line sensor, which we covered in the lecture. Talk to the instructors if you have any questions.
- In this function, we first fully charge the capacitors connected to P7 (P7.0-P7.7) for 10 $\mu$s. During this time, the P7 ports must be outputs. Read `Clock.h` in the `inc` folder for delay functions.
- Then, we set the P7 ports to inputs to read the capacitor values (high or low). 
- We let the capacitors get discharged. The discharge rates depend on the reflected light on the light-sensitive transistors.  
- The white reflective surface has more light on the transistor and conducts more current. This current discharges the capacitors.
- Notice that the white surface discharges faster, and the voltage falls more quickly. So as soon as the capacitors start discharging, we wait ~1 ms and then look at the values of P7. 
- We wait ~1 ms because, as we saw with the two oscilloscope readings (over a white surface and a black surface), the white surface would result in a 0, and the black surface would result in a 1 at 1 ms.
- But we don't want to hard-code 1 ms in `Reflectance_Read()`.  Instead, we let users choose their values by passing them through the function argument - `wait_us`.  Depending on the light condition, you may need a different value, and you don't want to modify this function every time you need a different value.  You write once and use it forever.  
- Now, follow the instructions inside `Reflectance_Read` to complete it.
- Ensure you update the `ReflectanceResult` variable every time the `Reflectance_Read` function is executed.
- Turn on the chassis power by pressing the push button discussed [here](lab6-before_you_start).
- Run `Program6_2` to test your code.  Look at the hex and binary values on the LCD.
- Use the `Registers` tab to watch the P7 registers.  Demo P7IN showing sensor values as you move the robot on the black line as shown below.
- Ensure you click `Continuous Refresh`.

```{image} ./figures/Lab06_P7IN_values.gif
:scale: 85%
:align: center
```

:::{warning}
If you want to give a video demo, use a screen recorder such as OBS Studio - it is free. Do not record your screen with a mobile device or a webcam. If you do so, **you will get -50 points**.
:::


```{admonition} Q&A
Q: I am confused by the charge capacitor section. I realize that the discharge rate determines whether the surface is black or white, but I don't know how to translate this to C code.
<br>
A: P7 is connected to the capacitors. You have to set P7 to output and output 1 to charge the capacitors, then change it to input to read the charge in the capacitors after a few microseconds.
<br>
Q: How do you "read" from P7.0? Are there any specific steps that the question is looking for?
<br>
A: I believe you can do P7->IN to read from all pins.
<br>
Q: What should Reflectance_Read return when it's in the middle, mine is returning 0, which causes the position to think it's off to the left or right?
<br>
A: If your line is too small and you're in between the middle sensors, it will return 0. If the two middle ones see the line, you should get 0x18 i think.
<br>
A: Yeah, it's just too small. Thank you

```



### Complete `Reflectance_Position`

- In this section, you will develop an algorithm to determine the robot’s position relative to a line.
- Using the flowchart shown below, write the algorithm for `Reflectance_Position()` in `Reflectance.c`.

```{image} ./figures/Lab06_Flowchart.PNG
:scale: 25%
:align: center
```

- Before reading the instructions below, sit back and try to get your algorithm to find the robot’s distance from the black line using the line sensor. The sensor returns 8-bit data from `Reflectance_Read`.
- If you come up with a better algorithm (not necessarily faster but easier to understand and implement), you will get bonus points.  Talk to your instructor before implementing it.
- The `const int32_t Weight[8]` array represents each sensor's distance from the center in the unit of 0.1 mm (to avoid floating point numbers). 
- The `const int32_t Mask[8]` array can be used to **select** a specific sensor's data (this is known as masking). Note that each hexadecimal value only sets one bit at a time, i.e., each hexadecimal value corresponds to one sensor.
- Suppose the robot is off to the left and the sensor reading (the function argument, `data`, is used for this) is 0b01100000 or 0x70.  
    - The function should return $(-238 + -142)/2 = -190$, which means the robot is 19.0 mm off to the left.   
    - You will use a for-loop to check if `data` contains the binary bit in the `Mask`. For example, the first `Mask` value is 0x01, and  the first binary bit of `data` (0x70) is 0, and we can tell the black line is _not_ under the sensor associated with the bit. How would you write this in C? - bit masking. 
    - We keep going until the `Mask` value is 0x20 or 0b00100000 - the sixth bit is 1.  The sixth bit of `data` is 1, and we can tell the black line is under the sixth sensor. In the same way, we can tell the black line is also under the seventh sensor. 
    - Now, you need to find the average value of the two weights located at the sixth and seventh of the `Weight[]` array, which results in $(-142 + -238)/2 = -190$. 

```{admonition} Q&A
Q: What purpose does the Mask[i] variable have in the if statement for when the bot is on the line? I understand we have to take into account each individual line sensor, but I don't quite understand how
<br>
A: Mask compares the total value in data to the individual bits. If the bit is on, the corresponding line sensor reads the line.
```

- If the robot is far away from the line, the sensor data will be 0x00. In this case, the function should decide whether the robot is on the right or left side of the line. The function must return $33.4~mm + Offset (5~mm)$ if it is on the right side and $-33.4 mm - Offset$ otherwise.
- Use the `prevSign` variable to keep track of which side the robot was. If it was off to the right when the `Reflectance_Positon` was called last time, `prevSign` should be 1. Otherwise, it should be -1.  
- The `prevSign` is a **static** variable, which preserves the value after the function ends.

:::{important}
The **static** declaration can be applied to internal variables (variables inside a function).  Internal **static** variables are local to a particular function just as automatic variables are, but unlike automatics, they remain in existence rather than coming and going each time the function is called.  This means that internal **static** variables provide private, permanent storage within a single function. Ref: Kernighan & Ritchie, The C Programming Language, 2nd ed, Addison-Wesley.
:::

- When the robot is completely off the line, `data` will be 0. In this case, you need to return either the leftmost $(-334) - Offset (50) = -384$ or the rightmost $(334) + Offset(50) = 384$.

:::{warning}
Do not use hard-coded numbers in your code - you will lose points! For example, do not write `return 384`.  Otherwise, you need to find and replace every instance of the hard-coded numbers if we later decide to change the offset to a different value, say 70.    
:::

- Use `Program6_2` to test your program.  It should display the robot’s position from the black line as shown below.

```{image} ./figures/Lab06_LCd.gif
:scale: 100 %
:align: center
```
<br>

<video src="https://youtu.be/mUW5b_e-ess"></video>

This lab has been adapted from [TI-RSLK MAX Solderless
Maze Edition Curriculum](https://university.ti.com/en/faculty/ti-robotics-system-learning-kit/ti-rslk-max-edition-curriculum)
