---
path: '/part-7/4-data-processing'
title: 'Data processing'
hidden: false
---

<text-box variant='learningObjectives' name="Learning objectives">

After this section

- You will know how to use a module to process CSV files
- You will know how to use a module to process JSON files
- You will be able to retrieve and read files from the internet

</text-box>

## Reading CSV files

CSV is such a simple format that so far we have accessed the with hand-written code. There is, however, a ready-made module in the Python standard library for working with CSV files: [csv](https://docs.python.org/3/library/csv.html). It works like this:

```python
import csv

with open("test.csv") as my_file:
    for line in csv.reader(my_file, delimiter=";"):
        print(line)
```

The above code reads all lines in the CSV file `test.csv`, separates the contents of each line into a list using the delimiter `;`, and prints each list. So, assuming the contents of the line are as follows:

```x
012121212;5
012345678;2
015151515;4
```

The code would print out this:

<sample-output>

['012121212', '5']
['012345678', '2']
['015151515', '4']

</sample-output>

Since the CSV format is so simple, what's the use of having a separate module when we can just as well use the `split` function? Well, for one, the way the module is built, it will also work correctly if the values in the file are strings, which may also contain the delimiter character. If some line in the file looked like this

```x
"aaa;bbb";"ccc;ddd"
```

the above code would produce this:

<sample-output>

['aaa;bbb', 'ccc;ddd']

</sample-output>

Using the `split` function would also split within the strings, which would likely break the data, and our program in the process.

## Reading JSON files

CSV is just one of many machine-readable data formats. [JSON](https://www.json.org/json-en.html) is another, and it is used often when data has to be transferred between applications.

JSON files are text files with a strict format, which is perhaps a little less accessible to the human eye than the CSV format. The following example uses the file `courses.json`, which contains information about some courses:

```x
[
    {
        "name": "Introduction to Programming",
        "abbreviation": "ItP",
        "periods": [1, 3]
    },
    {
        "name": "Advanced Course in Programming",
        "abbreviation": "ACiP",
        "periods": [2, 4]
    },
    {
        "name": "Database Application",
        "abbreviation": "DbApp",
        "periods": [1, 2, 3, 4]
    }
]
```

The structure of a JSON file might look quite familiar to you by now. The JSON file above looks exactly like a Python list, which contains three Python dictionaries.

The standard library has a module for working with JSON files: [json](https://docs.python.org/3/library/json.html). The function `loads` takes any argument passed in a JSON format and transforms it into a Python data structure. So, processing the `courses.json` file with the code below

```python
import json

with open("courses.json") as my_file:
    data = my_file.read()

courses = json.loads(data)
print(courses)
```

would print out the following:

<sample-output>

[{'name': 'Introduction to Programming', 'abbreviation': 'ItP', 'periods': [1, 3]}, {'name': 'Advanced Course in Programming', 'abbreviation': 'ACiP', 'periods': [2, 4]}, {'name': 'Database Application', 'abbreviation': 'DbApp', 'periods': [1, 2, 3, 4]}]

</sample-output>

If we also wanted to print out the name of each course, we could expand our program with a `for` loop:

```python
for course in courses:
    print(course["name"])
```

<sample-output>

Introduction to Programming
Advanced Course in Programming
Database Application

</sample-output>


<programming-exercise name='Handling JSON files' tmcname='part07-12_json_files'>

Let's have a look at a JSON file, which contains some information about students in the following format:

```json
[
    {
        "name": "Peter Pythons",
        "age": 27,
        "hobbies": [
            "coding",
            "knitting"
        ]
    },
    {
        "name": "Jean Javanese",
        "age": 24,
        "hobbies": [
            "coding",
            "rock climbing",
            "reading"
        ]
    }
]
```

Please write a function named `print_persons(filename: str)`, which reads a JSON file in the above format, and prints the contents as shown below. The file may contain any number of entries.

<sample-output>

Peter Pythons 27 years (coding, knitting)
Jean Javanese 24 years (coding, rock climbing, reading)

</sample-output>

The hobbies should be listed in the same order as they appear in the JSON file.

</programming-exercise>

## Retrieving a file from the internet

The Python standard library also contains modules for dealing with online content, and one useful function is [urllib.request.urlopen](
https://docs.python.org/3/library/urllib.request.html#urllib.request.urlopen). You are encouraged to have a look at the entire module, but the following example should be enough for you to get to grips with the function. It can be used to retrieve content from the internet, so it can be processed in your programs.

The following code would print out the contents of the University of Helsinki front page:

```python
import urllib.request

my_request = urllib.request.urlopen("https://helsinki.fi")
print(my_request.read())
```

Pages intended for human eyes do not usually look very pretty when their code is printed out. In the following examples, however, we will work with machine-readable _data_ from an online source. Much of the machine-readable data available online is in JSON format.

<programming-exercise name='Course statistics' tmcname='part07-13_course_statistics'>

#### Retrieving the list of active courses

At the address <https://studies.cs.helsinki.fi/stats-mock/api/courses> you will find basic information about some of the courses offered by the University of Helsinki Department of Computer Science, in JSON format.

Please write a function named `retrieve_all()`, which retrieves the data of all the courses which are currently active (the field `enabled` has the value `true`). These should be returned as a list of tuples, in the following format:

<sample-output>

<pre>
[
    ('Full Stack Open 2020', 'ofs2019', 2020, 201),
    ('DevOps with Docker 2019', 'docker2019', 2019, 36),
    ('DevOps with Docker 2020', 'docker2020', 2020, 36),
    ('Beta DevOps with Kubernetes', 'beta-dwk-20', 2020, 28)
]
</pre>

</sample-output>

Each tuple contains the following fields from the original data:

- the name of the course: `fullName`
- `name`
- `year`
- the sum of the values listed in `exercises`


**NB**: It is essential that you retrieve the data with the function `urllib.request.urlopen`, or the automated tests may not work correctly.

**NB2:** The tests are designed so that they slightly modify the data retrieved from the internet, to make sure you do not hard-code your return values.

**NB3:** Some Mac users have come across the following issue:

```sh
File "/Library/Frameworks/Python.framework/Versions/3.8/lib/python3.8/urllib/request.py", line 1353, in do_open
    raise URLError(err)
urllib.error.URLError: <urlopen error [SSL: CERTIFICATE_VERIFY_FAILED] certificate verify failed: unable to get local issuer certificate (_ssl.c:1124)>
```

The solution depends on how Python is installed on your machine. In some cases executing the following in a terminal helps:

```sh
cd "/Applications/Python 3.8/"
sudo "./Install Certificates.command
```

The path used in the `cd` command above depends on the version of Python you have installed. The path may also be, for example, `"/Applications/Python 3.9/"`.

[Various solutions](https://stackoverflow.com/questions/27835619/urllib-and-ssl-certificate-verify-failed-error) to the problem have been suggested.

One trick some have found useful:

```python
import urllib.request
import json
import ssl # add this library to your import section

def retrieve_all():
    # add the following line to the beginning of all your functions
    context = ssl._create_unverified_context()
    # the rest of your function
```

Another potential workaround:

 ```python
import urllib.request
import certifi # add this library to your import section
import json

def retrieve_all():
    address = "https://studies.cs.helsinki.fi/stats-mock/api/courses"
    # add a second argument to the function call
    request = urllib.request.urlopen(address, cafile=certifi.where())
    # the rest of your function
```

#### Retrieving the data for a single course

Each course also has its own URL, where more specific weekly data about the course is available. The URLs follow the format <https://studies.cs.helsinki.fi/stats-mock/api/courses/****/stats>, where you would replace the stars with the contents of the field `name` for the course you want to access.

For example, the data for the course `docker2019` is at the address <https://studies.cs.helsinki.fi/stats-mock/api/courses/docker2019/stats>.

Please write a function named `retrieve_course(course_name: str)`, which returns statistics for the specified course, in dictionary format.

For example, the function call `retrieve_course("docker2019")` would return a dictionary with the following contents:

<sample-output>

<pre>
{
    'weeks': 4,
    'students': 220,
    'hours': 5966,
    'hours_average': 27,
    'exercises': 4988,
    'exercises_average': 22
}
</pre>

</sample-output>

The values in the dictionary are determined as follows:

- `weeks`: the number of JSON object literals retrieved
- `students`: the maximum number of students in all the weeks
- `hours`: the sum of all `hour_total` values in the different weeks
- `hours_average`: the `hours` value divided by the `students` value (rounded down to the closest integer value)
- `exercises`: the sum of all `exercise_total` values in the different weeks
- `exercises_average`: the `exercises` value divided by the `students` value (rounded down to the closest integer value)

**NB**: See the notices in Part 1 of the exercise, as they apply here, too.

**NB2**: The Python [math](https://docs.python.org/3/library/math.html) module has a useful function for rounding down integers.

</programming-exercise>

<programming-exercise name='Who cheated' tmcname='part07-14_who_cheated'>

The file `start_times.csv` contains individual start times for a programming exam, in the format `name;hh:mm`. An example:

```csv
jarmo;09:00
timo;18:42
kalle;13:23
```

Additionally, the file `submissions.csv` contains points and handin times for individual exercises. The format here is `name;task;points;hh:mm`. An example:

```csv
jarmo;1;8;16:05
timo;2;10;21:22
jarmo;2;10;19:15
jne...
```

Your task is to find the students who spent over 3 hours on the exam tasks. That is, any student whose _any_ task was handed in over 3 hours later than their exam start time is labelled a cheater. There may be more than one submission for the same task for each student. You may assume all times are within the same day.

Please write a function named `cheaters()`, which returns a list containing the names of the students who cheated

</programming-exercise>

<programming-exercise name='Who cheated, version 2' tmcname='part07-15_who_cheated_2'>

You have the CSV files from the previous exercise at your disposal again. Please write a function named `final_points()`, which returns the final exam points received by the students, in a dictionary format, following these criteria:

* If there are multiple submissions for the same task, the submission with the highest number of points is taken into account.
* If the submission was made over 3 hours after the start time, the submission is ignored.

The tasks are numbered 1 to 8, and each submission is graded with 0 to 6 points.

In the dictionary returned the key should be the name of the student, and the value the total points received by the student.

Hint: nested dictionaries might be a good approach when processing the tasks and submission times of each student.

</programming-exercise>

## Looking for modules

The official Python documentation contains information on all modules available in the standard library:

* https://docs.python.org/3/library/

In addition to the standard library, the internet is full of freely available Python modules for different purposes. Some commonly used modules are listed here:

* https://wiki.python.org/moin/UsefulModules

<programming-exercise name='Spell checker, version 2' tmcname='part07-16_spellchecker_2'>

In this exercise you will write an improved version of the Spell checker from the [previous part](/part-6/1-reading-files).

Just like in the previous version, the program should ask the user to type in a line of text. Your program should then perform a spell check, and print out feedback to the user, so that all misspelled words have stars around them. Additionally, _the program should print out a list of suggestions for the misspelled words_.

Please have a look at the following two examples.

<sample-output>

write text: **We use ptython to make a spell checker**
<pre>
We use *ptython* to make a spell checker
suggestions:
ptython: python, pythons, typhon
</pre>

</sample-output>

<sample-output>

write text: **this is acually a good and usefull program**
<pre>
this is *acually* a good and *usefull* program
suggestions:
acually: actually, tactually, factually
usefull: usefully, useful, museful
</pre>

</sample-output>

The suggestions should be determined with the function [get\_close\_matches](https://docs.python.org/3/library/difflib.html#difflib.get_close_matches) from the Python standard library module [difflib](https://docs.python.org/3/library/difflib.html).

**NB**: For the automatic tests to work correctly, please use the function with the "default settings". That is, please pass only two arguments to the function: the misspelled word, and the word list.

</programming-exercise>

<!---
<quiz id="311e3116-a763-50b5-b79e-056fdccb3394"></quiz>
-->
