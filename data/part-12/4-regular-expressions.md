---
path: '/part-12/4-regular-expressions'
title: 'Regular expressions'
hidden: false
---

<text-box variant='learningObjectives' name="Learning objectives">

After this section

- You will know what regular expressions are
- You will be able to use regular expressions in your own programs

</text-box>

We have already established that Python is an excellent environment for processing text. One additional powerful tool for text processing is _regular expressions_, often shortened as _regex_ or _regexp_. They are a way of selecting and searching for strings which follow a certain pattern. This section introduces you to the basics of regular expression, but you will find much more information online, including in the Python [tutorial](https://docs.python.org/3/howto/regex.html).

## What are regular expressions?

Regular expressions are not just a Python feature. They represent, in a way, a programming language within a programming language. They are, to an extent, compatible across many different programming languages. Regular expressions have their own specific syntax. The idea is to define a collection of strings which follow certain rules.

Let's begin with a simple example, before diving deeper into the syntax:

```python
import re

words = ["Python", "Pantone", "Pontoon", "Pollute", "Pantheon"]

for word in words:
    # the string should begin with "P" and end with "on"
    if re.search("^P.*on$", word):
        print(word, "found!")
```

<sample-output>

Python found!
Pontoon found!
Pantheon found!

</sample-output>

We need to `import` the `re` module in order to use regular expressions in Python. The `re` module contains many functions for working with regular expressions. In the example above, the `search` function takes two string arguments: the pattern string, and the target string where the pattern is looked for.

This second example looks for any numbers in a string. The `findall` function returns a list of all the instances which match the pattern:

```python
import re

sentence = "First, 2 !#third 44 five 678xyz962"

numbers = re.findall("\d+", sentence)

for number in numbers:
    print(number)
```

<sample-output>

2
44
678
962

</sample-output>

## The syntax of regular expressions

Let's get familiar with the basic syntax of regular expressions. Most of the following examples make use of this testing program:

```python
import re

expression = input("Please type in an expression: ")

while True:
    input_string = input("Please type in a string: ")
    if input_string == "":
        break
    if re.search(expression, input_string):
        print("Found!")
    else:
        print("Not found.")
```

### Alternate substrings

The vertical bar `|`, also called the pipe character, allows you to match alternate substrings. Its significance is thus _or_. For example, the expression `911|112` matches strings which include either the substring `911` or the substring `112`.

An example with the testing program:

<sample-output>

Please type in an expression: **aa|ee|ii**
Please type in a string: **aardvark**
Found!
Please type in a string: **feelings**
Found!
Please type in a string: **radii**
Found!
Please type in a string: **smooch**
Not found.
Please type in a string: **continuum**
Not found.

</sample-output>


### Groups of characters

Square brackets are used to signify groups of accepted characters. For example, the expression `[aeio]` would match all strings which contain any of the characters a, e, i, or o. 

A dash is also allowed for matching ranges of characters. For example, the expression `[0-68a-d]` would match all strings which contain a digit between 0 and 6, or an eight, or a character between a and d. In this notation all ranges are _inclusive_.

Combining two sets of brackets lets you match two consecutive characters. For example, the expression `[1-3][0-9]` would match any two digit number between 10 and 39, inclusive.

An example with the testing program:

<sample-output>

Please type in an expression: **[C-FRSO]**
Please type in a string: **C**
Found!
Please type in a string: **E**
Found!
Please type in a string: **G**
Not found.
Please type in a string: **R**
Found!
Please type in a string: **O**
Found!
Please type in a string: **T**
Not found.

</sample-output>

### Repeated matches

Any part of an expression can be repeated with the following operators:

* `*` repeats for any number of times, including zero
* `+` repeats for any number of times, but at least once
* `{m}` repeats for exactly `m` times

These operators work on the part of the expression immediately preceding the operator. For example, the expression `ba+b` would match the substrings `bab`, `baab` and `baaaaaaaaaaab`, among others. The expression `A[BCDE]*Z` would match the substrings `AZ`, `ADZ` or `ABCDEBCDEBCDEZ`, among others.

An example with the testing program:

<sample-output>

Please type in an expression: **1[234]\*5**
Please type in a string: **15**
Found!
Please type in a string: **125**
Found!
Please type in a string: **145**
Found!
Please type in a string: **12342345**
Found!
Please type in a string: **126**
Not found.
Please type in a string: **165**
Not found.

</sample-output>


### Other special characters

A dot is a wildcard character which can match any single character. For example, the expression `c...o` would match any five character substring beginning with a `c` and ending with an `o`, such as `c-3po` or `cello`.

The `^` character specifies that the match must be at the beginning of the string, and `$` specifies that the match must be at the end of the string. These can also be used to exclude from the matches any other characters than those specified:

<sample-output>

Please type in an expression: **\^[123]\*$**
Please type in a string: **4**
Not found.
Please type in a string: **1221**
Found!
Please type in a string: **333333333**
Found!

</sample-output>

Sometimes you need to match for the special characters reserved for regular expression syntax. The backslash `\` can be used to _escape_ special characters. So, the expression `1+` matches one or more numbers `1`, but the expression `1\+` matches the string `1+`.

<sample-output>

Please type in an expression: **^\\\***
Please type in a string: **moi\***
Not found.
Please type in a string: **m\*o\*i**
Not found.
Please type in a string: **\*moi**
Found!

</sample-output>

Round brackets can be used to group together different parts of the expression. For example, the expression `(ab)+c` would match the substrings `abc`, `ababc` and `ababababababc`, but not the strings `ac` or `bc`, as the entire substring `ab` would have to appear at least once.

<sample-output>

Please type in an expression: **^(jabba).\*(hut)$**
Please type in a string: **jabba the hut**
Found!
Please type in a string: **jabba a hut**
Found!
Please type in a string: **jarjar the hut**
Not found.
Please type in a string: **jabba the smut**
Not found.

</sample-output>

<programming-exercise name='Regular expressions' tmcname='part12-14_regular_expressions'>

Here are some exercises for familiarizing yourself with regular expression syntax.

## Days of the week

Using a regular expression, please write a function named `is_dotw(my_string: str)`. The function should return `True` if the string passed as an argument contains an abbreviation for a day of the week (Mon, Tue, Wed, Thu, Fri, Sat, Sun).

Some examples of how the function should work:

```python
print(is_dotw("Mon"))
print(is_dotw("Fri"))
print(is_dotw("Tui"))
```

<sample-output>

True
True
False

</sample-output>

## Check for vowels

Please write a function named `all_vowels(my_string: str)` which uses a regular expression to check whether all characters in the given string are vowels.

Some examples of how the function should work:

```python
print(all_vowels("eioueioieoieou"))
print(all_vowels("autoooo"))
```

<sample-output>

True
False

</sample-output>

## Time of day

Please write a function named `time_of_day(my_string: str)` which uses a regular expression to check whether a string in the format `XX:YY:ZZ` is a valid time in the 24-hour format, with two digits each for hours, minutes and seconds.

Some examples of how the function should work:

```python
print(time_of_day("12:43:01"))
print(time_of_day("AB:01:CD"))
print(time_of_day("17:59:59"))
print(time_of_day("33:66:77"))
```

<sample-output>

True
False
True
False

</sample-output>

</programming-exercise>

## Grand finale

To finish off this part of the material, let's work some more on objects and classes by building a slightly more extensive program. This exercise does not necessarily involve regular expressions, but the sections on [functions as arguments](/part-12/1-functions-as-arguments) and [list comprehensions](/part-11/1-list-comprehensions) will likely be useful.

You may also find the example set in [part 10](/part-10/4-application-development) helpful.

<programming-exercise name='Hockey statistics' tmcname='part12-15_hockey_statistics'>

In this exercise you will build an application for examining hockey league statistics from the NHL in a couple of different ways.

The exercise template contains two JSON files: `partial.json` and `all.json`. The first of these is mostly meant for testing. The latter contains a lot of data, as all the NHL player stats for the 2019-20 season are included in the file.

The entry for a single player is in the following format:

```json
{
    "name": "Patrik Laine",
    "nationality": "FIN",
    "assists": 35,
    "goals": 28,
    "penalties": 22,
    "team": "WPG",
    "games": 68
}
```

Both files contain a list of entries in the above format.

If you need a refresher on handling JSON files, please take a look at [part 7 of this course material](/part-7/4-data-processing#reading-json-files).

## Search and list

Please write an interactive application which first asks for the name of the file, and then offers the following functions:

- search by name for a single player's stats
- list all the abbreviations for team names in alphabetical order
- list all the abbreviations for countries in alphabetical order

These functionalities grant you one exercise point. Your application should now work as follows:

<sample-output>

file name: **partial.json**
read the data of 14 players

commands:
0 quit
1 search for player
2 teams
3 countries
4 players in team
5 players from country
6 most points
7 most goals

command: **1**
name: **Travis Zajac**
<pre>
Travis Zajac         NJD   9 + 16 =  25
</pre>

command: **2**
BUF
CGY
DAL
NJD
NYI
OTT
PIT
WPG
WSH

command: **3**
CAN
CHE
CZE
SWE
USA

command: **0**

</sample-output>

NB: the printout format for a player must be exactly as follows:

<sample-output>

<pre>
Leon Draisaitl       EDM  43 + 67 = 110
Connor McDavid       EDM  34 + 63 =  97
Travis Zajac         NJD   9 + 16 =  25
Mike Green           EDM   3 +  8 =  11
Markus Granlund      EDM   3 +  1 =   4
123456789012345678901234567890123456789
</pre>

</sample-output>

The last line in the sample above is there to help you calculate the widths of the different fields in the output; you should not print the numbers line yourself in your final submission. 

The abbreviation for the team is printed from the 22nd character onwards. The `+` sign is the 30th character and the `=` sign is the 35th character. All the fields should be justified to the right edge. All whitespace is space characters.

F-strings are probably the easiest way to achieve the required printout. The process is similar to [this exercise](/part-6/1-reading-files#programming-exercise-course-grading-part-3) from part 6.

## List players by points

These two functionalities will grant you a second exercise point:

- list players in a specific team in the order of points scored, from highest to lowest. Points equals _goals_ + _assists_
- list players from a specific country in the order of points scored, from highest to lowest

Your application should now work as follows:

<sample-output>

file name: **partial.json**
read the data of 14 players

commands:
0 quit
1 search for player
2 teams
3 countries
4 players in team
5 players from country
6 most points
7 most goals

command: **4**
team: **OTT**
<pre>
Drake Batherson      OTT   3 +  7 =  10
Jonathan Davidsson   OTT   0 +  1 =   1
</pre>

command: **5**
country: **CAN**
<pre>
Jared McCann         PIT  14 + 21 =  35
Travis Zajac         NJD   9 + 16 =  25
Taylor Fedun         DAL   2 +  7 =   9
Mark Jankowski       CGY   5 +  2 =   7
Logan Shaw           WPG   3 +  2 =   5
</pre>

command: **0**

</sample-output>

## Most successful players

These two functionalities will grant you a third exercise point:

- list of `n` players who've scored the most points
  - if two players have the same score, whoever has scored the higher number of goals comes first
- list of `n` players who've scored the most goals
  - if two players have the same number of goals, whoever has played the lower number of games comes first

Your application should now work as follows:

<sample-output>

file name: **partial.json**
read the data of 14 players

commands:
0 quit
1 search for player
2 teams
3 countries
4 players in team
5 players from country
6 most points
7 most goals

command: **6**
how many: **2**
<pre>
Jakub Vrana          WSH  25 + 27 =  52
Jared McCann         PIT  14 + 21 =  35
</pre>

command: **6**
how many: **5**

<pre>
Jakub Vrana          WSH  25 + 27 =  52
Jared McCann         PIT  14 + 21 =  35
John Klingberg       DAL   6 + 26 =  32
Travis Zajac         NJD   9 + 16 =  25
Conor Sheary         BUF  10 + 13 =  23
</pre>

command: **7**
how many: **6**

<pre>
Jakub Vrana          WSH  25 + 27 =  52
Jared McCann         PIT  14 + 21 =  35
Conor Sheary         BUF  10 + 13 =  23
Travis Zajac         NJD   9 + 16 =  25
John Klingberg       DAL   6 + 26 =  32
Mark Jankowski       CGY   5 +  2 =   7
</pre>

command: **0**

</sample-output>

</programming-exercise>

Please respond to a quick questionnaire on this part of the course.

<quiz id="1e16612c-1004-5cf5-9525-6669104f50ec"></quiz>
