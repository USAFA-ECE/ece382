# 🔬 Lab 11 Serial Ports

## 📌 Objectives

- Students should be able to develop device drivers for the Universal Asynchronous Receiver/Transmitter (UART) and Synchronous Peripheral Interface (SPI).
- Students should be able to measure UART and SPI signals using a logic analyzer and analyze the binary bits.

```{note}
The best thing about a Boolean is even if you are wrong, you are only off by a bit.
```

## 📜 Synopsis

The objective of this lab is to develop software modules that communicate with external devices connected by the most commonly-available serial communication interfaces.  The Nokia 5110 is connected to the MSP432 by a serial peripheral interface (SPI) and your PC’s USB port is connected to the MSP432 via a universal asynchronous receiver-transmitter (UART). 


## 💻 Procedure

(lab11-uart)=
### Write UART functions in `UART0.c`

1. Write `UART0_Init()`
    - Follow the steps in the code and Appendix 1 in Lecture 11.
2. Write `UART0_OutChar()`
    - Wait for TXBUF to be empty (UCxTXIFG)
    - Send data using TXBUF
    - Hint: Read `UART0_InChar()` and Lecture 11.
3. Write `UART0_OutString()`
    - A NULL-terminated string is a string with the NULL character at the end of it.
    - The NULL character is 0 (ASCII: 0), not '0' (ASCII: 0x30). You can find the ASCII table on Valvano pp. 60.
    - Transmit one character in the string at a time until you reach the end of the string. Do not transmit the NULL character.  Do not rewrite the code you already wrote. 

```{note}
The NULL character is not a null pointer, which is a pointer or reference that refers to an invalid object (or nothing). For example, if you have `int * x;`, the pointer `x` refers to nothing until you assign an address to it. 
```

- Debug (or F11) `Program11_1` but do not click `Resume` (or F8) yet.  
- Open Windows Device Manager and go to Ports.
- Find the COM port for `XDS110 Class Application/User UART`
- Go to CCS and open the `Terminal` tab under the `View` menu.
- Open a new terminal and select the COM port you found in Device Manager.
- Click `Resume` (or F8) to continue `Program11_1`.
- It will print strings and numbers in the serial terminal as shown below. 

```{image} ./figures/Lab11_SerialTerminal.gif
:width: 760
:align: center
```
<br>

- Take a screenshot of the serial terminal displaying the header strings and a few lines of data in the while-loop.  You can pause your program and use the Snip \& Sketch tool to take a screenshot.  
- Submit the screenshot in Gradescope.


```{admonition} Q&A
Q: For the UCAxBRW problem, when figuring out the value to put in, how does the system handle decimal points? Is there a rounding that should be done?
<br>
A: Round to the nearest integer. The tolerance is +/- 5%
<br>
<br>
Q: Did anyone else have the problem where the terminal was showing only high numbers under the "Serial Terminal" option (COM9-COM12)? My app keeps crashing when I try to run the terminal on one of these. I'm getting the right values otherwise and my code works fine.
<br>
A: It can be.  It depends on how many serial devices have been connected to your computer.  I once had something like COM24.
```


### Write UART functions in `UART2.c`

- Write `UART2_Init()`, `UART2_OutChar()`, and `UART2_OutString()`. They should be very similar to those in `UART0.c`.
- `UART2_Init()` takes `baudrate` as a function argument. So, users can choose their own baudrates when they initialize UART2 whereas the UART0 baud rate is fixed at 115,200 bps.   
- Replace _Baek_ in `char Name[] = "Baek"` before `Program11_2` with the first three letters of your last name.  Use your first name if your last name is less than three letters long. 
- Connect Moku:Gp Logic Analyzer to your LaunchPad.
    - Connect the 20-pin I/O port to Moku:Go as shown inside the red circle in the figure below.
    - Connect Pin 1 of the Logic Analyzer to P3.3 of your LaunchPad as shown inside the yellow circle.
    - Connect a Logic Analyzer's ground pin (black wire) to one of the LaunchPad ground pins as shown in the yellow circle.  


```{image} ./figures/Lab11_MokuConnection.png
:width: 760
:align: center
```

<br>

- Run `Program11_2` and start Moku:Go Logic Analyzer to measure the signal transmitted from the UART2 port of your LaunchPad as shown below. 
- Scroll your mouse wheel up or down to display the entire signal. If you don't have a scroll wheel, change the `Time span` value under the `Acquisition` tab.

```{image} ./figures/Lab11_LogicAnalyzerMeasure.gif
:width: 760
:align: center
```
<br>

- Take a screenshot and clearly annotate start bits (S), stop bits (E), and the binary bits of your three letters as shown below. 
- Add your three letters under the corresponding binary signals.
- You can find an ASCII binary table in {ref}`Resources:ASCII_Table`. 

```{image} ./figures/Lab11_Moku_UART_Fox.png
:width: 760
:align: center
```
<br>


### Write SPI functions in SPI\_B1

- Write `SPIB1_Init()`, `SPIB1_OutChar()`, and `SPIB1_OutString()`. 
- Run `Program11_3` and start Moku:Go Logic Analyzer to measure the signal transmitted from the SPI\_ B1 port of your LaunchPad. 
- Connect Moku:Gp Logic Analyzer to your LaunchPad.
    - Connect three Logic Analyzer probes to P6.2 (STE), P6.3 (CLK), and P6.4 (MOSI) of your LaunchPad.
    - Connect a Logic Analyzer's ground pin (black wire) to one of the LaunchPad ground pins.  
- Take a screenshot of the signal below and clearly annotate the binary bits of your three letters. 
- Add your three letters under the corresponding binary signals.

```{image} ./figures/Lab11_Moku_SPI_Fox.png
:width: 760
:align: center
```
<br>

````{admonition} Q&A
Q: My program will not leave the while loop initiated in 
```
while((EUSCI_B1->IFG&0x02)==0);
EUSCI_B1->TXBUF = data;
```
A: You probably did not initialize it properly.  Look at your init() function carefully.
````

<br>


## 🚚 Deliverables

### Deliverable 1 
- **[6 Points]** Run `Program11_1()` to transmit strings via USB/UART and take a screenshot of the serial terminal displaying the strings. Submit a screenshot of your Serial Terminal in Gradescope.

### Deliverable 2
- **[6 Points]** Run `Program11_2()` to transmit your name via UART. Submit a screenshot of Moku:Go Logic Analyzer. **Annotate three letters of your name and their binary values.**

### Deliverable 3 
- **[6 Points]** Run `Program11_3()` to transmit your name via SPI. Submit a screenshot of Moku:Go Logic Analyzer.  **Annotate three letters of your name and their binary values.** 

### Deliverable 4
- **[6.5 Points]** Push your code to your repository using git. Ensure you comment on your code.

<br>

This lab has been adapted from [TI-RSLK MAX Solderless
Maze Edition Curriculum](https://university.ti.com/en/faculty/ti-robotics-system-learning-kit/ti-rslk-max-edition-curriculum)


