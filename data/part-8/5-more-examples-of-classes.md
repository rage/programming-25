---
path: '/part-8/5-more-examples-of-classes'
title: 'More examples of classes'
hidden: false
---

<text-box variant='learningObjectives' name="Learning objectives">

After this section

- You will be able to create more versatile classes
- You will know how to add a `__str__` method to your class definitions

</text-box>


## Example 1: the Rectangle class

Let's have a look at a class which models a rectangle in two-dimensional space:

```python
class Rectangle:
    def __init__(self, left_upper: tuple, right_lower: tuple):
        self.left_upper = left_upper
        self.right_lower = right_lower
        self.width = right_lower[0]-left_upper[0]
        self.height = right_lower[1]-left_upper[1]

    def area(self):
        return self.width * self.height

    def perimeter(self):
        return self.width * 2 + self.height * 2

    def move(self, x_change: int, y_change: int):
        corner = self.left_upper
        self.left_upper = (corner[0]+x_change, corner[1]+y_change)
        corner = self.right_lower
        self.right_lower = (corner[0]+x_change, corner[1]+y_change)
```

A new `Rectangle` is created with two tuples as arguments. These tuples contain the x and y coordinates of the upper left corner and the lower right corner. The constructor calculates the height and width of the rectangle based on these values.

The methods `area` and `perimeter` calculate the area and perimeter of the rectangle based on the height and width. The method `move` moves the rectangle by the x and y values given as arguments.

The rectanlge is represented in a coordinate system where the x coordinates increase from left to right, and the y coordinates increase from top to bottom. This is a common way of handling coordinates in programming because it is often easier and more natural to consider the top left corner of the computer screen as the point where x and y equal zero.

The following program tests the `Rectangle` class:

```python
rectangle = Rectangle((1, 1), (4, 3))
print(rectangle.left_upper)
print(rectangle.right_lower)
print(rectangle.width)
print(rectangle.height)
print(rectangle.perimeter())
print(rectangle.area())

rectangle.move(3, 3)
print(rectangle.left_upper)
print(rectangle.right_lower)
```

<sample-output>

(1, 1)
(4, 3)
3
2
10
6
(4, 4)
(7, 6)

</sample-output>

## Printing an object

When you have an object created from a class defined by yourself, the default reaction to calling the `print` command with that object as its argument is not very informative:

```python
rectangle = Rectangle((1, 1), (4, 3))
print(rectangle)
```

The printout should look a bit like this:

<sample-output>

<__main__.Rectangle object at 0x000002D7BF148A90>

</sample-output>

Obviously, we want more control over what is printed out. The easiest way to do this is to add a special `__str__` method to the class definition. Its purpose is to return a snapshot of the state of the object in string format. If the class definition contains a `__str__` method, the value returned by the method is the one printed out when the `print` command is executed.

So, let's add a `__str__` method definition to our `Rectangle` class:

```python
class Rectangle:

    # ...the rest of the class goes here the same as above...

    # This method returns the state of the object in string format
    def __str__(self):
        return f"rectangle {self.left_upper} ... {self.right_lower}"
```

Now the `print` command produces something more user-friendly:

```python
rectangle = Rectangle((1, 1), (4, 3))
print(rectangle)
```

<sample-output>

rectangle (1, 1) ... (4, 3)

</sample-output>

The `__str__` method is perhaps more often used for formulating a string representation of the object with the `str` function, as seen in the following program:

```python
rectangle = Rectangle((1, 1), (4, 3))
str_rep = str(rectangle)
print(str_rep)
```

<sample-output>

rectangle (1, 1) ... (4, 3)

</sample-output>

There are many more special underscored methods which can be defined for classes. One rather similar to the `__str__` method is the `__repr__` method. Its purpose is to provide a technical representation of the state of the object. We will come across this method later.

<programming-exercise name='Stopwatch' tmcname='part08-13_stopwatch'>

The exercise template contains the following skeleton for the `Stopwatch` class:

```python
class Stopwatch:
    def __init__(self):
        self.seconds = 0
        self.minutes = 0
```

Please add to the class definition so that it works as follows:

```python
watch = Stopwatch()
for i in range(3600):
    print(watch)
    watch.tick()
```

<sample-output>

00:00
00:01
00:02
... many more lines printed out
00:59
01:00
01:01
... many, many more lines printed out
59:58
59:59
00:00
00:01

</sample-output>

So, the method `tick` adds one second to the stopwatch. The maximum value for both seconds and minutes is 59. Your class definition should also contain a `__str__` method, which returns a string representation of the state of the stopwatch, as shown in the example above.

**Hint:** it might make it easier to test the `tick` method if you temporarily set the initial values of the seconds and minutes to some value closer to 59 in the constructor. If you do change the initial values, remember to change them back before submitting.

</programming-exercise>

<programming-exercise name='Clock' tmcname='part08-14_clock'>

Please define a new class named `Clock` which expands on the capabilities of your `Stopwatch` class. It should work as follows:

```python
clock = Clock(23, 59, 55)
print(clock)
clock.tick()
print(clock)
clock.tick()
print(clock)
clock.tick()
print(clock)
clock.tick()
print(clock)
clock.tick()
print(clock)
clock.tick()
print(clock)

clock.set(12, 5)
print(clock)
```

<sample-output>
23:59:55
23:59:56
23:59:57
23:59:58
23:59:59
00:00:00
00:00:01
12:05:00
</sample-output>

As you can see above, the constructor should take initial values for the hours, minutes and seconds as arguments, and set these appropriately. The `tick` method adds one second to the clock. The `set` method sets new values for the hours and the minutes, and _sets the seconds to zero_.

</programming-exercise>

<programming-exercise name='LunchCard' tmcname='part08-15_lunchcard'>

At Unicafe, the student cafeteria at the University of Helsinki, students can pay for their lunch with a special debit card.

In this exercise you will write a class called `LunchCard`, with the purpose of emulating the functions provided by the cafeteria's debit card.

### The structure of the new class

Please create a new class named `LunchCard`.

First write the constructor for the class. It should take the initial balance available on the card as an argument, and save it as an attribute. This is provided for you in the skeleton below.

Next, write a `__str__` method, which returns a string containing the balance: "The balance is X euros". The available balance should be printed out with one decimal place precision. Please see the example below for usage.

Here is a skeleton implementation for the class:

```python
class LunchCard:
    def __init__(self, balance: float):
        self.balance = balance

    def __str__(self):
        pass
```

A usage example:

```python
card = LunchCard(50)
print(card)
```

Executing the above should produce the following printout:

<sample-output>

The balance is 50.0 euros

</sample-output>

### Paying for lunch

Please implement the following methods in your LunchCard class:

- `eat_lunch` subtracts 2.60 euros from the balance on the card
- `eat_special` subtracts 4.60 euros from the balance on the card

You can use the following main function to test your class:

```python
card = LunchCard(50)
print(card)

card.eat_lunch()
print(card)

card.eat_special()
card.eat_lunch()
print(card)
```

This should produce the following printout:

<sample-output>

The balance is 50.0 euros
The balance is 47.4 euros
The balance is 40.2 euros

</sample-output>

Make sure the balance is never allowed to reach numbers below zero:

```python
card = LunchCard(4)
print(card)

card.eat_lunch()
print(card)

card.eat_lunch()
print(card)
```

<sample-output>

The balance is 4.0 euros
The balance is 1.4 euros
The balance is 1.4 euros

</sample-output>

If there is not enough money on the card to pay for the lunch, the price of the lunch should not be subtracted from the balance.

### Depositing money on the card

Implement the `deposit_money` method in your `LunchCard` class.

The method increases the balance on the card by the amount given as an argument.

```python
card = LunchCard(10)
print(card)
card.deposit_money(15)
print(card)
card.deposit_money(10)
print(card)
card.deposit_money(200)
print(card)
```

<sample-output>

The balance is 10.0 euros
The balance is 25.0 euros
The balance is 35.0 euros
The balance is 235.0 euros

</sample-output>

The method should account for arguments below zero by [raising an exception](/part-6/3-errors#raising-exceptions) of type `ValueError`:

```python
card = LunchCard(10)
card.deposit_money(-10)
```

<sample-output>

File "testi.py", line 3, in lunchcard
ValueError: You cannot deposit an amount of money less than zero

</sample-output>

**NB:** this method should _raise_ an exception. Please see the instructions for raising exceptions in [part 6](/part-6/3-errors#raising-exceptions). Under no circumstances should the method itself print out anything - the example above is a printout from the Python interpreter coming across the exception.

### Multiple cards

Please write a main function which contains the following sequence of events:

- Create a lunch card for Peter. The initial balance on the card is 20 euros.
- Create a lunch card for Grace. The initial balance on the card is 30 euros.
- Peter eats a special
- Grace eats a regular lunch
- _Print out the balance on each card (on separate lines, with the name of the owner at the beginning of the line)_
- Peter deposits 20 euros
- Grace eats the special
- _Print out the balance on each card (on separate lines, with the name of the owner at the beginning of the line)_
- Peter eats a regular lunch
- Peter eats a regular lunch
- Grace deposits 50 euros
- _Print out the balance on each card (on separate lines, with the name of the owner at the beginning of the line)_

Body of the main program

```python
peters_card = LunchCard(20)
graces_card = LunchCard(30)
# the rest of your main function
```

Your main function should print out exactly the following:

<sample-output>

Peter: The balance is 15.4 euros
Grace: The balance is 27.4 euros
Peter: The balance is 35.4 euros
Grace: The balance is 22.8 euros
Peter: The balance is 30.2 euros
Grace: The balance is 72.8 euros

</sample-output>

</programming-exercise>

## Example 2: Task list

The following class `TaskList` models a list of tasks:

```python
class TaskList:
    def __init__(self):
        self.tasks = []

    def add_task(self, name: str, priority: int):
        self.tasks.append((priority, name))

    def get_next(self):
        self.tasks.sort()
        # The list method pop removes and returns the last item in a list
        task = self.tasks.pop()
        # Return the name of the task (the second item in the tuple)
        return task[1]

    def number_of_tasks(self):
        return len(self.tasks)

    def clear_tasks(self):
        self.tasks = []
```

The method `add_task` adds a new task to the list. Each task also has a priority attached, which is used for sorting the tasks. The method `get_next` removes and returns the task with the highest priority on the list. There is also the `number_of_tasks` method, which returns the number of tasks on the list, and finally the method `clear_tasks`, which clears the task list.

Within the object, the tasks are stored in a list. Each task is of a tuple containing the priority of the task and its name. The priority value is stored first, so that when the list is sorted, the task with the highest priority is the last item on the list. This is why we can then simply use the `pop` method to retrieve and remove the highest priority item.

Please have a look at the following program with the task list in action:

```python
tasks = TaskList()
tasks.add_task("studying", 50)
tasks.add_task("exercise", 60)
tasks.add_task("cleaning", 10)
print(tasks.number_of_tasks())
print(tasks.get_next())
print(tasks.number_of_tasks())
tasks.add_task("date", 100)
print(tasks.number_of_tasks())
print(tasks.get_next())
print(tasks.get_next())
print(tasks.number_of_tasks())
tasks.clear_tasks()
print(tasks.number_of_tasks())
```

<sample-output>

3
exercise
2
3
date
studying
1
0

</sample-output>

<programming-exercise name='Series' tmcname='part08-16_series'>

### A class named Series

Please write a class named `Series` with the following functionality:

```python
dexter = Series("Dexter", 8, ["Crime", "Drama", "Mystery", "Thriller"])
print(dexter)
```

<sample-output>

Dexter (8 seasons)
genres: Crime, Drama, Mystery, Thriller
no ratings

</sample-output>

The constructor should take the title, the number of seasons and a list of genres for the series as its arguments.

**Hint:** whenever you need to produce a string from a list containing strings, with a separating character of your choice in between the entries, you can use the `join` method as follows:

```python
genre_list = ["Crime", "Drama", "Mystery", "Thriller"]
genre_string = ", ".join(genre_list)
print(genre_string)
```

<sample-output>

Crime, Drama, Mystery, Thriller

</sample-output>

### Adding reviews

Please implement the method `rate(rating: int)` which lets you add a rating between 0 and 5 to any series object. You will also need to adjust the `__str__` method so that in case there are ratings, the method prints out the number of ratings added, and their average rounded to one decimal point.

```python
dexter = Series("Dexter", 8, ["Crime", "Drama", "Mystery", "Thriller"])
dexter.rate(4)
dexter.rate(5)
dexter.rate(5)
dexter.rate(3)
dexter.rate(0)
print(dexter)
```

<sample-output>

Dexter (8 seasons)
genres: Crime, Drama, Mystery, Thriller
5 ratings, average 3.4 points

</sample-output>

### Searching for series

Please implement these two functions which allow you to search through a list of series: `minimum_grade(rating: float, series_list: list)` and `includes_genre(genre: str, series_list: list)`.

Here is an example of how the new methods are used:

```python
s1 = Series("Dexter", 8, ["Crime", "Drama", "Mystery", "Thriller"])
s1.rate(5)

s2 = Series("South Park", 24, ["Animation", "Comedy"])
s2.rate(3)

s3 = Series("Friends", 10, ["Romance", "Comedy"])
s3.rate(2)

series_list = [s1, s2, s3]

print("a minimum grade of 4.5:")
for series in minimum_grade(4.5, series_list):
    print(series.title)

print("genre Comedy:")
for series in includes_genre("Comedy", series_list):
    print(series.title)
```

<sample-output>

a minimum rating of 4.5:
Dexter
genre Comedy:
South Park
Friends

</sample-output>

The code above and the automatic tests for this exercise assume your class contains an attribute `title`. If you used some other attribute name to refer to the name of the series, please change it before submitting.

</programming-exercise>

Please respond to a quick questionnaire on this week's materials.

<quiz id="5fa782a4-59cc-5e75-9f77-e3a2b35a81e2"></quiz>

