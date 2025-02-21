---
path: '/part-10/4-application-development'
title: 'Developing a larger application'
hidden: false
---

<text-box variant='learningObjectives' name="Learning objectives">

After this section

- You will be familiar with some basic principles of application development
- You will be comfortable with differentiating between the different parts of an application (user interface, program logic and file handling)
- You will have practiced writing your own somewhat larger application

</text-box>

Thus far in this course material we have covered a large number of Python features.

The Introduction to Programming course introduced control structures, such as while and for, functions, and basic data structures, such as lists, tuples and dictionaries. In principle, those tools are all that is needed to express anything a programmer may wish to express with Python.

On this Advanced Course in Programming, beginning in part 8 of the material, you have become familiar with classes and objects. Let's take a moment to consider when and _why_ they are necessary, if those basic tools from parts 1 to 7 should be enough.

## Managing complexity

Objects and classes are by no means necessary in every programming context. For example, if you are programming a smallish script for one-time use, objects are usually surplus to requirement. However, when you are programming something larger and more complicated, objects become very useful.

When programs grow in complexity, the amount of details quickly becomes unmanageable, unless the program is organised in some systematic way. Even some of the more complicated exercises on this course so far would have benefited from the examples set in this part of the material.

Fo decades the concept of [separation of concerns](https://en.wikipedia.org/wiki/Separation_of_concerns) has been one of the central principles in programming, and the larger field of computer science. Quoting from Wikipedia:

_Separation of concerns is a design principle for separating a computer program into distinct sections such that each section addresses a separate concern. A concern is a set of information that affects the code of a computer program._

Separating the program into sections, so that each has its own concern to handle, helps in managing the inevitable complexity of a computer program.

Functions are one way of organising a program into distinct, manageable wholes. Instead of writing a single script, the idea is to formulate small, separately verifiable functions which each solve some part of the larger problem.

Another common approach to managing larger programs is objects, through object oriented programming principles. There are benefits and drawbacks to both approaches, and each programmer has their own favourite. As we have seen so far, objects and classes allow us to collect all the data _and_ the code processing that data within a single unit, in the attributes and methods of an object. Furthermore, objects provide a way of encapsulating the data they control, so that other parts of the program do not have to worry about the internal details of an object.

## A worked example: phone book

How should a program be divided into classes and objects? This is by no means an easy question with a single acceptable answer, so we will proceed with an example. In part five you completed [a phone book application](/part-5/3-dictionary#programming-exercise-phone-book-version-2), and now we will implement something similar using object oriented programming principles.

Following the separation of concerns principle, a program should be divided into sections which each have their own cause to take care of. In object oriented programming this translates to the [single-responsibility principle](https://en.wikipedia.org/wiki/Single-responsibility_principle). Without going into the finer details, its fundamental purpose is clear from the name: _a single class and objects created based on it should have a single responsibility in the program_.

Object oriented programming is often used as a way of modelling real world objects and phenomena. A single object in the real world is modelled with a single class in program code. In the case of a phone book such objects might be
- a person
- a name
- a phone number

A name and a phone number may be thought of as just bits of data which do not merit their own classes, but a _person_ is a distinct physical entity in the real world, and in the programming world it could work as a class. A Person object would be responsible for tying together a name and the phone numbers attached to it.

A _phone book_ itself could be a good candidate for a class. Its responsibility would be to manage different person objects and the data they contain.

Now we have outlined the core of our application: _phone book_ and _person_ constitute the programming logic of our application, or _application logic_ in short. Our application would need some other classes, too.

It is usually a good idea to keep any interaction with a user separate from the application logic. It is, after all, a responsibility all on its own. In addition to the core application logic, our program should therefore contain a class which handles the user interface.

Furthermore, our phone book should have some means of persistent storage between executions. File handling is, again, a clearly separate responsibility, so it deserves a class of its own.

Now that we have an outline of the basic components of our program, the question arises: where should we begin programming? Again, there is no right or wrong answer, but it is often a good idea to start with some part of the application logic.

## Step 1: an outline for the application logic

Let's start with the class _PhoneBook_. A skeleton implementation could look like this:

```python
class PhoneBook:
    def __init__(self):
        self.__persons = []

    def add_number(self, name: str, number: str):
        pass

    def get_numbers(self, name: str):
        pass

```

This class consists of a list of persons along with methods for both adding and fetching data.

Each person may be connected with multiple numbers, so let's implement the internal structure of `persons` with a dictionary. A dictionary allows us to search for keys by name, and the value attached to a dictionary key can be a list. So far it looks like we don't really need a separate class to represent a person - an entry in a dictionary will do.

Let's implement the methods listed above, and test our phone book:

```python
class PhoneBook:
    def __init__(self):
        self.__persons = {}

    def add_number(self, name: str, number: str):
        if not name in self.__persons:
            # add a new dictionary entry with an empty list for the numbers
            self.__persons[name] = []

        self.__persons[name].append(number)

    def get_numbers(self, name: str):
        if not name in self.__persons:
            return None

        return self.__persons[name]

# code for testing
phonebook = PhoneBook()
phonebook.add_number("Eric", "02-123456")
print(phonebook.get_numbers("Eric"))
print(phonebook.get_numbers("Emily"))
```

This should print out the following:

<sample-output>

['02-123456']
None

</sample-output>

The method `get_numbers` returns `None` if a name is not included in the phone book. If the name is found, it returns the list of numbers attached to the name.

Whenever you make changes to a program it is _always_ worth testing that the code works as expected, before moving on to any other changes. The code used for testing is usually something that is deleted soon after, and as such you might think it's not worth the trouble to write any tests in the first place. In most cases this is not true. Testing is essential to good programming results.

A bug in the program should be caught and fixed as soon as possible. If you get into the habit of verifying the functionality of practically every new line of code, you will find that the bugs are usually easy to locate and fix, as you can be quite certain that the bug was caused by the most recent change. If you only test the program after adding dozens of lines of code, the potential sources for bugs also increase by dozens of times.

## Step 2: an outline for the user interface

With the core application logic out of the way, it is time to implement a text-based user interface. We will need a new class, `PhoneBookApplication`, with the following initial functionality:

```python
class PhoneBookApplication:
    def __init__(self):
        self.__phonebook = PhoneBook()

    def help(self):
        print("commands: ")
        print("0 exit")

    def execute(self):
        self.help()
        while True:
            print("")
            command = input("command: ")
            if command == "0":
                break

application = PhoneBookApplication()
application.execute()
```

This program doesn't do very much yet, but let's go through the contents. The constructor method creates a new PhoneBook, which is stored in a private attribute. The method `execute(self)` starts the program's text-based user interface, the core of which is the `while` loop, which keeps asking the user for commands until they type in the command for exiting. There is also a method for intructions, `help(self)`, which is called before entering the loop, so that the instructions are printed out.

Now, let's add some actual functionality. First, we implement adding new data to the phone book:

```python
class PhoneBookApplication:
    def __init__(self):
        self.__phonebook = PhoneBook()

    def help(self):
        print("commands: ")
        print("0 exit")
        print("1 add entry")

    def execute(self):
        self.help()
        while True:
            print("")
            command = input("command: ")
            if command == "0":
                break
            elif command == "1":
                name = input("name: ")
                number = input("number: ")
                self.__phonebook.add_number(name, number)

application = PhoneBookApplication()
application.execute()
```

If the user types in _1_ for adding a new number, the user interface asks for a name and a number, and adds these to the PhoneBook using the appropriate method defined in the class.

The only responsibility of the user interface is to communicate with the user. Any other functionality, such as storing a new name-number pair, is the responsibility of the PhoneBook object.

There is room for improvement in the structure of our user interface class. Let's create a method `add_entry(self)` which handles the command for adding a new entry:

```python
class PhoneBookApplication:
    def __init__(self):
        self.__phonebook = PhoneBook()

    def help(self):
        print("commands: ")
        print("0 exit")
        print("1 add entry")

    # separation of concerns in action: a new method for adding an entry
    def add_entry(self):
        name = input("name: ")
        number = input("number: ")
        self.__phonebook.add_number(name, number)

    def execute(self):
        self.help()
        while True:
            print("")
            command = input("command: ")
            if command == "0":
                break
            elif command == "1":
                self.add_entry()

application = PhoneBookApplication()
application.execute()
```

The _separation of concerns_ principle extends to the level of methods, too. We could have the entire functionality of the user interface in a single complicated `while` loop, but it is better to separate each functionality into its own method. The responsibility of the `execute()` method is just delegating the commands typed in by the user to the relevant methods. This helps with managing the growing complexity of our program. For example, if we want to later change the way adding entries works, it is immediately clear that we must then focus our efforts on the `add_entry()` method.

Let's include functionality for searching for entries in our user interface. This should have its own method, too:

```python
class PhoneBookApplication:
    def __init__(self):
        self.__phonebook = PhoneBook()

    def help(self):
        print("commands: ")
        print("0 exit")
        print("1 add entry")
        print("2 search")

    def add_entry(self):
        name = input("name: ")
        number = input("number: ")
        self.__phonebook.add_number(name, number)

    def search(self):
        name = input("name: ")
        numbers = self.__phonebook.get_numbers(name)
        if numbers == None:
            print("number unknown")
            return
        for number in numbers:
            print(number)

    def execute(self):
        self.help()
        while True:
            print("")
            command = input("command: ")
            if command == "0":
                break
            elif command == "1":
                self.add_entry()
            elif command == "2":
                self.search()
            else:
                self.help()

application = PhoneBookApplication()
application.execute()
```

We now have a simple working phone book application ready for testing. The following is an example run:

<sample-output>

commands:
0 exit
1 add entry
2 search

command: **1**
name: **Eric**
number: **02-123456**

command: **1**
name: **Eric**
number: **045-4356713**

command: **2**
name: **Eric**
02-123456
045-4356713

command: **2**
name: Emily
number unknown

command: **0**

</sample-output>

For such a simple application we have written quite a lot of code. If we'd written it all within the one `while` loop we could probably have gotten away with a lot less code. It is, however, quite easy to read the code, the structure is clear, and we should have no trouble adding new features.

## Step 3: importing data from a file

Let's assume we already have some phone numbers stored in a file, and we want to read this as the program starts up. The data file is in the following CSV format:

```csv
Eric;02-1234567;045-4356713
Emily;040-324344
```

Handling files is clearly its own area of responsibility, so it merits a class of its own:

```python
class FileHandler:
    def __init__(self, filename):
        self.__filename = filename

    def load_file(self):
        names = {}
        with open(self.__filename) as f:
            for line in f:
                parts = line.strip().split(';')
                name, *numbers = parts
                names[name] = numbers

        return names
```

The constructor method takes the name of the file as its argument. The method `load_file(self)` reads the contents of the file. It splits each line into two parts: a name and a list of numbers. It then adds these to a dictionary, using the name as the key and the list as the value.

The method uses a nifty Python feature: it is possible to first select some items from a list separately, and then take the rest of the items in a new list. You can see an example of this below. You may remember from [part 6](/part-6/1-reading-files#reading-csv-files) that the string method `split` returns a list.

```python
my_list = [1, 2, 3, 4, 5]
first, second, *rest = my_list
print(first)
print(second)
print(rest)
```

<sample-output>

1
2
[3, 4, 5]

</sample-output>

The `*` in front of the variable name `rest` in the assignment statement means that this last variable should contain all the remaining items in the list, from the third one onwards.

We should absolutely test the file handler separately before including it in our application:

```python
t = FileHandler("phonebook.txt")
print(t.load_file())
```

<sample-output>

{'Eric': ['02-1234567', '045-4356713'], 'Emily': ['040-324344']}

</sample-output>

As the file handler seems to work fine, we can add it to our application. Let's assume we want to read the file first thing every time the program is run. The logical place for reading the file would be the constructor of the `PhoneBookApplication` class:

```python
class PhoneBookApplication:
    def __init__(self):
        self.__phonebook = PhoneBook()
        self.__filehandler = FileHandler("phonebook.txt")

        # add the names and numbers from the file to the phone book
        for name, numbers in self.__filehandler.load_file().items():
            for number in numbers:
                self.__phonebook.add_number(name, number)

    # the rest of the program
```

This functionality should also be tested. Once we've made certain the contents of the file are accessible through the user interface of our application, we can move on to the next stage.

## Step 4: export data to a file

The final feature in our basic version of the application is saving the contents of the phone book back in the same file the data was read from.

This involves a change to the `PhoneBook` class. We need to be able to export the contents of the phone book:

```python
class PhoneBook:
    def __init__(self):
        self.__persons = {}

    # ...

    # return all entries (in dictionary format)
    def all_entries(self):
        return self.__persons
```

The actual saving to the file should be handled by the `FileHandler` class. Let's add the method `save_file` which takes a dictionary representation of the phone book as its argument:

```python
class FileHandler():
    def __init__(self, filename):
        self.__filename = filename

    def load_file(self):
        # ...

    def save_file(self, phonebook: dict):
        with open(self.__filename, "w") as f:
            for name, numbers in phonebook.items():
                line = [name] + numbers
                f.write(";".join(line) + "\n")
```

The saving should happen as the program exits. Let's add a method for this purpose to the user interface, and call it before breaking out of the `while` loop:

```python

class PhoneBookApplication:
    # the rest of the code for the user interface

    # a method which gets executed as the program exits
    def exit(self):
        self.__filehandler.save_file(self.__phonebook.all_entries())

    def execute(self):
        self.help()
        while True:
            print("")
            command = input("command: ")
            if command == "0":

                self.exit()
                break
            elif command == "1":
                self.add_entry()
            elif command == "2":
                self.search()
            else:
                self.help()
```

<programming-exercise name='Phone book expansion, version 1' tmcname='part10-10_phone_book_v1'>

In this exercise you will create a small expansion to the phone book application. The code from the above example is in the exercise template. Please add a command which lets the user search the phone book by number. After the addition the application should work as follows:

<sample-output>

commands:
0 exit
1 add entry
2 search
3 search by number

command: **1**
name: **Eric**
number: **02-123456**

command: **1**
name: **Eric**
number: **045-4356713**

command: **3**
number: **02-123456**
Eric

command: **3**
number: **0100100**
unknown number

command: **0**

</sample-output>

Please implement this addition with respect to the current structure of the program. This means that in the `PhoneBookApplication` class you should add an appropriate helper method to allow for the new functionality, and also add a new branch to the `while` loop. In the `PhoneBook` class you should add a method which allows for searching with a number.

**NB:** as you test your program and end up with lots of different numbers stored in the `phonebook.txt` file, there is a chance the local tests will not be passed if conflicting data is read from the file as the app starts. You can try emptying the contents of any `phonebook.txt` files you find in the directory for the exercise _before_ running the local tests. Which one of the files is used by your program may depend on your Visual Studio Code settings. Please have a look at the explanation in [part 6](/part-6/1-reading-files#what-if-visual-studio-code-cannot-find-my-file). If your solution is correct, the tests on the server should still pass normally, regardless.

</programming-exercise>

## Objects in a dictionary

In the next exercise you are asked to change your phone book so that the values in the dictionary are _objects_, not lists.

There is nothing intrinsically strange about this, but this is the first time on this course that something like this is suggested, so let's go through a simpler example before diving into the exercise.

Here we have an application which keeps track of how many exercises students have completed on a course. Each student's exercise count is stored in a simple object:

```python
class ExerciseCounter:
    def __init__(self):
        self.__exercises = 0

    def done(self):
        self.__exercises += 1

    def how_many(self):
        return self.__exercises
```

The following main function uses the above class:

```python
students = {}

print("let's do some exercises")
while True:
    name = input("student: ")
    if len(name) == 0:
        break

    # create a new object if it doesn't exist yet
    if not name in students:
        students[name] = ExerciseCounter()

    # add a new done exercise to the counter
    students[name].done()

print()
print("exercises completed:")

for student, exercises in students.items():
    print(f"{student}'s exercises: {exercises.how_many()}")
```

Running the above could look like this:

<sample-output>

let's do some exercises
student: **peter**
student: **sarah**
student: **andy**
student: **sarah**
student: **charlotte**
student: **charlotte**
student: **andy**
student: **sarah**
student:

exercises completed:
peter's exercises: 1
andy's exercises: 2
sarah's exercises: 3
charlotte's exercises: 2

</sample-output>

There are a couple of things to consider in the above example. When the user inputs a name, the program first checks if the name is already a key in the dictionary. If the name is not present, a new object is created and added as an entry in the dictionary:

```python
if not name in students:
    students[name] = ExerciseCounter()
```

After this we can be _sure_ the object exists, attached to the name of the student which is used as the key. Either it was just created, or it already existed from a previous iteration of the loop. Either way, we can now retrieve the object with the key, and call the method `done`:

```python
students[name].done()
```

The above line actually contains two separate events. We could just as well use a helper variable and write it on two separate lines of code:

```python
students_counter = students[name]
students_counter.done()
```

NB: Even though the object is here assigned to a helper variable, the object still exists in the dictionary just as before.  The helper variable contains a _reference_ to the object in the dictionary.

If you are not quite sure what actually happens in the code above, please do try it out with the [visualisation tool](http://www.pythontutor.com/visualize.html#mode=edit).

<programming-exercise name='Phone book expansion, version 2' tmcname='part10-11_phone_book_v2'>

In this exercise you will create another version of the `PhoneBookApplication`. You will add addresses to the data which can be attached to a name. For simplicity's sake the functionality for saving to file has been removed, and some other methods have been renamed to better accommodate the change.

## A separate class for a person's data

Please change the way the data of a person is handled. Implement a class named `Person`, which takes care of the phone numbers and addresses of persons. The class should work as follows:

```python
person = Person("Eric")
print(person.name())
print(person.numbers())
print(person.address())
person.add_number("040-123456")
person.add_address("Mannerheimintie 10 Helsinki")
print(person.numbers())
print(person.address())
```

<sample-output>

Eric
[]
None
['040-123456']
Mannerheimintie 10 Helsinki

</sample-output>

## PhoneBook uses the class Person

Please change the internal implementation of your application so that your `PhoneBook` class uses objects of class `Person` to store the data in the phone book. That is, the attribute `__persons` should still contain a dictionary, but the values should be Person-objects and not lists. The user of your application should notice no difference; the changes must not affect the user interface.

**WARNING:** whenever you make structural changes to your code, as described in this exercise, always take baby steps and test at every possible stage. Do not try and make all the changes at once. That is a sure way of **running into serious problems with your code**.

A suitable first step might be to write some code for checking the functionality of the `PhoneBook` class directly. For example, the following should at least not cause any errors:

```python
phonebook = PhoneBook()
phonebook.add_number("Eric", "02-123456")
print(phonebook.get_entry("Eric"))
print(phonebook.get_entry("Emily"))
```

Notice the new name for the method for fetching an entry from the phone book. The automatic tests do not check what the printout from your `get_entry` method is, but make sure no errors are raised by the above code, and that the result makes sense within your implementation.

When you've made the necessary changes in your program and have absolutely verified the functionality within the `PhoneBook` class, you can move on to the user interface, and see if everything still works as expected.

## Adding an address

Please implement the functionality for adding an address to an entry in your phone book. The program should work as follows:

<sample-output>

commands:
0 exit
1 add number
2 search
3 add address

command: **1**
name: **Eric**
number: **02-123456**

command: **3**
name: **Emily**
address: **Viherlaaksontie 7, Espoo**

command: **2**
name: **Eric**
02-123456
address unknown

command: **2**
name: **Emily**
number unknown
Viherlaaksontie 7, Espoo

command: **3**
name: **Eric**
address: **Linnankatu 75, Turku**

command: 2
name: **Eric**
02-123456
Linnankatu 75, Turku

command: **2**
name: **Wilhelm**
address unknown
number unknown

command: **0**

</sample-output>

**WARNING and hint:** as stated above in the previous exercise, do not try and make all the changes at once. That is a sure way of **running into serious problems with your code**.

First make sure your can reliably add addresses using the `PhoneBook` class directly. Once you have verified this, you can move on to the necessary changes in the user interface.

</programming-exercise>

## Some concluding remarks

The structure of the PhoneBook example above follows the fundamental principles of object oriented programming rather well. The central tenet is to identify the different responsibilities in the program and divide these logically among the different classes and methods. One of the main motivations for this division is managing complexity. Another important motive is that a logical division of responsibilities - modularity, in professional terminology - often makes the code easier to maintain and expand upon.

In the software packages being developed and used in the wider world, the overwhelmingly most expensive facet of development is maintenance and expansion, meaning debugging existing software and implementing new features. Correctly implemented modularity is financially a very important feature in software development.

There are some more object oriented programming principles worth highlighting here. The PhoneBook is a good example of how the core application logic can (and should) be separated from _both_ the user interface _and_ any data storage facilities. This is important for a couple of different reasons. First, this separation makes it possible to test the code in smaller units, one class and method at a time. Second, as the core logic is now independent of the interfaces to the outside world, it is possible to change, to an extent, the implementation of either the core logic or the interfaces, without breaking the entire application.

The file handling process in the PhoneBook application proceeds as follows: the program reads the file just once, as it starts up. After this all data is stored in variables in the program. When the program exits, it again stores all data, in practice overwriting the file. In most cases this is the recommended way to deal with external files, as editing the data in situ is often much more complicated.

There are many good guidebooks for learning about good programming practices. One such is [Clean Code](https://www.amazon.com/Clean-Code-Handbook-Software-Craftsmanship/dp/0132350882) by Robert Martin. The code examples in the book are implemented in Java, however, so working through the examples can be quite cumbersome at this point in your programming career, although the book itself is much recommended by the course staff. The themes of easily maintained, expandable, good quality code will be further explored on the courses
[Software Development Methods](https://studies.helsinki.fi/courses/cu/hy-CU-118024742-2020-08-01) and [Software Engineering](https://studies.helsinki.fi/courses/cu/hy-CU-118024909-2020-08-01).

Writing code according to established object oriented programming principles comes at a price. You will likely end up writing more code than you would, were you to write your implementation in one continuous bout of spaghetti code. One of the key skills of a porgrammer is to decide the best approach for each situation. Sometimes it is necessary to just hack something together quickly for immediate use. On the other hand, if in the foreseeable future it can be expected that the code will be reused, maintained or futher developed, either by you or, more critically, by someone else entirely, the readability and logical modularity of the program code become essential. More often than not, if it is worth doing, it is worth doing well, even in the very early stages of development.

To finish off this part of the material you will implement one more larger application.

<programming-exercise name='CourseRecords' tmcname='part10-12_course_records'>

Please write an interactive application for keeping track of your studies. The internal structure is up to you, but this would be a good opportunity to practice creating a similar structure as in the PhoneBook example above.

Your program should work as follows:

<sample-output>

1 add course
2 get course data
3 statistics
0 exit

command: **1**
course: **ItP**
grade: **3**
credits: **5**

command: **2**
course: **ItP**
ItP (5 cr) grade 3

command: **1**
course: **ItP**
grade: **5**
credits: **5**

command: **2**
course: **ItP**
ItP (5 cr) grade 5

command: **1**
course: **ItP**
grade: **1**
credits: **5**

command: **2**
course: **ItP**
ItP (5 cr) grade 5

command: **2**
course: **Introduction to Java**
no entry for this course

command: **1**
course: **ACiP**
grade: **1**
credits: **10**

command: **1**
course: **ItAI**
grade: **2**
credits: **5**

command: **1**
course: **Algo101**
grade: **4**
credits: **1**

command: **1**
course: **CompModels**
grade: **5**
credits: **8**

command: **3**
5 completed courses, a total of 29 credits
mean 3.4
grade distribution
5: xx
4: x
3:
2: x
1: x

command: **0**

</sample-output>

Each course name should result in a single entry in the records. A grade may be raised by re-entering the course details, but the grade should never be lowered.

This exercise is worth two exercise points. The first is granted after the commands 1, 2 and 0 work correctly in your program. The second is granted if command 3 also works as expected.

</programming-exercise>

## Epilogue

To finish off this part of the material, let's return to the user interface of the phone book example for a moment.

```python
class PhoneBookApplication:
    def __init__(self):
        self.__phonebook = PhoneBook()
        self.__filehandler = FileHandler("phonebook.txt")

    # the rest of the program

application = PhoneBookApplication()
application.execute()
```

A `PhoneBookApplication` object contains both a `PhoneBook` object and a `FileHandler` object. The name of the file passed to the FileHandler is, at the moment, hard-coded into the `PhoneBookApplication` class. This is a completely irrelevant detail when it comes to the _user interface_ of the application. In fact, it breaks the _separation of concerns_ principle: where a `PhoneBook` object saves its contents should be of no concern to a `PhoneBookApplication`, yet if we wanted to change the location, we'd have to change the code of `PhoneBookApplication`.

It would be a better idea to create a FileHandler object somewhere _outside_ the `PhoneBookApplication` class, and pass it as an argument to the application:

```python
class PhoneBookApplication:
    def __init__(self, storage_service):
        self.__phonebook = PhoneBook()
        self.__storage_service = storage_service

    # the rest of the user interface

# create a FileHandler
storage_service = FileHandler("phonebook.txt")
# pass it as an argument to PhoneBookApplication's constructor
application = PhoneBookApplication(storage_service)
application.execute()
```

This removes an _unnecessary dependency_ from the `PhoneBookApplication` class. If the name of the file changes, the user interface no longer needs to be changed. We just need to pass a different argument to the constructor:

```python
class PhoneBookApplication:
    def __init__(self, filename):
        self.__phonebook = PhoneBook()
        self.__filename = filename

    # the rest of the user interface

# use a different filename
storage_service = FileHandler("new_phonebook.txt")
application = PhoneBookApplication(storage_service)
application.execute()
```

This change also allows us to consider more exotic storage locations, for instance, a cloud service on the internet. We just need to implement a class which uses the cloud service, and offers `PhoneBookApplication` the exact same methods as `FileHandler`.

An instance of this new "cloud handler" class can be passed as an argument to the constructor, and not a single line of code has to be changed in the user interface:

```python
class CloudHandler:
    # code for saving the contents of the phone book
    # in a cloud service on the internet

storage_service = CloudHandler("amazon-cloud", "username", "passwrd")
application = PhoneBookApplication(storage_service)
application.execute()
```

As you have seen before, using techniques like this carries a price tag, as there is more code to write, so a programmer needs to consider whether that is an acceptable tradeoff.

The technique outlined above is called _dependency injection_. As the name implies, the idea is to provide any dependency required by an object from _outside_ the object. It is a very useful tool in a programmer's toolbox, as it makes it easier to implement new features in programs and facilitates automatic testing. This theme will be further explored on the aforementioned courses [Software Development Methods](https://studies.helsinki.fi/courses/cu/hy-CU-118024742-2020-08-01) and [Software Engineering](https://studies.helsinki.fi/courses/cu/hy-CU-118024909-2020-08-01).

Please respond to a quick questionnaire on this part of the course.

<quiz id="f56ab364-0556-5d73-90d1-12c47de1fe75"></quiz>

