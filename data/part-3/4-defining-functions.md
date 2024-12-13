---
path: '/part-3/4-defining-functions'
title: 'Defining functions'
hidden: false
---

<text-box variant='learningObjectives' name="Learning objectives">

After this section

- You will know how to write and call your own functions
- You will understand what is meant by the argument and the parameter of a function
- You will be able to define parameters in your own functions

</text-box>

We have already used functions such as `len`, `print` and `input` in our programs. These are functions built into Python, and so they are always ready at our disposal, no matter which environment we are programming in. However, it is also possible to define your own functions.

## The function definition

Before a function can be used, it must be defined. Any function definition begins with the keyword `def`, short for _define_. Then comes the _name_ of the function, followed by parentheses and a colon character. This is called the _header_ of the function. After this, indented just like `while` and `if` blocks, comes the _body_ of the function.

For example, the following code defines the function `message`:

```python
def message():
    print("This is my very own function!")
```

If the program above is executed, nothing seems to happen. This is because the code in the body of the function is only executed when the function is _called_.

Calling a function is as simple as mentioning its name in your code. Adding a function call to the end of the above program, like so

```python
def message():
    print("This is my very own function!")

message()
```
results in a printed statement:

<sample-output>

This is my very own function!

</sample-output>

When a function has been defined it can be called multiple times:

```python
def message():
    print("This is my very own function!")

message()
message()
message()
```

<sample-output>

This is my very own function!
This is my very own function!
This is my very own function!

</sample-output>

<text-box variant='hint' name='Testing your own functions'>

NB: from now on the majority of the exercises on this course will ask you to write your own function(s).

When a program consists of only functions, executing it doesn't seem to have any effect. The following code doesn't print out anything, even though there is a print statement:

```python
def greet():
    print("Hi!")
```

The reason nothing is printed out is that the code within the body of the `greet` function is only executed when the function is called.

The "main" program below the function should contain appropriate function calls, so that the program can be tested. In fact, Python treats all code that is not within function definitions as part of the _main function_, which gets executed when the file itself is evaluated or executed. So, lets add a function call:

```python
def greet():
    print("Hi!")

# All code not within function definitions is part of
# the main function of the program
# Calling our function:

greet()
```

**Important**: on this course the automatic tests that are run on the exercise files require an empty main function. No commands should be left in the main function of your solution. That is, any code that you yourself use for testing must be contained in a specially defined `if` block:

```python
def greet():
    print("Hi!")

# Write your main function within a block like this:
if __name__ == "__main__":
    greet()
```

Any code left outside the above block causes an error:

<img src="3_4_1.png">

The purpose of this is to make sure that your solution gets tested on a clean slate, as the tests often check what your functions print out. It is worth noting that the tests will not execute any code from within the `if __name__ == "__main__"` block, so no code that is needed to fulfil the requirements of the exercise should be placed within the block.

</text-box>

<in-browser-programming-exercise name="Seven Brothers" tmcname="part03-28_seven_brothers">

Please write a function named `seven_brothers`. When the function is called, it should print out the names of the seven brothers in alphabetical order, as in the example below. See the [similarly named exercise in part 1](/part-1/1-getting-started#programming-exercise-fix-the-code-seven-brothers) for more details about the brothers.


<sample-output>

Aapo
Eero
Juhani
Lauri
Simeoni
Timo
Tuomas

</sample-output>

</in-browser-programming-exercise>

## Function arguments

Functions often take one or more _arguments_, which may affect what the function does. For example, the built-in Python functions `print` and `input` take as argument(s) the text that is to be displayed:

```python
print("Hi!")                           # argument is the string "Hi!"
name = input("What is your name? ")    # argument is the string "What is your name? "
print(name)                            # argument is the value of the variable name
```

It was mentioned before that the terms _argument_ and _parameter_ are often used to refer to the same thing. The distinction is that while _argument_ is used with the data passed to the function when the function is called, _inside_ the function the arguments are assigned to variables called _parameters_. So, approximately, when the function is called, we call the passed bits of data arguments, but when we are defining the function, we call them parameters.

This may seem like a futile semantic distintion, and to make things even muddier, not all sources follow this definition. On this course we attempt to keep the distinction clear, however, as knowing the correct terminology will help you understand other sources besides this course material.

Let's define some functions that take arguments. In the function definition, the parameters are defined within the parentheses after the function name:

```python
def hello(target):
    print("Hello", target)
```

Calling this function twice, like so

```python
hello("Emily")
hello("world!")
```

prints out two different greetings:

<sample-output>

Hello Emily
Hello world!

</sample-output>

Let's take a closer look at the function definition:

```python
def hello(target):
    print("Hello", target)
```

On the first line, in the function header, we defined that this function takes an argument, and assigns it to a parameter named `target`. In the body of the function the `print` command uses the value stored in `target`.

When the function is called, the parameter `target` has the value given as an argument in the function call. For example, the following function call

```python
name = "Alan"
hello(name)
```

causes the parameter `target` to be set to the value `"Alan"`.

The names of functions and their parameters follow the same principles as the names of variables. They should be descriptive, and contain primarily lowercase letters and underscore characters. Again, there are some exceptions to these guidelines, but we will ignore those for now.

<in-browser-programming-exercise name="The first character" tmcname="part03-29_first_character">

The exercise contains the outline of the function `first_character`. Please complete it so that it prints out the first character of the string it takes as its argument.

```python
def first_character(text):
     # write your code here

# testing the function:
if __name__ == "__main__":
    first_character('python')
    first_character('yellow')
    first_character('tomorrow')
    first_character('heliotrope')
    first_character('open')
    first_character('night')
```

<sample-output>

p
y
t
h
o
n

</sample-output>

</in-browser-programming-exercise>

<text-box variant='hint' name='Testing your functions with arguments'>

Whenever your function takes one or more arguments, it is recommended to test it with various different arguments.

Pay special heed to "special cases" specific to the type of argument you are using. How will your function behave if the argument is zero or a negative number, or a floating point number instead of an integer? What happens if the argument is an empty string?

If the exercise assignment doesn't explicitly tell you to include function calls, you may freely include your own within the main function `if` block as explained above. The tests will ignore everything within the if block.

</text-box>

## More examples

Let's have a look at some more examples of functions which take arguments. In the following function definition the parameter is a number:

```python
def squared(x):
    print(f"The square of the number {x} is {x * x}")

squared(2)
squared(5)
```

<sample-output>

The square of the number 2 is 4
The square of the number 5 is 25

</sample-output>

Meanwhile, in this function definition there is an `if` statement within the body of the function:

```python
def hello(name):
    if name == "Emily":
        print("Hello", name)
    else:
        print("Hi", name)

hello("Emily")
hello("Mark")
```

<sample-output>

Hello Emily
Hi Mark

</sample-output>

This function takes two arguments:

```python
def sum(x, y):
    result = x + y
    print(f"The sum of the arguments {x} and {y} is {result}")

sum(1, 2)
sum(5, 24)
```

<sample-output>

The sum of the arguments 1 and 2 is 3
The sum of the arguments 5 and 24 is 29

</sample-output>

The function also includes the helper variable `result`, which it uses to store the sum of its arguments.

Notice how the names of the parameters within the function definition have no relation to any variables outside it. We might just as well call the above function like this:

```python
x = 100
y = 30
sum(1, 2)
sum(x + y, 10)
```

This should print out

<sample-output>

The sum of the arguments 1 and 2 is 3
The sum of the arguments 130 and 10 is 140

</sample-output>

In the first function call the parameters are assigned the values `x = 1` and `y = 2`. In the second function call they are assigned the values `x = 130` and `y = 10`, regardless of the similarly named variables used in the function call.

We will come back to function definitions in the beginning of the next part of the course.

<!--a similar warning is in sections 3-4, 4-6 and 5-1, check them all if you're changing this-->
## Warning: using global variables within functions

In the examples above we saw that it is possible to assign new variables within function definitions. The function can also see variables assigned outside it, in the main function. Such variables are called _global_ variables.

Using global variables from within functions is usually a bad idea. Among other issues, doing so may cause bugs which are difficult to trace.

Below is an example of a function which uses a global variable "by mistake":

```python
# this is a global variable
name = "Betty"

def hello(given_name):
    # using the global variable instead of the parameter by mistake
    print("Hello", name)

hello("Steve")
hello("Betty")
```

<sample-output>

Hello Betty
Hello Betty

</sample-output>

No matter how many different arguments we call the function with, it will always print out the value `"Betty"` stored in the global variable.

<in-browser-programming-exercise name="Mean" tmcname="part03-30_mean">

Please write a function named `mean`, which takes three integer arguments. The function should print out the arithmetic mean of the three arguments.

```python
mean(5, 3, 1)
mean(10, 1, 1)
```

<sample-output>

3.0
4.0

</sample-output>

</in-browser-programming-exercise>

<in-browser-programming-exercise name="Print many times" tmcname="part03-31_print_many_times">

Please write a function named `print_many_times(text, times)`, which takes a string and an integer as arguments. The integer argument specifies how many times the string argument should be printed out:

```python
print_many_times("hi", 5)

print()

text = "All Pythons, except one, grow up"
times = 3
print_many_times(text, times)
```
<sample-output>

hi
hi
hi
hi
hi

All Pythons, except one, grow up.
All Pythons, except one, grow up.
All Pythons, except one, grow up.

</sample-output>

</in-browser-programming-exercise>

<in-browser-programming-exercise name="A square of hashes" tmcname="part03-32_square_of_hashes">

Please write a function named `hash_square(length)`, which takes an integer argument. The function prints out a square of hash characters, and the argument specifies the length of the side of the square.

```python
hash_square(3)
print()
hash_square(5)
```

<sample-output>

<pre>
###
###
###

#####
#####
#####
#####
#####
</pre>

</sample-output>

</in-browser-programming-exercise>

<in-browser-programming-exercise name="Chessboard" tmcname="part03-33_chessboard">

Please write a function named `chessboard`, which prints out a chessboard made out of ones and zeroes. The function takes an integer argument, which specifies the length of the side of the board. See the examples below for details:

```python
chessboard(3)
print()
chessboard(6)
```

<sample-output>

<pre>
101
010
101

101010
010101
101010
010101
101010
010101
</pre>

</sample-output>

</in-browser-programming-exercise>


<in-browser-programming-exercise name="A word squared" tmcname="part03-34_word_squared">

Please write a function named `squared`, which takes a string argument and an integer argument, and prints out a square of characters as specified by the examples below.

```python
squared("ab", 3)
print()
squared("aybabtu", 5)
```

<sample-output>

<pre>
aba
bab
aba

aybab
tuayb
abtua
ybabt
uayba
</pre>

</sample-output>

</in-browser-programming-exercise>

<!---
A quiz to review the contents of this section:

<quiz id="b04f1d8b-d207-5a8c-abbd-e42a3af7c12d"></quiz>
-->

Please respond to a quick questionnaire on this week's materials.

<quiz id="59f2a9b1-9713-5fcf-bbaf-285b4fbf008a"></quiz>
