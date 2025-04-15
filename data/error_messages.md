---
path: "/error_messages"
title: "Common error messages"
hidden: false
information_page: true
sidebar_priority: 2000
separator_after: "Introduction to Programming"
---

This page contains information about the most common error messages you may come across when completing the programming exercises on this course.

### My printout looks identical to the example in the instructions, but my submission still fails

Make sure your program doesn't print any extra whitespace, such as space characters. Notice that the default behaviour of the `print` function is to add a space between any strings separated with a comma.

```python
    print("Hello","World!")    # This prints out: Hello World!
```

### SyntaxError: bad input on line [line number]

This error message usually appears when there is a typo in your code which the Python interpreter finds hard to classify more specifically. For example, there may be a colon character missing from the end of an `if` statement, or a keyword, such as `while`, may have been misspelled. The only way to fix this problem is to inspect the line indicated in the error message.

```python
    number1 = 1
    number1 = 2
    if number1 < number2    # ':' missing
        print('number2 is greater')
```

If the line indicated in the error message looks correct, the error may often be one line above or below the line indicated, so check around the issue, too.

**NB: The programming exercises in the early parts of this course use a framework called Skulpt to run Python code in the browser.** Skulpt is quite limited compared to a regular Python interpreter, and thus the error messages printed out are often less informative. For example, the `bad input on line` error message may refer to many different programming errors, and it is difficult to find out the true cause of the error just based on this message.

### SyntaxError: unindent does not match any outer indentation level on line [line number]

Your code is indented incorrectly at the line indicated in the error message. For example, all lines within an `if` block must be indented the same. To fix this error, indent all lines within a block of code with the exact same amount of whitespace.
The following code would cause this error:

```python
    if True:
        print('Indented correctly')
       print('Indented incorrectly!')
```

### NameError: name [variable name] is not defined on line [line number]

You are trying to refer to a variable or object which does not exist at that specific point in your program. It may be that the variable has not yet been assigned a value, or there is a typo in the variable name. It may also be the case that you have defined a variable inside a function, and are trying to refer to that same variable outside the function.

```python
    person = input('Please type in your name: ')
    input('Please type in your age: ')

    print("Hi", pearson)                # error: person was typed pearson
    print("You are", age, "years old")  # error: the variable age has not been defined
```

### TypeError: unsupported operand type(s) for Add: 'int' and 'str' on line [line number]

You may be trying to add an integer and a string together, without first converting the string into an integer value. Strings can be converted into integers with the `int()` function. A similar error message may appear if you try to perform other arithmetic operations, such as division or subtraction, on strings.

It may also be the case that you are trying to create a new string by combining a string and an integer. You should first convert the integer into a string with the `str()` function.

```python
    my_age = input("Please type in your age: ")
    my_name = input("Please type in your name: ")

    print(my_age//2)   # error: the variable my_age has not been converted into an integer
```

### TypeError: cannot concatenate 'str' and 'int' objects on line [line number]

See above.

### Pylance is crashing

This is usually caused by the TMC plugin having too many exercises open at the same time. You can try closing unused exercises from the TMC menu in the VS code sidebar.
Click the TMC menu open, under my courses open the courses you have, and click on "close all" on parts which you are no longer working on.
You can always reopen the exercises if you decide to return to the part.
