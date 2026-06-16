<div align="center">
<a href="https://github.com/effjy/learning-c/"><img src="titles/learning-c-title.svg" height="52" alt="Learning C"></a>
</div>

# Learning to Code in C — A Complete Beginner's Guide

Welcome! This document is a hands-on, from-scratch introduction to the **C programming language**. It assumes you have *never written a line of C* before. By the end you will understand what a program is, how to write one, how to turn it into something your computer can run, and you will have solved **5 exercises** that grow in difficulty.

Read it top to bottom the first time. Later you can jump back to any section as a reference.

---

## Table of Contents

1. [What is C and why learn it?](#1-what-is-c-and-why-learn-it)
2. [How a C program becomes a running program (compilation)](#2-how-a-c-program-becomes-a-running-program-compilation)
3. [Setting up your tools](#3-setting-up-your-tools)
4. [Your first program, line by line](#4-your-first-program-line-by-line)
5. [Variables and data types](#5-variables-and-data-types)
6. [Operators and expressions](#6-operators-and-expressions)
7. [Input and output](#7-input-and-output)
8. [Control flow: if, else, switch](#8-control-flow-if-else-switch)
9. [Loops: while, do-while, for](#9-loops-while-do-while-for)
10. [Functions](#10-functions)
11. [Arrays and strings](#11-arrays-and-strings)
12. [Pointers (the scary part, made simple)](#12-pointers-the-scary-part-made-simple)
13. [Structs](#13-structs)
14. [Memory: the stack, the heap, and malloc](#14-memory-the-stack-the-heap-and-malloc)
15. [Coding style and indentation](#15-coding-style-and-indentation)
16. [The 5 Exercises](#16-the-5-exercises)
17. [Where to go next](#17-where-to-go-next)

---

## 1. What is C and why learn it?

**C** is a programming language created in the early 1970s by Dennis Ritchie at Bell Labs. Despite its age, it is still one of the most important languages in the world. Here is why:

- **It is everywhere.** Operating systems (Linux, parts of Windows and macOS), databases, web servers, embedded devices (microwaves, cars, routers), and the runtimes of newer languages (Python, Ruby, PHP) are written in C.
- **It is small and close to the machine.** C gives you a thin layer over the hardware. There is very little "magic" happening behind your back. This makes it an *excellent teacher*: learning C teaches you how computers actually work — memory, addresses, bytes.
- **It is fast.** Because it is close to the hardware and does little automatically, C programs are among the fastest you can write.
- **It is foundational.** The syntax of C (curly braces `{}`, semicolons `;`, `if`/`for`/`while`) was borrowed by C++, Java, JavaScript, C#, Go, Rust, and many more. Learn C and a huge family of languages becomes familiar.

The trade-off: C does **not** hold your hand. It will not stop you from making mistakes (like reading memory you do not own). This is powerful but demands care. That care is exactly the skill this guide builds.

> **Mental model:** Think of C as giving you direct keys to the building. Higher-level languages give you a concierge who fetches things for you. C is more work, but you learn where every room is.

---

## 2. How a C program becomes a running program (compilation)

This is one of the most important ideas for a beginner, so we go slowly.

When you write C, you write **source code** — plain text in a file ending in `.c`. Your computer's processor (CPU) cannot read this text. The CPU only understands **machine code**: raw numbers that represent instructions.

A **compiler** is a program that translates your human-readable `.c` file into machine code that the CPU can execute. The most common C compilers are **GCC** (the GNU Compiler Collection) and **Clang**.

The journey from text to running program has four stages. You usually run them all with one command, but it helps to know they exist:

```
your_code.c
     │
     ▼  (1) PREPROCESSING — handles lines starting with #
expanded code
     │
     ▼  (2) COMPILATION — translates C into assembly for your CPU
assembly code
     │
     ▼  (3) ASSEMBLY — turns assembly into machine code "object files" (.o)
object file
     │
     ▼  (4) LINKING — combines object files + libraries into one executable
   a.out  (the runnable program)
```

1. **Preprocessing:** Lines that start with `#` (like `#include <stdio.h>`) are instructions to the *preprocessor*, not the compiler proper. `#include` literally pastes the contents of another file into yours. This is how your program gains access to ready-made functions like `printf`.
2. **Compilation:** The expanded C code is translated into *assembly*, a low-level human-readable form of machine instructions.
3. **Assembly:** The assembly is turned into binary **object code** stored in `.o` files.
4. **Linking:** Object files are stitched together with **libraries** (collections of pre-written, already-compiled code) to produce a single **executable** file you can run.

The key takeaway: **editing the `.c` file changes nothing until you recompile.** Beginners are often confused when their changes "do nothing" — it is almost always because they forgot to recompile.

---

## 3. Setting up your tools

You need two things: a **text editor** (to write code) and a **compiler** (to build it).

### The compiler

**On Linux (Debian/Ubuntu):**
```bash
sudo apt update
sudo apt install build-essential
```
`build-essential` includes GCC, the `make` tool, and standard libraries.

**On macOS:**
```bash
xcode-select --install
```
This installs Clang, which responds to the `gcc` command too.

**On Windows:**
The easiest path is **WSL** (Windows Subsystem for Linux), then follow the Linux steps. Alternatively install **MinGW-w64** or **MSYS2** to get GCC natively.

### Verify it works

Open a terminal and type:
```bash
gcc --version
```
If you see version information rather than "command not found", you are ready.

### A text editor

Anything that saves plain text works: VS Code, Vim, Nano, Emacs, Sublime Text. Avoid word processors like Microsoft Word — they add invisible formatting that breaks source files. For beginners, **VS Code** with the official C/C++ extension is a comfortable choice.

### The compile-and-run cycle

Every time you change your code, you repeat this loop:

```bash
gcc hello.c -o hello   # compile hello.c into an executable named "hello"
./hello                # run the executable
```

Breaking down `gcc hello.c -o hello`:
- `gcc` — invoke the compiler.
- `hello.c` — your source file (the input).
- `-o hello` — the `-o` flag means "**o**utput". The next word, `hello`, is the name of the executable to produce. Without `-o`, GCC names the output `a.out` by default.
- `./hello` — run it. The `./` means "in the current directory". It is required because, for safety, the shell does not search the current folder for programs automatically.

**Useful flags to add from day one:**
```bash
gcc -Wall -Wextra -std=c17 hello.c -o hello
```
- `-Wall` — turn on **all** the common **warnings**. Warnings catch likely mistakes that are not technically errors. *Always use this.*
- `-Wextra` — even more warnings.
- `-std=c17` — use the 2017 version of the C standard, so the compiler agrees with modern C rules.

> **Golden rule:** Treat warnings as errors to be fixed, not noise to be ignored. A warning today is a crash tomorrow.

---

## 4. Your first program, line by line

Create a file called `hello.c` and type this in **exactly**:

```c
#include <stdio.h>

int main(void)
{
    printf("Hello, world!\n");
    return 0;
}
```

Compile and run it:
```bash
gcc -Wall hello.c -o hello
./hello
```
You should see:
```
Hello, world!
```

Now we dissect **every single piece**, because understanding this tiny program teaches you the skeleton every C program shares.

### `#include <stdio.h>`
- The `#` makes this a **preprocessor directive** (recall Section 2).
- `include` tells the preprocessor to paste in the contents of another file.
- `<stdio.h>` is the **st**andar**d** **i**nput/**o**utput **h**eader. It declares functions for printing and reading, including `printf`. The angle brackets `< >` mean "look in the system's standard library folders". (Your own files use quotes: `#include "myfile.h"`.)
- Without this line, the compiler would not know what `printf` is.

### (the blank line)
- Blank lines mean nothing to the compiler. They exist purely to make code readable for humans. Use them to separate logical groups.

### `int main(void)`
- This declares a **function** named `main`. A function is a named block of code.
- `main` is special: it is the **entry point**. When your program runs, execution *always* begins at `main`. Every C program must have exactly one.
- `int` before the name is the **return type**. It promises that `main` will hand back an integer when it finishes.
- `(void)` is the **parameter list**. `void` here means "this function takes no inputs". (You will later see `main(int argc, char **argv)` which receives command-line arguments — ignore that for now.)

### `{` and `}`
- Curly braces delimit a **block** — the body of the function. Everything between `{` and `}` belongs to `main`. Braces always come in pairs.

### `printf("Hello, world!\n");`
- `printf` means "**print** **f**ormatted". It writes text to the screen.
- The text inside double quotes `"..."` is a **string literal** — a sequence of characters.
- `\n` is an **escape sequence** meaning **newline**. It moves the cursor to the next line, like pressing Enter. The backslash `\` starts the escape; `n` is the code for newline. Without it, the next output would jam onto the same line.
- The line ends with a **semicolon** `;`. In C, *almost every statement ends with a semicolon*. It is how the compiler knows one instruction has ended. Forgetting it is the #1 beginner error.

### `return 0;`
- This ends `main` and hands the value `0` back to the operating system.
- By convention, `0` means "**success — everything went fine**". Any non-zero number signals an error. This is how scripts and other programs know whether yours worked.

### Recap skeleton
Almost every program you write starts from this shape:
```c
#include <stdio.h>

int main(void)
{
    /* your code goes here */
    return 0;
}
```

> `/* ... */` is a **comment** — text the compiler ignores, written for humans. You can also use `//` for a single-line comment that runs to the end of the line.

---

## 5. Variables and data types

A **variable** is a named box in memory that holds a value. Before you can use a variable you must **declare** it, which means telling the compiler its **type** (what kind of data it holds) and its **name**.

```c
int age = 30;
```
Read this as: "create an `int` (integer) variable called `age`, and put the value `30` in it." Breaking it down:
- `int` — the type.
- `age` — the name you chose.
- `=` — the **assignment** operator: "put the value on the right into the box on the left".
- `30` — the initial value.
- `;` — end of statement.

You can declare without assigning, then assign later:
```c
int score;        /* declared, but holds garbage until set */
score = 100;      /* now it holds 100 */
```
> **Warning:** A declared-but-not-assigned variable contains *whatever random bytes were already in that memory*. Reading it before assigning is a classic bug. Always initialize.

### The core built-in types

| Type | What it stores | Example | Typical size |
|------|----------------|---------|--------------|
| `int` | Whole numbers | `-42`, `0`, `1000` | 4 bytes |
| `unsigned int` | Whole numbers, **no negatives** | `0`, `4000000000` | 4 bytes |
| `long` | Bigger whole numbers | `9000000000L` | 8 bytes |
| `float` | Decimal numbers (single precision) | `3.14f` | 4 bytes |
| `double` | Decimal numbers (more precise) | `3.14159265` | 8 bytes |
| `char` | A single character / small integer | `'A'`, `'\n'` | 1 byte |
| `_Bool` / `bool` | True or false | `true`, `false` | 1 byte |

Notes:
- A `char` holds **one** character, written in **single** quotes: `'A'`. Do not confuse `'A'` (a single character) with `"A"` (a string — see Section 11). Internally a `char` is just a small number; `'A'` is really the number 65 (its ASCII code).
- `float` and `double` store **floating-point** numbers. Prefer `double` unless you have a specific reason; it is more accurate.
- For `bool`, `true`, and `false` you must `#include <stdbool.h>`.

### Constants

If a value should never change, mark it `const`:
```c
const double PI = 3.14159265;
```
The compiler will now reject any attempt to modify `PI`. This documents intent and prevents accidents.

### Naming rules
- Names may contain letters, digits, and underscores `_`, but may **not start with a digit**.
- Names are **case-sensitive**: `age`, `Age`, and `AGE` are three different variables.
- Choose descriptive names. `numberOfStudents` beats `n` once code grows.

---

## 6. Operators and expressions

An **operator** combines values to produce a new value. A combination of values and operators is an **expression** (e.g., `2 + 3` is an expression that evaluates to `5`).

### Arithmetic operators
```c
int a = 7, b = 2;
a + b   /* 9  addition */
a - b   /* 5  subtraction */
a * b   /* 14 multiplication */
a / b   /* 3  division — NOTE: integer division truncates! */
a % b   /* 1  modulo: the remainder of a / b */
```
**The integer-division trap:** When both operands are integers, `/` throws away the fractional part. `7 / 2` is `3`, not `3.5`. To get `3.5`, at least one operand must be floating point: `7.0 / 2` gives `3.5`. This surprises every beginner at least once.

### Assignment and shortcuts
```c
int x = 10;
x += 5;   /* same as x = x + 5;  → 15 */
x -= 3;   /* x = x - 3           → 12 */
x *= 2;   /* x = x * 2           → 24 */
x /= 4;   /* x = x / 4           → 6  */
x++;      /* increment by 1      → 7  */
x--;      /* decrement by 1      → 6  */
```

### Comparison operators (result is true/1 or false/0)
```c
a == b   /* equal to       — NOTE the DOUBLE equals */
a != b   /* not equal to */
a <  b   /* less than */
a >  b   /* greater than */
a <= b   /* less than or equal */
a >= b   /* greater than or equal */
```
**The `=` vs `==` trap:** A single `=` *assigns*; a double `==` *compares*. Writing `if (x = 5)` instead of `if (x == 5)` is a famous bug — it assigns 5 to x and is always "true". `-Wall` warns you about this.

### Logical operators (combine true/false values)
```c
(a > 0) && (b > 0)   /* AND — true only if BOTH are true */
(a > 0) || (b > 0)   /* OR  — true if EITHER is true */
!(a > 0)             /* NOT — flips true/false */
```

### Operator precedence
Like maths, `*` and `/` happen before `+` and `-`. When unsure, **use parentheses** to make order explicit and readable:
```c
int result = 2 + 3 * 4;        /* 14, because * goes first */
int clear  = 2 + (3 * 4);      /* same, but obvious to a reader */
```

---

## 7. Input and output

You already met `printf` for output. Now we learn to print *variables* and to *read* input from the user. Both use **format specifiers** — placeholders starting with `%` that say "insert a value of this type here".

### Printing variables with `printf`
```c
int   age    = 30;
double height = 1.82;
char  grade  = 'A';

printf("Age: %d\n", age);
printf("Height: %.2f metres\n", height);
printf("Grade: %c\n", grade);
printf("%s is %d years old.\n", "Sam", age);
```
Output:
```
Age: 30
Height: 1.82 metres
Grade: A
Sam is 30 years old.
```
How it works: `printf` scans the string left to right, printing characters until it hits a `%`. At each `%` it pulls the next argument (after the string) and formats it.

| Specifier | Used for | 
|-----------|----------|
| `%d` | `int` (decimal integer) |
| `%f` | `float` / `double` |
| `%.2f` | floating point with 2 decimal places |
| `%c` | a single `char` |
| `%s` | a string |
| `%lf` | `double` (when reading with `scanf`) |
| `%x` | integer in hexadecimal |
| `%%` | a literal percent sign |

> The **order matters**: the first `%` matches the first argument after the string, the second `%` the second argument, and so on. A mismatch between specifier and argument type causes wrong output or crashes — and `-Wall` warns you.

### Reading input with `scanf`
`scanf` ("scan formatted") reads typed input into variables. The twist that trips up everyone: you must pass the **address** of the variable using `&` (the "address-of" operator — much more on this in Section 12).

```c
int age;
printf("Enter your age: ");
scanf("%d", &age);            /* note the & before age */
printf("Next year you will be %d.\n", age + 1);
```
Why the `&`? `scanf` needs to *store* a value into your variable, so it needs to know *where the variable lives* in memory. `&age` means "the address of age". Forgetting the `&` is the #1 `scanf` bug and usually crashes the program.

> **A note on `scanf` and strings:** Reading whole lines of text safely is surprisingly tricky with `scanf`. For real programs prefer `fgets`. We keep to numbers and single values here to stay beginner-friendly.

---

## 8. Control flow: if, else, switch

By default, statements run top to bottom, one after another. **Control flow** lets your program make decisions and choose different paths.

### `if`
```c
int temperature = 30;

if (temperature > 25) {
    printf("It is hot.\n");
}
```
- The **condition** in parentheses `( )` is evaluated. If it is true (non-zero), the block in `{ }` runs. If false, the block is skipped.

### `if` / `else`
```c
if (temperature > 25) {
    printf("It is hot.\n");
} else {
    printf("It is not hot.\n");
}
```
Exactly one of the two blocks runs.

### `if` / `else if` / `else` chains
```c
if (score >= 90) {
    printf("Grade A\n");
} else if (score >= 80) {
    printf("Grade B\n");
} else if (score >= 70) {
    printf("Grade C\n");
} else {
    printf("Fail\n");
}
```
Conditions are checked **top to bottom**. The first true one runs, and the rest are skipped. Order matters: if you checked `>= 70` first, an A student would be labelled C.

> **Always use braces `{ }`**, even for a single statement. Omitting them is legal but a notorious source of bugs (the "goto fail" security flaw was caused by exactly this).

### `switch`
When you compare one variable against many fixed values, `switch` is cleaner:
```c
int day = 3;

switch (day) {
    case 1:
        printf("Monday\n");
        break;
    case 2:
        printf("Tuesday\n");
        break;
    case 3:
        printf("Wednesday\n");
        break;
    default:
        printf("Another day\n");
        break;
}
```
- `switch (day)` evaluates `day` once and jumps to the matching `case`.
- `break` exits the switch. **Without `break`, execution "falls through"** into the next case — a real trap. Almost always you want a `break` at the end of each case.
- `default` runs if no case matched (like a final `else`).

---

## 9. Loops: while, do-while, for

A **loop** repeats a block of code. This is what computers are best at: doing the same thing many times, quickly.

### `while` — "repeat as long as a condition holds"
```c
int i = 1;
while (i <= 5) {
    printf("%d ", i);
    i++;                /* CRUCIAL: move toward ending the loop */
}
/* prints: 1 2 3 4 5 */
```
- Before each pass, the condition `i <= 5` is checked. If true, the body runs; if false, the loop ends.
- **You must change something inside the loop** that eventually makes the condition false. Forgetting `i++` here gives an **infinite loop** that runs forever. (Press `Ctrl+C` to stop a runaway program.)

### `do-while` — "run once, then repeat while a condition holds"
```c
int n;
do {
    printf("Enter a positive number: ");
    scanf("%d", &n);
} while (n <= 0);
```
The body runs **before** the check, so it always executes **at least once**. Useful for input validation like the above.

### `for` — the compact counting loop
```c
for (int i = 0; i < 5; i++) {
    printf("%d ", i);
}
/* prints: 0 1 2 3 4 */
```
The `for` header has **three parts separated by semicolons**:
1. **Initialization** `int i = 0` — runs once, before the loop starts.
2. **Condition** `i < 5` — checked before each pass; loop continues while true.
3. **Update** `i++` — runs after each pass.

A `for` loop is just a `while` loop with all three parts gathered in one tidy place. Prefer `for` when you are counting a known number of times.

### Controlling loops: `break` and `continue`
```c
for (int i = 0; i < 10; i++) {
    if (i == 5) break;       /* leave the loop entirely */
    if (i % 2 == 0) continue;/* skip to the next iteration */
    printf("%d ", i);
}
/* prints: 1 3  (stops at 5, skips evens) */
```
- `break` exits the loop immediately.
- `continue` skips the rest of the current pass and jumps to the next one.

### Nested loops
Loops can contain loops. This prints a small grid:
```c
for (int row = 1; row <= 3; row++) {
    for (int col = 1; col <= 3; col++) {
        printf("(%d,%d) ", row, col);
    }
    printf("\n");           /* newline after each row */
}
```

---

## 10. Functions

A **function** is a reusable, named block of code. You have used `main`, `printf`, and `scanf`. Now you write your own. Functions let you **avoid repetition** and **break a big problem into small, named pieces**.

### Anatomy of a function
```c
int add(int a, int b)
{
    int sum = a + b;
    return sum;
}
```
- `int` (before the name) — the **return type**: what kind of value the function gives back.
- `add` — the **name**.
- `(int a, int b)` — the **parameters**: inputs the function receives. Each has a type and a name.
- `{ ... }` — the **body**.
- `return sum;` — hands `sum` back to whoever called the function, and ends it.

### Calling a function
```c
int main(void)
{
    int result = add(3, 4);   /* result becomes 7 */
    printf("3 + 4 = %d\n", result);
    return 0;
}
```
`add(3, 4)` runs `add` with `a = 3` and `b = 4`, and the whole expression evaluates to the returned value, `7`.

### Functions that return nothing: `void`
If a function does work but has no value to return, its return type is `void`:
```c
void greet(const char *name)
{
    printf("Hello, %s!\n", name);
    /* no return value needed */
}
```

### Declaration order and prototypes
C reads your file top to bottom. A function must be **known** before it is called. Two ways to satisfy this:

1. **Define it above** `main`. Simple for small programs.
2. **Declare a prototype** at the top, define it later. A prototype is the function's header followed by a semicolon:
```c
#include <stdio.h>

int add(int a, int b);          /* prototype: "this function exists" */

int main(void)
{
    printf("%d\n", add(2, 5));
    return 0;
}

int add(int a, int b)           /* the actual definition, lower down */
{
    return a + b;
}
```
Prototypes let you organize code naturally (with `main` at the top) while keeping the compiler happy.

### Pass-by-value (important!)
When you pass a variable to a function, C passes a **copy** of its value. Changes inside the function do **not** affect the original:
```c
void tryToChange(int x) { x = 999; }

int main(void)
{
    int n = 5;
    tryToChange(n);
    printf("%d\n", n);   /* still 5! the function changed only its copy */
    return 0;
}
```
To let a function modify the caller's variable, you must pass its **address** with a pointer — which is exactly what Section 12 is for.

---

## 11. Arrays and strings

### Arrays
An **array** is a fixed-size sequence of values **of the same type**, stored back-to-back in memory.
```c
int scores[5];                       /* room for 5 ints */
scores[0] = 90;                      /* first element  */
scores[1] = 85;
scores[4] = 60;                      /* last element   */
```
Or declare and fill at once:
```c
int scores[5] = {90, 85, 70, 65, 60};
```
**Indexing starts at 0.** An array of size 5 has valid indices `0, 1, 2, 3, 4`. The first element is `scores[0]`; the last is `scores[4]`, **not** `scores[5]`.

> **The most dangerous trap in C:** C does **not** check array bounds. Writing `scores[5]` or `scores[100]` does not produce an error — it silently reads or corrupts memory you do not own. This causes crashes and security holes. *You* are responsible for staying in range.

Loop over an array with a `for` loop:
```c
int sum = 0;
for (int i = 0; i < 5; i++) {
    sum += scores[i];
}
printf("Total: %d\n", sum);
```

### Strings are arrays of `char`
C has no dedicated string type. A **string is just an array of `char`** ending with a special **null terminator** `'\0'` (a byte with value 0) that marks where the text stops.
```c
char name[6] = "Sam";
```
In memory this is: `'S'` `'a'` `'m'` `'\0'` and two unused bytes. The `'\0'` is added automatically for string literals. It is why `"Sam"` (3 visible letters) needs at least 4 bytes. Functions like `printf("%s", ...)` print characters until they hit the `'\0'`.

> A missing `'\0'` means string functions keep reading past your data into random memory — another classic bug.

### Working with strings: `<string.h>`
The standard library `<string.h>` provides string helpers:
```c
#include <string.h>

char greeting[20] = "Hello";
printf("Length: %zu\n", strlen(greeting));   /* 5 — counts up to '\0' */
strcat(greeting, ", world");                 /* append → "Hello, world" */
strcpy(greeting, "Restart");                 /* copy/overwrite */
if (strcmp(greeting, "Restart") == 0) {      /* compare; 0 means equal */
    printf("They match.\n");
}
```
- `strlen` — length, not counting `'\0'`.
- `strcat` — concatenate (append) one string onto another.
- `strcpy` — copy a string into a buffer.
- `strcmp` — compare two strings; returns `0` when equal. **Do not** compare strings with `==` (that compares addresses, not contents).

> **Buffer size matters:** the destination array must be big enough to hold the result *plus* the `'\0'`. Overflowing it corrupts memory. This is why functions like `strncat`/`strncpy` (which take a size limit) are preferred in serious code.

---

## 12. Pointers (the scary part, made simple)

Pointers have a fearsome reputation. They are actually a simple idea: **a pointer is a variable that stores a memory address** — the *location* of another variable, rather than a value itself.

Picture memory as a long street of numbered houses (addresses). A normal variable is a house with something inside. A pointer is a slip of paper with a house number written on it.

### Two operators
- `&x` — the **address-of** operator. Gives the address where `x` lives. ("Where is x?")
- `*p` — the **dereference** operator. Gives the value stored at the address `p` holds. ("What is at the place p points to?")

### Declaring and using a pointer
```c
int  x = 42;
int *p = &x;          /* p holds the ADDRESS of x */

printf("%d\n", x);    /* 42  — the value of x */
printf("%p\n", (void *)p);  /* e.g. 0x7ffd... — the address */
printf("%d\n", *p);   /* 42  — follow p to reach x's value */

*p = 100;             /* change x THROUGH the pointer */
printf("%d\n", x);    /* 100 — x changed! */
```
- `int *p` declares a pointer to an `int`. The `*` in the declaration means "this is a pointer".
- `p = &x` stores x's address in p.
- `*p` reads or writes the value at that address. Assigning `*p = 100` changes `x` itself, because `p` points to `x`.

### Why pointers matter: letting functions modify their caller's variables
Recall Section 10: functions get *copies*. Pointers solve this. Pass the **address**, and the function can change the original:
```c
void addTen(int *n)       /* receives an ADDRESS */
{
    *n = *n + 10;         /* modify the value at that address */
}

int main(void)
{
    int value = 5;
    addTen(&value);       /* pass the address of value */
    printf("%d\n", value);/* 15 — really changed this time! */
    return 0;
}
```
This is exactly why `scanf("%d", &age)` needs the `&`: it passes the address so `scanf` can store into your variable.

### Arrays and pointers are close cousins
The name of an array, used by itself, acts as the address of its first element. This is why arrays passed to functions can be modified by them — you are really passing a pointer.

### The null pointer
A pointer that points to nothing is set to `NULL`:
```c
int *p = NULL;
```
Always check a pointer is not `NULL` before dereferencing it. **Dereferencing `NULL` (or an uninitialized pointer) crashes your program** — the dreaded "segmentation fault". A pointer you have not set points to a random address; never dereference it.

> **Beginner survival rules for pointers:**
> 1. Always initialize pointers (to a real address or to `NULL`).
> 2. Never dereference `NULL` or an uninitialized pointer.
> 3. `&` makes an address; `*` follows one. They are opposites.

---

## 13. Structs

A **struct** (structure) groups several related variables into one custom type. Where an array holds many values of the *same* type, a struct holds values of *different* types under one name.

```c
struct Point {
    int x;
    int y;
};
```
This defines a new type, `struct Point`, with two **members** (also called fields): `x` and `y`. Use it like any type:
```c
struct Point p;
p.x = 3;            /* the dot . accesses a member */
p.y = 7;
printf("(%d, %d)\n", p.x, p.y);
```
Initialize all members at once:
```c
struct Point q = {10, 20};   /* x = 10, y = 20 */
```

### Structs with mixed types
```c
struct Person {
    char  name[50];
    int   age;
    double height;
};

struct Person alice = {"Alice", 30, 1.70};
printf("%s is %d.\n", alice.name, alice.age);
```

### `typedef` — a shorter name
Writing `struct Point` everywhere is verbose. `typedef` creates an alias:
```c
typedef struct {
    int x;
    int y;
} Point;

Point p = {1, 2};    /* now just "Point", no "struct" keyword */
```

### Pointers to structs and the `->` operator
When you have a *pointer* to a struct, use `->` instead of `.` to reach a member:
```c
Point p = {1, 2};
Point *ptr = &p;
printf("%d\n", ptr->x);    /* ptr->x means (*ptr).x */
ptr->x = 99;               /* modifies p.x through the pointer */
```
`ptr->x` is just convenient shorthand for `(*ptr).x`: dereference, then access the member. You will use `->` constantly when passing structs to functions (passing the address avoids copying the whole struct).

---

## 14. Memory: the stack, the heap, and malloc

C gives you direct control over memory. Programs use two main regions:

### The stack
- Where your **local variables** live (variables declared inside functions).
- Managed **automatically**: memory is reserved when a function is entered and released when it returns.
- Fast, but **limited in size** and **short-lived** — a local variable vanishes when its function returns.

### The heap
- A large pool of memory you manage **manually**.
- Use it when you need memory that **outlives a function**, or whose **size you do not know until runtime** (e.g., an array sized by user input).

### Allocating heap memory: `malloc` and `free`
```c
#include <stdlib.h>     /* for malloc and free */

int n = 5;
int *arr = malloc(n * sizeof(int));   /* ask for room for n ints */

if (arr == NULL) {                    /* ALWAYS check it succeeded */
    printf("Out of memory!\n");
    return 1;
}

for (int i = 0; i < n; i++) {
    arr[i] = i * i;                   /* use it like a normal array */
}

free(arr);              /* give the memory back when done */
arr = NULL;             /* avoid accidentally reusing freed memory */
```
Breaking it down:
- `malloc(size)` — **m**emory **alloc**ate. Asks the system for `size` **bytes** and returns a pointer to the first byte (or `NULL` if it failed).
- `sizeof(int)` — gives the size of one `int` in bytes (usually 4). `n * sizeof(int)` is the room for `n` ints. Always compute sizes with `sizeof` rather than guessing.
- `free(arr)` — returns the memory to the system so it can be reused.

### The two cardinal sins of heap memory
1. **Memory leak:** allocating with `malloc` but never calling `free`. The memory stays reserved forever (until the program ends), wasting it. In long-running programs this is fatal.
2. **Use-after-free / double-free:** using or freeing memory you already freed. This corrupts your program and is a major source of security vulnerabilities.

> **Rule:** every `malloc` must be matched by exactly one `free`. Set the pointer to `NULL` after freeing so mistakes become obvious (dereferencing `NULL` crashes loudly; dereferencing freed memory fails silently and unpredictably).

> **Tip:** Run your program under a memory checker like **Valgrind** (`valgrind ./myprogram`) to catch leaks and invalid accesses automatically. It is one of the best tools a C programmer has.

---

## 15. Coding style and indentation

The compiler ignores whitespace, but **humans do not**. Consistent style makes bugs easier to spot and code easier to share. None of the following is enforced by the compiler — it is enforced by professionalism.

### Indentation
**Indentation** means adding leading spaces so that code *inside* a block sits to the right of the code that contains it. It visually reveals the structure.

Bad (legal, but unreadable):
```c
int main(void){
int total=0;
for(int i=0;i<10;i++){
if(i%2==0){
total+=i;
}}
printf("%d\n",total);
return 0;}
```
Good (same program, properly indented):
```c
int main(void)
{
    int total = 0;

    for (int i = 0; i < 10; i++) {
        if (i % 2 == 0) {
            total += i;
        }
    }

    printf("%d\n", total);
    return 0;
}
```
Notice how each `{` opens a new level and everything inside is pushed one step right. The matching `}` returns to the previous level. You can *see* what belongs to the loop and what belongs to the `if`.

### Conventions worth adopting
- **One indent level per block.** Use a consistent width — **4 spaces** is the most common. Pick spaces or tabs and never mix them.
- **Spaces around operators:** `a + b`, not `a+b`. `for (int i = 0; ...)`, not `for(int i=0;...)`.
- **One statement per line.**
- **Braces on every block,** even single-statement `if`s (see Section 8).
- **Descriptive names:** `studentCount`, not `sc`.
- **Comment the *why*, not the obvious *what*.** `i++; // move to next student` is noise; explain reasoning the code cannot show.
- **Keep functions short** — if a function does not fit on a screen, consider splitting it.

### Automatic formatters
Tools like **`clang-format`** reformat your code to a consistent style instantly:
```bash
clang-format -i myfile.c     /* -i edits the file in place */
```
Let the tool handle layout so you can focus on logic.

---

## 16. The 5 Exercises

Work through these in order — each uses ideas from the one before. **Try to solve each yourself first.** A full worked solution follows every problem, but you learn by struggling, not by reading.

For each exercise: create a `.c` file, compile with `gcc -Wall -Wextra -std=c17 file.c -o file`, and run with `./file`.

---

### Exercise 1 — Hello, *you* (variables + output)

**Goal:** Print a personalized greeting using variables.

**Task:** Declare a string for your name and an `int` for your age. Print a sentence like: `Hi, my name is Alex and I am 25 years old.`

**Concepts:** variables (Section 5), `printf` and format specifiers (Section 7).

<details>
<summary>Show solution</summary>

```c
#include <stdio.h>

int main(void)
{
    char name[] = "Alex";   /* let the compiler size the array */
    int  age    = 25;

    printf("Hi, my name is %s and I am %d years old.\n", name, age);
    return 0;
}
```
**Why it works:** `%s` is replaced by the string `name`, `%d` by the integer `age`. The arguments line up with the specifiers in order. `char name[]` lets the compiler count the letters and add the `'\0'` automatically.
</details>

---

### Exercise 2 — Even or odd (input + conditionals)

**Goal:** Read a number and decide whether it is even or odd.

**Task:** Ask the user for an integer with `scanf`. Use the modulo operator `%` to test it. Print `"even"` or `"odd"`.

**Concepts:** `scanf` and `&` (Section 7), `if`/`else` (Section 8), `%` operator (Section 6).

<details>
<summary>Show solution</summary>

```c
#include <stdio.h>

int main(void)
{
    int number;

    printf("Enter an integer: ");
    scanf("%d", &number);            /* & passes number's address */

    if (number % 2 == 0) {           /* remainder 0 → divisible by 2 */
        printf("%d is even.\n", number);
    } else {
        printf("%d is odd.\n", number);
    }
    return 0;
}
```
**Why it works:** `number % 2` is the remainder after dividing by 2. It is `0` for even numbers and `1` for odd ones. Note `==` (comparison), not `=` (assignment).
</details>

---

### Exercise 3 — Sum and average of N numbers (loops + arrays)

**Goal:** Read several numbers, then report their sum and average.

**Task:** Ask how many numbers the user has (call it `n`). Read `n` numbers into an array using a loop. Compute and print the sum and the average. *(Assume `n` is at most 100.)*

**Concepts:** arrays (Section 11), `for` loops (Section 9), integer-vs-float division (Section 6).

<details>
<summary>Show solution</summary>

```c
#include <stdio.h>

int main(void)
{
    int numbers[100];
    int n;

    printf("How many numbers? ");
    scanf("%d", &n);

    if (n < 1 || n > 100) {              /* guard against bad input */
        printf("Please enter between 1 and 100.\n");
        return 1;                        /* non-zero = error exit */
    }

    int sum = 0;
    for (int i = 0; i < n; i++) {
        printf("Number %d: ", i + 1);
        scanf("%d", &numbers[i]);        /* store into slot i */
        sum += numbers[i];
    }

    double average = (double)sum / n;    /* cast to avoid integer division */

    printf("Sum: %d\n", sum);
    printf("Average: %.2f\n", average);
    return 0;
}
```
**Why it works:** The loop runs `n` times, reading into `numbers[i]` and adding to `sum`. The crucial detail is `(double)sum` — this **cast** converts `sum` to a `double` *before* dividing, so we get a real average (e.g., `3.50`) instead of truncated integer division. `%.2f` prints two decimal places.
</details>

---

### Exercise 4 — A reusable function: is it prime? (functions + loops)

**Goal:** Write a function that tests whether a number is prime, and use it.

**Task:** Write `int isPrime(int n)` that returns `1` if `n` is prime and `0` otherwise. In `main`, print all prime numbers from 2 to 50 using this function.

**Concepts:** functions (Section 10), return values, loops (Section 9), `break` (Section 9).

<details>
<summary>Show solution</summary>

```c
#include <stdio.h>

/* Returns 1 if n is prime, 0 otherwise. */
int isPrime(int n)
{
    if (n < 2) {
        return 0;                 /* 0 and 1 are not prime */
    }
    for (int divisor = 2; divisor * divisor <= n; divisor++) {
        if (n % divisor == 0) {   /* found a factor → not prime */
            return 0;
        }
    }
    return 1;                     /* no factors found → prime */
}

int main(void)
{
    printf("Primes from 2 to 50:\n");
    for (int i = 2; i <= 50; i++) {
        if (isPrime(i)) {
            printf("%d ", i);
        }
    }
    printf("\n");
    return 0;
}
```
**Why it works:** A prime has no divisors other than 1 and itself. We only need to test divisors up to the square root of `n` — that is what `divisor * divisor <= n` checks (a clever way to avoid computing a square root). The instant we find any factor we `return 0` early. If the loop finishes with no factor, the number is prime, so we `return 1`. In `main`, `if (isPrime(i))` uses the returned 1/0 directly as a true/false condition.
</details>

---

### Exercise 5 — A dynamic to-do list (structs + pointers + heap memory)

**Goal:** Combine everything: store a list of tasks whose size is chosen at runtime, using a struct and heap allocation.

**Task:** Define a `Task` struct holding a description (`char[100]`) and a `done` flag (`int`). Ask the user how many tasks they have, `malloc` an array of that many `Task`s, read each description, then print the list. Free the memory at the end.

**Concepts:** structs (Section 13), `malloc`/`free` (Section 14), pointers (Section 12), `fgets` for safe text input.

<details>
<summary>Show solution</summary>

```c
#include <stdio.h>
#include <stdlib.h>     /* malloc, free */
#include <string.h>     /* strcspn */

typedef struct {
    char description[100];
    int  done;
} Task;

int main(void)
{
    int count;

    printf("How many tasks? ");
    scanf("%d", &count);
    getchar();                       /* consume the leftover newline */

    if (count < 1) {
        printf("Nothing to do!\n");
        return 0;
    }

    /* Allocate room for `count` Task structs on the heap. */
    Task *tasks = malloc(count * sizeof(Task));
    if (tasks == NULL) {             /* always check malloc */
        printf("Out of memory.\n");
        return 1;
    }

    /* Read each task description. */
    for (int i = 0; i < count; i++) {
        printf("Task %d description: ", i + 1);
        fgets(tasks[i].description, 100, stdin);     /* safe line read */
        tasks[i].description[strcspn(tasks[i].description, "\n")] = '\0';
        tasks[i].done = 0;           /* start as not done */
    }

    /* Print the list. */
    printf("\nYour to-do list:\n");
    for (int i = 0; i < count; i++) {
        printf("  [%c] %s\n",
               tasks[i].done ? 'x' : ' ',
               tasks[i].description);
    }

    free(tasks);                     /* release the heap memory */
    tasks = NULL;
    return 0;
}
```
**Why it works, piece by piece:**
- `typedef struct { ... } Task;` defines our custom type bundling a description and a done-flag.
- `malloc(count * sizeof(Task))` reserves exactly enough heap memory for `count` tasks, regardless of how many the user wants — something a fixed array cannot do. We check the result against `NULL`.
- `getchar()` after `scanf` discards the leftover newline character so the first `fgets` is not skipped — a classic mixing-`scanf`-and-`fgets` gotcha.
- `fgets(..., 100, stdin)` reads a whole line safely without overflowing the 100-byte buffer (unlike `scanf("%s")`, which stops at spaces and can overflow).
- `strcspn(..., "\n")` finds the position of the newline `fgets` keeps, and we overwrite it with `'\0'` to trim it.
- `tasks[i].done ? 'x' : ' '` is the **ternary operator**: `condition ? valueIfTrue : valueIfFalse`. It prints `x` for done tasks, a space otherwise.
- `free(tasks)` returns the memory; setting `tasks = NULL` prevents accidental reuse. Every `malloc` is matched by one `free` — no leak.

Run it under Valgrind to confirm zero leaks:
```bash
gcc -Wall -Wextra -g -std=c17 todo.c -o todo
valgrind ./todo
```
</details>

---

## 17. Where to go next

You now know the core of C: types, operators, I/O, control flow, loops, functions, arrays, strings, pointers, structs, and manual memory management. That is genuinely most of the language — C is small. Mastery comes from *practice*, not more features.

Suggested next steps:
- **Build small programs:** a calculator, a number-guessing game, a word counter, a simple linked list.
- **Read** *The C Programming Language* by Kernighan & Ritchie ("K&R") — the classic, written by C's creators.
- **Learn the toolchain deeper:** `make`/`Makefile`s to automate builds, `gdb` to debug interactively, `valgrind` for memory checking.
- **Explore the standard library:** `<math.h>`, `<time.h>`, file I/O with `fopen`/`fclose`/`fprintf`.
- **Study data structures in C:** linked lists, stacks, queues, trees — implementing these by hand cements pointers and memory.

**The single most important habit:** compile with `-Wall -Wextra`, read every warning, and never ignore one. C trusts you completely — earn that trust with care.

Happy hacking! 🛠️

