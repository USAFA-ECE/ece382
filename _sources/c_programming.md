# 💻 C Programming

(faq-c_programming)=
## C Programming Language

### _What is the operator precedence in C?_
 
What will the value of Y be when you have Y = X & 0xE0 >> 2? Are you anticipating that the operation will involve bit masking followed by shifting, as in (P4->IN & 0xE0) >> 2? In C/C++, it's important to note that << and >> take precedence over & (bitwise AND). Therefore, X & 0xE0 >> 2 is equivalent to P4->IN & (0xE0 >> 2) or P4->IN & 0x03.

Remember the arithmetic order of operations; for instance, 2 + 2 x 2 equals 6, not 8. If you're uncertain about the precedence order, it's advisable to use parentheses. I may not recall the entire precedence order, but I follow best coding practices by using parentheses. Otherwise, you might spend hours or even days debugging your code. For further reference, you can check operator precedence on the table below. 

Here is a quiz:

One of these is correct.  One is incorrect. Or are they both the same?
```C
while( (SysTick->CTRL & 0x00010000) == 0 );
```
or
```C
while( SysTick->CTRL & 0x00010000 == 0 );   
``` 

You can find the precedence order in the textbook on page 111 or below:

![Operator Precedence](./figures/OperatorPrecedence.png)
 
**_Are there different versions of C?_**

Indeed, there are several versions or variations of the C programming language standard. These versions include C79, C89, C99, C11, and C17, among others. You can find more detailed information about these different versions on this Wikipedia page: https://en.wikipedia.org/wiki/C_(programming_language)#History


The most recent standard is C17, but the most widely used versions are likely C89 (also known as ANSI C) and C79 (K&R C). CCS offers support for K&R, C89, C99, and C11.

If you want to switch between these variations, you can do so by going to project properties, then navigating to CCS Build > Advanced Options > Language Options, and selecting your preferred variation, as shown in the image below:

![C Dialect](./figures/C_Dialect.png)

It's worth noting that the default dialect in CCS is C89, but ECE382 employs C11 to facilitate features like variable declaration within for-loops, as seen in the example: `(int i = 0; ... )`.


### _What is the switch statement, and how does it work?_

The switch statement is a multi-way decision that tests whether an expression matches one of several constant integer values and branches accordingly.

```C
switch (expression) {
    case const-expr:  
        statements
        break;
    case const-expr:  
        statements
        break;
    default:
        statements
        break;
}
``` 

If you don't add `break`, the program will flow through the following case statements.  Here is an example

```C
int nwhite, nother, ndigit[10]; 

while ((ch == getchar()) != EOF) {
    switch (ch) {
        case '0':   case '1':  case '2' :  case '3': case '4' :       
        case '5':   case '6':  case '7' :  case '8': case '9' :   
        // if ch is between '0' and '9'
            ndigit[ch-'0']++;
            break;
        case ' ': 
        case '\n':
        case '\t':   
        // if ch is a space, \n, or \t
            nwhite++;
            break;
        default:
            nother++;
            break;
    }
}
``` 
Reference: B. Kernighan & D. Ritchie, "The C Programming Language," 2nd ed, pp. 58, 1988, Pearson Education.**

### _What is the _static_ qualifier?_

The functions within C files are inherently global functions. Even if they are not declared in the associated .h files, these functions can be accessed by other files, potentially leading to a compilation error if two functions with the same name exist in separate C files.

So, how can you restrict a function's visibility to only the file it's defined in? You can achieve this by using the **_static_** qualifier. This makes the functions private, ensuring that they remain within the scope of the C file in which they are defined.

Similarly, variables declared outside of functions are global variables, and like functions, they are accessible to other files. These variables can also be made static to restrict their visibility, effectively making them private and hidden from other files. It's worth noting that this is different from the concept of static variables inside a function.

## C Programming Language, 2nd Ed.  by Kernighan and Ritche

Ritche invented the C programming language.  This is the **best book** for C programming and the most popular one. 
[Amazon link](https://www.amazon.com/Programming-Language-2nd-Brian-Kernighan/dp/0131103628)

![C Programming](./figures/C_Programming.jpg)