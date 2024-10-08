# 🔬 Lab 11 Serial Ports

## 📌 Objectives

- Students should be able to develop device drivers for the Universal Asynchronous Receiver/Transmitter (UART) and Synchronous Peripheral Interface (SPI).
- Students should be able to measure UART and SPI signals using a logic analyzer and analyze the binary bits.

```{note}
The best thing about a Boolean is even if you are wrong, you are only off by a bit.
```

## 📜 Synopsis

In this lab, our aim is to develop software modules capable of communicating with external devices connected by the most commonly-available serial communication interfaces.  Specifically, we will establish communication between the MSP432 and the Nokia 5110 using a Serial Peripheral Interface (SPI), and we will connect our PC's USB port to the MSP432 using a Universal Asynchronous Receiver-Transmitter (UART).


## 💻 Procedure

(lab11-uart)=
### Write UART functions in `UART0.c`

1. Write `UART0_Init()`
    - Follow the steps in the code and Appendix 1 from Lecture 11.
2. Write `UART0_OutChar()`
    - Wait for TXBUF to be empty (UCxTXIFG)
    - Send data using TXBUF
    - Hint: Read `UART0_InChar()` and Lecture 11.
3. Write `UART0_OutString()`
    - A NULL-terminated string is a string with the NULL character at the end of it.
    - The NULL character is 0 (ASCII: 0), not '0' (ASCII: 0x30). You can find the ASCII table on ECE 382 Ref Guide, Valvano pp. 60, or [here](Resources:ASCII_Table).
    - Transmit one character in the string at a time until you reach the end of the string. Do not transmit the NULL character.  Do not rewrite the code you already wrote. 

    ```{note}
    The NULL character should not be confused with a null pointer, which is a pointer or reference that points to an invalid object or to nothing. For instance, consider the declaration `int* x;` where the pointer x initially doesn't point to any valid address until you assign one to it.
    ```

1. Debug (or F11) `Program11_1` but do not click `Resume` (or F8) yet.  
1. Open Windows Device Manager and go to Ports.
1. Find the COM port for `XDS110 Class Application/User UART`
1. Go to CCS and open the `Terminal` tab under the `View` menu.
1. Open a new terminal and select the COM port you found in Device Manager.
1. Click `Resume` (or F8) to continue `Program11_1`.
1. It will print strings and numbers in the serial terminal as shown below. 

    ```{image} ./figures/Lab11_SerialTerminal.gif
    :width: 760
    :align: center
    ```
    <br>

1. Take a screenshot of the serial terminal displaying the header strings and a few lines of data in the while-loop.  You can pause your program and use the Snip \& Sketch tool to take a screenshot.  
1. Submit the screenshot on Gradescope.


    ```{admonition} Q&A
    Q: Did anyone else have the problem where the terminal was showing only high numbers under the "Serial Terminal" option (COM9-COM12)? My app keeps crashing when I try to run the terminal on one of these. I'm getting the right values otherwise and my code works fine.
    <br>
    A: It can be.  It depends on how many serial devices have been connected to your computer.  I once had something like COM24.
    ```


### Test UART0 with Moku:Go

1. Replace _Baek_ in `char Name[] = "Baek"` before `Program11_2` with the first three letters of your last name.  Use your first name if your last name is less than three letters long. 

    ```{warning}
    Turn off the robot before connecting the logic analyzer to it. Otherwise, you might short-circuit and damage the board!
    ```

1. Connect Moku:Go Logic Analyzer to your LaunchPad.
    - Connect the 20-pin I/O port to Moku:Go.
    - Connect Pin 1 of the Logic Analyzer to the TXD port on your LaunchPad.
    - Connect a Logic Analyzer's ground pin (black wire) to one of the LaunchPad ground pins.  

    ```{image} ./figures/Lab11_MokuConnection.png
    :width: 460
    :align: center
    ```

1. Run `Program11_2` and start Moku:Go Logic Analyzer to measure the signal transmitted from the UART2 port of your LaunchPad as shown below. 
1. Scroll your mouse wheel up or down to display the entire signal. If you don't have a scroll wheel, change the `Time span` value under the `Acquisition` tab.

    ```{image} ./figures/Lab11_LogicAnalyzerMeasure.gif
    :width: 760
    :align: center
    ```
    <br>

1. Take a screenshot and clearly annotate start bits (S), stop bits (E), and the binary bits of your three letters as shown below. 
1. Add your three letters under the corresponding binary signals.
1. You can find an ASCII binary table [here](Resources:ASCII_Table).

    ```{image} ./figures/Lab11_Moku_UART_Fox.png
    :width: 760
    :align: center
    ```
    <br>


### Write SPI functions in SPI_A3 and Test with Moku:Go

1. Implement `SPIA3_Init()`, `SPIA3_Wait4Tx()`,  `SPIA3_WriteTxBuffer()`, `SPIA3_OutChar()`, and `SPIA3_OutString()`. For guidance on how to use these functions, refer to `Nokia5110_Init()`, `Nokia5110_OutChar()`, and `datawrite()` in the `Nokia5110.c` file. **Hint**: `SPIA3_OutChar()` should integrate the operations of `SPIA3_Wait4Tx()` and `SPIA3_WriteTxBuffer()`.  Do not call these functions directly within the `SPIA3_OutChar()`; Instead, incorporate their logic directly into the function.

1. Run `Program11_3` and start Moku:Go Logic Analyzer to measure the signal transmitted from the SPI_A3 port of your LaunchPad. 

    ```{warning}
    Turn off the robot before connecting the logic analyzer to it. Otherwise, you might short-circuit and damage the board!
    ```

1. Connect Moku:Go Logic Analyzer to your LaunchPad.
    - Connect three Logic Analyzer probes to P9.4 (STE), P9.5 (CLK), and P9.7 (MOSI) of your LaunchPad.
    - Connect a Logic Analyzer's ground pin (black wire) to one of the LaunchPad ground pins.  
1. Take a screenshot of the signal below and clearly annotate the binary bits of your three letters. 
1. Add your three letters under the corresponding binary signals.

    ```{image} ./figures/Lab11_Moku_SPI_Fox.png
    :width: 760
    :align: center
    ```
    <br>

    ```{note}
    `Program11_3` bypasses the Nokia5110 module and directly accesses the SPIA3 serial port, which means the LCD cannot be used for this section.
    ```


## 🚚 Deliverables

### Deliverable 1 
- **[6 Points]** Run `Program11_1()` to transmit strings via USB/UART and take a screenshot of the serial terminal displaying the strings. Submit a screenshot of your Serial Terminal on Gradescope.

### Deliverable 2
- **[6 Points]** Run `Program11_2()` to transmit your name via UART. Submit a screenshot of Moku:Go Logic Analyzer. **Annotate three letters of your name and their binary values.**

### Deliverable 3 
- **[6 Points]** Run `Program11_3()` to transmit your name via SPI. Submit a screenshot of Moku:Go Logic Analyzer.  **Annotate three letters of your name and their binary values.** 

### Deliverable 4
- **[6.5 Points]** Push your code to your repository using git. Ensure you comment on your code.

<br>

This lab was originally adapted from the [TI-RSLK MAX Solderless Maze Edition Curriculum](https://university.ti.com/en/faculty/ti-robotics-system-learning-kit/ti-rslk-max-edition-curriculum) and has since been significantly modified.



