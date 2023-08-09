# 💎 Resources

## Textbook Errata 😵

Embedded Systems: Introduction to Robotics, Johnathan W. Valvano, First Edition, 2019. ISBN: 978-1074544300.

**304-304, Program 13.2**

NVIC->IP[2] = (NVIC->IP[2]&0xFFFFFF00)|0x00000040; // priority 2

should read

NVIC->IP[8] = 0x40; // priority 2

**pp.310, Program 13.5** also has an error, but we don't use Timer32 in ECE 382.


(Resources:ASCII_Table)=
## ASCII Table
 
[Here is an online version](https://www.sciencebuddies.org/science-fair-projects/references/ascii-table)
 
![ASCII](./figures/ASCII.png)


## TI-RSLK Max Pin Map

![Pin Map](./figures/TI-RSLK_Max_Pin_Map.png)




## Dr. Valvano's Lecture Videos
TI Robotics System Learning Kit (TI-RSLK) Max Edition Lecture Videos
 
Check out this link for lecture videos we used in 2019.
https://university.ti.com/en/faculty/ti-robotics-system-learning-kit/ti-rslk-max-edition-curriculum


## Capt Beyer's Lecture videos
[2020 Lecture Videos](https://www.youtube.com/playlist?list=PLLvuo5HBf25HKbK18J28lJNyPkltAxfln)



