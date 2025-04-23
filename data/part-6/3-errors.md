---
path: '/part-6/3-errors'
title: 'Handling errors'
hidden: false
---

<text-box variant='learningObjectives' name="Learning objectives">

After this section

- You will know how to handle invalid input
- You will understand what are exceptions in programming
- You will be familiar with the most common exception types in Python
- You will be able to handle exceptions in your own programs

</text-box>

The are two basic categories of errors that come up in programming contexts:

1. Syntax errors, which prevent the execution of the program
2. Runtime errors, which halt the execution

Errors in category 1 are usually easy to fix, as the Python interpreter flags the error location when attempting to execute the program. Common syntax errors include a missing colon at the end of a header line, or a missing quotation mark at the end of a string.

Errors in category 2 can be harder to spot, as it may happen that they only occur at a certain point in the execution of a program, and only in certain circumstances. The program may work just fine in most situations, but halt due to an error in a specific marginal case. We will now concentrate on handling these types of errors.

## Input validation

Many errors that come up during the execution of a program have to do with invalid input. Some examples include:

* missing or empty input values in mandatory fields, such as empty strings when the length of the string is critical
* negative values where only positive values are accepted, such as -15 as the amount of an ingredient in a recipe
* missing files or typos in filenames
* values that are too small or too large, for example when working with dates and times
* invalid indexes, such as trying to access index 3 in the string "hey"
* values of a wrong type, such as strings when integers are expected

Fortunately, we as programmers can prepare for most errors. Let's have a look at a program which asks the user for their age, and makes sure it is an acceptable number (between 0 and 150, in this case):

```python
age = int(input("Please type in your age: "))
if age >= 0 and age <= 150:
    print("That is a fine age")
else:
    print("This is not a valid age")
```

<sample-output>

Please type in your age: **25**
That is a fine age

</sample-output>

<sample-output>

Please type in your age: **-3**
This is not a valid age

</sample-output>

As long as the user types in an integer value, our input validation seems to work fine. But what if they type in a string?

<sample-output>

Please type in your age: **twenty-three**
ValueError: invalid literal for int() with base 10: 'twenty-three'

</sample-output>

The `int` function is unable to parse the input string `twenty-three` as a valid integer value. The execution halts and the above error message is printed.

## Exceptions

Errors that occur while the program is already running are called _exceptions_. It is possible to prepare for exceptions, and handle them so that the execution continues despite them occurring.

Exception handling in Python is accomplished with `try` and `except` statements. The idea is that if something within a `try` block causes an exception, Python checks if there is a corresponding `except` block. If such a block exists, it is executed and the program then continues as if nothing happened.

Let's change the above example so that the program is prepared for the `ValueError` exception:

```python
try:
    age = int(input("Please type in your age: "))
except ValueError:
    age = -1

if age >= 0 and age <= 150:
    print("That is a fine age")
else:
    print("This is not a valid age")
```

<sample-output>

Please type in your age: **twenty-three**
This is not a valid age

</sample-output>

We can use the `try` block to flag that the code within the block may cause an error. In the `except` statement directly after the block the relevant error is mentioned. In the above example we covered only a `ValueError` exception. If the exception had some other cause, the execution would still have halted, despite the `try` and `except` blocks.

In the above example, if the error is caught, the value of `age` is set to -1. This is an invalid input value which we have already programmed behaviour for, as the program expects the age of the user to be greater than 0.

In the following example we have a function `read_integer`, which asks the user to type in an integer value, but the function is also prepared for invalid input. The function keeps asking for integers until the user types in a valid input value.

```python
def read_integer():
    while True:
        try:
            input_str = input("Please type in an integer: ")
            return int(input_str)
        except ValueError:
            print("This input is invalid")

number = read_integer()
print("Thank you!")
print(number, "to the power of three is", number**3)
```

<sample-output>

Please type in an integer: **three**
This input is invalid
Please type in an integer: **aybabtu**
This input is invalid
Please type in an integer: **5**
Thank you!
5 to the power of three is 125

</sample-output>

Sometimes it is enough to catch exceptions with a try-except structure, without doing anything about them. That is, we can just ignore the situation in the `except` block.

If we were to change the above example so that we only accepted integers smaller than 100, the results could look like this:

```python
def read_small_integer():
    while True:
        try:
            input_str = input("Please type in an integer: ")
            number = int(input_str)
            if number < 100:
                return number
        except ValueError:
            pass # this command doesn't actually do anything

        print("This input is invalid")

number = read_small_integer()
print(number, "to the power of three is", number**3)
```

<sample-output>

Please type in an integer: **three**
This input is invalid
Please type in an integer: **1000**
This input is invalid
Please type in an integer: **5**
Thank you!
5 to the power of three is 125

</sample-output>

Now the `except` block only contains the command `pass`, which doesn't do anything. Python does not allow empty blocks, so the command is necessary.

<programming-exercise name='Reading input' tmcname='part06-17_read_input'>

Please write a function named `read_input`, which asks the user for input until the user types in an integer which falls within the bounds given as arguments to the function. The function should return the final valid integer value typed in by the user.

An example of the function in action:

```python
number = read_input("Please type in a number: ", 5, 10)
print("You typed in:", number)
```

<sample-output>

Please type in a number: **seven**
You must type in an integer between 5 and 10
Please type in a number: **-3**
You must type in an integer between 5 and 10
Please type in a number: **8**
You typed in: 8

</sample-output>

</programming-exercise>

## Typical errors

Here is a selection of typical errors you will likely come across, along with some situations where they may occur.

**ValueError**

This error is often thrown when the argument passed to a function is somehow invalid. For example, the function call `float("1,23")`causes an error, because decimals are always separated by a point in Python, and here we have a comma. 

**TypeError**

This error occurs when a value is of the wrong type. For example, the function call `len(10)` causes a `TypeError`, because the function `len` requires a value whose length can be calculated, such as a string or a list.

**IndexError**

This common error occurs when trying to refer to an index which doesn't exist. For example, the expression `"abc"[5]` causes an `IndexError`, because the string in question has no index 5.

**ZeroDivisionError**

As the name implies, this error is thrown when trying to divide by zero, which we know from mathematics to always be a bad idea. For example, if we try to determine the arithmetic mean of values in a list with the formula `sum(my_list) / len(my_list)`, but our list has length zero, this error will occur.

**Exceptions in file handling**

Some common errors when working with files are **FileNotFoundError** (when trying to access a file which doesn't exist), **io.UnsupportedOperation** (when trying to perform an operation on a file which is not supported by the mode in which the file is opened) or **PermissionError** (the program lacks necessary permissions to access the file).

## Handling multiple exceptions at once

There may be more than one `except` block attached to each `try` block. For example, the following program can handle both a `FileNotFoundException` and a `PermissionError`:

```python
try:
    with open("example.txt") as my_file:
        for line in my_file:
            print(line)
except FileNotFoundError:
    print("The file example.txt was not found")
except PermissionError:
    print("No permission to access the file example.txt")
```

Sometimes it is not necessary to specify the error the program prepares for. Especially when dealing with files, it is often enough to know that an error has occurred, and safely exit the program. It is not always necessary to know _why_ the error occurred. If we need to cover for all possible exceptions, we can use the `except` block without specifying the error:

```python

try:
    with open("example.txt") as my_file:
        for line in my_file:
            print(line)
except:
    print("There was an error when reading the file.")

```

NB: the `except` statement here covers all possible errors, even those caused by the programming mistakes. Only syntax errors will not be caught by this, as they prevent the code from being executed in the first place.

For example, the following program will always throw an error, because the variable name `my_file` is written as `myfile` on the third line.

```python
try:
    with open("example.txt") as my_file:
        for line in myfile:
            print(line)
except:
    print("There was an error when reading the file.")
```

An `except` block can hide the actual error: the problem here was not caused by file handling as such, but by the variable name which was misspelled. Without the `except` block the error thrown would be shown, and the cause could be found more easily. Therefore it is usually a good idea to use only `except` blocks specifically declared for certain error types.

## Passing exceptions

If executing a function causes an exception, and this exception is not handled, it is passed on to the section of code which called the function, and so forth up the call chain, until it reaches the main function level. If it is not handled there, either, the execution of the program halts, and the exception is usually printed out for the user to see.

In the following example we have the function `testing`. If it causes an exception, this is not handled within the function itself, but in the main function:

```python
def testing(x):
    print(int(x) + 1)

try:
    number = input("Please type in a number: ")
    testing(number)
except:
    print("Something went wrong")
```

<sample-output>

Please type in a number: **three**
Something went wrong

</sample-output>

## Raising exceptions

You can also raise exceptions, with the command `raise`. It may seem like an odd idea to purposefully cause errors in your programs, but it can, in fact, be a very useful mechanism.

For instance, it can sometimes be a good idea to raise an error when detecting invalid parameters. So far we have usually printed out messages when validating input, but if we are writing a function which is executed from elsewhere, just printing something out can go unnoticed when the function is called. Raising an error can make debugging easier.

In the following example we have a function which calculates factorials (for example, the factorial of the number 5 is 1 * 2 * 3 * 4 * 5). If the argument passed to the function is negative, the function raises an error:

```python
def factorial(n):
    if n < 0:
        raise ValueError("The input was negative: " + str(n))
    k = 1
    for i in range(2, n + 1):
        k *= i
    return k

print(factorial(3))
print(factorial(6))
print(factorial(-1))
```

<sample-output>

6
720
Traceback (most recent call last):
  File "test.py", line 11, in <module>
    print(factorial(-1))
  File "test.py", line 3, in factorial
    raise ValueError("The input was negative: " + str(n))
ValueError: The input was negative: -1

</sample-output>


<programming-exercise name='Parameter validation' tmcname='part06-18_parameter_validation'>

Please write a function named `new_person(name: str, age: int)`, which creates and returns a tuple containing the data in the arguments. The first element should be the name and the second the age.

If the values stored in the parameter variables are not valid, the function should throw a `ValueError` exception.

Invalid parameters in this case include:

* name is an empty string
* name contains less than two words
* name is longer than 40 characters
* age is a negative number
* age is greater than 150

</programming-exercise>

<programming-exercise name='Incorrect lottery numbers' tmcname='part06-19_incorrect_lottery_numbers'>

The file `lottery_numbers.csv` containts winning lottery numbers in the following format:

<sample-data>

week 1;5,7,11,13,23,24,30
week 2;9,13,14,24,34,35,37
...etc...

</sample-data>

Each line should contain a header `week x`, followed by seven integer numbers which are all between 1 and 39 inclusive.

The file has been corrupted. Lines in the file may contain the following kinds of errors (these exact lines may not be present in the file, but errors in a similar format will be):

The week number is incorrect:

<sample-data>

week zzc;1,5,13,22,24,25,26

</sample-data>

One or more numbers are not correct:

<sample-data>

week 22;1,**,5,6,13,2b,34

</sample-data>

Too few numbers:

<sample-data>

week 13;4,6,17,19,24,33

</sample-data>

The numbers are too small or large:

<sample-data>

week 39;5,9,15,35,39,41,105

</sample-data>

The same number appears twice:

<sample-data>

week 41;5,12,3,35,12,14,36

</sample-data>

Please write a function named `filter_incorrect()`, which creates a file called `correct_numbers.csv`. The file should contain only those lines from the original file which are in the correct format.

</programming-exercise>

<!---
A quiz to review the contents of this section:

<quiz id="69694e01-4c47-5b9d-8a00-b0d96a477dc7"></quiz>
-->
