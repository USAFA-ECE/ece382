# 🔬 Lab 15 ADC

## 📌 Objectives

- Students should be able to implement the ADC module to input analog data into the microcontroller.
- Students should be able to use periodic interrupts to sample the ADC at a regular rate.
- Students should be able to develop a function in C that converts raw ADC samples to distances.
- Students should be able to evaluate the performance of the data acquisition system.
- Students should be able to use an automated test approach called black-box functional testing to verify that their algorithm is operating correctly.
- Students should be able to classify the states of the robot using the distance measurements from three sensors.

:::{note}
Before software can be reusable, it first has to be usable. 
:::


## 📜 Synopsis

In this lab, we will convert raw ADC samples into the distance from the analog domain to the digital domain. Three convert functions will be used to take input from the 3 IR distance sensors and convert it to a distance in millimeters. The range of the IR distance sensor is about 70 to 800 mm, and the resolution is about 1 mm at 200 mm.

The figure below shows the relationship between the output voltage of the IR sensor and the distance to the wall. Notice the non-monotonic behavior of the sensor. For example, if the system records a sensor value of 2 V, it could mean 30 mm or 120 mm. During the robot challenge, you will endeavor to keep the robot away from the wall, so you will assume the sensor distance is greater than 70 mm. Due to the nature of the robot challenges, we are not interested in distances beyond 800 mm.
 
```{image} ./figures/Lab15_SensorResponse.png
:width: 560
:align: center
```
<br>

We will develop a function that converts raw ADC samples into distance measured from the wall. Let $n$ be a 14-bit sample from the ADC (0 to 16383), and $d$ be the distance in mm from the sensor to the wall. Then the distance in mm from the sensor to the wall is given by

\begin{equation*}
d = \frac{m}{n - c}
\end{equation*}

where $m$ and $c$ are calibration coefficients to be empirically determined. Let $L$ be the distance from the center of the robot to the wall, which is given by

\begin{equation*}
L = d + r = \frac{m}{n - c} + r
\end{equation*}

where $r$ is the distance from the center of the robot to the distance sensor. You must measure $r$ for each sensor.

The second task is to classify the situation into one of many possible scenarios using three distance values. The robot has three distance sensors: left, center, and right. Each sensor measures the distance from the center of the robot to the wall in millimeters. The center of the robot is the single reference point, and the three distances are measured from that common reference point. The four possible scenarios as the robot approaches a decision point are shown below. The variables left, center, and right are defined as the distance from the center of the robot to the wall.


```{image} ./figures/Lab15_FirstCases.png
:width: 560
:align: center
```

<br>
Four more scenarios are shown below as the robot approaches a decision point.


```{image} ./figures/Lab15_SecondCases.png
:width: 560
:align: center
```
<br>


## 💻 Procedure

### Setup

- Go to ECE382 Teams > General > Files > Class Materials > SourceFiles.
- Download `Lab15_ADCmain.c` from the Teams folder into your `/workspace/Lab15_ADC` folder using Windows File Explorer (not Code Composer Studio). **Caution**: It must be in the `Lab15_ADC` folder, not in the `inc` folder.
- Download `IRDistance.c` and `IRDistance.h` from the Teams folder into your `/workspace/inc` folder using the Windows File Explorer (not Code Composer Studio). **Caution**: It must be in the `inc` folder, not your project folder.

### Complete functions in `ADC14.c`

- **This is part of Homework 15** 
- Complete `ADC0_InitSWTriggerCh17_14_16()` and `ADC_In17_14_16()` in `ADC14.c`.
- Follow the instructions inside `ADC14.c` to complete it.
- Copy and paste the code in Gradescope to submit your Homework 15.

Before starting this lab, check the solutions posted in Gradescope to ensure you have the correct code.  If the solution is not published in Gradescope, go through your code with your instructor.

- The component block diagram of this lab is shown below. 

```{image} ./figures/Lab15_CBD.png
:width: 760
:align: center
```
<br>

### Demo `Program15_1()`

:::{important}
You should understand the main code provided in every module. No such code will be provided for your final project, and you must write your main.c from scratch.  
:::

- Complete `LCDOut1()` in `Lab15_ADCmain.c`. 
- Demo `Program15_1()` running with raw digital sensor values as shown below. 
- Ignore the distance values on the LCD for this part. 
- **Note**: The chassis board powers the distance sensors. 

```{image} ./figures/Lab15_ADC.png
:width: 260
:align: center
```
<br>

<center>
<iframe width="560" height="315" src="https://www.youtube.com/embed/oI-ZincragE" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
</center>
<br>


### Calibrate your sensors

For each IR sensor, you will empirically determine the two calibration constants ($m$ and $c$). Each sensor will have its own $m$ and $c$ constants. To determine these values, go to [Calibration](./lab15_calibration.ipynb).


### Complete functions in `IRDistance.c`

- Complete `LeftConvert()`, `RightConvert()`, `CenterConvert()` in `IRDistance.c`.
- You found the calibration coefficients $n$ and $c$ in the previous section.
- As discussed in the Synopsis section, the functions must return the actual distance, $L$, from **the center of the robot to the wall**.
- The maximum measurement distance for the sensor is 800 mm, so if the ADC value is less than the ADC value for 800 mm (2244 in the example on the calibration(./lab15_calibration.ipynb) page), your function should return $800$.
- All the values you enter in CCS should be integers.
- ADCMAX_xxxx is the ADC value at 800mm.
- Do not use hard-coded numbers. Use enum and #define. 
- Demo `Program15_1` running with accurate distance values on the LCD. Use the distances of 150, 200, 250, and 300 mm for your demo.  The error should be less than 50 mm for the demo.  Use one of the paper rulers (donated by IKEA) in the lab for the demo. A live demo is preferred, but a video demo is also acceptable. For a video demo, **add your voice to clearly state the distances you are testing**.


<center>
<iframe width="560" height="315" src="https://www.youtube.com/embed/BFKF1ZMtGAc" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
</center>
<br>



### Write and test the `Classify()` function.

- Open `Classifier.h` under the `inc` folder. 
- Add the following state representations to the enumerated type:
```C
ClassificationError = 0
LeftTooClose = 1
RightTooClose = 2
CenterTooClose = 4
Straight = 8
LeftTurn = 9
RightTurn = 10
TeeJoint = 11
LeftJoint = 12
RightJoint = 13
CrossRoad = 14
Blocked = 15
```
<br>

:::{note}
We put the enumerated types in the header file so that any code files that include the .h file can access these constants.
:::

- Open `Classifier.c` in the project folder.
- We will use `#define` constants to specify the bounds to make it easier to understand the classification algorithm.  You will adjust these values during the final project to match the actual distances within the maze.
```C
#define SIDEMIN    212   // smallest side distance to the wall in mm
#define SIDEMAX    354   // largest side distance to wall in mm
#define CENTERMIN  150   // min distance to the wall in the front
#define CENTEROPEN 600   // distance to the wall between open/blocked
#define IRMIN      50    // min possible reading of IR sensor
#define IRMAX      800   // max possible reading of IR sensor
```
<br>

- Create the following classification algorithm that will return the following:
    - ClassificationError (0) if any of the sensors are less than 50 mm or greater than 800 mm.
    - LeftTooClose (1) error if the left sensor is less than  212 mm.
    - RightTooClose (2) error if the right sensor is less than  212 mm.
    - CenterTooClose (4) error if the center sensor is less than 150 mm.
    - With a combination of the last three conditions, there will be possible to have multiple simultaneous danger conditions. For example, 5 will signify both CenterTooClose and LeftTooClose. 7 will mean all three directions are too close.
    - The rest (Straight (8) - Blocked (15)) depend on the state of the sensors. 

First, consider the center sensor. As the robot approaches the intersection, the center sensor will be used to classify the difference between the set of {Blocked, Right Turn, Left Turn, and Tee Joint} where the center sensor is less than 600 mm and the set of {Straight, Right Joint, Left Joint, and Cross Road} where the center sensor greater than or equal to 600 mm. Because there could be a long straight road, there is no maximum acceptable value for the sensors.

We use numbers derived from a road that is 400 mm wide and side sensors that are placed at 45 degrees. If the robot is in the middle of the road with the straight or blocked scenarios, both side sensors placed at 45 degrees will read 283 mm. If the robot is within 50 mm from the center of the road with the straight or blocked scenarios, the side sensors could range from 212 (SIDEMIN) to 354 mm (SIDEMAX). The 354 (SIDEMAX) threshold will be used to classify whether or not it is possible to turn left or right at the next intersection. Less than 354 (SIDEMAX) means the turn path is not possible; 354 (SIDEMAX) or above means the turn path is possible. 

- Complete the `Classify()` function inside `Classifier.c`
- Open `Lab15_ADCmain.c` in the project folder.
- Scroll down to the `main()` function and ensure the line with `Program15_2()` is uncommented while the other two are commented.
- Add the following variables to the Expressions tab: left, center, right, result, truth,  and errors.
- Run `main()` by selecting the debug tool.
- Observe the value of `errors`. If greater than 0, debug your `Classify()` function by placing breakpoints and observing the state of your program.

Note that `Program15_2` only tests corner cases. For the `Classify()` function, each of the three possible inputs can vary from 50 to 800. Therefore, there are $751^3$ ($423,564,751$) possible inputs. An exhaustive test (`Program15_3`) would evaluate them all. However, due to the nature of the problem, we can reduce the input values to a small subset of values around the threshold values. Using the knowledge of how the system works to select strategic values to test is called _corner cases_. In particular, we can reduce the number of test values from 751 to 18 with minimal loss of testing accuracy.  We will only test values that are $\pm 1$ from the threshold values of 50, 150, 212, 354, 600, and 800. 

### Demo the `Classify()` function.

- Complete the `LCDOut4()` function to display data on the LCD.
- Use the maze in the lab to test 7 _too close_ cases and eight normal cases.
- Demo `Program15_4()` displaying the classification on the LCD. You must show at least three error cases (enum scenario <= 7) and at least five non-error cases (enum scenario > 7).  A live demo is preferred, but a video demo is also acceptable. For a video demo, **add your voice to clearly state the classification you are testing**. Push your code to your repository using git. Comment your code.


## 🚚 Deliverables

### Deliverable 1 
- **[10 Points]** Demo `Program15_1()` displaying raw ADC data on the LCD. The distance values on the LCD are not valid yet. Push your code to your repository using git. Comment your code.

### Deliverable 2 
- **[10 Points]** Calibrate your sensors. Submit three plots for your right, center, and left sensors. 

### Deliverable 3 
- **[10 Points]** Demo `Program15_1()` with accurate distances on LCD. You must pick three distances for each sensor; 100, 300, and 500 mm. Use the ruler in the lab for the demo. A live demo is preferred, but a video demo is also acceptable. For a video demo, **add your voice to clearly state the distances you are testing**. Push your code to your repository using git. Comment your code.

### Deliverable 4 
- **[10 Points]** Provide a screenshot of the Expressions tab after running `Program15_2()`. Ensure left, center, right, result, truth, and errors are in the Expressions tab.  Note: If you provide a screenshot before running the program, you will get a grade of 0.

### Deliverable 5 
- **[10 Points]** Demo `Program15_4()` displaying the classification on the LCD. You must show at least three error cases (enum scenario <= 7) and at least five non-error cases (enum scenario > 7).  A live demo is preferred, but a video demo is also acceptable. For a video demo, **add your voice to clearly state the classification you are testing**. Push your code to your repository using git. Comment your code.


This lab has been adapted from the TI RSLK: [The Solderless Maze Edition curriculum](https://university.ti.com/en/faculty/ti-robotics-system-learning-kit/ti-rslk-max-edition-curriculum)
