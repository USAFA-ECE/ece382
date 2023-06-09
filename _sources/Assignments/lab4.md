# 🔬 Lab 4 Subroutines

## 📌 Objectives
- Students should be able to write assembly procedures (also called subroutines or functions) that comply with the AAPCS.
- Students should be able to write assembly conditional instructions and arithmetic operations.

```{note}
There are only two kinds of programming languages: those people always bitch about and those nobody uses. -Bjarne Stroustrup (creator of C++)
```

## 📜 Synopsis


```{tip}
**START EARLY!** You need to write only 20 - 25 lines of assembly code, but it may take **much longer** than you expect. Start early and seek EI if needed!
```

In this lab, you will write a program that encrypts and decrypts a message using a simple cryptography technique. A simple, yet effective encryption technique is to `XOR` a piece of information with a key and send the result. The receiver must have the key in order to decrypt the message - which is accomplished simply by `XOR`-ing the encrypted data with the key.

Let's say I wanted to send the binary byte `0b01100011` and my key was `0b11001010`. To encrypt, I `XOR` the two - the resulting byte is `0b10101001`. To decrypt, I `XOR` the result with the key - providing the original message, `0b01100011`.

A plain text message of arbitrary length is stored in ROM. Your job is to encrypt and then decrypt it given a key, which is also stored in ROM. The contents of the message are [ASCII Characters](../resources.md) each character is encoded in a single byte. All messages will end with the null character ('\0', ASCII value 0).


### ARM Architecture Procedure Call Standard (AAPCS)

The Arm architecture places few restrictions on how _general_ purpose registers are used. If you want your code to interact with code that is written by someone else, or with code that is produced by a compiler, then you need to agree rules for register usage. For the Arm architecture, these rules are called the Procedure Call Standard (PCS).

The PCS specifies:

- Which registers are used to pass arguments into the function.
- Which registers are used to return a value to the function doing the calling, known as the caller.
- Which registers the function being called, which is known as the callee, can corrupt.
- Which registers the callee cannot corrupt.

reference: https://developer.arm.com/documentation/102374/0100/Procedure-Call-Standard

To elaborate on the standard:

- If there is one input parameter, it is passed in R0; two, R0-R1; three, R0-R2; four, R0-R3
- If there is an output parameter, it is returned in R0
- Functions can modify R0-R3, and R12 freely
- If a function wishes to use R4-R11 it must save and restore them using the stack.
- If a function calls another, then it must save and restore LR
- Functions must balance the stack

The entire AAPCS can be found <a href="https://github.com/ARM-software/abi-aa/releases/download/2022Q1/aapcs32.pdf" target="_blank">here</a>


```{important}
It’s important to ensure that your assembly code complies with the AAPCS (ARM Architecture Procedure Call Standard). The AAPCS defines the standard for how subroutines are called and how registers are used in ARM assembly language.

The AAPCS is important because it ensures that code written by different people can be used together without any issues. It also makes it easier to write code that can be reused in different projects.

If you’re not sure whether your code complies with the AAPCS, you can check the ARM documentation or ask your instructor for help.
```


## 💻 Procedure

### Setup
- Connect the LaunchPad to your computer via the provided USB cable.
- Open Code Composer Studio (CCS) and select your workspace.
- Ensure your Project Explorer is open on the left of the CCS screen.
- Open the `Lab04_Cryptography` project by double-clicking it.


### Write the `Encrypt` subroutine

- Open `Lab04_Granger.asm` in the `Lab04_Cryptography` project.
- Ensure the `main` function at line 58 in `Lab04_Granger.asm` is **not commented** out. Otherwise, uncomment it by deleting the semicolon(`;`) at the beginning of the line;
``` ASM
;main:    .asmfunc
```
- Ensure the `main` function at line 58 in `Lab04_Potter.asm` is **commented** out. Otherwise, comment it out by adding the semicolon(`;`) at the beginning of the line;
- Read `Lab04_Granger.asm` carefully.  You need to understand the code before writing code in `cryptography.asm`.
- Open `cryptography.asm`. and read the comments carefully.  

**Complete the `Encrypt` function**

- You need to load addresses into registers.
- Then, load a byte of the message one at a time and then call the `XOR_bytes` function to encrypt it.  Which register do you need to use to pass arguments to a function?

```{warning}
Do **NOT** implement your own "xor".  You **must** use the provided `XOR_bytes` function. It is declared at line 42, `.global XOR_bytes` and defined inside `xor_bytes.obj`, which is already complied. So, the actual implementation is hidden, and you cannot tell what registers are being used inside the function. All you know is it takes two arguments (say x and y) and returns the exclusive-or of x and y.  The `XOR_bytes` function complies with the AAPCS.
```

- After you xor two bytes, store the encrypted message byte to the memory.
- Then, you need to check if the **unencrypted** message byte is the end of message (EOM), which is '#'.  If you loaded the message byte into R0-R3, it may have been corrupted after calling the `XOR_bytes` function because "functions can modify R0-R3, and R12 freely."  But you have to use R0-R3 to pass arguments. How can you preserve the message byte?
- If the **unencrypted** message byte is '#', then you are done. Otherwise, retrieve the next byte of the message to encrypt and repeat.  

```{note}
For '#', use the constant `EOM` defined in `cryptography.asm` at line 44. 
```


```{admonition} Q&A
Q: How would it be possible to return a multi-byte message into a single register if I can only XOR one at a time?
<br>
A: It is not possible. You should load, XOR, and store one byte at a time.
<br>

Q: I am struggling with the CMP statement in decrypt after I call the XOR_bytes. I do not know what comparing the R0 returned from XOR_bytes to the null character does.
<br>
A: The null character is the last character of the message, and that is why you're comparing each char to it. It is to check if that's the end of the message, like a terminating char. Also, you want to compare the decrpyted byte to the null character so you can tell when to leave the loop.
<br>

Q: Is there an easy way to check if the encrypted message is correct?
<br>
A: If so, it is not encryption. 😆  An encrypted message will not be readable characters in the Characters view (It will probably display a bunch of dots).  Change it to the 32-bit Hex view. If you want to know whether your encryption works correctly or not, you need to manually XOR one byte at a time and compare it.
```

### Write the `Decrypt` subroutine

- Complete the `Decrypt` function.
- You need to load addresses into registers.
- Then, load a byte of the encrypted message one at a time and then call the `XOR_bytes` to decrypt it.  Which register do you need to use to pass arguments to a function?

```{warning}
Do **NOT** implement your own "xor".  You **must** use the provided `XOR_bytes` function.
```

- After you xor two bytes, store the decrypted message byte to the memory.
- Then, you need to check if the **decrypted** message byte is EOM.  
- If the **decrypted** message byte is EOM, then you are done. Otherwise, retrieve the next byte of the message to decrypt and repeat.  

- After execution of `Lab04_Granger.asm` provide the following screenshots.
    * The addresses of each of the variables stored in RAM and ROM (use the Memory Browser and the pointers you created. You can search the Memory Browser by variable name or memory address).
    * The contents of `enc_msg`. Use Memory Browser and change the encoding style to **32-bit Hex**.
    * The contents of `dec_msg`. Use Memory Browser and change the encoding style to the **correct** format so that everyone can verify that the code works correctly.

```{tip}
What is the correct encoding sytle? Your code encrypts and decrypts "Wingardium Leviosa!" After decrypting the spell, what do you expect to see?
```

```{image} ./figures/Lab04_WingardiumLeviosa2.gif
:width: 640
:align: center
```

```{important}
It’s important to remember that function calls will overwrite the LR register and could overwrite registers R0-R3 and R12. If you’re calling multiple subroutines, you’ll need to make sure that you save the values of these registers before calling the subroutine and restore them after the subroutine has finished executing. You can use the PUSH and POP instructions to save and restore the values of these registers. 
```

### Complete and execute `Lab04_Potter.asm`

You intercepted the following encrypted message between Dr. Baek and Dr. York. You also overheard Dr. Baek telling Dr. York that the secret key is _the number of staircases at Hogwarts_.  Use your decrypt function to decrypt the Harry Potter's spell and provide a screenshot of your Memory Browser showing the decrypted message.

```asm
spell    .word 0xEBFEF6CB, 0xAEE1FAED, 0xFCFAEFDE, 0xE3FBE0E1, 0x00008EAF
```

- Open `Lab04_Potter.asm` in the `Lab04_Cryptography` project.
- Ensure the `main` function at line 58 in `Lab04_Potter.asm` is **not commented** out. Otherwise, uncomment it by deleting the semicolon(`;`) at the beginning of the line;
```asm
;main:    .asmfunc
```
- Ensure the `main` function at line 58 in `Lab04_Granger.asm` is **commented** out. Otherwise, comment it by adding a semicolon(`;`) at the beginning of the line;
- Replace the `key` value at line 48 with the secret key. Please do not write "the number of staircases at Hogwarts" verbatim. You can ask Siri, Google, or ChatGPT.  It should be a 1-byte number. You don't need to convert it to hex. You can enter a decimal number.
- Execute `Lab04_Potter.asm` to decrypt the spell and provide the screenshot of your Memory Browser showing the decrypted message. You must select the correct encoding style showing the spell. 

```{image} ./figures/Lab04_ExpectoPatronum.gif
:width: 640
:align: center
```

## 🚚 Deliverables

```{warning}
Your code **must be compilable**.  If your code throws any compile errors, you will get **a grade of 0** for the coding part.
```

### Deliverable 1
- **[12 Points]** Complete `cryptography.asm` and push your code to your repository. `Lab04_Granger.asm` will use your code to encrypt and decrypt the message in it. **Write comments in your code.**

### Deliverable 2
- **[12 Points]** Execute `Lab04_Granger.asm` and take screenshots of the following and submit them via Gradescope:

    * **[6 Points]** The contents of `enc_msg` after execution of `Lab04_Granger.asm`. Use the Memory Browser and change the format to **32-bit Hex**.

    * **[6 Points]** The contents of `dec_msg` after execution of `Lab04_Granger.asm`. Use Memory Browser and change the encoding style to the **correct** format so that everyone can verify that the code works correctly.


### Deliverable 3
- **[5.5 Points]** Execute `Lab04_Potter.asm` to decrypt `spell` and provide the screenshot of your Memory Browser showing the decrypted message.


