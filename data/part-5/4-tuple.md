---
path: '/part-5/4-tuple'
title: 'Tuple'
hidden: false
---

<text-box variant='learningObjectives' name="Learning objectives">

After this section

- You will be familiar with the tuple data type
- You will be able to create tuples from various types of values
- You will know the difference between a tuple and a list
- You will be able to name some typical use cases for tuples

</text-box>

Tuple is a data structure which is, in many ways, similar to a list. The most important differences between the two are:

* Tuples are enclosed in parentheses `()`, while lists are enclosed in square brackets `[]`
* Tuples are _immutable_, while the contents of a list may change

The following bit of code creates a tuple containing the coordinates of a point:

```python
point = (10, 20)
```

The items stored in a tuple are accessed by index, just like the items stored in a list:

```python
point = (10, 20)
print("x coordinate:", point[0])
print("y coordinate:", point[1])
```

<sample-output>

x coordinate: 10
y coordinate: 20

</sample-output>

The values stored in a tuple cannot be changed after the tuple has been defined. The following will _not_ work:

```python
point = (10, 20)
point[0] = 15
```

<sample-output>

TypeError: 'tuple' object does not support item assignment

</sample-output>

<programming-exercise name='Create a tuple' tmcname='part05-23_create_tuple'>

Please write a function named `create_tuple(x: int, y: int, z: int)`, which takes three integers as its arguments, and creates and returns a tuple based on the following criteria:

1. The first element in the tuple is the smallest of the arguments
2. The second element in the tuple is the greatest of the arguments
3. The third element in the tuple is the sum of the arguments

An example of its use:

```python

if __name__ == "__main__":
    print(create_tuple(5, 3, -1))

```

<sample-output>

(-1, 5, 7)

</sample-output>


</programming-exercise>

<programming-exercise name='The oldest person' tmcname='part05-24_oldest_person'>

Please write a function named `oldest_person(people: list)`, which takes a list of tuples as its argument. In each tuple, the first element is the name of a person, and the second element is their year of birth. The function should find the oldest person on the list and return their name.

An example of the function in action:

```python
p1 = ("Adam", 1977)
p2 = ("Ellen", 1985)
p3 = ("Mary", 1953)
p4 = ("Ernest", 1997)
people = [p1, p2, p3, p4]

print(oldest_person(people))
```

<sample-output>

Mary

</sample-output>

</programming-exercise>

<programming-exercise name='Older people' tmcname='part05-25_older_people'>

In this exercise we are handling tuples just like the ones described in the previous exercise.

Please write a function named `older_people(people: list, year: int)`, which selects all those people on the list who were born _before_ the year given as an argument. The function should return the names of these people in a new list.

An example of its use:

```python
p1 = ("Adam", 1977)
p2 = ("Ellen", 1985)
p3 = ("Mary", 1953)
p4 = ("Ernest", 1997)
people = [p1, p2, p3, p4]

older = older_people(people, 1979)
print(older)
```

<sample-output>

[ 'Adam', 'Mary' ]

</sample-output>

</programming-exercise>

## What is the purpose of a tuple?

Tuples are ideal for when there is a set collection of values which are in some way connected. For example, when there is a need to handle the x and y coordinates of a point, a tuple is a natural choice, because coordinates will always consist of two values:

```python
point = (10, 20)
```

Technically it is of course possible to also use a list to store these:

```python
point = [10, 20]
```

A list is a collection of consecutive items in a certain order. The size of a list may also change. When we are storing the coordinates of a point, we want to store the x and y coordinates specifically, not an arbitrary list containing those values.

Because tuples are immutable, unlike lists, they can be used as keys in a dictionary. The following bit of code creates a dictionary, where the keys are coordinate points:

```python
points = {}
points[(3, 5)] = "monkey"
points[(5, 0)] = "banana"
points[(1, 2)] = "harpsichord"
print(points[(3, 5)])
```

<sample-output>
monkey
</sample-output>

Attempting a similar dictionary definition using lists would _not_ work:

```python
points = {}
points[[3, 5]] = "monkey"
points[[5, 0]] = "banana"
points[[1, 2]] = "harpsichord"
print(points[[3, 5]])
```

<sample-output>

TypeError: unhashable type: 'list'

</sample-output>

## Tuples without parentheses

The parentheses are not strictly necessary when defining tuples. The following two variable assignments are identical in their results:

```python
numbers = (1, 2, 3)
```

```python
numbers = 1, 2, 3
```

This means we can also easily return multiple values using tuples. Let's have alook at he following example:

```python
def minmax(my_list):
  return min(my_list), max(my_list)

my_list = [33, 5, 21, 7, 88, 312, 5]

min_value, max_value = minmax(my_list)
print(f"The smallest item is {min_value} and the greatest item is {max_value}")
```

<sample-output>

The smallest item is 5 and the greatest item is 312

</sample-output>

This function returns two values in a tuple. The return value is assigned to two variables at once:

```python
min_value, max_value = minmax(my_list)
```

Using parentheses may make the notation more clear. On the left hand side of the assignment statement we also have a tuple, which contains two variable names. The values contained within the tuple returned by the function are assigned to these two variables.

```python
(min_value, max_value) = minmax(my_list)
```

You may remember the dictionary method `items` in the previous section. We used it to access all the keys and values stored in a dictionary:

```python
my_dictionary = {}

my_dictionary["apina"] = "monkey"
my_dictionary["banaani"] = "banana"
my_dictionary["cembalo"] = "harpsichord"

for key, value in my_dictionary.items():
    print("key:", key)
    print("value:", value)
```

Tuples are at work here, too. The method `my_dictionary.items()` returns each key-value pair as a tuple, where the first item is the key and the second item is the value.

Another common use case for tuples is swapping the values of two variables:

```python
number1, number2 = number2, number1
```

The assignment statement above swaps the values stored in the variables `number1` and `number2`. The result is identical to what is achieved with the following bit of code, using a helper variable:

```python
helper_var = number1
number1 = number2
number2 = helper_var
```

<programming-exercise name='Student database' tmcname='part05-26_student_database'>

In this series of exercises you will create a simple student database. Before diving in, please spend a moment reading through the instructions and thinking about what sort of data structures are necessary for organising the data stored by your program.

#### adding students

First write a function named `add_student`, which adds a new student to the database. Also write a preliminary version of the function `print_student`, which prints out the information of a single student.

These function are used as follows:

```python
students = {}
add_student(students, "Peter")
add_student(students, "Eliza")
print_student(students, "Peter")
print_student(students, "Eliza")
print_student(students, "Jack")
```

Your program should now print out

<sample-output>

<pre>
Peter:
 no completed courses
Eliza:
 no completed courses
Jack: no such person in the database
</pre>

</sample-output>

#### adding completed courses

Please write a function named `add_course`, which adds a completed course to the information of a specific student in the database. The course data is a tuple consisting of the name of the course and the grade:

```python
students = {}
add_student(students, "Peter")
add_course(students, "Peter", ("Introduction to Programming", 3))
add_course(students, "Peter", ("Advanced Course in Programming", 2))
print_student(students, "Peter")
```

When some courses have been added, the information printed out changes:

<sample-output>

<pre>
Peter:
 2 completed courses:
  Introduction to Programming 3
  Advanced Course in Programming 2
 average grade 2.5
</pre>

</sample-output>

#### repeating courses

Courses with grade 0 should be ignored when adding course information. Additionally, if the course is already in the database in that specific student's information, the grade recorded in the database should never be lowered if the course is repeated.

```python
students = {}
add_student(students, "Peter")
add_course(students, "Peter", ("Introduction to Programming", 3))
add_course(students, "Peter", ("Advanced Course in Programming", 2))
add_course(students, "Peter", ("Data Structures and Algorithms", 0))
add_course(students, "Peter", ("Introduction to Programming", 2))
print_student(students, "Peter")
```

<sample-output>

<pre>
Peter:
 2 completed courses:
  Introduction to Programming 3
  Advanced Course in Programming 2
 average grade 2.5
</pre>

</sample-output>

#### summary of database

Please write a function named `summary`, which prints out a summary based on all the information stored in the database.

```python
students = {}
add_student(students, "Peter")
add_student(students, "Eliza")
add_course(students, "Peter", ("Data Structures and Algorithms", 1))
add_course(students, "Peter", ("Introduction to Programming", 1))
add_course(students, "Peter", ("Advanced Course in Programming", 1))
add_course(students, "Eliza", ("Introduction to Programming", 5))
add_course(students, "Eliza", ("Introduction to Computer Science", 4))
summary(students)
```

This should print out

<sample-output>

<pre>
students 2
most courses completed 3 Peter
best average grade 4.5 Eliza
</pre>

</sample-output>

</programming-exercise>

<programming-exercise name="A square of letters" tmcname="part05-27_letter_square">

This final exercise in this part is a relatively demanding problem solving task. It can be solved in many different ways. Even though this current section in the material covers tuples, tuples are not necessarily the best way to go about solving this.

Please write a program which prints out a square of letters as specified in the examples below. You may assume there will be at most 26 layers.

<sample-output>

Layers: **3**
<pre>
CCCCC
CBBBC
CBABC
CBBBC
CCCCC
</pre>

</sample-output>

<sample-output>

Layers: **4**
<pre>
DDDDDDD
DCCCCCD
DCBBBCD
DCBABCD
DCBBBCD
DCCCCCD
DDDDDDD
</pre>

</sample-output>

**NB:** this exercise doesn't ask you to write any functions, so you should __not__ place any code within an `if __name__ == "__main__"` block.

</programming-exercise>

<!---
A quiz to review the contents of this section:

<quiz id="69694e01-4c47-5b9d-8a00-b0d96a477dc7"></quiz>
-->


Please respond to a quick questionnaire on this week's materials.

<quiz id="34daa09c-da82-53df-be28-02a22704bf7e"></quiz>
