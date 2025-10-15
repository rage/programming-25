---
path: '/part-6/4-scope-of-variables'
title: 'Local and global variables'
hidden: false
---

<text-box variant='learningObjectives' name="Learning objectives">

After this section

- You will know what is meant by a local variable
- You will know how the scope of a variable affects how it is used
- You will know what the Python keyword `global`means
- You will be able to use local and global variables in the correct contexts

</text-box>

The _scope_ of a variable refers to the sections of a program where a variable is accessible. A _local_ variable is only accessible in a defined section of the program, while a _global_ variable is available for use in any section of the program.

## Local variables

Variables defined within a Python function are local variables, only available within the function. This applies to both function parameters, and other variables defined within the function definition. A variable which is local to a function _does not exist outside the function_.

In the following example we are trying to access the variable `x` in the main function, but this causes an error:

```python
def testing():
    x = 5
    print(x)

testing()
print(x)
```

<sample-output>

5
NameError: name 'x' is not defined

</sample-output>

The variable `x` only exists while the function `testing` is being executed. Other functions or the main function cannot access the variable.

## Global variables

Variables defined within the main function are global variables. We previously defined the main function as those sections of code in a Python program which do not fall within any other function. The value stored in a global variable can be accessed from any other function in the program, so the following does not cause any errors:

```python
def testing():
    print(x)

x = 3
testing()
```

<sample-output>

3

</sample-output>

A global variable cannot be changed directly from within another function. The following function _has no effect_ on the value stored in the global variable:

```python
def testing():
    x = 5
    print(x)

x = 3
testing()
print(x)
```

<sample-output>

5
3

</sample-output>

Here the function `testing` creates a _new, local_ variable `x`, which "masks" the global variable while the function is being executed. This variable has the value 5, but it is a different variable than the global `x` which is defined in the main function.

But what would the following code do?

```python
def testing():
    print(x)
    x = 5

x = 3
testing()
print(x)
```

<sample-output>

UnboundLocalError: local variable 'x' referenced before assignment

</sample-output>


The function `testing` assigns a value to the variable `x`, so Python interprets `x` to be a local variable instead of the global variable of the same name. The function attempts to access the variable before it is defined, so there is an error.

If we wish to specify that we mean to change the global variable within a function, we will need the Python keyword `global`:

```python
def testing():
    global x
    x = 3
    print(x)

x = 5
testing()
print(x)
```

<sample-output>

3
3

</sample-output>

Now the assignment `x = 3` within the function also affects the main function. All sections of the program are using the same global variable `x`.

## When should you use global variables?

Global variables are not a way to bypass function parameters or return values, and they should not be used as such. That is, it is _possible_ to write a function which stores its results directly in a global variable:

```python
def calculate_sum(a, b):
    global result
    result = a + b

calculate_sum(2, 3)
print(result)
```

It is better to write a function with a return value, as we are used to do by now:

```python
def calculate_sum(a, b):
    return a + b

result = calculate_sum(2, 3)
print(result)
```

The advantage of the latter approach is that the function is an _independent_ whole. It has certain, defined parameters, and it returns a result. It has no side effects, so it can be tested and changed independently of the other sections of the program.

Global variables are useful in situations where we need to have some common, "higher level" information available to all functions in the program. The following is an example of just such a situation:

```python
def calculate_sum(a, b):
    global count
    count += 1
    return a + b

def calculate_difference(a, b):
    global count
    count += 1
    return a - b


count = 0
print(calculate_sum(2, 3))
print(calculate_sum(5, 5))
print(calculate_difference(5, 2))
print(calculate_sum(1, 0))
print("There were", count, "function calls")
```

<sample-output>

5
10
3
1
There were 4 function calls

</sample-output>

In this case we want to keep track of how many times either of the functions were called during the execution of the program. The global variable `count` is useful here, because we can increment it from the functions themselves as they are executed, but still access the final value in the main function.

## Passing data from one function to another, revisited

<!--- see also section 4-6, some significant overlap-->
If a program consists of multiple functions, the question of passing data from one function to another often comes up.

When we touched upon this topic [previously](/part-4/6-strings-and-lists#passing-data-from-one-function-to-another), we had a program which asks the user for some integer values, prints them out, and performs some statistical analysis on the numbers. The program was divided into three separate functions:

```python
def input_from_user(how_many: int):
    print(f"Please type in {how_many} numbers:")
    numbers = []

    for i in range(how_many):
        number = int(input(f"Number {i+1}: "))
        numbers.append(number)

    return numbers

def print_result(numbers: list):
    print("The numbers are: ")
    for number in numbers:
        print(number)

def analyze(numbers: list):
    mean = sum(numbers) / len(numbers)
    return f"There are altogether {len(numbers)} numbers, the mean is {mean}, the smallest is {min(numbers)} and the greatest is {max(numbers)}"

# the main function using these functions
inputs = input_from_user(5)
print_result(inputs)
analysis_result = analyze(inputs)
print(analysis_result)
```

An example of the program's execution:

<sample-output>

Please type in 5 numbers:
Number 1: 10
Number 2: 34
Number 3: -32
Number 4: 99
Number 5: -53
The numbers are:
10
34
-32
99
-53
There are altogether 5 numbers, the mean is 11.6, the smallest is -53 and the greatest is 99

</sample-output>

The basic principle here is that the main function "stores" the data processed by the program. Here, this means the numbers typed in by the user, which are stored in the variable `inputs`.

If the numbers are needed in some function, the variable is passed as an argument, as seen above when the functions `print_result` and `analyze` are called. If the function produces a result that is relevant elsewhere in the program, the function returns this with a `return` statement, as seen with the functions `input_from_user` and `analyse` above.

As always in programming, there are many ways to arrive at the same functionality. It would be possible to use the keyword `global` and have the functions directly access the `inputs` variable defined in the main function. There are good reasons why [this is not a smart move](https://softwareengineering.stackexchange.com/questions/148108/why-is-global-state-so-evil), however. When many different functions can access and potentially alter a variable directly, it becomes difficult to reliably track the program's state, which can make the program unpredictable. This is particularly true as the number of functions involved increases, which is common in large software projects.

In conclusion, passing data into and out of functions is best handled by arguments and return values.

You could also separate the implicit main function in the example above into its own, explicit `main` function. Then the variable `inputs` would no longer be a global variable, but instead a local variable within the `main` function:

```python
# your main function goes here
def main():
    inputs = input_from_user(5)
    print_result(inputs)
    analysis_result = analyze(inputs)

    print(analysis_result)

# run the main function
main()
```

<!---
A quiz to review the contents of this section:

<quiz id="69694e01-4c47-5b9d-8a00-b0d96a477dc7"></quiz>
-->

Please respond to a quick questionnaire on this week's materials.

<quiz id="979b7162-29ef-5fd8-816b-cf1ff80eff23"></quiz>
