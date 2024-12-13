---
path: '/part-9/6-more-examples-with-classes'
title: 'More examples with classes'
hidden: false
---

<text-box variant='learningObjectives' name="Learning objectives">

After this section

- You will be familiar with some more examples of classes and objects
- You will be able to use default values for parameters in your methods

</text-box>

The following example consists of two classes. The class `Point` is a model for a point in two-dimensional space. The class `Line` is a model for a line segment between two points. The code below is commented; please read the comments in order to understand how the classes work.

```python
import math

class Point:
    """ The class represents a point in two-dimensional space """

    def __init__(self, x: float, y: float):
        # These attributes are public because any value is acceptable for x and y
        self.x = x
        self.y = y

    # This class method returns a new Point at origo (0, 0)
    # It is possible to return a new instance of the class from within the class
    @classmethod
    def origo(cls):
        return Point(0, 0)

    # This class method creates a new Point based on an existing Point
    # The original Point can be mirrored on either or both of the x and y axes
    # For example, the Point (1, 3) mirrored on the x-axis is (1, -3)
    @classmethod
    def mirrored(cls, point: "Point", mirror_x: bool, mirror_y: bool):
        x = point.x
        y = point.y
        if mirror_x:
            y = -y
        if mirror_y:
            x = -x

        return Point(x, y)

    def __str__(self):
        return f"({self.x}, {self.y})"


class Line:
    """ The class represents a line segment in two-dimensional space """

    def __init__(self, beginning: Point, end: Point):
        # These attributes are public because any two Points are acceptable
        self.beginning = beginning
        self.end = end

    # This method uses the Pythagorean theorem to calculate the length of the line segment
    def length(self):
        sum_of_squares = (self.end.x - self.beginning.x) ** 2 + (self.end.y - self.beginning.y) ** 2
        return math.sqrt(sum_of_squares)

    # This method returns the Point in the middle of the line segment
    def centre_point(self):
        centre_x = (self.beginning.x + self.end.x) / 2
        centre_y = (self.beginning.y + self.end.y) / 2
        return Point(centre_x, centre_y)

    def __str__(self):
        return f"{self.beginning} ... {self.end}"
```

```python
point = Point(1,3)
print(point)

origo = Point.origo()
print(origo)

point2 = Point.mirrored(point, True, True)
print(point2)

line = Line(point, point2)
print(line.length())
print(line.centre_point())
print(line)
```

<sample-output>

(1, 3)
(0, 0)
(-1, -3)
6.324555320336759
(0.0, 0.0)
(1, 3) ... (-1, -3)

</sample-output>

## Default values of parameters

In Python programming you can generally set a default value for any parameter. Default values can be used in both functions and methods.

If a parameter has a default value, you do not have to include a value as an argument when calling the function. If an argument is given, the default value is ignored. If not, the default value is used.

Default values are often used in constructors. If it can be expected that not all information is available when an object is created, it is better to include a default value in the definition of the constructor method than to force the client to take care of the issue. This makes using the class easier from the client's point of view, but it also ensures the integrity of the object. For instance, with a set default value we can be sure that an "empty" value is always the same, unless the client specifically wants to supply something different. If a default value is not set, it is up to the client to provide an "empty" value. This could be, for example, an empty string `""`, the special empty object `None`, or the string `"not set"`.

Let's have a look at yet another class representing a student. When creating a new Student object the client must provide a name and a student number. The student number is private and should not be changed later. Additionally, a Student object has attributes for study credits and notes, which have default values set in the constructor. New values can be passed as arguments to the constructor, but they can also be left out so that the default values are used instead. Please have a look at the comments in the code to better understand what each method does.

```python
class Student:
    """ This class models a student """

    def __init__(self, name: str, student_number: str, credits: int = 0, notes: str = ""):
        # calling the setter method for the name attribute
        self.name = name

        if len(student_number) < 5:
            raise ValueError("A student number should have at least five characters")

        self.__student_number = student_number

        # calling the setter method for the credits attribute
        self.credits = credits

        self.__notes = notes

    @property
    def name(self):
        return self.__name

    @name.setter
    def name(self, name):
        if name != "":
            self.__name = name
        else:
            raise ValueError("The name cannot be an empty string")

    @property
    def student_number(self):
        return self.__student_number

    @property
    def credits(self):
        return self.__credits

    @credits.setter
    def credits(self, op):
        if op >= 0:
            self.__credits = op
        else:
            raise ValueError("The number of study credits cannot be below zero")

    @property
    def notes(self):
        return self.__notes

    @notes.setter
    def notes(self, notes):
        self.__notes = notes

    def summary(self):
        print(f"Student {self.__name} ({self.student_number}):")
        print(f"- credits: {self.__credits}")
        print(f"- notes: {self.notes}")
```

```python
# Passing only the name and the student number as arguments to the constructor
student1 = Student("Sally Student", "12345")
student1.summary()

# Passing the name, the student number and the number of study credits
student2 = Student("Sassy Student", "54321", 25)
student2.summary()

# Passing values for all the parameters
student3 = Student("Saul Student", "99999", 140, "extra time in exam")
student3.summary()

# Passing a value for notes, but not for study credits
# NB: the parameter must be named now that the arguments are not in order
student4 = Student("Sandy Student", "98765", notes="absent in academic year 20-21")
student4.summary()
```

<sample-output>

Student Sally Student (12345):
- credits: 0
- notes:
Student Sassy Student (54321):
- credits: 25
- notes:
Student Saul Student (99999):
- credits: 140
- notes: extra time in exam
Student Sandy Student (98765):
- credits: 0
- notes: absent in academic year 20-21

</sample-output>

NB: there is no setter method for the attribute `student_number` as the student number is not supposed to change.

There is one rather significant snag when using default values for parameters. The following example modelling yet another kind of student will shed more light on this:

```python
class Student:
    def __init__(self, name, completed_courses=[]):
        self.name = name
        self.completed_courses = completed_courses

    def add_course(self, course):
        self.completed_courses.append(course)
```

```python
student1 = Student("Sally Student")
student2 = Student("Sassy Student")

student1.add_course("ItP")
student1.add_course("ACiP")

print(student1.completed_courses)
print(student2.completed_courses)
```

<sample-output>

['ItP', 'ACiP']
['ItP', 'ACiP']

</sample-output>

Adding completed courses to Sally's list also adds those courses to Sassy's list. In fact, these two are the exact same list, as Python reuses the reference stored in the default value. Creating the two new Student objects in the above example is equivalent to the following:

```python
courses = []
student1 = Student("Sally Student", courses)
student2 = Student("Sassy Student", courses)
```

The default values of parameters should never be instances of more complicated, mutable data structures, such as lists. The problem can be circumvented by making the following changes to the constructor of the `Student` class:

```python
class Student:
    def __init__(self, name, completed_courses=None):
        self.name = name
        if completed_courses is None:
            self.completed_courses = []
        else:
            self.completed_courses = completed_courses

    def add_course(self, course):
        self.completed_courses.append(course)
```

```python
student1 = Student("Sally Student")
student2 = Student("Sassy Student")

student1.add_course("ItP")
student1.add_course("ACiP")

print(student1.completed_courses)
print(student2.completed_courses)
```

<sample-output>

['ItP', 'ACiP']
[]

</sample-output>

## The Grand Finale

Even though the following exercise finishes off this part of the material, the techniques required to solve it were all covered already in the section named [objects as attributes](/part-9/2-objects-as-attributes). Specifically, you are not required to use the `@property` decorator or default values for parameters in this exercise. This exercise is very similar to the exercises [a box of presents](/part-9/2-objects-as-attributes#programming-exercise-a-box-of-presents) and [the shortest person in the room](/part-9/2-objects-as-attributes#programming-exercise-the-shortest-person-in-the-room).


#### Important information regarding the next exercise
Please note that there is an issue resulting from an update in Python, which conflicts with the inbuilt library and the original file name for this exercise. If you experience any issues, we recommend redownloading the exercise folder. Once you have obtained the new local test files, you can use either "code.py" or "code_1.py" as the file name. While using Visual Studio Code, you may receive notifications about problems in the test file. However, these notifications can be safely ignored, as they are caused by the test's inability to import from either the "code.py" or "code_1.py" files.

<programming-exercise name='Item, Suitcase and Cargo hold' tmcname='part09-15_item_suitcase_hold'>

In this series of exercises you will create the classes `Item`, `Suitcase` and `Cargo Hold`, which will let you further practice working on objects which contain references to other objects.

## Item

Please create a class named `Item` which is used to create items of different kinds. Each item has a name and a weight (in kilograms).

You can use the following code to test your class:

```python
book = Item("ABC Book", 2)
phone = Item("Nokia 3210", 1)

print("Name of the book:", book.name())
print("Weight of the book:", book.weight())

print("Book:", book)
print("Phone:", phone)
```

Your program should print out this:

<sample-output>

Name of the book: ABC Book
Weight of the book: 2
Book: ABC Book (2 kg)
Phone: Nokia 3210 (1 kg)

</sample-output>

An `Item` should provide the methods `weight` and `name`, which return the values stored in those attributes.

The name and weight should be encapsulated within the class. The following code should not work:

```python
book = Item("ABC Book", 2)
book.weight = 10
```

## Suitcase

Please write a class named `Suitcase`. You should be able to pack items into a suitcase. A suitcase also has a maximum combined weight for the items stored within.

Your class should contains the following members:

- a constructor which takes the maximum weight as an argument
- a method named `add_item` which adds the item given as an argument to the suitcase. The method has no return value.
- a `__str__` method which returns a string in the format "x items (y kg)"

The class should make sure that the combined weight of the items stored within any `Suitcase` does not exceed the maximum weight set for that instance. If the maximum weight would be exceeded when the `add_item` method is called, the new item should not be added to the suitcase.

Your class should work as follows:

```python
book = Item("ABC Book", 2)
phone = Item("Nokia 3210", 1)
brick = Item("Brick", 4)

suitcase = Suitcase(5)
print(suitcase)

suitcase.add_item(book)
print(suitcase)

suitcase.add_item(phone)
print(suitcase)

suitcase.add_item(brick)
print(suitcase)
```

Executing the above should print out

<sample-output>

0 items (0 kg)
1 items (2 kg)
2 items (3 kg)
2 items (3 kg)

</sample-output>

## Mind your language

The notification "1 items" is not very grammatical. Instead, it should say "1 item". Please make the required changes to your `__str__` method.

The previous example should now print out

<sample-output>

0 items (0 kg)
1 item (2 kg)
2 items (3 kg)
2 items (3 kg)

</sample-output>

## All the items

Please add the following methods to your `Suitcase` class definition:

- `print_items` prints out all the items stored in the suitcase
- `weight` returns an integer number representing the combined weight of all the items stored in the suitcase

Your class should now work with the following program:

```python
book = Item("ABC Book", 2)
phone = Item("Nokia 3210", 1)
brick = Item("Brick", 4)

suitcase = Suitcase(10)
suitcase.add_item(book)
suitcase.add_item(phone)
suitcase.add_item(brick)

print("The suitcase contains the following items:")
suitcase.print_items()
combined_weight = suitcase.weight()
print(f"Combined weight: {combined_weight} kg")
```

Executing the above program should print out this:

<sample-output>

The suitcase contains the following items:
ABC Book (2 kg)
Nokia 3210 (1 kg)
Brick (4 kg)
Combined weight: 7 kg

</sample-output>

If you have implemented your `Suitcase` class with more than two instance variables, please make the required changes so that each instance has only two data attributes: the maximum weight and a list of items within.

## The heaviest item

Please add a new method to your `Suitcase` class: `heaviest_item` should return the `Item` which is the heaviest. If there are two or more items with the same, heaviest weight, the method may return any one of these. The method should return a reference to an object. If the suitcase is empty, the method should return `None`.

Your class should now work with the following program:

```python
book = Item("ABC Book", 2)
phone = Item("Nokia 3210", 1)
brick = Item("Brick", 4)

suitcase = Suitcase(10)
suitcase.add_item(book)
suitcase.add_item(phone)
suitcase.add_item(brick)

heaviest = suitcase.heaviest_item()
print(f"The heaviest item: {heaviest}")
```

Executing the above program should print out this:

<sample-output>

The heaviest item: Brick (4 kg)

</sample-output>

## Cargo hold

Please write a class named `CargoHold` with the following methods:

- a constructor which takes the maximum weight as an argument
- a method named `add_suitcase` which adds the suitcase given as an argument to the cargo hold
- a `__str__` method which returns a string in the format "x suitcases, space for y kg"

The class should make sure that the combined weight of the items stored within any `CargoHold` does not exceed the maximum weight set for that instance. If the maximum weight would be exceeded when the `add_suitcase` method is called, the new suitcase should not be added to the cargo hold.

Your class should now work with the following program:

```python
cargo_hold = CargoHold(1000)
print(cargo_hold)

book = Item("ABC Book", 2)
phone = Item("Nokia 3210", 1)
brick = Item("Brick", 4)

adas_suitcase = Suitcase(10)
adas_suitcase.add_item(book)
adas_suitcase.add_item(phone)

peters_suitcase = Suitcase(10)
peters_suitcase.add_item(brick)

cargo_hold.add_suitcase(adas_suitcase)
print(cargo_hold)

cargo_hold.add_suitcase(peters_suitcase)
print(cargo_hold)
```

<sample-output>

0 suitcases, space for 1000 kg
1 suitcase, space for 997 kg
2 suitcases, space for 993 kg

</sample-output>

## The contents of the cargo hold

Please add a method named `print_items` to your `CargoHold` class. It should print out all the items in all the suitcases within the cargo hold.

Your class should now work with the following program:

```python
book = Item("ABC Book", 2)
phone = Item("Nokia 3210", 1)
brick = Item("Brick", 4)

adas_suitcase = Suitcase(10)
adas_suitcase.add_item(book)
adas_suitcase.add_item(phone)

peters_suitcase = Suitcase(10)
peters_suitcase.add_item(brick)

cargo_hold = CargoHold(1000)
cargo_hold.add_suitcase(adas_suitcase)
cargo_hold.add_suitcase(peters_suitcase)

print("The suitcases in the cargo hold contain the following items:")
cargo_hold.print_items()
```

Executing the above program should print out this:

<sample-output>

The suitcases in the cargo hold contain the following items:
ABC Book (2 kg)
Nokia 3210 (1 kg)
Brick (4 kg)

</sample-output>

</programming-exercise>

Please respond to a quick questionnaire on this week's materials.

<quiz id="7d931659-2dd3-5557-bd3b-a409dd1ebfa2"></quiz>

