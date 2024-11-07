
### Level 2: Advanced Control

In Lab 17, we implemented a simple heading controller for a wall-following robot. Now, for this level, we aim to enhance the heading control by integrating a speed regulator. This addition ensures the maintenance of a constant speed, unaffected by variations in battery power or external disturbances, such as increased friction while turning.

```{image} ./figures/Project_AdvControl.png
:width: 520
:align: center
```

**Requirements:**
- <span style="color:blue"> Implement your code in `Level2.c`. Avoid implementing your final project in the Lab 17 files. (Updated on 24 Nov 2023) </span>  
- Select three distinct speeds with visually noticeable differences. For instance, choose 50 rpm, 100 rpm, and 150 rpm.
- Adjust heading control parameters as necessary for each speed.
- Ensure that your robot is initially positioned at least 3 inches (or 7.5 cm) away from the center line. 
- <s>Your robot should stabilize to the steady state within 1.5 seconds.</s>  <span style="color:blue">Your robot should reach the stable state for only the highest speed within 2.0 seconds. The other two slower speeds can exceed 2.0 seconds. </span>  

- In your final report, include step responses for both speed and heading for each speed you have chosen. Provide the control parameters in the report and submit associated videos for each speed.


<center>
<iframe width="560" height="315" src="https://www.youtube.com/embed/pXxThhx4R4g?si=qcGjUYJav6CQJTol" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>
</center>
<br>

