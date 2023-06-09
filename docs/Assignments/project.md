# 🚀 Final Project

:::{attention}
- Read this page thoroughly before you start working on this final project.   
:::

## 📌 Objectives

- Students should be able to combine previous modules into a system that solves a complex task.
- Students should be able to build on and utilize almost all modules throughout the course.

```{note}
First, solve the problem. Then, write the code. 
```

## 📜 Agenda

- Culmination of embedded systems: A system that performs a specifically dedicated operation where the computer is embedded inside the machine.
- Allows you to perform duties typical of the engineering profession:
    - Given a system that works (hopefully), how can you build on it?
    - How can we combine multiple systems that work to build a more complex system?
- Challenge Levels:
    - Level 1: Basic line follow - there and back.
    - Level 2: Complex line follow - search for treasure.
    - Level 3: Maze exploration.

## 🎮 Final project gamesmanship

- Demo and coding
    - Read the presentation and final report requirements before you start coding.
    - Start early – earn early-bird bonus points. If you don’t start early, you may be unable to finish it on time.
    - Use the code from Lab17.
    - Extensively use LCD/UART to debug high-level behaviors.
    - Change tuning parameters such as Kp during run-time, not compile-time.  Use the LCD, bump sensors, and switches. 
    - **Cease work for presentation and report**
        - If your robot completes only the halfway point for Level 1 (one way) and can’t return to the start, you will probably lose about 5 points.
        - Your analysis in the presentation/report is much more important than completing the maze. 
        - Do not spend too much time on the bonus points. You don't want to lose 30 points on your report to earn 20 bonus points. 

- Presentation
    - Use visual aids a lot! Figures, tables, and graphs are more helpful than words.
    - Ensure you discuss everything in the Presentation section.
    - Timing is important – 7 minutes --> Practice!

- Report
    - Read the template carefully and do not miss anything there.
    - Use figures and tables to support your analysis and results.

## 💻 Procedure

### Requirements for all levels

- Do not use hard-coded numbers. Use `#define` and `const`.  For example, do not use

```C
    TimerA1_Init(&Controller, 500); 
```
but use
```C
    // user TimerA1 to run the controller at 1000 Hz
    uint16_t const period_2us = 500;        // T = 1ms
    TimerA1_Init(&Controller, period_2us);  // f = 1000 Hz controller loop
```

- Add comments.  A lot of comments!
- Turns must be accomplished using the tachometers for Levels 1 \& 2, with no delays.
- You can demo in person, but you still need to submit video demos associated with the plots in your report. 
- There must be no delays in your Interrupt Service Routines (ISRs). No loops, waits, sleeps, etc. 5 points per level will be taken off if delays or loops exist in ISR, for example

```C
void Controller(void){  // called every 1 ms
  Motor_TurnRight(3000, 3000);  // for 180 deg turn
  Clock_Delay1ms(100);  // BAD
}

void Controller(void){// called every 1 ms
  Motor_TurnRight(3000, 3000);  // for 180 deg turn
  while(LeftSteps_deg < 180) {   // BAD
      Tachometer_GetSteps(&LeftSteps_deg, &RightSteps_deg);
  }
}
```

- All controller code must go in the ISR. 5 points per level will be taken off if the controller code is in main instead of an ISR.

```C
void main(void)
  :
  while(1){   // no time-critical operations such as 
    LCDOut(); // can be interrupted at any time
    // No state transition
    // No motor control or sensing
    // If you remove everything inside this while-loop,
    // your robot should still be able to complete the level.
    // For example, Program17_x.c
  }
}
```
<br>


### Level 1 Demo: Basic line following - there and back

Your robot must utilize the line and bump sensors to follow a line through a maze.

**Requirements:**
- Your robot must be able to follow the white line on the left of the maze shown in the figure below. 
- When it gets to the end of the line, it will bump into a wall, move backward at least 5cm, and turn around.
- No IR distance sensors are allowed to use.
- It will then return to the start.
- When it hits the start, it will alternately flash the red and blue LEDs (forever).
- Your robot can run at any speed.
- When the robot moves backward and turns around, you must use the distance at least one of the wheels travels. Do not use the time elapsed.   
- You **must** use a finite state machine to control the state transitions (following line > moving backward > turning around > following line > blinking).
- **[3 Bonus Points]** Demo three round trips and flash the red and blue LEDs at the end of the third trip. You can still earn this bonus points on top of your early bird bonus points, but your early bird demo should be complete by the early bird due date. 

```{image} ./figures/Proj_Maze.jpg
:width: 660
:align: center
```
<br>

<center>
<iframe width="560" height="315" src="https://www.youtube.com/embed/c8-lUruprA8" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
<br>
Video Credit: C22 Catherine Grebe
</center>
<br>

### Level 2 Demo: Complex line following - search for treasure

Your robot must utilize the line and bump sensors to follow a line through a maze.

**Requirements:**
- Your robot must be able to follow the white line on the right of the maze. 
- Your robot can use the reflectance sensors and bump switches to explore the maze. No IR distance sensors are allowed to use.
- When the robot gets to the goal, it must detect a tape pattern (e.g., 10101010) and stop before hitting the wall.
- When it reaches the goal, it will flash the red and blue LEDs alternately.
- Your robot can run at any speed.
- Your robot must explore all the white lines. 
- Your robot does not know the maze ahead of time.
- **[3 Bonus Points]** Return to the start without hitting the walls. 


<center>
<iframe width="560" height="315" src="https://www.youtube.com/embed/Hq8rBYZTQrg" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
<br>
Video Credit: C22 Allison Burba
</center>
<br>

### Level 3 Demo: Maze exploration

Your robot must utilize distance sensors to explore a maze.

**Requirements:**
- Your robot must be able to explore the maze using distance sensors.
- Your robot must reach the goal and stop before hitting the wall. _You may use the reflectance sensors to detect the goal._
- No bump switches are allowed to use to explore the maze.
- You **must** use the Classifier.c file to determine the state of the robot.
- **You can assume your robot knows the maze ahead of time.**
- **[5 Bonus Points]** Arrive at the goal within 11.0 seconds.
- **[5 Bonus Points]** Return to the start and stop before hitting the wall.

<center>
<iframe width="560" height="315" src="https://www.youtube.com/embed/2m8tce8ZYoI" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
</center>
<br>


### Report

Reference the example report, `ECE382_Project Report Template.docx` in Teams under Files > Class Materials, for details on report content. Use `Component Block Diagram.pptx` to help you create your component diagrams for each of the three levels.


- **[10 Points]** Introduction/Purpose
    - Describe the problem.
    - Discuss the requirements.
    - What assumptions did you make going into the project?
- **[20 Points]** Design 
    - Discuss design choices for each level (e.g., values for PWM_AVERAGE, Kp, Ki).
    - Include a component diagram that matches your code for each level. Points will be taken off if you don't include a subsystem that is in your code or if you include a subsystem (like TimerA2) that is not in your code. **Ensure your component diagrams are large enough to read.**
    - What timers did you use, what did they do, and at what frequencies?
    - Discuss the design of your controller.
    - Discuss any issues and problems you encountered. How did you overcome these?
- **[20 Points]** Debugging and testing
    - How did you debug your program during run-time?
    - When your robot performed something unexpected in the maze, how did you find the problems?
    - Do not discuss compile-time debugging, such as syntax errors.
    - How did your methods help you debug and test your code?
    - You must discuss the details of debugging and testing methods.
- **[40 Points]** Analysis and Results
    - This is the most essential part of the final project.
    - Discuss your results for each level. You must include measurements - plots.
    - Discuss how long it takes for your robot to complete the task. Attach Table 1 in the template. 
    - Discuss the performance of the robot using step responses. 
    - If you had unresolved issues, discuss how you would fix them if you had more time.
    - Discussed what unique things you did to make your robot better than others.
    - What levels were you unable to accomplish and why?    
- **[10 Points]** Conclusion
    - Conclude your work
    - Concisely summarize the results. 
    - Emphasize the overall result of the project.

:::{Attention}
Submit your report in Gradescope. **Select questions and pages to indicate where your responses are located as shown below. You will lose points if you do not.** 
:::

```{image} ./figures/Proj_GradescopeSubmission.gif
:width: 740
:align: center
```
<br>


### Presentation

- Provide a **7-minute** presentation that discusses the following topics:
- Figures/tables/graphs are more helpful than words.

- **[10 Points]** Purpose: Briefly describe the following
    - Describe the problem.
    - Discuss the requirements.
    - What assumptions did you make going into the project?
- **[25 Points]** Design: Briefly describe the following
    - Discuss the design of your controller.
    - Discuss any issues and problems you encountered. How did you overcome these?
- **[40 Points]** Analysis and Results
    - This is the most essential part of the final project.
    - Discuss your results for each level. You must include measurements - plots.
    - What levels were you unable to accomplish and why?
    - What issues still exist in your robot?
    - Did you do anything unique to make your robot better than others?
- **[10 Points]** Conclusion
    - Conclude your work
- **[10 Points]** Questions: 
    - Be prepared to answer questions about the code that you have written.
    - Be prepared to answer questions about how you would implement additional functionality to your project.
    - The question component will be an additional 2 minutes.
- **[5 Points]** Timing
    - You will lose points for going over the 7-minute limit.

:::{Attention}
Send your presentation file (**pptx**) to your instructor NLT L39 0700. Export your Powerpoint file to a **pdf** file and submit it in Gradescope NLT L39 0700. **Select questions and pages to indicate where your responses are located. You will lose points if you do not.** 
:::

### Timeline

- Turn-in Requirements:
    - L39 0700: Demo.
    - L39 0700: Powerpoint slides
    - L39/40: Final presentaion
    - T40 2359: Final report & Code

- Demo: due L39 0700
    - Early bird **[5 Bonus Points]** Finish any one level by <strike>M37 (Wed 30 Nov) 2359</strike> <font color='red'>T38 (Thu 1 Dec) 2359</font>

    - Early bird **[5 Bonus Points]** Finish any two levels by M38 (Fri 2 Dec)  <strike>0700</strike> <font color='red'>2359</font>
    - Late Demos: You can use grace days, but all products must be submitted NLT T40 2359 (by the Dean's policy). You may still lose points on your final presentation (See the Final Presentation section).
    - No grace days can be used for early bird demos. 
    
- Final Presentation: L39 and L40
    - All cadets must submit their presentation files to their instructors NLT L39 0700. 
        - M-day sections: M39 0700.
        - T-day sections: T39 0700.
    - Send your presentation file (**pptx**) to your instructor NLT L39 0700. 
    - Export your Powerpoint file to a **pdf** file and submit it in Gradescope NLT L39 0700.
    - Even though you do not have successful demos (whether you use grace days or not), you still need to discuss all three levels based on what you have.
    - You won't lose any points for late demos if you use grace days. However, you may lose points on your final presentation if the analysis parts of the demos are insufficient. 
    - No grace days can be used for the presentation.

- Report: T40 2359.
    - No grace days can be used. All products must be submitted by T40 at midnight. 

## 🚚 Deliverables

### Deliverable 1: [100 + 26 Bonus Points] Demo and Code.
- **[55 Points]** Demo.
- **[45 Points]** Code
    - **[15 Points]** 5 points per level are taken off if delays or loops exist in ISR.
    - **[15 Points]** 5 points per level are taken off if the controller code is in the main instead of an ISR.
    - **[15 Points]** At most, 5 points per level are taken off for bad coding practices (e.g., poor comments or using hard-coded numbers instead of variables or enumerated types.)
- **[0 Points]** Uncompilable code. If your Level 1 demo works but your Level 2 code is not compilable, you will still get a grade of 0 for the entire Deliverable 1. Always submit compilable code!

### Deliverable 2: [100 Points] Presentation.
- **[10 Points]** Purpose
- **[25 Points]** Design
- **[40 Points]** Analysis and Results
- **[10 Points]** Conclusion
- **[10 Points]** Questions
- **[5 Points]** Timing

### Deliverable 3: [100 Points] Report 
- **[10 Points]** Introduction
- **[20 Points]** Design 
- **[20 Points]** Debugging and testing
- **[40 Points]** Analysis and Results
- **[10 Points]** Conclusion

### Deliverable 4: [0 Points] Return your robot
- You must return your robot to your instructor **in person** by Tuesday 13 Dec 1600.
- Ensure your robot has no loose parts. Tighten all the screws before returning your robots.
- **[-20 Points]** Up to 20 points will be deducted if your robot has any loose parts. 

## 🏇 Final Race
- TBD













