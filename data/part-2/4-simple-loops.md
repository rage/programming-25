---
path: '/part-2/4-simple-loops'
title: 'Simple loops'
hidden: false
---

<text-box variant='learningObjectives' name="Learning objectives">

After this section

- You will know what a loop means in programming
- You will be able to use a `while True` loop in your programs
- You will know how to use the `break` command to break out of a loop

</text-box>

We have now covered conditional structures in some detail. Another central technique in programming is repetition, or _iteration_. Together these form the fundamental control structures any programmer must master. They are called control structures because essentially they allow you to control which lines of code get executed when. While conditional structures allow you to _choose between_ sections of code, iteration structures allow you to _repeat_ sections of code. They are often called _loops_ because they allow the program to "loop back" to some line that was already executed before. The process of executing one repetition of a loop is also referred to as an iteration of the loop.

This section introduces a simple `while` loop. Its structure is similar to the conditional statements we already covered. In the next part we will delve into some more sophisticated examples.

Let's have a look at a program which asks the user to type in a number and then prints out the number squared. This continues until the user types in -1.

```python
while True:
    number = int(input("Please type in a number, -1 to quit: "))

    if number == -1:
        break

    print(number ** 2)

print("Thanks and bye!")
```

Running the program could look like this:

<sample-output>

Please type in a number, -1 to quit: **2**
4
Please type in a number, -1 to quit: **4**
16
Please type in a number, -1 to quit: **10**
100
Please type in a number, -1 to quit: **-1**
Thanks and bye!

</sample-output>

As you can see above, the program asks for several numbers, thanks to the `while` statement in the program. When the user types in -1, the `break` command is executed, which exits the loop and execution continues from the first line after the `while` block.

With loops, it is crucial that there is always a way to exit the loop at some point in the code, otherwise the repetition could go on forever. To illustrate this, let's change the above example a little:

```python
number = int(input("Please type in a number, -1 to quit: "))
while True:
    if number == -1:
        break

    print(number ** 2)

print("Thanks and bye!")
```

In this version the program asks the user to type in a number _outside the loop_. If the user types in any other number than -1, the loop is never exited from. This forms an _infinite loop_, which means the block of code within the loop is repeated endlessly:

<sample-output>

Please type in a number, -1 to quit: **2**
4
4
4
4
4
4
4
4
(continued ad infinitum...)

</sample-output>

The following program has a similar structure to the example above the infinite loop, but the user experience is quite different. This program allows the user to proceed only if they type in the correct PIN _1234_:

```python
while True:
    code = input("Please type in your PIN: ")
    if code == "1234":
        break
    print("Incorrect...try again")

print("Correct PIN entered!")
```

<sample-output>

Please type in your PIN: **0000**
Incorrect...try again
Please type in your PIN: **9999**
Incorrect...try again
Please type in your PIN: **1234**
Correct PIN entered!

</sample-output>

<in-browser-programming-exercise name="Shall we continue?" tmcname="part02-15_shall_we_continue">

Let's create a program along the lines of the example above. This program should print out the message "hi" and then ask "Shall we continue?" until the user inputs "no". Then the program should print out "okay then" and finish. Please have a look at the example below.

<sample-output>

hi
Shall we continue? **yes**
hi
Shall we continue? **oui**
hi
Shall we continue? **jawohl**
hi
Shall we continue? **no**
okay then

</sample-output>

</in-browser-programming-exercise>

<in-browser-programming-exercise name="Input validation" tmcname="part02-16_input_validation">

Please write a program which asks the user for integer numbers.

If the number is below zero, the program should print out the message "Invalid number".

If the number is above zero, the program should print out the square root of the number using the Python `sqrt` function.

In either case, the program should then ask for another number.

If the user inputs the number zero, the program should stop asking for numbers and exit the loop.

Below you'll find a reminder of how the `sqrt` function is used. Remember to `import` it in the beginning of the program.

```python
# sqrt function will not work without this line in the beginning of the program
from math import sqrt

print(sqrt(9))
```

<sample-output>

3.0

</sample-output>

An example of expected behaviour of your program:

<sample-output>

Please type in a number: **16**
4.0
Please type in a number: **4**
2.0
Please type in a number: **-3**
Invalid number
Please type in a number: **1**
1.0
Please type in a number: **0**
Exiting...

</sample-output>

</in-browser-programming-exercise>

<in-browser-programming-exercise name="Fix the code: Countdown" tmcname="part02-17_countdown">

This program should print out a countdown. The code is as follows:

```python
number = 5
print("Countdown!")
while True:
  print(number)
  number = number - 1
  if number > 0:
    break

print("Now!")
```

This should print out

<sample-output>

Countdown!
5
4
3
2
1
Now!

</sample-output>

However, the program doesn't quite work. Please fix it.

</in-browser-programming-exercise>

<in-browser-programming-exercise name="Repeat password" tmcname="part02-18_repeat_password">

Please write a program which asks the user for a password. The program should then ask the user to type in the password again. If the user types in something else than the first password, the program should keep on asking until the user types the first password again correctly.

Have a look at the expected behaviour below:

<sample-output>

Password: **sekred**
Repeat password: **secret**
They do not match!
Repeat password: **cantremember**
They do not match!
Repeat password: **sekred**
User account created!

</sample-output>

</in-browser-programming-exercise>

## Loops and helper variables

Let's make the PIN checking example a bit more realistic. This version gives the user only three attempts at typing in a PIN.

The program uses two helper variables. The variable `attempts` keeps track of how many times the user has typed in a PIN. The variable `success` is set to either `True` or `False` based on whether the user is successful in signing in.

```python
attempts = 0

while True:
    code = input("Please type in your PIN: ")
    attempts += 1

    if code == "1234":
        success = True
        break

    if attempts == 3:
        success = False
        break

    # this is printed if the code was incorrect AND there have been less than three attempts
    print("Incorrect...try again")

if success:
    print("Correct PIN entered!")
else:
    print("Too many attempts...")
```

<sample-output>

Please type in your PIN: **0000**
Incorrect...try again
Please type in your PIN: **1234**
Correct PIN entered!

</sample-output>

<sample-output>

Please type in your PIN: **0000**
Incorrect...try again
Please type in your PIN: **9999**
Incorrect...try again
Please type in your PIN: **4321**
Too many attempts...

</sample-output>

The loop is exited _either_ when the user types the correct PIN _or_ if there have been too many attempts. The `if` statement after the loop checks the value of the variable `success` and prints out a message accordingly.

## Debugging print statements in loops

Adding loops to programs also adds to the potential sources of bugs. It becomes even more important to master the use of debugging print statements as introduced in the [first section of this part](/part-2/1-programming-terminology).

Let's have a look at a program almost identical to the previous example, but with one crucial difference:

```python
attempts = 0

while True:
    code = input("Please type in your PIN: ")
    attempts += 1

    if attempts == 3:
        success = False
        break

    if code == "1234":
        success = True
        break

    print("Incorrect...try again")

if success:
    print("Correct PIN entered!")
else:
    print("Too many attempts...")
```

This version acts strangely when the user types in the correct code on the third attempt:

<sample-output>

Please type in your PIN: **0000**
Incorrect...try again
Please type in your PIN: **9999**
Incorrect...try again
Please type in your PIN: **1234**
Too many attempts...

</sample-output>

So, let's try and find the cause by adding some strategic debugging print statements inside the loop:

```python
while True:
    print("beginning of the while block:")
    code = input("Please type in your PIN: ")
    attempts += 1

    print("attempts:", attempts)
    print("condition1:", attempts == 3)
    if attempts == 3:
        success = False
        break

    print("code:", code)
    print("condition2:", code == "1234")
    if code == "1234":
        success = True
        break

    print("Incorrect...try again")
```

<sample-output>

beginning of the while block:
Please type in your PIN: **2233**
attempts: 1
condition1: False
code: 2233
condition2: False
Incorrect...try again
beginning of the while block:
Please type in your PIN: **4545**
attempts: 2
condition1: False
code: 4545
condition2: False
Incorrect...try again
beginning of the while block:
Please type in your PIN: **1234**
attempts: 3
condition1: True
Too many attempts...

</sample-output>

From the above printouts we can see that during the third iteration of the loop the condition of the first `if` statement is `True`, and the loop is exited. This iteration never gets to the second `if` statement, which checks whether the code was typed in correctly:

```python
  while True:
    # ....

    # this block is executed too early
    if attempts == 3:
        success = False
        break

    # the third iteration never gets this far
    if code == "1234":
        success = True
        break
```

The order of conditional statements, or of different branches within a conditional statement, is a common cause for bugs, especially in loops. Debugging print statements are often the simplest way of finding their cause.

<in-browser-programming-exercise name="PIN and number of attempts" tmcname="part02-19_pin_and_number_of_attempts">

Please write a program which keeps asking the user for a PIN code until they type in the correct one, which is _4321_. The program should then print out the number of times the user tried different codes.

<sample-output>

PIN: **3245**
Wrong
PIN: **1234**
Wrong
PIN: **0000**
Wrong
PIN: **4321**
Correct! It took you 4 attempts

</sample-output>

If the user gets it right on the first try, the program should print out something a bit different:

<sample-output>

PIN: **4321**
Correct! It only took you one single attempt!

</sample-output>

</in-browser-programming-exercise>


<in-browser-programming-exercise name="The next leap year" tmcname="part02-20_next_leap_year">

Please write a program which asks the user for a year, and prints out the next leap year.

<sample-output>

Year: **2023**
The next leap year after 2023 is 2024

</sample-output>

If the user inputs a year which is a leap year (such as 2024), the program should print out the following leap year:

<sample-output>

Year: **2024**
The next leap year after 2024 is 2028

</sample-output>

</in-browser-programming-exercise>

## Concatenating strings with the + operator

The above example with PIN checking used a helper variable `attempts` to keep track of how many times the user had tried to type in a code:

```python
attempts = 0

while True:
    code = input("Please type in your PIN: ")
    attempts += 1
    # ...
```

The variable is set to zero outside the loop, and each iteration increases its value by one.

A similar idea of incrementation works with string variables as well. The program could, for instance, keep track of all the PIN codes the user typed in:

```python

codes = ""
attempts = 0

while True:
    code = input("Please type in your PIN: ")
    attempts += 1
    codes += code + ", "
    # ...
```

The helper variable is initialized to _an empty string_, that is, a string with no characters in it:

```python
codes = ""
```

With each iteration the string gets longer, as the code the user typed in is added, along with a comma:

```python
    code = input("Please type in your PIN: ")
    codes += code + ", "
```

If the user types in the codes _1111 2222 1234_, at the end of the program's execution the value of `codes` would be

<sample-output>

1111, 2222, 1234,

</sample-output>


<in-browser-programming-exercise name="Story" tmcname="part02-21_story">

### Part 1

Please write a program which keeps asking the user for words. If the user types in `end`, the program should print out the story the words formed, and finish.

<sample-output>

Please type in a word: **Once**
Please type in a word: **upon**
Please type in a word: **a**
Please type in a word: **time**
Please type in a word: **there**
Please type in a word: **was**
Please type in a word: **a**
Please type in a word: **girl**
Please type in a word: **end**
Once upon a time there was a girl

</sample-output>

### Part 2

Change the program so that the loop ends also if the user types in the same word twice in a row.

<sample-output>

Please type in a word: **It**
Please type in a word: **was**
Please type in a word: **a**
Please type in a word: **dark**
Please type in a word: **and**
Please type in a word: **stormy**
Please type in a word: **night**
Please type in a word: **night**
It was a dark and stormy night

</sample-output>

</in-browser-programming-exercise>

<in-browser-programming-exercise name="Working with numbers" tmcname="part02-22_working_with_numbers">

### Pre-task

Please write a program which asks the user for integer numbers. The program should keep asking for numbers until the user types in zero.

<sample-output>

Please type in integer numbers. Type in 0 to finish.
Number: **5**
Number: **22**
Number: **9**
Number: **-2**
Number: **0**

</sample-output>

### Part 1: Count

After reading in the numbers the program should print out how many numbers were typed in. The zero at the end should not be included in the count.

You will need a new variable here to keep track of the numbers typed in.

<sample-output>

... the program asks for numbers
Numbers typed in 4

</sample-output>

### Part 2: Sum

The program should also print out the sum of all the numbers typed in. The zero at the end should not be included in the calculation.

The program should now print out the following:

<sample-output>

... the program asks for numbers
Numbers typed in 4
The sum of the numbers is 34

</sample-output>

### Part 3: Mean

The program should also print out the mean of the numbers. The zero at the end should not be included in the calculation. You may assume the user will always type in at least one valid non-zero number.

<sample-output>

... the program asks for numbers
Numbers typed in 4
The sum of the numbers is 34
The mean of the numbers is 8.5

</sample-output>

#### Part 4: Positives and negatives

The program should also print out statistics on how many of the numbers were positive and how many were negative. The zero at the end should not be included in the calculation.

<sample-output>

... the program asks for numbers
Numbers typed in 4
The sum of the numbers is 34
The mean of the numbers is 8.5
Positive numbers 3
Negative numbers 1

</sample-output>

</in-browser-programming-exercise>

<!--

A quiz to review the contents of this section:

<quiz id="63a51999-e525-5f1d-a333-b26392a5585b"></quiz>

-->

Please respond to a quick questionnaire on this week's materials.

<quiz id="5ab8aa39-8d52-5fb7-ac91-253bba93c84b"></quiz>
