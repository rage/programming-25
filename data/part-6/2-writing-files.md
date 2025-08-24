---
path: '/part-6/2-writing-files'
title: 'Writing files'
hidden: false
---

<text-box variant='learningObjectives' name="Learning objectives">

After this section

- You will know how to create files with Python code
- You will be able to write text based data to a file
- You will know how to create a CSV file

</text-box>

So far we have read data from files, but it is naturally also possible to write data to files. Typically a program processes data and stores the results in a file, so they can be used later or processed further by some other program.

We can create a new file every time we want to write data to a file, but we can also append new data to the end of an existing file. In both cases we use the `open` function from the previous section. For writing files the function requires a second argument.

## Creating a new file

If you want to create a new file, you would call the `open` function with the additional argument `w`, to signify that the file should be opened in write mode. So, the function call could look like this:

```python
with open("new_file.txt", "w") as my_file:
    # code to write something to the file
```

**NB: if the file already exists, all the contents will be overwritten**. It pays to be very careful when creating new files.

With the file open you can write data to it. You can use the method `write`, which takes the string that is to be written as its argument.

```python
with open("new_file.txt", "w") as my_file:
    my_file.write("Hello there!")
```

When you execute the program, a new file named `new_file.txt` appears in the directory. The contents would look like this:

<sample-data>

Hello there!

</sample-data>

If you want line breaks in the file, you will have to add them by hand - the `write` function doesn't work exactly like the more familiar `print` function, though they are similar. So, the following program

```python
with open("new_file.txt", "w") as my_file:
    my_file.write("Hello there!")
    my_file.write("This is the second line")
    my_file.write("This is the last line")
```

would result in a file with these contents:

<sample-data>

Hello there!This is the second lineThis is the last line

</sample-data>

Line breaks are achieved by adding new line characters `\n` to the argument strings:

```python
with open("new_file.txt", "w") as my_file:
    my_file.write("Hello there!\n")
    my_file.write("This is the second line\n")
    my_file.write("This is the last line\n")
```

Now the contents of `new_file.txt` would look like this:

<sample-data>

Hello there!
This is the second line
This is the last line

</sample-data>

<programming-exercise name='Inscription' tmcname='part06-10_inscription'>

Please write a program which asks for the name of the user and then creates an "inscription" in a file specified by the user. Please see the example below.

<sample-output>

Whom should I sign this to: **Ada**
Where shall I save it: **inscribed.txt**

</sample-output>

The contents of the file `inscribed.txt` would be

<sample-data>

Hi Ada, we hope you enjoy learning Python with us! Best, Mooc.fi Team

</sample-data>

**NB:** this exercise doesn't ask you to write any functions, so you should __not__ place any code within an `if __name__ == "__main__"` block.

</programming-exercise>

## Appending data to an existing file

If you want to append data to the end of a file, instead of overwriting the entire file, you should open the file in append mode with the argument `a`.

If the file doesn't yet exist, append mode works exactly like write mode.

The following program opens the file `new_file.txt` and appends a couple of lines of text to the end:

```python
with open("new_file.txt", "a") as my_file:
    my_file.write("This is the 4th line\n")
    my_file.write("And yet another line.\n")
```

After this program is executed the contents of the file would look like this:

<sample-output>

Hello there!
This is the second line
This is the last line
This is the 4th line
And yet another line.

</sample-output>

In programming practice, appending data to files is not a very common task.

More often a file is read, processed and overwritten in its entirety. For example, when the contents should change in the _middle_ of the file, it is usually easiest to overwrite the entire file.

<programming-exercise name='Diary' tmcname='part06-11_diary'>

Please write a program which works as a simply diary. The diary entries should be saved in the file `diary.txt`. When the program is executed, it should first read any entries already in the file.

NB: the automatic tests for this exercise will change the contents of the file. If you want to keep its contents, first make a copy of the file under a different name.

The program should work as follows:

<sample-output>

1 - add an entry, 2 - read entries, 0 - quit
Function: **1**
Diary entry: **Today I ate porridge**
Diary saved

1 - add an entry, 2 - read entries, 0 - quit
Function: **2**
Entries:
Today I ate porridge
1 - add an entry, 2 - read entries, 0 - quit
Function: **1**
Diary entry: **I went to the sauna in the evening**
Diary saved

1 - add an entry, 2 - read entries, 0 - quit
Function: **2**
Entries:
Today I ate porridge
I went to the sauna in the evening
1 - add an entry, 2 - read entries, 0 - quit
Function: **0**
Bye now!

</sample-output>

When the program is executed for the second time, this should happen:

<sample-output>

1 - add an entry, 2 - read entries, 0 - quit
Function: **2**
Entries:
Today I ate porridge
I went to the sauna in the evening
1 - add an entry, 2 - read entries, 0 - quit
Function: **0**
Bye now!

</sample-output>

**NB:** this exercise doesn't ask you to write any functions, so you should __not__ place any code within an `if __name__ == "__main__"` block.

</programming-exercise>

## Writing CSV files

CSV files can be written line by line with the `write` method just like any other file. The following example creates the file `coders.csv`, with each line containing the name, working environment, favourite language and years of experience of a single programmer. The fields are separated by a semicolon.

```python
with open("coders.csv", "w") as my_file:
    my_file.write("Eric;Windows;Pascal;10\n")
    my_file.write("Matt;Linux;PHP;2\n")
    my_file.write("Alan;Linux;Java;17\n")
    my_file.write("Emily;Mac;Cobol;9\n")
```

Executing this program would result in the following file:

<sample-output>

Eric;Windows;Pascal;10
Matt;Linux;PHP;2
Alan;Linux;Java;17
Emily;Mac;Cobol;9

</sample-output>

What if the data to be written is stored in computer memory in a list?

```python
coders = []
coders.append(["Eric", "Windows", "Pascal", 10])
coders.append(["Matt", "Linux", "PHP", 2])
coders.append(["Alan", "Linux", "Java", 17])
coders.append(["Emily", "Mac", "Cobol", 9])
```

We can build the string we want to write as an f-string, and write the ready line to the file like so:

```python
with open("coders.csv", "w") as my_file:
    for coder in coders:
        line = f"{coder[0]};{coder[1]};{coder[2]};{coder[3]}"
        my_file.write(line+"\n")
```

If each list of coder data was very long, with many more items, building the string by hand would be quite cumbersome. We can use a `for` loop to build the string instead:

```python
with open("coders.csv", "w") as my_file:
    for coder in coders:
        line = ""
        for value in coder:
            line += f"{value};"
        line = line[:-1]
        my_file.write(line+"\n")
```

## Clearing file contents and deleting files

Sometimes it is necessary to clear the contents of an existing file. Opening the file in write mode and closing the file immediately will achieve just this:

```python
with open("file_to_be_cleared.txt", "w") as my_file:
    pass
```

Now the `with` block only contains the command `pass`, which doesn't actually do anything. Python does not allow empty blocks, so the command is necessary here.

It is possible to also bypass the `with` block by using the following oneliner:

```python
open('file_to_be_cleared.txt', 'w').close()
```

<text-box variant='hint' name='Deleting files'>

You can also delete a file entirely. We will have to ask for help from the operating system to achieve this:

```python
# the command to delete files is in the os module
import os

os.remove("unnecessary_file.csv")
```

NB: this will not work when running the automatic tests on the course servers due to technical limitations in the testing environment. If you are asked to clear the contents of a file, use the methods described above.

</text-box>


<programming-exercise name='Filtering the contents of a file' tmcname='part06-12_filtering_file_contents'>

The file `solutions.csv` contains some solutions to mathematics problems:

```csv
Arto;2+5;7
Pekka;3-2;1
Erkki;9+3;11
Arto;8-3;4
Pekka;5+5;10
...jne...
```

As you can see above, on each line the format is `name_of_student;problem;result`. All the operations are either addition or subtraction, and each has exactly two operands.

Please write a function named `filter_solutions()` which

* Reads the contents of the file `solutions.csv`
* writes those lines which have a _correct_ result into the file `correct.csv`
* writes those lines which have an _incorrect_ result into the file `incorrect.csv`

Using the example above, the file `correct.csv` would contain the lines 

```sh
Arto;2+5;7
Pekka;3-2;1
Pekka;5+5;10
```

The other two would be in the file `incorrect.csv`.

Please write the lines in the same order as they appear in the original file. Do not change the original file.

NB: the function should have the exact same result, no matter how many times it is called. That is, it shouldn't matter if the function is called once

```python
filter_solutions()
```

or multiple times in a row

```python
filter_solutions()
filter_solutions()
filter_solutions()
filter_solutions()
```

After the execution, the contents of the files `correct.csv` and `incorrect.csv` should be exactly the same in either case.

</programming-exercise>

<programming-exercise name='Store personal data' tmcname='part06-13_store_personal_data'>

Please write a function named `store_personal_data(person: tuple)`, which takes a tuple containing some identifying information as its argument.

The tuple contains the following elements:

* Name (string)
* Age (integer)
* Height (float)

This should be processed and written into the file `people.csv`. The file may already contain some data; the new entry goes to the end of the file. The data should be written in the format

name;age;height

Each entry should be on a separate line. If we call the function with the argument `("Paul Paulson", 37, 175.5)`, the function should write this line to the end of the file:

`Paul Paulson;37;175.5`

</programming-exercise>

## Handling data in a CSV format

Let's write a program which assesses students' performance on a course. The program reads a CSV file, which contains weekly exercise points received by the students. The program then calculates the points total and determines the grade attained by each student. Finally, the program creates a CSV file containing the points total and grade for each student.

The CSV file given as input to the program looks like this:

<sample-data>

Peter;4;2;3;5;4;0;0
Paula;7;2;8;3;5;4;5
Susan;3;4;3;5;3;4;4
Emily;6;6;5;5;0;4;8

</sample-data>

The program logic is divided into three functions: reading the file and processing the contents into an accessible format, determining the grade, and writing the file. 

The file is read following the principles covered in the previous section. The data is stored in a dictionary, where the key is the student's name, and the value is a list of the points received by the student, in integer format:

```python
def read_weekly_points(filename):
    weekly_points = {}
    with open(filename) as my_file:
        for line in my_file:
            parts = line.split(";")
            point_list = []
            for points in parts[1:]:
                point_list.append(int(points))
            weekly_points[parts[0]] = point_list

    return weekly_points
```

The second function is for determining the grade based on the points received. This function is in turn used by the third function, which writes the results to the file.

```python
def grade(points):
    if points < 20:
        return 0
    elif points < 25:
        return 1
    elif points < 30:
        return 2
    elif points < 35:
        return 3
    elif points < 40:
        return 4
    else:
        return 5

def save_results(filename, weekly_points):
    with open(filename, "w") as my_file:
        for name, point_list in weekly_points.items():
            point_sum = sum(point_list)
            my_file.write(f"{name};{point_sum};{grade(point_sum)}\n")
```

This structure lets us write a very simple main function. Notice how the filenames for the files whch are read and written are given as arguments in the main function:

```python
weekly_points = read_weekly_points("weekly_points.csv")
save_results("results.csv", weekly_points)
```

When the main function is executed, the contents of the file `results.csv` created as a result looks like this:

<sample-data>

Peter;18;0
Paula;34;3
Susan;26;2
Emily;41;5

</sample-data>

Notice how each function defined above is relatively simple, and they all have a single responsibility. This is a common and advisable approach when programming larger wholes. The single reponsibility principle makes verifying functionality easier. It also makes it easier to make changes to the program later, and to add new features.

Say we wanted to add a function for printing out the grade for a single student. We already have a function which determines the student's grade, so we can use this in our new function:

```python
def get_grade(student_name, weekly_points):
    for name, point_list in weekly_points.items():
        if name == student_name:
            return grade(sum(point_list))


weekly_points = read_weekly_points("weekly_points.csv")
print(get_grade("Paula", weekly_points))

```

<sample-data>

3

</sample-data>

If we determine a certain functionality in the program needs fixing, in a well designed program the change will affect only some select sections of code, and it will be easier to determine where the changes should be made. For example, if we wanted to change the grade boundaries, we'd only need to implement the change in the function for determining the grade, and it would work also in all the other functions utilizing this function. If the code for this single functionality was implemented in multiple places, there would be a definite risk that we would not remember to change all the instances when changing the functionality.

<programming-exercise name='Course grading, part 4' tmcname='part06-14_course_grading_part_4'>

Let's revisit the course grading project from the previous section. 

As we left if last time, the program read and processed files containing student information, completed exercises and exam results. We'll add a file containing information about the course. An example of the format of the file:

<sample-data>

<pre>

name: Introduction to Programming
study credits: 5
</pre>

</sample-data>

The program should then create two files. There should be a file called `results.txt` with the following contents:

<sample-data>

<pre>
Introduction to Programming, 5 credits
======================================
name                          exec_nbr  exec_pts. exm_pts.  tot_pts.  grade
pekka peloton                 21        5         9         14        0
jaana javanainen              27        6         11        17        1
liisa virtanen                35        8         14        22        3
</pre>

</sample-data>

The statistics section is identical to the results printed out in part 3 of the project. The only addition here is the header section. 

Additionally, there should be a file called `results.csv` with the following format:

<sample-data>

<pre>
12345678;pekka peloton;0
12345687;jaana javanainen;1
12345699;liisa virtanen;3
</pre>

</sample-data>

When the program is executed, it should look like this:

<sample-output>

Student information: **students1.csv**
Exercises completed: **exercises1.csv**
Exam points: **exam_points1.csv**
Course information: **course1.txt**
Results written to files results.txt and results.csv

</sample-output>

That is, the program only asks for the names of the input files. All output should be written to the files. The user will only see a message confirming this.

**NB:** this exercise doesn't ask you to write any functions, so you should __not__ place any code within an `if __name__ == "__main__"` block.

</programming-exercise>



<programming-exercise name='Word search' tmcname='part06-15_word_search'>

The exercise template includes the file `words.txt`, which contains words in English.

Please write a function named `find_words(search_term: str)`. It should return a list containing all the words in the file which match the search term.

The search term may include lowercase letters and the following wildcard characters:

* A dot `.` means that any single character is acceptable in its place. For example, `ca.` would yield words like _cat_ and _car_, `p.ng` would yield words like _ping_ and _pong_, and `.a.e` would yield words like _sane_, _care_ and _late_.
* An asterisk `*` at the _end_ of the search term means that any word which _begins_ with the search term is acceptable. An asterisk at the _beginning_ of the search term means that any word which _ends_ with the search term is acceptable. For example, `ca*` would yield words like _california_, _cat_, _caring_ and _catapult_, while `*ane` would yield words like _crane_, _insane_ and _aeroplane_. There can only ever be a single asterisk in the search term.
* If there are no wildcard characters in the search term, only words which match the search term exactly are returned. 

You may assume both wildcards are never used in the same search term.

The words in the file are all written in lowercase. You may also assume the argument to the function will be in lowercase entirely.

If no matching words are found, the function should return an empty list.

Hint: the Pythons string methods `startswith()` and `endswith()` may be useful here. You can search for more information about them online.

An example of the function in action:

```python
print(find_words("*vokes"))
```

<sample-output>

['convokes', 'equivokes', 'evokes', 'invokes', 'provokes', 'reinvokes', 'revokes']

</sample-output>

</programming-exercise>

<programming-exercise name='Dictionary stored in a file' tmcname='part06-16_dictionary_file'>

Please write a program which functions as a dictionary. The user can type in new entries or look for existing entries.

The program should work as follows:

<sample-output>

1 - Add word, 2 - Search, 3 - Quit
Function: **1**
The word in Finnish: **auto**
The word in English: **car**
Dictionary entry added
1 - Add word, 2 - Search, 3 - Quit
Function: **1**
The word in Finnish: **roska**
The word in English: **garbage**
Dictionary entry added
1 - Add word, 2 - Search, 3 - Quit
Function: **1**
The word in Finnish: **laukku**
The word in English: **bag**
Dictionary entry added
1 - Add word, 2 - Search, 3 - Quit
Function: **2**
Search term: **bag**
roska - garbage
laukku - bag
1 - Add word, 2 - Search, 3 - Quit
Function: **2**
Search term: **car**
auto - car
1 - Add word, 2 - Search, 3 - Quit
Function: **2**
Search term: **laukku**
laukku - bag
1 - Add word, 2 - Search, 3 - Quit
Function: **3**
Bye!

</sample-output>

The dictionary entries should be written to a file called `dictionary.txt`. The program should first read the contents of the file. New entries are written to the end of the file whenever they are added to the dictionary.

The format of the data stored in the dictionary is up to you.

**NB:** the automatic tests for this exercise may change the contents of the file. If you want to keep its contents, first make a copy of the file under a different name.

**NB2:** this exercise doesn't ask you to write any functions, so you should __not__ place any code within an `if __name__ == "__main__"` block.


</programming-exercise>

<!---
A quiz to review the contents of this section:

<quiz id="69694e01-4c47-5b9d-8a00-b0d96a477dc7"></quiz>
-->
