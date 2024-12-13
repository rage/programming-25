---
path: '/part-7/6-more-features'
title: 'More Python features'
hidden: false
---

<text-box variant='learningObjectives' name="Learning objectives">

After this section

- You will be familiar with some more Python features

</text-box>

To finish off this course, here you will find various useful Python features.

## Single line conditionals

The following two statements produce the exact same results:

```python
if x%2 == 0:
    print("even")
else:
    print("odd")
```

```python
print("even" if x%2 == 0 else "odd")
```

In the latter example we have a conditional expression on a single line: `a if [condition] else b`. The value of this expression evaluates to `a` if the condition is true, and `b` if it is false. This structure is sometimes referred to as a _ternary operator_.

Conditional expressions can be very useful when you need to assign something _conditionally_. For example, if you had the variables `x` and `y`, and you wanted to either increment or set the value of `y` depending on the parity of `x`, you could write it in a normal `if else` statement, like so:

```python
if x%2 == 0:
    y += 1
else:
    y = 0
```

The same could be achieved with a nifty one-liner:

```python
y = y + 1 if x%2 == 0 else 0
```

## An "empty" block

You may remember from the previous part that you are not allowed to have an empty block in a Python program. If you need to have a block of code which does nothing, for example when testing some other functionality, the `pass` command will let you do this. You could, for instance, write a function which does nothing:

```python
def testing():
    pass
```

This function would simply return immediately. Leaving the `pass` command out, i.e. having a completely empty block, would produce an error.

```python
def testing():  # this causes an error!
```

## Loops with else blocks

In Python, loops can have `else` blocks, too. This section of code is executed if the loop finishes normally.

For example, in the following example we are looking through a list of numbers. If there is an even number on the list, the program prints out a message and the loop is broken. If there are no even numbers, the loop finishes normally, but a different message is then printed out.

```python
my_list = [3,5,2,8,1]
for x in my_list:
    if x%2 == 0:
        print("found an even number", x)
        break
else:
    print("there were no even numbers")
```

A more traditional way to achieve this would be to use a helper variable to remember whether the desired item was found:

```python
my_list = [3,5,2,8,1]
found = False
for x in my_list:
    if x%2 == 0:
        print("found an even number", x)
        found = True
        break
if not found:
    print("there were no even numbers")
```

Using a `for else` statement saves us the trouble of writing a separate variable.

## Default parameter value

A Python function can have a default parameter value. It is used whenever no argument is passed to the function. See the following example:

```python
def say_hello(name="Emily"):
    print("Hi there,", name)

say_hello()
say_hello("Eric")
say_hello("Matthew")
say_hello("")
```

<sample-output>

Hi there, Emily
Hi there, Eric
Hi there, Matthew
Hi there,

</sample-output>

NB: an empty string is still a string, so the default parameter is not used if an empty string is passed to the function.

## A variable number of parameters

You can also define a function with a variable number of parameters, by adding a star before the parameter name. All the remaining arguments passed to the function are contained in a tuple, and can be accessed through the named parameter.

The following function counts the number and sum of its arguments:

```python
def testing(*my_args):
    print("You passed", len(my_args), "arguments")
    print("The sum of the arguments is", sum(my_args))

testing(1, 2, 3, 4, 5)
```

<sample-output>

You passed 5 arguments
The sum of the arguments is 15

</sample-output>

<programming-exercise name='Your own programming language' tmcname='part07-18_own_programming_language'>

In this exercise you will write your own programming language executor. You can use any techniques and skills you have learnt on this course thus far.

The programs will consist of rows, and each row will be in one of the following formats:

* `PRINT [value]`: prints the value
* `MOV [variable] [value]`: assigns the value to the variable
* `ADD [variable] [value]`: adds the value to the variable
* `SUB [variable] [value]`: subtracts the value from the variable
* `MUL [variable] [value]`: multiplies the variable by the value
* `[location]:`: names a line of code, so it can be jumped to from elsewhere
* `JUMP [location]`: jumps to the location specified
* `IF [condition] JUMP [location]`: if the condition is true, jump to the location specified
* `END`: finish execution

The square brackets above are just a notation to signify operands; see below for usage examples.

The program is executed line by line from the first line onwards. The execution ends when the executor comes across the command `END`, or when there are no more lines to execute.

Each program has 26 pre-defined variables, named `A` to `Z`. Each variable has the value 0 when the program begins. The notation `[variable]` refers to one of these 26 variables.

All the values processed by the program are integer numbers. The notation `[value]` refers either to a value stored in a variable, or an integer number typed in directly.

The notation `[location]` refers to any name of a location which consists of lowercase letters `a` to `z` and/or numbers `0` to `9`. Two different locations may not have the same name.

The notation `[condition]` refers to any expression in the format `[value] [comparison] [value]`, where `[comparison]` is one of the following operators: `==`, `!=`, `<`, `<=`, `>` and `>=`.

Please write a function named `run(program)`, which takes a list containing the program commands as its argument. Each item on the list is a line of code in the program. The function should return a new list, which contains the results of the `PRINT` commands executed during the program's run.

You may assume the function will only be given programs which are entirely in the correct format. You do not have to implement any input validation or error handling.

This exercise is worth two points. You will receive one point if the commands `PRINT`, `MOV`, `ADD`, `SUB`, `MUL` and `END` are working correctly. You will receice another point if the rest of the commands, which are used to implement loops, also work.

Below are some examples, which you may also use for testing. Example 1:

```python
program1 = []
program1.append("MOV A 1")
program1.append("MOV B 2")
program1.append("PRINT A")
program1.append("PRINT B")
program1.append("ADD A B")
program1.append("PRINT A")
program1.append("END")
result = run(program1)
print(result)
```

<sample-output>

[1, 2, 3]

</sample-output>

Example 2:

```python
program2 = []
program2.append("MOV A 1")
program2.append("MOV B 10")
program2.append("begin:")
program2.append("IF A >= B JUMP quit")
program2.append("PRINT A")
program2.append("PRINT B")
program2.append("ADD A 1")
program2.append("SUB B 1")
program2.append("JUMP begin")
program2.append("quit:")
program2.append("END")
result = run(program2)
print(result)
```

<sample-output>

[1, 10, 2, 9, 3, 8, 4, 7, 5, 6]

</sample-output>

Example 3 (factorial):

```python
program3 = []
program3.append("MOV A 1")
program3.append("MOV B 1")
program3.append("begin:")
program3.append("PRINT A")
program3.append("ADD B 1")
program3.append("MUL A B")
program3.append("IF B <= 10 JUMP begin")
program3.append("END")
result = run(program3)
print(result)
```

<sample-output>

[1, 2, 6, 24, 120, 720, 5040, 40320, 362880, 3628800]

</sample-output>

Example 4 (prime numbers):

```python
program4 = []
program4.append("MOV N 50")
program4.append("PRINT 2")
program4.append("MOV A 3")
program4.append("begin:")
program4.append("MOV B 2")
program4.append("MOV Z 0")
program4.append("test:")
program4.append("MOV C B")
program4.append("new:")
program4.append("IF C == A JUMP error")
program4.append("IF C > A JUMP over")
program4.append("ADD C B")
program4.append("JUMP new")
program4.append("error:")
program4.append("MOV Z 1")
program4.append("JUMP over2")
program4.append("over:")
program4.append("ADD B 1")
program4.append("IF B < A JUMP test")
program4.append("over2:")
program4.append("IF Z == 1 JUMP over3")
program4.append("PRINT A")
program4.append("over3:")
program4.append("ADD A 1")
program4.append("IF A <= N JUMP begin")
result = run(program4)
print(result)
```

<sample-output>

[2, 3, 5, 7, 11, 13, 17, 19, 23, 29, 31, 37, 41, 43, 47]

</sample-output>

</programming-exercise>

Please respond to the course feedback questionnaire. The questionnaire results help us improve the course.

<quiz id="d62984be-5496-56b5-a725-ea811a8ba38b"></quiz>

