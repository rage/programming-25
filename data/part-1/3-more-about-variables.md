---
path: "/part-1/3-more-about-variables"
title: "More about variables"
hidden: false
---

<text-box variant='learningObjectives' name='Learning objectives'>

After this section

- You will be able to use variables in different contexts
- You will know what kind of data can be stored in variables
- You will understand the difference between strings, integers and floating point numbers

</text-box>

Please fill in this questionnaire before getting started with this section. You will get one exercise point for answering.

<quiz id="514abebe-f5ca-54dc-bb62-feb9311f3e08"></quiz>



Variables are needed for various purposes in programming. You can use variables to store any information that will be needed later in the program's execution.

In Python programming variables are created like so:

`variable_name = ...`

Here `...` means the value stored in the variable.

For example, when you used the `input` command to read a string from the user, you stored the string in a variable and then used the variable later in your program:

```python
name = input("What is your name? ")
print("Hi, " + name)
```

<sample-output>

What is your name? **Ghosty**
Hi, Ghosty

</sample-output>

The value stored in a variable can also be defined using other variables:

```python
given_name = "Paul"
family_name = "Python"

name = given_name + " " + family_name

print(name)
```

<sample-output>

Paul Python

</sample-output>

Here the values stored in the three variables are not obtained from user input. They remain the same every time the program is executed. This is called _hard-coding_ data into the program.

## Changing the value of a variable

As implied by the name _variable_, the value stored in a variable can change. In the previous section we noticed that the new value replaces the old one.

During the execution of the following program, the variable `word` will have three different values:

```python
word = input("Please type in a word: ")
print(word)

word = input("And another word: ")
print(word)

word = "third"
print(word)
```

<sample-output>

Please type in a word: **first**
first
And another word: **second**
second
third

</sample-output>

The value stored in the variable changes each time the variable is assigned a new value.

The new value of a variable can be derived from its old value. In the following example the variable `word` is first assigned a value based on user input. Then it is assigned a new value, which is the old value with three exclamation marks added to the end.

```python
word = input("Please type in a word: ")
print(word)

word = word + "!!!"
print(word)
```

<sample-output>

Please type in a word: **test**
test
test!!!

</sample-output>

<text-box variant="hint" name="Choosing a good name for a variable">

* It is often useful to name variables according to what they are used for. For example, if the variable contains a word, the name `word` is a better choice than, say, `a`.

* There is no set limit to the length of a variable name in Python, but there are some other limitations. A variable name should begin with a letter, and it can only contain letters, numbers and underscores &#95;.

* Lowercase and uppercase letters are different characters. The variables `name`, `Name` and `NAME` are all different variables. While this rule has a few exceptions, we will ignore those for now.

* It is a common programming practice in Python to use only lowercase characters in variable names. If the variable name consists of multiple words, use an underscore between the words. While this rule also has a few exceptions, we will ignore those for now.

</text-box>

## Integers

Thus far, we have only stored strings in variables, but there are also many other types of information we will want to store and access later. Let's have a look at integers first. Integers are numbers that do not have a decimal or fractional part, such as `-15`, `0` and `1`.

The following program creates the variable `age`, which contains an integer value.

```python
age = 24
print(age)
```

The program prints out just this:

<sample-output>

24

</sample-output>

Notice the lack of quotation marks here. In fact, if we were to add quotation marks around the number, this would mean our variable would no longer be an integer, but a string instead. A string can contain numbers, but it is processed differently.

So, why does it matter that variables have a type, when the following program still prints out the same thing twice?

```python
number1 = 100
number2 = "100"

print(number1)
print(number2)
```

<sample-output>

100
100

</sample-output>

Variable types matter because different operations affect different types of variables in different ways. Let's have a look at an example:

```python
number1 = 100
number2 = "100"

print(number1 + number1)
print(number2 + number2)
```

This prints out the following:

<sample-output>

200
100100

</sample-output>

For integer values the `+` operator means addition, but for string values it means concatenation, or "stringing together".

Not all operators are available for all types of variables. While numbers can be divided using the division operator `/`, attempting to divide a string by a number causes an error:

```python
number = "100"
print(number / 2)
```

<sample-output>
TypeError: unsupported operand type(s) for /: 'str' and 'int'
</sample-output>

## Combining values when printing

Similarly, the following program will not work, because `"The result is "` and `result` are of two different types:

```python
result = 10 * 25
# the following line produces an error
print("The result is " + result)
```

The program does not print out anything, but instead throws an error:

<sample-output>

TypeError: unsupported operand type(s) for +: 'str' and 'int'

</sample-output>

Here, Python tells us that combining two different types of values will not work just like that. In this case, `"The result is "` is of type string, while the value stored in `result` is of type integer.

If we do want to print out a string and an integer in a single command, the integer can be cast as a string with the `str` function, and the two strings can then be combined normally. For example, this would work:

```python
result = 10 * 25
print("The result is " + str(result))
```

<sample-output>

The result is 250

</sample-output>

The `print` command also has built-in functionalities that support combining different types of values. The simplest way is to add a comma between the values. All the values will be printed out regardless of their type:

```python
result = 10 * 25
print("The result is", result)
```

<sample-output>

The result is 250

</sample-output>

Notice that there is an automatically added whitespace character between the values separated by a comma here.

## Printing with f-strings

What if we want to have more flexibility and control over what we print out? So called _f-strings_ are another way of formatting printouts in Python. The syntax can initially look a bit confusing, but in the end f-strings are often the simplest way of formatting text.

With f-strings the previous example would look like this:

```python
result = 10 * 25
print(f"The result is {result}")
```

Let's break this apart. In the very beginning of the string we are printing out there is the character _f_. This tells Python that what follows is an f-string. Within the string, enclosed in curly brackets, is the variable name `result`. The value it contains becomes a part of the printed string. The printout is exactly the same as in the previous examples:

<sample-output>

The result is 250

</sample-output>

A single f-string can contain multiple variables. For example this code

```python
name = "Mark"
age = 37
city = "Palo Alto"
print(f"Hi {name}, you are {age} years old. You live in {city}.")
```

prints out this:

<sample-output>

Hi Mark, you are 37 years old. You live in Palo Alto.

</sample-output>

It is difficult to create a printout exactly like this using the comma notation in the `print` command. For example, this program

```python
name = "Mark"
age = 37
city = "Palo Alto"
print("Hi", name, ", you are", age, "years old. You live in", city, ".")
```

prints out the following:

<sample-output>

Hi Mark , you are 37 years old. You live in Palo Alto .

</sample-output>

Notice the automatically inserted whitespace between each comma-separated part of the printout. Preventing `print` from adding the extra spaces is technically possible, but not worth the trouble given that we can instead use f-strings.

In its simplicity the comma notation of the `print` command can often be useful, but it does sometimes cause more trouble than it's worth. F-strings are usually a more reliable option. In part 4 you will learn more about the handy features of f-strings when it comes to formatting printouts.

<text-box variant="hint" name="F-strings and Python versions">

If you are using an older version of Python, f-strings may not work. They were introduced in Python version 3.6. Later on during the course you will install Python on your own computer. Unfortunately, the more modern versions of Python are not always available for older operating systems. If that is the case with your computer, when there are exercises requiring the use of f-strings, you can always try them out in the in-browser exercise templates in these early parts of this course.

</text-box>

<in-browser-programming-exercise name="Extra space" tmcname="part01-10b_extra_space" height=400px>

Your friend is working on an app for jobseekers. She sends you this bit of code:

```python
name = "Tim Tester"
age = 20
skill1 = "python"
level1 = "beginner"
skill2 = "java"
level2 = "veteran"
skill3 = "programming"
level3 = "semiprofessional"
lower = 2000
upper = 3000

print("my name is ", name, " , I am ", age, "years old")
print("my skills are")
print("- ", skill1, " (", level1, ")")
print("- ", skill2, " (", level2, ")")
print("- ", skill3, " (", level3, " )")
print("I am looking for a job with a salary of", lower, "-", upper, "euros per month")
```

The program should print out _exactly_ the following:

<sample-output>

<pre>
my name is Tim Tester, I am 20 years old

my skills are
 - python (beginner)
 - java (veteran)
 - programming (semiprofessional)

I am looking for a job with a salary of 2000-3000 euros per month
</pre>

</sample-output>

The code works almost correctly, but not quite. This exercise has very strict tests, which check the output for every single bit of whitespace.

Please fix the code so that the printout looks right. Notice especially how the comma notation in the `print` command automatically inserts a space around the different comma-separated parts.

The easiest way to transform the code so that it meets requirements is to use f-strings.

Hint: you can print an empty line by adding an empty `print` command, or by adding the newline character `\n` into your string.

Do remember to be extra careful when formatting printouts also in the future on this course. Some of the exercises have tests that require your output to be exactly as specified in the examples given. For example, please use actual whitespace characters in your code, instead of ASCII character codes for whitespace, or some such.

</in-browser-programming-exercise>

## Floating point numbers

`Floating point number` or _float_ is a term you will come across often in programming. It refers to numbers with a decimal point. They can be used much in the same way as integer values.

This program calculates the mean of three floating point numbers:

```python
number1 = 2.5
number2 = -1.25
number3 = 3.62

mean = (number1 + number2 + number3) / 3
print(f"Mean: {mean}")
```

<sample-output>

Mean: 1.6233333333333333

</sample-output>

<in-browser-programming-exercise name="Arithmetics" tmcname="part01-11_arithmetics">

This program already contains two integer variables, `x` and `y`:

```python
x = 27
y = 15
```

Please complete the program so that it also prints out the following:

<sample-output>

27 + 15 = 42
27 - 15 = 12
27 * 15 = 405
27 / 15 = 1.8

</sample-output>

The program should work correctly even if the values of the variables are changed. That is, if the first two lines are replaced with this

```python
x = 4
y = 9
```

the program should print out the following:

<sample-output>

4 + 9 = 13
4 - 9 = -5
4 * 9 = 36
4 / 9 = 0.4444444444444444

</sample-output>

</in-browser-programming-exercise>

<in-browser-programming-exercise name="Fix the code: Print a single line" tmcname="part01-12_print_a_single_line">

Each `print` command usually prints out a line of its own, complete with a change of line at the end. However, if the `print` command is given an additional argument `end = ""`, it will not print a line change.

For example:

```python
print("Hi ", end="")
print("there!")
```

<sample-output>

Hi there!

</sample-output>

Please fix this program so that the entire calculation, complete with result, is printed out on a single line. Do not change the number of `print` commands used.

```python

print(5)
print(" + ")
print(8)
print(" - ")
print(4)
print(" = ")
print(5 + 8 - 4)
```

</in-browser-programming-exercise>

<!--

A quiz to review the contents of this section:

<quiz id="49bf296e-41d1-5982-89a9-c784b630eaee"></quiz>

-->
