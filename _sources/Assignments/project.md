# 🚀 Final Project

```{attention}
- Read this page thoroughly before you start working on this final project.   
```

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

### Timeline

- L36 0700: Design Presentation slides (Gradescope and Instructors)
    - All cadets must submit their presentation files to their instructors NLT L36 0700. 
    - **_No grace days_** can be used for the powerpoint slides.
- L36/37: Design Presentations
- L39 0700: Demos Due 
    - Early bird **[3 Bonus Points]** Finish any one level by T37 (Thu 30 Nov Dec) 2359
    - Late Demos: You can use grace days, but all products must be submitted NLT T40 2359 (by the Dean's policy). 
    - No grace days can be used for early bird demos. 
- L39: Race (during your section)
- T40 2359: Final report & Code
    - No grace days can be used. All products must be submitted by T40 at midnight. 

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
void Controller(void){          // called every 1 ms
  Motor_TurnRight(3000, 3000);  // for 180 deg turn
  Clock_Delay1ms(100);          // BAD
}

void Controller(void){          // called every 1 ms
  Motor_TurnRight(3000, 3000);  // for 180 deg turn
  while(LeftSteps_deg < 180) {  // BAD
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

### Level 1 Demo (50 Points): Returning Home

Use the tachometers and bump sensors to navigate the maze and return home.

**Requirements:**
- Utilize the tachometers and bump switches for maze navigation; however, the use of IR distance sensors is strictly prohibited.
- When the robot reaches the designated `home` area, identified by three lines, it should come to a complete stop before making any contact with the wall.
- Upon successfully reaching home, your robot is expected to exhibit alternating flashes of red and blue LEDs, with each color lasting 0.5 seconds.
- Your robot is free to navigate the maze at any speed.
- Your robot begins the exploration without prior knowledge of the maze. 
- Home is identified at the origin (0,0), and the starting position is fixed at (340, 0) in millimeters. Your robot knows the coordinates of Home and the starting position.
- **[3 Bonus Points]** Upon successfully reaching home, your robot now knows way back to the starting point. Return to the the starting point. 

```{image} ./figures/Proj_Maze.jpg
:width: 660
:align: center
```
<br>

<center>
<iframe width="560" height="315" src="https://www.youtube.com/embed/Hq8rBYZTQrg" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
<br>
Video Credit: C22 Allison Burba
</center>
<br>

### Level 2 Demo (50 Points): Maze exploration

Use the distance sensors to explore a maze.

**Requirements:**
- Your robot must be able to explore the maze using distance sensors.
- Your robot must reach the goal and stop before hitting the wall. 
- No bump switches are allowed to use to explore the maze.
- You **must** use the Classifier.c file to determine the state of the robot.
- **You can assume your robot knows the maze ahead of time.**
- **[5 Bonus Points]** Arrive at the goal within 11.0 seconds.
- **[5 Bonus Points]** Return to the start and stop before hitting the wall.

<center>
<iframe width="560" height="315" src="https://www.youtube.com/embed/2m8tce8ZYoI" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
</center>
<br>

### Design Presentation (50 Points)

- Send your presentation file (**pptx**) to your instructor NLT L36 0700. 
- Export your Powerpoint file to a **pdf** file and submit it in Gradescope NLT L36 0700.
- Provide a **5-minute** presentation that discusses the following topics:
- Figures/tables/graphs are more helpful than words.


- **[5 Points]** Purpose: Briefly describe the following
    - Describe the problem.
    - Discuss the requirements.
    - What assumptions did you make going into the project?
- **[20 Points]** Design: Briefly describe the following
    - Discuss design choices for each level (e.g., values for PWM_AVERAGE, Kp, Ki).
    - Include a component diagram that matches your code for each level. Points will be taken off if you don't include a subsystem that is in your code or if you include a subsystem (like TimerA2) that is not in your code. **Ensure your component diagrams are large enough to read.**
    - What timers did you use, what did they do, and at what frequencies? 
    - Discuss the design of your controller.
- **[20 Points]** Debugging and testing
    - How would you perform high-level debugging such as misclassification, being traped at a corner, etc?
    - When your robot performs something unexpected in the maze, how would you find the problems?
    - Do not discuss compile-time debugging, such as syntax errors.
    - How would your methods help you debug and test your code?
    - You must discuss the details of debugging and testing methods.
- **[5 Points]** Questions: 
    - Be prepared to answer questions about the code that you have written.
    - Be prepared to answer questions about how you would implement additional functionality to your project.
    - The question component will be an additional 2 minutes.
- **Timing**:
    - You will lose points for going over the 5-minute limit and your presentation will be halted at 6 minutes and you will not get credit for the parts you don't discuss.

```{Attention}
Send your presentation file (**pptx**) to your instructor NLT L39 0700. Export your Powerpoint file to a **pdf** file and submit it in Gradescope NLT L39 0700. **Select questions and pages to indicate where your responses are located. You will lose points if you do not.** 
```


### Report (100 Points)

Reference the example report, `ECE382_Project Report Template.docx` in Teams under Files > Class Materials, for details on report content. Use `Component Block Diagram.pptx` to help you create your component diagrams for each of the three levels.

    - Even though you do not have successful demos (whether you use grace days or not), you still need to discuss all three levels based on what you have.



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
- **[50 Points]** Analysis and Results
    - Discuss any issues and problems you encountered. How did you overcome these?

    - This is the most essential part of the final project.
    - Discuss your results for each level. You must include measurements - plots.
    - Discuss how long it takes for your robot to complete the task. Attach Table 1 in the template. 
    - Discuss the performance of the robot using step responses. 
    - If you had unresolved issues, discuss how you would fix them if you had more time.
    - Discussed what unique things you did to make your robot better than others.
    - What levels were you unable to accomplish and why?    

    - What levels were you unable to accomplish and why?
    - What issues still exist in your robot?
    - Did you do anything unique to make your robot better than others?

- **[10 Points]** Conclusion
    - Conclude your work
    - Concisely summarize the results. 
    - Emphasize the overall result of the project.

```{attention}
Submit your report in Gradescope. **Select questions and pages to indicate where your responses are located as shown below. You will lose points if you do not.** 
```

```{image} ./figures/Proj_GradescopeSubmission.gif
:width: 740
:align: center
```
<br>

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
- 1st place: 20 bonus points and your name will be on the plaque in the lab.
- 2nd place: 15 bonus points
- 3rd and 4th places: 10 bonus points
- Same as Level 3 - Your robot must reach the goal and stop before hitting the wall.
- You can change any code you want.
- You will be given 2 minutes - You can re-flash your program multiple times within 2 minutes, but I would not do it.  Instead, I would change the P values and PWM_AVERAGE using the LCD and switches.

- Last year's results:
    - 1st place: Roger Moomaw - 10.56 sec
    - 2nd place: Joe Cates-Beier - 10.62 sec
    - 3rd place: Lizzie Kim - 32.63 sec
    - The rest did not finish. so we had another round for the 4th place
        - 4th place: Ryan Lilly - 10.77











