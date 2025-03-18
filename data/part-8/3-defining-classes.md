---
path: '/part-8/3-defining-classes'
title: 'Defining classes'
hidden: false
---

<text-box variant='learningObjectives' name="Learning objectives">

After this section

- You will know how to define your own classes
- You will be able to create objects based on classes you've defined yourself
- You will know how to write a constructor
- You are familiar with the parameter name `self`
- You will know what attributes are and how they are used

</text-box>

A class is defined with the keyword `class`. The syntax is as follows:

```python
class NameOfClass:
    # class defition goes here
```

Classes are usually named in _PascalCase_, also known as _UpperCamelCase_. This means that all the words in the class name are written together, without spaces, and each word is capitalised. The following class names follow this convention:

* `Weekday`
* `BankAccount`
* `LibraryDatabase`
* `PythonCourseGrades`

A single class definition should represent a single whole, the contents of which should be atomically linked together in some way. In more complicated programs classes can contain members of other classes. For example, the class `Course` could contain objects of class `Lecture`, `ExerciseSession` etc.

Lets have a look at a skeleton of a class definition. The functionalities are still missing at this point.

```python
class BankAccount:
    pass
```

The above piece of code tells Python that here we are defining a class named `BankAccount`. The class does not contain any functionality yet, but we can still create an object based on the class.

Lets have a look at a program where two variables are added to a `BankAccount` object: `balance` and `owner`. Any variables attached to an object are called its _attributes_, or more specifically, _data attributes_, or sometimes _instance variables_. 

The attributes attached to an object can be accessed through the object:

```python
class BankAccount:
    pass

peters_account = BankAccount()
peters_account.owner = "Peter Python"
peters_account.balance = 5.0

print(peters_account.owner)
print(peters_account.balance)
```

<sample-output>

Peter Python
5.0

</sample-output>

The data attributes are available only through the object they are attached to. Each `BankAccount` object created based on the `BankAccount` class has its own values attached to the data attributes. Those values can be accessed by referring to the object in question:

```python
account = BankAccount()
account.balance = 155.50

print(account.balance) # This refers to the data attribute balance attached to the account
print(balance) # THIS CAUSES AN ERROR, as there is no such independent variable available, and the object reference is missing
```

## Adding a constructor

In the above example we saw that a new instance of a class can be created by calling the constructor method of the class like so: `NameOfClass()`. Above we then attached data attributes to the object separately, but it is often more convenient to pass these initial values of attributes directly as the object is created. In the above example we first had a `BankAccount` object without these attributes, and the attributes only existed after they were explicitly declared.

Declaring attributes outside the constructor results in a situation where different instances of the same class can have different attributes. The following code produces an error because we now have another `BankAccount` object, `paulas_account`, which does not contain the same attributes:

```python
class BankAccount:
    pass

peters_account = BankAccount()
peters_account.owner = "Peter"
peters_account.balance = 1400

paulas_account = BankAccount()
paulas_account.owner = "Paula"

print(peters_account.balance)
print(paulas_account.balance) # THIS CAUSES AN ERROR
```

So, instead of declaring attributes after each instance of the class is created, it is usually a better idea to initialize the values of the attributes as the class constructor is called. As the `BankAccount` class definition is currently just a skeleton, the constructor method is implicitly assumed by the Python interpreter, but it is possible to define your own constructor methods, and that is exactly what we will do now.

A constructor method is a method declaration with the special name `__init__`, usually included at the very beginning of a class definition.

Lets have a look at a `BankAccount` class with a constructor method added in:

```python
class BankAccount:

    # The constructor
    def __init__(self, balance: float, owner: str):
        self.balance = balance
        self.owner = owner
```

The name of the constructor method is always `__init__`. Notice the _two underscores on both sides_ of the word `init`.

The first parameter in a constructor definition is always named `self`. This refers to the object itself, and is necessary for declaring any attributes attached to the object. The assignment

`self.balance = balance`

assigns the balance received as an argument to the balance attribute of the object. It is a common convention to use the same variable names for the parameters and the data attributes defined in a constructor, but the variable names `self.balance` and `balance` above refer to _two different variables_:

* The variable `self.balance` is an attribute of the object. Each `BankAccount` object has its own balance.

* The variable `balance` is a parameter in the constructor method `__init__`. Its value is set to the value passed as an argument to the method as the constructor is called (that is, when a new instance of the class is created).

Now that we have defined the parameters of the constructor method, we can pass the desired initial values of the data attributes as arguments as a new object is created:

```python
class BankAccount:

    # The constructor
    def __init__(self, balance: float, owner: str):
        self.balance = balance
        self.owner = owner

# As the method is called, no argument should be given for the self parameter
# Python assigns the value for self automatically
peters_account = BankAccount(100, "Peter Python")
paulas_account = BankAccount(20000, "Paula Pythons")

print(peters_account.balance)
print(paulas_account.balance)
```

<sample-output>

100
20000

</sample-output>

It is now much easier to work with the `BankAccount` objects, as the values can be passed at object creation, and the resulting two separate instances can be handled more predictably and uniformly. Declaring data attributes in the constructor also ensures the attributes are actually declared, and the desired initial values are always given by the programmer using the class.

It is still possible to change the initial values of the data attributes later in the program:

```python
class BankAccount:

    # The constructor
    def __init__(self, balance: float, owner: str):
        self.balance = balance
        self.owner = owner

peters_account = BankAccount(100, "Peter Python")
print(peters_account.balance)

# Change the balance to 1500
peters_account.balance = 1500
print(peters_account.balance)

# Add 2000 to the balance
peters_account.balance += 2000
print(peters_account.balance)
```

<sample-output>

100
1500
3500

</sample-output>

Let's have a look at another example of classes and objects. We'll write a class which models a single draw of lottery numbers:

```python
from datetime import date

class LotteryDraw:

    def __init__(self, round_week: int, round_date: date, numbers: list):
        self.round_week = round_week
        self.round_date = round_date
        self.numbers = numbers


# Create a new LotteryDraw object
round1 = LotteryDraw(1, date(2021, 1, 2), [1,4,8,12,13,14,33])

# Tulostetaan tiedot
print(round1.round_week)
print(round1.round_date)

for number in round1.numbers:
    print(number)
```

<sample-output>

1
2021-01-02
1
4
8
12
13
14
33

</sample-output>

As you can see above, the attributes can be of any type. Here, each LotteryDraw object has attributes of type `list` and `date`.

<programming-exercise name='Book' tmcname='part08-05_book'>

Please write a class named `Book` with the attributes `name`, `author`, `genre` and `year`, along with a constructor which assigns initial values to these attributes.

Your class should work like this:

```python
python = Book("Fluent Python", "Luciano Ramalho", "programming", 2015)
everest = Book("High Adventure", "Edmund Hillary", "autobiography", 1956)

print(f"{python.author}: {python.name} ({python.year})")
print(f"The genre of the book {everest.name} is {everest.genre}")
```

<sample-output>

Luciano Ramalho: Fluent Python (2015)
The genre of the book High Adventure is autobiography

</sample-output>

</programming-exercise>

<programming-exercise name='Three classes' tmcname='part08-06_three_classes'>

Please write the three classes specified below. Each class should have exactly the same names and types of attributes as listed.

Please also include a constructor in each class. The constructor should take the initial values of the attributes as its arguments, in the order listed below.

1. Class Checklist
- attribute header (string)
- attribute entries (list)

2. Class Customer
- attribute id (string)
- attribute balance (float)
- attribute discount (integer)

3. Class Cable
- attribute model (string)
- attribute length (float)
- attribute max_speed (integer)
- attribute bidirectional (Boolean)

</programming-exercise>

## Using objecs formed from your own classes

Objects formed from your own class definitions are no different from any other Python objects. They can be passed as arguments and return values just like any other object. We could, for example, write some helper functions for working with bank accounts:

```python
# this function creates a new bank account object and returns it
def open_account(name: str):
    new_account =  BankAccount(0, name)
    return new_account

# this function adds the amount passed as an argument to the balance of the bank account also passed as an argument
def deposit_money_on_account(account: BankAccount, amount: int):
    account.balance += amount

peters_account = open_account("Peter Python")
print(peters_account.balance)

deposit_money_on_account(peters_account, 500)

print(peters_account.balance)
```

<sample-output>

0
500

</sample-output>

<programming-exercise name='Define class: Pet' tmcname='part08-07_pet'>

Please define the class `Pet`. The class should include a constructor, which takes the initial values of the attributes `name`, `species` and `year_of_birth` as its arguments, in that specific order.

Please also write a function named `new_pet(name: str, species: str, year_of_birth: int)` _outside the class definition_. The function should create and return a new object of type `Pet`, as defined in the class `Pet`.

An example of how the function is used:

```python
fluffy = new_pet("Fluffy", "dog", 2017)
print(fluffy.name)
print(fluffy.species)
print(fluffy.year_of_birth)
```

<sample-output>

Fluffy
dog
2017

</sample-output>

</programming-exercise>

<programming-exercise name='The older book' tmcname='part08-08_older_book'>

Please write a function named `older_book(book1: Book, book2: Book)` which takes two objects of type `Book` as its arguments. The function should print out a message with the details of whichever is the older. It should print out a different message if the two books were written in the same year. Please see the examples below.

```python
python = Book("Fluent Python", "Luciano Ramalho", "programming", 2015)
everest = Book("High Adventure", "Edmund Hillary", "autobiography", 1956)
norma = Book("Norma", "Sofi Oksanen", "crime", 2015)

older_book(python, everest)
older_book(python, norma)
```

<sample-output>

High Adventure is older, it was published in 1956
Fluent Python and Norma were published in 2015

</sample-output>

</programming-exercise>

<programming-exercise name='Books of a genre' tmcname='part08-09_books_of_genre'>

Please write a function named `books_of_genre(books: list, genre: str)` which takes a list of objects of type `Book` and a string representing a genre as its arguments.

The function should return a _new_ list, which contains the books with the desired genre from the original list. Please see the examples below.

```python
python = Book("Fluent Python", "Luciano Ramalho", "programming", 2015)
everest = Book("High Adventure", "Edmund Hillary", "autobiography", 1956)
norma = Book("Norma", "Sofi Oksanen", "crime", 2015)

books = [python, everest, norma, Book("The Snowman", "Jo Nesbø", "crime", 2007)]

print("Books in the crime genre:")
for book in books_of_genre(books, "crime"):
    print(f"{book.author}: {book.name}")
```

<sample-output>

Books in the crime genre:
Sofi Oksanen: Norma
Jo Nesbø: The Snowman

</sample-output>

</programming-exercise>
