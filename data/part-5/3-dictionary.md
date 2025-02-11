---
path: '/part-5/3-dictionary'
title: 'Dictionary'
hidden: false
---

<text-box variant='learningObjectives' name="Learning objectives">

After this section

- You will be familiar with the dictionary data structure
- You will be able to use a dictionary with different types of keys and values
- You will know how to traverse through the contents of a dictionary
- You will be able to name some typical use cases for dictionaries

</text-box>

Lists can be handy in many situations, but they are limited by the fact that the items are accessed through indexes; 0, 1, 2, and so forth. If you want to find some item in a list, you will either have to know its index, or, at worst, traverse through the entire list.

Another central data structure in Python is the _dictionary_. In a dictionary, the items are indexed by _keys_. Each key maps to a _value_. The values stored in the dictionary can be accessed and changed using the key.

## Using a dictionary

The following example shows you how the dictionary data structure works. Here is a simple dictionary from Finnish to English:

```python
my_dictionary = {}

my_dictionary["apina"] = "monkey"
my_dictionary["banaani"] = "banana"
my_dictionary["cembalo"] = "harpsichord"

print(len(my_dictionary))
print(my_dictionary)
print(my_dictionary["apina"])
```

<sample-output>

3
{'apina': 'monkey', 'banaani': 'banana', 'cembalo': 'harpsichord'}
monkey

</sample-output>

The notation `{}` creates an empty dictionary, to which we can now add content. Three key-value pairs are added:`"apina"` maps to `"monkey"`, `"banaani"` maps to `"banana"`, and `"cembalo"` maps to `"harpsichord"`. Finally, the number of key-value pairs in the dictionary is printed, along with the entire dictionary, and the value mapped to the key `"apina"`.

After defining the dictionary we could also use it with user input:

```python
word = input("Please type in a word: ")
if word in my_dictionary:
    print("Translation: ", my_dictionary[word])
else:
    print("Word not found")
```

Notice the use of the `in` operator above. When used on a variable of type dictionary, it checks whether the first operand is among the keys stored in the dictionary. Given different inputs, this program might print out the following:

<sample-output>

Please type in a word: **apina**
Translation: monkey

</sample-output>

<sample-output>

Please type in a word: **pöllö**
Word not found

</sample-output>

## What can be stored in a dictionary?

The data type is called dictionary, but it does not have to contain only strings. For example, in the following dictionary the keys are strings, but the values are integers:

```python
results = {}
results["Mary"] = 4
results["Alice"] = 5
results["Larry"] = 2
```

Here the keys are integers and the values are lists:

```python
lists = {}
lists[5] = [1, 2, 3]
lists[42] = [5, 4, 5, 4, 5]
lists[100] = [5, 2, 3]
```

## How keys and values work

Each key can appear only once in the dictionary. If you add an entry using a key that already exists in the dictionary, the original value mapped to that key is replaced with the new value:

```python
my_dictionary["suuri"] = "big"
my_dictionary["suuri"] = "large"
print(my_dictionary["suuri"])
```

<sample-output>

large

</sample-output>

All keys in a dictionary must be _immutable_. So, a list cannot be used as a key, because it can be changed. For example, executing the following code causes an error:

```python
my_dictionary[[1, 2, 3]] = 5
```

<sample-output>

TypeError: unhashable type: 'list'

</sample-output>

<text-box variant="hint" name="Hash table">

Notice the word 'unhashable' in the error message above. This is a reference to the inner workings of the dictionary data type. Python stores the contents of a dictionary in a _hash table_. Each key is reduced to a _hash value_, which determines where the key is stored in computer memory. The error message above indicates that a list cannot be processed into a hash value, so it cannot be used as a key in a dictionary.

The _Data Structures and Algorithms_ courses will further explore hash tables.

</text-box>

Unlike keys, the _values_ stored in a dictionary can change, so any type of data is acceptable as a value. A value can also be mapped to more than one key in the same dictionary.

<programming-exercise name='Times ten' tmcname='part05-14_times_ten'>

Please write a function named `times_ten(start_index: int, end_index: int)`, which creates and returns a new dictionary. The keys of the dictionary should be the numbers between `start_index` and `end_index` inclusive

The value mapped to each key should be the key times ten.

For example:

```python
d = times_ten(3, 6)
print(d)
```

<sample-output>

{3: 30, 4: 40, 5: 50, 6: 60}

</sample-output>

</programming-exercise>

<programming-exercise name='Factorials' tmcname='part05-15_factorials'>

Please write a function named `factorials(n: int)`, which returns the factorials of the numbers 1 to `n` in a dictionary. The number is the key, and the factorial of that number is the value mapped to it.

A reminder: the factorial of the number `n` is written `n`! and is calculated by multiplying the number by each integer smaller than itself. For example, the factorial of 4 is 4 * 3 * 2 * 1 = 24.

An example of the function in action:

```python
k = factorials(5)
print(k[1])
print(k[3])
print(k[5])
```

<sample-output>

1
6
120

</sample-output>

</programming-exercise>

## Traversing a dictionary

The familiar `for item in collection` loop can be used to traverse a dictionary, too. When used on the dictionary directly, the loop goes through the keys stored in the dictionary, one by one. In the following example, all keys and values stored in the dictionary are printed out:

```python
my_dictionary = {}

my_dictionary["apina"] = "monkey"
my_dictionary["banaani"] = "banana"
my_dictionary["cembalo"] = "harpsichord"

for key in my_dictionary:
    print("key:", key)
    print("value:", my_dictionary[key])
```

<sample-output>

key: apina
value: monkey
key: banaani
value: banana
key: cembalo
value: harpsichord

</sample-output>

Sometimes you need to traverse the entire contents of a dictionary. The method `items` returns all the keys and values stored in the dictionary, one pair at a time:

```python

for key, value in my_dictionary.items():
    print("key:", key)
    print("value:", value)
```

In the examples above, you may have noticed that the keys are processed in the same order as they were added to the dictionary. As the keys are processed based on a hash value, the order should not usually matter in applications. In fact, in many older versions of Python the order is not guaranteed to follow the time of insertion.

## Some more advanced ways to use dictionaries

Let's have a look at a list of words:

```python
word_list = [
  "banana", "milk", "beer", "cheese", "sourmilk", "juice", "sausage",
  "tomato", "cucumber", "butter", "margarine", "cheese", "sausage",
  "beer", "sourmilk", "sourmilk", "butter", "beer", "chocolate"
]
```

We would like to analyze this list of words in different ways. For instance, we would like to know how many times each word appears in the list.

A dictionary can be a useful tool in managing this kind of information. In the example below, we go through the items in the list one by one. Using the words in the list as keys in a new dictionary, the value mapped to each key is the number of times the word has appeared:

```python
def counts(my_list):
    words = {}
    for word in my_list:
        # if the word is not yet in the dictionary, initialize the value to zero
        if word not in words:
            words[word] = 0
        # increment the value
        words[word] += 1
    return words

# call the function
print(counts(word_list))
```

The program prints out the following:

<sample-output>

{'banana': 1, 'milk': 1, 'beer': 3, 'cheese': 2, 'sourmilk': 3, 'juice': 1, 'sausage': 2, 'tomato': 1, 'cucumber': 1, 'butter': 2, 'margarine': 1, 'chocolate': 1}

</sample-output>

What if we wanted to categorize the words based on the initial letter in each word? One way to accomplish this would be to use dictionaries:

```python
def categorize_by_initial(my_list):
    groups = {}
    for word in my_list:
        initial = word[0]
        # initialize a new list when the letter is first encountered
        if initial not in groups:
            groups[initial] = []
        # add the word to the appropriate list
        groups[initial].append(word)
    return groups

groups = categorize_by_initial(word_list)

for key, value in groups.items():
    print(f"words beginning with {key}:")
    for word in value:
        print(word)
```
The structure of the function is very similar to the previous exercise but this time the values mapped to the keys are lists. The program prints out the following:

<sample-output>

words beginning with b:
banana
beer
butter
beer
butter
beer
words beginning with m:
milk
margarine
words beginning with c:
cheese
cucumber
cheese
chocolate
words beginning with s:
sourmilk
sausage
sausage
sourmilk
sourmilk
words beginning with j:
juice
words beginning with t:
tomato

</sample-output>

<programming-exercise name='Histogram' tmcname='part05-16_histogram'>

Please write a function named `histogram`, which takes a string as its argument. The function should print out a histogram representing the number of times each letter occurs in the string. Each occurrence of a letter should be represented by a star on the specific line for that letter.

For example, the function call `histogram("abba")` should print out

<sample-output>

<pre>
a **
b **
</pre>

</sample-output>

while `histogram("statistically")` should print out

<sample-output>

<pre>
s **
t ***
a **
i **
c *
l **
y *
</pre>

</sample-output>

</programming-exercise>

<programming-exercise name='Phone book, version 1' tmcname='part05-17_phone_book_v1'>

Please write a phone book application. It should work as follows:

<sample-output>

command (1 search, 2 add, 3 quit): **2**
name: **peter**
number: **040-5466745**
ok!
command (1 search, 2 add, 3 quit): **2**
name: **emily**
number: **045-1212344**
ok!
command (1 search, 2 add, 3 quit): **1**
name: **peter**
040-5466745
command (1 search, 2 add, 3 quit): **1**
name: **mary**
no number
command (1 search, 2 add, 3 quit): **2**
name: **peter**
number: **09-22223333**
ok!
command (1 search, 2 add, 3 quit): **1**
name: **peter**
09-22223333
command (1 search, 2 add, 3 quit): **3**
quitting...

</sample-output>

As you can see above, each name can be attached to a single number only. If a new entry with the same name is added, the number attached to the old entry is replaced with the new number.

**NB:** this exercise doesn't ask you to write any functions, so you should __not__ place any code within an `if __name__ == "__main__"` block.

</programming-exercise>

<programming-exercise name='Phone book, version 2' tmcname='part05-18_phone_book_v2'>

Please write an improved version of the phone book application. Each entry should now accommodate multiple phone numbers. The application should work otherwise exactly as above, but this time _all_ numbers attached to a name should be printed.

<sample-output>

command (1 search, 2 add, 3 quit): **2**
name: **peter**
number: **040-5466745**
ok!
command (1 search, 2 add, 3 quit): **2**
name: **emily**
number: **045-1212344**
ok!
command (1 search, 2 add, 3 quit): **1**
name: **peter**
040-5466745
command (1 search, 2 add, 3 quit): **1**
name: **mary**
no number
command (1 search, 2 add, 3 quit): **2**
name: **peter**
number: **09-22223333**
ok!
command (1 search, 2 add, 3 quit): **1**
name: **peter**
040-5466745
09-22223333
command (1 search, 2 add, 3 quit): **3**
quitting...

</programming-exercise>

## Removing keys and values from a dictionary

It is naturally possible to also remove key-value pairs from the dictionary. There are two ways to accomplish this. The first is the command `del`:

```python
staff = {"Alan": "lecturer", "Emily": "professor", "David": "lecturer"}
del staff["David"]
print(staff)
```

<sample-output>

{'Alan': 'lecturer', 'Emily': 'professor'}

</sample-output>

If you try to use the `del` command to delete a key which doesn't exist in the dictionary, there will be an error:

```python
staff = {"Alan": "lecturer", "Emily": "professor", "David": "lecturer"}
del staff["Paul"]
```

<sample-output>

<pre>
>>> del staff["Paul"]
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
KeyError: 'Paul'
</pre>

</sample-output>

So, before deleting a key you should check if it is present in the dictionary:

```python
staff = {"Alan": "lecturer", "Emily": "professor", "David": "lecturer"}
if "Paul" in staff:
  del staff["Paul"]
  print("Deleted")
else:
  print("This person is not a staff member")
```

The other way to delete entries in a dictionary is the method `pop`:

```python
staff = {"Alan": "lecturer", "Emily": "professor", "David": "lecturer"}
deleted = staff.pop("David")
print(staff)
print(deleted, "deleted")
```

<sample-output>

{'Alan': 'lecturer', 'Emily': 'professor'}
lecturer deleted

</sample-output>

As you can see above, `pop` also returns the value from the deleted entry.

By default, `pop` will also cause an error if you try to delete a key which is not present in the dictionary. It is possible to avoid this by giving the method a second argument, which contains a _default return value_. This value is returned in case the key is not found in the dictionary. The special Python value `None` will work here:

```python
staff = {"Alan": "lecturer", "Emily": "professor", "David": "lecturer"}
deleted = staff.pop("Paul", None)
if deleted == None:
  print("This person is not a staff member")
else:
  print(deleted, "deleted")
```

<sample-output>

This person is not a staff member

</sample-output>

NB: if you need to delete the contents of the entire dictionary, and try to do it with a for loop, like so

```python
staff = {"Alan": "lecturer", "Emily": "professor", "David": "lecturer"}
for key in staff:
  del staff[key]
```

you will receive an error message:

<sample-output>

RuntimeError: dictionary changed size during iteration

</sample-output>

When traversing a collection with a `for` loop, the contents may not change while the loop is in progress.

Luckily, there is a dictionary method for just this purpose:

```python
staff.clear()
```

<programming-exercise name='Invert a dictionary' tmcname='part05-19_invert_dictionary'>

Please write a function named `invert(dictionary: dict)`, which takes a dictionary as its argument. The dictionary should be inverted in place so that values become keys and keys become values.

An example of its use:

```python
s = {1: "first", 2: "second", 3: "third", 4: "fourth"}
invert(s)
print(s)
```

<sample-output>

{"first": 1, "second": 2, "third": 3, "fourth": 4}

</sample-output>

**NB:** the principles regarding lists covered [here](/part-5/2-references#using-lists-as-parameters-in-functions) also hold for dictionaries passed as arguments.

If you have trouble completing this exercise, the [visualisation tool](http://www.pythontutor.com/visualize.html#mode=edit) might help you understand what your code is or isn't doing.

</programming-exercise>

<programming-exercise name='Numbers spelled out' tmcname='part05-20_numbers_spelled_out'>

Please write a function named `dict_of_numbers()`, which returns a new dictionary. The dictionary should have the numbers from 0 to 99 as its keys. The value attached to each key should be the number spelled out in words. Please have a look at the example below:

```python
numbers = dict_of_numbers()
print(numbers[2])
print(numbers[11])
print(numbers[45])
print(numbers[99])
print(numbers[0])
```

<sample-output>

two
eleven
forty-five
ninety-nine
zero

</sample-output>

NB: Please don't formulate each spelled out number by hand. Figure out how you can use loops and dictionaries in your solution.

</programming-exercise>

## Using dictionaries for structured data

Dictionaries are very useful for structuring data. The following code will create a dictionary which contains some personal data:

```python
person = {"name": "Pippa Python", "height": 154, "weight": 61, "age": 44}
```

This means that we have here a person named Pippa Python, whose height is 154, weight 61, and age 44. The same information could just as well be stored in variables:

```python
name = "Pippa Python"
height = 154
weight = 61
age = 44
```

The advantage of a dictionary is that it is a collection. It collects related data under one variable, so it is easy to access the different components. This same advantage is offered by a list:

```python
person = ["Pippa Python", 153, 61, 44]
```

With lists, the programmer will have to remember what is stored at each index in the list. There is nothing to indicate that `person[2]` contains the weight and `person[3]` the age of the person. When using a dictionary this problem is avoided, as each bit of data is accessed through a named key.

Assuming we have defined multiple people using the same format, we can access their data in the following manner:

```python
person1 = {"name": "Pippa Python", "height": 154, "weight": 61, "age": 44}
person2 = {"name": "Peter Pythons", "height": 174, "weight": 103, "age": 31}
person3 = {"name": "Pedro Python", "height": 191, "weight": 71, "age": 14}

people = [person1, person2, person3]

for person in people:
    print(person["name"])

combined_height = 0
for person in people:
    combined_height += person["height"]

print("The average height is", combined_height / len(people))
```

<sample-output>

Pippa Python
Peter Pythons
Pedro Python
The average height is 173.0

</sample-output>

<programming-exercise name='Movie database' tmcname='part05-21_movie_database'>

Please write a function named `add_movie(database: list, name: str, director: str, year: int, runtime: int)`, which adds a new movie object into a movie database.

The database is a list, and each movie object in the list is a dictionary. The dictionary should contain the following keys.

* name
* director
* year
* runtime

The values attached to these keys are given as arguments to the function.

An example of its use:

```python
database = []
add_movie(database, "Gone with the Python", "Victor Pything", 2017, 116)
add_movie(database, "Pythons on a Plane", "Renny Pytholin", 2001, 94)
print(database)
```

<sample-output>

[{"name": "Gone with the Python", "director": "Victor Pything", "year": 2017, "runtime": 116}, {"name": "Pythons on a Plane", "director": "Renny Pytholin", "year": 2001, "runtime": 94}]

</sample-output>

</programming-exercise>

<programming-exercise name='Find movies' tmcname='part05-22_find_movies'>

Please write a function named `find_movies(database: list, search_term: str)`, which processes the movie database created in the previous exercise. The function should formulate a new list, which contains only the movies whose title includes the word searched for. Capitalisation is irrelevant here. A search for `ana` should return a list containing both `Anaconda` and `Management`.

An example of its use:

```python
database = [{"name": "Gone with the Python", "director": "Victor Pything", "year": 2017, "runtime": 116},
{"name": "Pythons on a Plane", "director": "Renny Pytholin", "year": 2001, "runtime": 94},
{"name": "Dawn of the Dead Programmers", "director": "M. Night Python", "year": 2011, "runtime": 101}]

my_movies = find_movies(database, "python")
print(my_movies)
```

<sample-output>

[{"name": "Gone with the Python", "director": "Victor Pything", "year": 2017, "runtime": 116}, {"name": "Pythons on a Plane", "director": "Renny Pytholin", "year": 2001, "runtime": 94}]

</sample-output>

</programming-exercise>

At this point in the course, you can choose to participate in a research study related to learning programming. Participation is voluntary and individual participants cannot be identified from the data gathered in the study. You can freely quit the experiment at any point. [Click here to begin the study!](https://runestone.academy/ns/books/published/p3pt/index.html)

<!---
A quiz to review the contents of this section:

<quiz id="6361eeca-a2e2-5577-892c-749706d754f0"></quiz>
-->
