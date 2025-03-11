---
path: '/part-8/2-classes-and-objects'
title: 'Classes and objects'
hidden: false
---

<text-box variant='learningObjectives' name="Learning objectives">

After this section

- You will know what a class is
- You will understand what the connection between a class and an object is
- You will know what is meant by object oriented programming

</text-box>

In the previous section we worked with lists, tuples, dictionaries and strings. These are all rather special cases in Python programming. Python syntax features a unique, pre-defined method of declaring an object belonging to each of these types:

```python
# Lists are declared with square brackets
my_list = [1,2,3]

# Strings are declared with quotation marks
my_string = "Hi there!"

# Dictionaries are declared with curly brackets
my_dict = {"one": 1, "two:": 2}

# Tuples are declared with parentheses
my_tuple = (1,2,3)
```

When some other type of object is declared, we need to call a special initialization function called a _constructor_. Let's have a look at working with fractions through the `Fraction` class.

```python
# we are using the class Fraction from the module fractions
from fractions import Fraction

# create some new fractions
half = Fraction(1,2)

third = Fraction(1,3)

another = Fraction(3,11)

# print these out
print(half)
print(third)
print(another)

# Fractions can be added together, for example
print(half + third)
```

<sample-output>

1/2
1/3
3/11
5/6

</sample-output>

As you can see above, constructor method calls look a little different than the normal method calls we have come across before. For one, they are not attached to any object with the dot notation (as the constructor call is needed to create an object in the first place). The constructor method is also capitalized: `half = Fraction(1,2)`. Let's have a closer look at how objects are constructed by getting familiar with the concept of the _class_.

## A class is the blueprint of an object

We have already used the term _class_ in the material many times. For instance, in the example above we imported the `Fraction` class from the module `fractions`. New fraction objects were created by calling the _constructor_ method of the `Fraction` class.

A class definition contains the structure and functionalities of any object which represents it. That is why classes are sometimes referred to as the blueprints of objects. So, a class definition tells you what kind of data an object contains, and defines also the methods which can be used on the object. _Object oriented programming_ refers to a programming paradigm where the functionality of the program is tied into the use of classes and objects created based on them.

A single class definition can be used to create multiple objects. As mentioned before, objects are independent. Changes made to one object generally do not affect the other objects representing the same class. Each object has its own unique set of data attributes. It might be helpful to consider this simplification of the class-object relationship:

* a class defines the variables
* when an object is created, those variables are assigned values

So, we can use an object of type `Fraction` to access the numerator and denominator of a fractional number:

```python
from fractions import Fraction

number = Fraction(2,5)

# Print the numerator
print(number.numerator)

# ...and the denominator
print(number.denominator)
```

<sample-output>

2
5

</sample-output>

The `Fraction` class definition contains declarations for the variables `numerator` and `denominator`. Each object created based on the class has its own specific values assigned to these variables.

Similarly, objects created based on the `date` class each contain their own unique values for the year, month and day of the date:

```python
from datetime import date

xmas_eve = date(2020, 12, 24)
midsummer = date(2020, 6, 20)

# print only the month attribute of both objects
print(xmas_eve.month)
print(midsummer.month)
```

<sample-output>

12
6

</sample-output>

The `date` class definition contains declarations of the `year`, `month` and `day` variables. When a new `date` object is created based on the class, these variables are assigned values. Each object has its own unique values assigned to these variables.

## Functions which work with objects

Passing an object as an argument to a function should be familiar to you by now, as we have done so many times on this course so far. Let's have a look at the following example. Here we have a function which checks if the `date` object passed as an argument falls on a weekend:

```python
def is_it_weekend(my_date: date):
    weekday = my_date.isoweekday()
    return weekday == 6 or weekday == 7
```

This function uses the method [isoweekday](https://docs.python.org/3/library/datetime.html#datetime.date.isoweekday), which is defined in the `date` class definition, and returns an integer value so that if the date given is a Monday, it returns 1, and if it is a Tuesday, it returns 2, and so forth.

You can use the above function like this:

```python
xmas_eve = date(2020, 12, 24)
midsummer = date(2020, 6, 20)

print(is_it_weekend(xmas_eve))
print(is_it_weekend(midsummer))
```

<sample-output>

False
True

</sample-output>

## Methods vs variables

When working with an object of type `date` you may notice there is a slight difference between how the variables contained in the object are accessed, as opposed to how the methods attached to the objects are used:

```python
my_date = date(2020, 12, 24)

# calling a method
weekday = my_date.isoweekday()

# accessing a variable
my_month = my_date.month

print("The day of the week:", weekday)
print("The month:", my_month)
```

<sample-output>

The day of the week: 4
The month: 12

</sample-output>

The day of the week the date falls on is available through the _method_ `isoweekday`:

```python
weekday = my_date.isoweekday()
```

This is a method call, so there are parentheses after the name of the method. Leaving the parentheses out does not cause an error, but the results are weird:

```python
weekday =  my_date.isoweekday
print("The day of the week:", weekday)
```

<sample-output>

The day of the week: <built-in method isoweekday of datetime.date object at 0x10ed66450>

</sample-output>

The month of a `date` object is a variable, so the value attached can be accessed with a _reference_.

```python
my_month = my_date.month
```

Notice there are _no parentheses_ here. Adding parentheses _would_ cause an error:

```python
my_month = my_date.month()
```

<sample-output>

Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: 'int' object is not callable

</sample-output>

<programming-exercise name='List of years' tmcname='part08-03_list_years'>

Please write a function named `list_years(dates: list)` which takes a list of `date` type objects as its argument. The function should return a new list, which contains the _years in the original list in chronological order_, from earliest to latest.

An example of the function in action:

```python
date1 = date(2019, 2, 3)
date2 = date(2006, 10, 10)
date3 = date(1993, 5, 9)

years = list_years([date1, date2, date3])
print(years)
```

<sample-output>

[1993, 2006, 2019]

</sample-output>

</programming-exercise>


<programming-exercise name='Shopping list' tmcname='part08-04_shopping_list'>

The exercise template contains a pre-defined `ShoppingList` class, which can be used to model a shopping list. Your task is to add a method to the class definition. You do not need to change any methods already defined.

Assuming we have a `ShoppingList` object referenced in a variable named `shopping_list`, the object can be handled with the following methods:

```python

print(shopping_list.number_of_items())
print(shopping_list.item(1))
print(shopping_list.amount(1))
print(shopping_list.item(2))
print(shopping_list.amount(2))

```

<sample-output>

2
Bananas
4
Milk
1

</sample-output>

We can also do this:

```python
# the items on the shopping list are indexed from 1
for i in range(1, shopping_list.number_of_items()+1):
    item = shopping_list.item(i)
    amount = shopping_list.amount(i)
    print(f"{item}: {amount} units")
```


<sample-output>

Bananas 4 units
Milk 1 units

</sample-output>

As you can see, a `ShoppingList` works a bit like a normal list, but it is accessed via the methods provided by the ShoppingList class. Unlike normal Python lists, indexing starts from 1, not 0.

Please write a function named `total_units(my_list: ShoppingList)`, which takes a `ShoppingList` type object as its argument. The function should calculate the total number of units listed, and return the value.

You can use the following code to test your function:

```python
if __name__ == "__main__":
    my_list = ShoppingList()
    my_list.add("bananas", 10)
    my_list.add("apples", 5)
    my_list.add("pineapple", 1)

    print(total_units(my_list))
```

<sample-output>

16

</sample-output>

**NB:** the definition of the `ShoppingList` class is already included in the exercise template. You do not need to use an `import` statement to import it, unlike in the examples above with the Python standard library classes `Fraction` and `date`.

</programming-exercise>
