<br>
<img style="float:left" src="http://ipython.org/_static/IPy_header.png" />
<br>

# Session 1: Orientation

<br>
Welcome to the *IPython Notebook*. Through this interface, you'll be learning a
lot of things:

* A Programming language: **Python**
* A Python library: **NLTK**
* Overlapping research areas: **Corpus linguistics**, **Natural language
processing**, **Distant reading**
* Additional skills: **Regular Expressions**, some **Shell commands**, and
**tips on managing your data**

You can head
[here](https://github.com/resbaz/lessons/blob/master/nltk/README.md) for the
fully articulated overview of the course, but we'll almost always stay within
IPython.
Remember, everything we cover here will remain available to you after ResBaz is
over, including these Notebooks. It's all accessible at the [ResBaz
GitHub](https://github.com/resbaz/lessons/tree/master/nltk).

**Any questions before we begin?**

Alright, we're off!

## Text as data

Programming languages like Python are great for processing data. In order to
apply it to *text*, we need to think about our text as data.
This means being aware of how text is structured, what extra information might
be encoded in it, and how to manage to give the best results.

## What is the Natural Language Toolkit?

<br>
We'll be covering some of the theory behind corpus linguistics later on, but
let's start by looking at some of the tasks NLTK can help you with.

NLTK is a Python Library for working with written language data. It is free and
extensively documented. Many areas we'll be covering are treated in more detail
in the NLTK Book, available free online from [here](http://www.nltk.org/book/).

> Note: NLTK provides tools for tasks ranging from very simple (counting words
in a text) to very complex (writing and training parsers, etc.). Many advanced
tasks are beyond the scope of this course, but by the time we're done, you
should understand Python and NLTK well enough to perform these tasks on your
own!

We will start by importing NLTK, setting a path to NLTK resources, and
downloading some additional stuff.

```python
import nltk # imports all the nltk basics

# for cloud-based ipython only:
#user_nltk_dir = "/home/researcher/nltk_data" # specify our data directory
#if user_nltk_dir not in nltk.data.path: # make sure nltk can access this dir
    #nltk.data.path.insert(0, user_nltk_dir)
#nltk.download("book", download_dir=user_nltk_dir) # download book materials to data directory
```

Oh, we've got to import some corpora used in the book as well...

```python
from nltk.book import *  # asterisk means 'everything'
```

Importing the book has assigned variable names to ten corpora. We can call these
names easily:

```python
#text1
text2
#text3
```

### Exploring vocabulary

NLTK makes it really easy to get basic information about the size of a text and
the complexity of its vocabulary.

*len* gives the number of symbols or 'tokens' in your text. This is the total
number of words and items of punctuation.

*set* gives you a list of all the tokens in the text, without the duplicates.

Hence, **len(set(text3))** will give you the total number unique tokens.
Remember this still includes punctuation.

sorted() places items in the list into alphabetical order, with punctuation
symbols and capitalised words first.

```python
len(text3)
```

```python
len(set(text3))
```

```python
sorted(set(text3)) [:50]
```

We can investigate the *lexical richness* of a text. For example, by dividing
the total number of words by the number of unique words, we can see the average
number of times each word is used.
We can also count the number of times a word is used and calculate what
percentage of the text it represents.

```python
len(text3)/len(set(text3))
```

```python
text4.count("American")
```

**Challenge!**

How would you calculate the percentage of Text 4 that is taken up by the word
"America"?

```python
100.0*text4.count("America")/len(text4) 
```

### Exploring text - concordances, similar contexts, dispersion

'Concordance' shows you a word in context and is useful if you want to be able
to discuss the ways in which a word is used in a text.
'Similar' will find words used in similar contexts; remember it is not looking
for synonyms,
although the results may include synonyms

```python
text1.concordance("monstrous")
```

```python
text1.similar("monstrous")
```

```python
text2.similar("monstrous")
```

```python
text2.common_contexts(["monstrous", "very"])
```

Python also lets you create graphs to display data.
To represent information about a text graphically, import the Python library
*numpy*. We can then generate a dispersion plot that shows where given words
occur in a text.

```python
import numpy
# allow visuals to show up in this interface-
% matplotlib inline 
text1.dispersion_plot(["whale"])
```

**Challenge!**
<br>
Create a dispersion plot for the terms "citizens", "democracy", "freedom",
"duties" and "America" in the innaugural address corpus.
What do you think it tells you?

```python
text4.dispersion_plot(["citizens", "democracy", "freedom", "duties", "America"]) # plot five words longitudinally
```

## How Python works

We've seen a bit now of how NLTK can help you to interrogate a text. Let's back
up and talk about Python itself and the environment we're using.

```python
# A simple welcome message printer.
# Anything after a hash is ignored
# Run a cell with shift+enter

condition = True
if condition is True:
    print 'Welcome to Python and the IPython Notebook.'
```

Success! *And so it begins ... *

## The IPython Notebook

Before we start coding, we should familiarise ourselves with the IPython
Notebook interface. Click *Help* --\> *User interface tour* to begin.
<br>
Keyboard shortcuts come in very handy. Click *Help* --\> *Keyboard shortcuts* to
get an overview. *The more you code, the less you'll want to use your mouse!*

## Python: core concepts

If you're new to Python, there are a few core concepts that will help you
understand how everything works. Here, we'll cover:

* Significant whitespace
* Input/output types
* Commands and arguments
* Defining functions
* Importing libraries, functions, etc.

### Significant Whitespace

One thing that makes Python unique is that whitespace at the start of the line
(use a tab for consistency!) is meaningful.
In many other languages, whitespace at the start of lines is simply a
readability convention.

```python
# Fix this whitespace problem!

string = 'user'
if string == 'user':
    print 'Phew, fixed.'
```

So, whitespace tells both Python and human readers where things start and stop.
You should be able to get different kinds of output depending on how you indent
the code below.

```python
# \n means 'newline character'---i.e. print a line break
print 'Python\nis\n'
for i in ['very', 'really', 'truly']:  # repeat three times, quite arbitrarily
    print i + '\n'
    if i is 'truly':  # nested conditional
        print 'interesting!'
    else:
        print 'complicated!'
print 'day.'  # at present, this occurs after the three repetitions.
```

### Input/Output Types

* Python understands different *types* of input, including *string*, *unicode
string*, *integer*, *item*, *tuple* and *dict*.
* Different types of information behave in different ways, and the ways they are
represented visually are different as well.
* You need to always make sure your input types are correct, or Python won't
know what to do with them.
* For example, if you're trying to do maths, you'll want to be working with
*integers*:

```python
1 + 2  # integer plus integer
```

```python
1 + '2'  # integer plus string
```

Note the error message. These will help you to understand what went wrong.
The first part looks like gobbledygook, but the rest is helpful. Line 3 of error
tells us what command
was executing when the error happened, which can assist in isolating a problem.
The leading digit is the line number.
Line 4 actually tells us what the error was - that's what we would have googled
if we were looking for a solution.

You can determine the type of data stored in a variable with *type()*. Below are
the most common types. Note how quotation marks, and brackets are used to
distinguish between them when writing code.

```python
var = 'A string'
print type(var)
var = u'A unicode string'
print type(var)
var = 42
print type(var)
var = ['item']
print type(var)
var = ('item', 'another')
print type(var)
var = {'entry': 7}
print type(var)
```

Sometimes you can sometimes easily convert between types.

```python
secondnumber = '2'
1 + int(secondnumber)
```

... and sometimes it's not so easy:

```python
adjectives = ['wicked', 'radical', 'awesome']
more_adjectives = str(adjectives) + 'overwhelmed'
print more_adjectives
```

### Basic syntax

Python has *variables* and *commands*. Commands may have *arguments* and
*options*.

> IPython highlights your code automatically, which can help you read it faster
and spot problems.

```python
from math import sqrt  # importing math library and square root function
avariable = 50  # make a variable that is 50 as integer
answer = sqrt(avariable)  # figure out the answer by issuing a command with avariable as an argument
print answer  # tell us
```

```python
# This example has two arguments
from math import pow  # importing pow function (pow is short for 'power')
avariable = 50
answer = pow(avariable, 3)  # 50 to the power of 3
print answer
```

## Advantages of IPython

So, we've been writing Python code in an IPython notebook. Why?

1. The main strength of IPython is that you can run bits of code individually,
so you don't have to keep repeating things. For example, if you scroll up to the
last function and replace the 50 with 2, you can re-run that code and get the
new answer.
2. IPython allows you to display images alongside code, and to save the input
and output together.
3. IPython makes learning a bit easier, as mistakes are easier to find and do
not break an entire workflow.

You can get more information on IPython, including how to install it on your own
machine, at the [IPython Homepage](http://ipython.org).

So, the last thing to do in this session is to discuss what you all make of
IPython. Can anyone see potential for Python in their own research? What are you
working on, anyway?

Anything you're struggling with so far?

# Session 2: Functions, lists and variables

Welcome back. How's everybody settling in?

At this point, we want to dive more deeply into general Python functionality. We
want to define some functions, manipulate some lists, and understand better what
variables are and what they do.

### Defining a function

You may wish to repeat an operation multiple times looking at different texts or
different terms within a text. Instead of re-entering the formula every time,
you can assign a name and set of actions to a particular task.

We've just created a simple function that welcomed you and told you the time.

Previously, we calculated the lexical diversity of a text. In NLTK, we can
create a function called **lexical diversity** that runs a single line of code.
We can then call this function to quickly determine the lexical density of a
corpus or subcorpus.
Advantages of functions:
1. Save you typing
2. You can be sure you're doing exactly the same operation every time
<br>
> **Note** Learn to love tab-completion! Typing the first one or two letters of
a command you've used previously then hitting tab
will auto-complete that command, saving you typing (i.e. time and mistakes!).

<markdown cell>
**Challenge!**

Using a function, determine which of the nine texts in the NLTK Book has the
highest lexical diversity score.

```python
def lexical_diversity(text):
    return len(text)/len(set(text))
```

```python
#After the function has been defined, we can run it:
lexical_diversity(text2)
```

The parentheses are important here as they sepatate the the task, that is the
work of the function, from the data that the function is to be performed on.
The data in parentheses is called the argument of the function. When we use a
function, we say that we 'call' it.

Other functions that we've used already include len() and sorted() - these were
predefined. *lexical_diversity()* is one we set up ourselves; note that it's
conventional to put a set of parentheses after a function, to make it clear what
we're talking about.

### Lists

Python treats a text as a long list of words. First, we'll make some lists of
our own, to give you an idea of how a list behaves.

```python
sent1 = ['Call', 'me', 'Ishmael', '.']
```

```python
sent1
```

```python
len(sent1)
```

The opening sentences of each of our texts have been pre-defined for you. You
can inspect them by typing in 'sent2' etc.

You can add lists together, creating a new list containing all the items from
both lists. You can do this by typing out the two lists or you can add two or
more pre-defined lists. This is called concatenation.

```python
sent4 + sent1
```

We can also add an item to the end of a list by appending. When we append(), the
list itself is updated.

```python
sent1.append('Please')
sent1
```

There are some things we can do to make it easier to read the contents of a
string. Note that we get some brackets and so on when we try to print the items
in a list as a string.

```python
fruitsalad = []  # declare an empty list
fruitsalad.append('watermelon')  # add watermelon
fruitsalad.append('orange')  # add orange
print 'Our fruit salad contains: ' + str(fruitsalad)
```

If we want to print our ingredients in a nicer looking form, we might use a
function like *join()*

```python
fruitsalad = []
fruitsalad.append('watermelon')
fruitsalad.append('orange')
listasastring = ''.join(fruitsalad)  # create a string with all the list items joined together
print 'Our fruit salad contains: ' + listasastring
```

... whoops! Still ugly. We didn't put anything in between the '' to use as a
delimiter.

```python
fruitsalad.append('canteloupe')
listasastring = ', '.join(fruitsalad)  # note the comma and space in quotation marks
print 'Our fruit salad contains: ' + listasastring
```

###  Indexing Lists

We can navigate this list with the help of indexes. Just as we can find out the
number of times a word occurs in a text, we can also find where a word first
occurs. We can navigate to different points in a text without restriction, so
long as we can describe where we want to be.

```python
text4.index('awaken')
```

This works in reverse as well. We can ask Python to locate the 158th item in our
list (note that we use square brackets here, not parentheses)

```python
text4[158]
```

As well as pulling out individual items from a list, indexes can be used to pull
out selections of text from a large corpus to inspect. We call this slicing

```python
text5[16715:16735]
```

If we're asking for the beginning or end of a text, we can leave out the first
or second number. For instance, [:5] will give us the first five items in a list
while [8:] will give us all the elements from the eighth to the end.

```python
text2[:10]
text4[145700:]
```

To help you understand how indexes work, let's create one.

We start by defining the name of our index and then add the items. You probably
won't do this in your own work, but you may want to manipulate an index in other
ways. Pay attention to the quote marks and commas when you create your test
sentence.

```python
sent = ['The', 'quick', 'brown', 'fox', 'jumps', 'over', 'the', 'lazy', 'dog']
sent[0]
sent[8]
```

Note that the first element in the list is zero. This is because we are telling
Python to go zero steps forward in the list. If we use an index that is too
large (that is, we ask for something that doesn't exist), we'll get an error.

We can modify elements in a list by assigning new data to one of its index
values. We can also replace a slice with new material.

```python
sent[2] = 'furry'
sent[7] = 'spotty'
sent
```

###  Defining variables

In Python, we give the items we're working with names, a process called
assignment. For instance, in the NLTK corpus, 'Sense and Sensibility' has been
assigned the name 'text2', which is much easier to work with.
We also assigend the name 'sent' to the sentence that we created in the previous
exercise, so that we could then instruct Python to do various things with it.
Assigning a variable in python looks like this:
variable = expression
You can call your variables (almost) anything you like, but it's a good idea to
pick names that will be meaningful and easy to type. You can't use words that
already have a meaning in Python, such as import, def, or not. If you try to use
a word that is reserved, you'll get a syntax error.

*Challenge*
. Create a list called 'opening' that consists of the phrase "It was a dark and
stormy night; the rain fell in torrents"
. Create a variable called 'clause' that contains the contents of 'opening', up
to the semi-colon
. Create a variable called 'alphabetised' that contains the contents of 'clause'
sorted alphabetically
. Print 'alphabetised'

```python
opening = ['It', 'was', 'a', 'dark', 'and', 'stormy', 'night', ';', 'the', 'rain', 'fell', 'in', 'torrents']
clause = opening[0:7]
alphabetised = sorted(clause)
```

Note that assigning a variable just causes Python to remember that information
without generating any output.

If you want Python to show you the result, you have to ask for it (this is a
good thing when you assign a variable to a very long list!).

```python
clause
```

```python
alphabetised
```

### Frequency distributions

We can use Python's ability to perform statistical analysis of data to do
further exploration of vocabulary. For instance, we might want to be able to
find the most common or least common words in a text. We'll start by looking at
frequency distribution.

```python
fdist1 = FreqDist(text1)
```

```python
fdist1.most_common(50)
```

```python
fdist1['whale']
```

```python
fdist1.plot(50, cumulative = True)
```

*Challenge!*

Create a function called "Common_Words" and use it to compare the 15 most common
words of four of the texts in the NLTK book.
Discuss what you found with your neighbour.

As well as counting individual words, we can count other features of vocabulary,
such as how often words of different lengths occur. We do this by putting
together a number of the commands we've already learned.

We could start like this:

     [len(w) for w in text1]

... but this would print the length of every word in the whole book, so let's
skip that bit!

```python
fdist2= FreqDist(len(w) for w in text1)
```

```python
fdist2.max()
```

```python
fdist2.freq(3)
```

These last two commands tell us that the most common word length is 3, and that
these 3 letter words account for about 20% of the book.
We can see this just by visually inspecting the list produced by
*fdist2.most_common()*, but if this list were too long to inspect readily, or we
didn't want to print it, there are other ways to explore it.

It is possible to select the longest words in a text, which may tell you
something about its vocabulary and style

```python
v = set(text4)
long_words = [word for word in v if len(word) > 15]
sorted(long_words)
```

We can fine-tune our selection even further by adding other conditions. For
instance, we might want to find long words that occur frequently (or rarely).

**Challenge!**

Can you find all the words in a text that are more than seven letters long and
occur more than seven times?

```python
fdist3 = FreqDist(text5)
sorted(w for w in set(text5) if len(w) > 7 and fdist3[w] > 7)
```

There are a number of functions defined for NLTK's frequency distributions:

 | Function | Purpose  |
 |--------------|------------|
 | fdist = FreqDist(samples) | create a frequency distribution containing the
given samples |
 | fdist[sample] += 1 | increment the count for this sample |
 | fdist['monstrous']  | count of the number of times a given sample occurred |
 | fdist.freq('monstrous') | frequency of a given sample |
 | fdist.N()  |  total number of samples |
 | fdist.most_common(n)   |  the n most common samples and their frequencies |
 | for sample in fdist:   |  iterate over the items in fdist, when in the loop,
we refer to each item as sample |
 | fdist.max() | sample with the greatest count |
 | fdist.tabulate()   |  tabulate the frequency distribution |
 | fdist.plot()  |   graphical plot of the frequency distribution |
 | fdist.plot(cumulative=True) | cumulative plot of the frequency distribution |
 | fdist1 < fdist2 | test if samples in fdist1 occur less frequently than in
fdist2 |

We can also find words that typically occur together, which tend to be very
specific to a text or genre of texts. We'll talk more about these features and
how to use them later.

```python
text4.collocations()
```

We can also use numerical operators to refine the types of searches we ask
Python to run. We can use the following relational operators:

 |  Relational | Meaning |
 |--------------:|:------------|
 | <    |  less than |
 | <=   |   less than or equal to |
 | ==  |    equal to (note this is two "=" signs, not one) |
 | !=   |   not equal to |
 | \>   |   greater than |
 | \>= |   greater than or equal to |

*Challenge!*
Using one of the pre-defined sentences in the NLTK corpus, use the relational
operators above to find:

1. Words longer than four characters
2. Words of four or more characters
3. Words of exactly four characters

```python
# Words longer than four characters:
v = set(text4)
morefour_words = [word for word in v if len(word) > 4]
print morefour_words[:10]
```

```python
# Words of four or more characters:
v = set(text4)
fourormore_words = [word for word in v if len(word) >= 4]
print fourormore_words[:10]
```

```python
# Words of exactly four characters:
v = set(text4)
four_words = [word for word in v if len(word) == 4]
print four_words[:10]
```

We can also look for features such as letter combinations, upper and lowercase
letters, and digits. Some operators you might like to use are:


 | Operator  | Purpose  |
 |--------------|------------|
 | s.endswith(t)  |  test if s ends with t |
 | t in s         |  test if t is a substring of s |
 | s.islower()    |  test if s contains cased characters and all are lowercase |
 | s.isupper()    |  test if s contains cased characters and all are uppercase |
 | s.isalpha()    |  test if s is non-empty and all characters in s are
alphabetic |
 | s.isalnum()    |  test if s is non-empty and all characters in s are
alphanumeric |
 | s.isdigit()    |  test if s is non-empty and all characters in s are digits |
 | s.istitle()    |  test if s contains cased characters and is titlecased (i.e.
all words in s have initial capitals) |

```python
sorted(w for w in set(text1) if w.endswith('ableness'))
```

```python
sorted(n for n in sent7 if n.isdigit())
```

Have a play around with some of these operators in the cells below.

```python

```

```python

```

```python
# You can insert more cells via the menu bar if you need to!
```

**Bonus!**

You'll remember right at the beginning we started looking at the size of the
vocabulary of a text, but there were two problems with the results we got from
using:

     len(set(text1)).

This count includes items of punctuation and treats capitalised and non-
capitalised words as different things (*This* vs *this*). We can now fix these
problems. We start by getting rid of capitalised words, then we get rid of the
punctuation and numbers.

```python
len(set(word.lower() for word in text1))
```

```python
len(set(word.lower() for word in text1 if word.isalpha()))
```

<img style="float:left"
src="http://images.catsolonline.com/cache/custom/CEN/CE651-250x250.jpg" />
<br>

You've completed the introduction to Python Natural Language Toolkit! To
summarise, here's what you've learned so far:

* How to navigate the iPython notebook
* How Python uses whitespace
* Why functions are useful and how to define one
* How to use basic Python commands to start exploring features of a text

We'll practice all these commands in the following lessons ... so don't panic!

It's a lot to take in and it will probably take a while before you feel really
comfortable.

# Session 3: Common NLTK tasks

<br>
In this session we provide an quick introduction to the field of *corpus
linguistics*. We then engage with common uses of NLTK within these areas, such
as sentence segmentation, tokenisation and stemming. Often, NLTK has inbuilt
methods for performing these tasks. As a learning exercise, however, we will
sometimes build basic tools from scratch.

## Corpus linguistics

Though corpus linguistics has been around since the 1950s, it is only in the
last 20 years that its methods have been made available to individual
researchers. GUIs including [Wordsmith
Tools](http://www.lexically.net/wordsmith/) and
[AntConc](http://www.laurenceanthony.net/software.html).

Alongside the development of GUIs, there has also been a shift from *general,
balanced corpora* (corpora seeking to represent a language generally) toward
*specialised corpora* (corpora containing texts of one specific type, from one
speaker, etc.). More and more commonly, texts are taken from the Web.

> **Note:** We'll discuss building corpora from online texts in a bit more
detail tomorrow afternoon.

After a long period of resistance, corpus linguistics has gained acceptence
within a number of research areas. A few popular applications are within:

* **Lexicography** (creating usage-based definitions of words and locating real
examples)
* **Language pedagogy** (advanced language learners can use a concordancing GUI
or collocation tests to understand how certain words are used in the target
language)
* **Discourse analysis** (researching how meaning is made beyond the level of
the clause/sentence)

Notably, corpus linguistic methods have been embraced within the emerging
paradigm of Digital Humanities, where it's sometimes called *distant reading*.

### Corpora and discourse

As hardware, software and data become more and more available, people have
started using corpus linguistic methods for discourse-analytic work. Paul Baker
refers the combination of corpus linguistics and (critical) discourse analysis
as a [*useful methodological synergy*](#ref:baker). Corpora bring objectivity
and empiricism to a qualitative, interpretative tradition, while discourse-
analytic methods provide corpus linguistics with a means of contextualising
abstracted results.

Within this area, researchers rely on corpora to varying extents. In *corpus-
driven* discourse analysis, researchers interpret the corpus based on the
findings of the corpus interrogation. In *corpus-assisted* discourse analysis,
researchers may use corpora to provide evidence about the way a given
person/idea/discourse is commonly represented by certain people/in certain
publications etc.

Our work here falls under the *corpus-driven* heading, as we are exploring the
dataset without any major hypotheses in mind.

> **Note:** Some linguists remain skeptical of corpus linguistics generally. In
a well-known critique, Henry Widdowson ([2000, p. 6-7](#ref:widdowson)) said:
>
> Corpus linguistics \[...\] (there) is no doubt that this is an immensely
important development in descriptive linguistics. That is not the issue here.
The quantitative analysis of text by computer reveals facts about actual
language behaviour which are not, or at least not immediately, accessible to
intuition. There are frequencies of occurrence of words, and regular patterns of
collocational co-occurrence, which users are unaware of, though they must be
part of their competence in a procedural sense since they would not otherwise be
attested. They are third person observed data ('When do they use the word X?')
which are different from the first person data of introspection ('When do I use
the word X?'), and the second person data of elicitation ('When do you use the
word X?'). Corpus analysis reveals textual facts, fascinating profiles of
produced language, and its concordances are always springing surprises. They do
indeed reveal a reality about language usage which was hitherto not evident to
its users.
>
> But this achievement of corpus analysis at the same time necessarily defines
its limitations. For one thing, since what is revealed is contrary to intuition,
then it cannot represent the reality of first person awareness. We get third
person facts of what people do, but not the facts of what people know, nor what
they think they do: they come from the perspective of the observer looking on,
not the introspective of the insider. In ethnomethodogical terms, we do not get
member categories of description. Furthermore, it can only be one aspect of what
they do that is captured by such quantitative analysis. For, obviously enough,
the computer can only cope with the material products ofwhat people do when they
use language. It can only analyse the textual traces of the processes whereby
meaning is achieved: it cannot account for the complex interplay of linguistic
and contextual factors whereby discourse is enacted. It cannot produce
ethnographic descriptions of language use. In reference to Hymes's components of
communicative competence (Hymes 1972), we can say that corpus analysis deals
with the textually attested, but not with the encoded possible, nor the
contextually appropriate.
>
> To point out these rather obvious limitations is not to undervalue corpus
analysis but to define more clearly where its value lies. What it can do is
reveal the properties of text, and that is impressive enough. But it is
necessarily only a partial account of real language. For there are certain
aspects of linguistic reality that it cannot reveal at all. In this respect, the
linguistics of the attested is just as partial as the linguistics of the
possible.

## Loading a corpus

First, we have to load a corpus. We'll use a text file containing posts to an
Australian online forum for discussing politics. It's full of very interesting
natural language data!

```python
from IPython.display import display
from IPython.display import HTML
HTML('<iframe src=http://www.ozpolitic.com/forum/YaBB.pl?board=global width=700 height=350></iframe>')
```

This file is available online, at the [ResBaz
GitHub](https://github.com/resbaz). We can ask Python to get it for us.

> Later in the course, we'll discuss how to extract data from the Web and turn
this data into a corpus.

```python
from urllib import urlopen # a library for working with urls
url = "https://raw.githubusercontent.com/resbaz/lessons/master/nltk/corpora/oz_politics/ozpol.txt" # define the url
raw = urlopen(url).read() # download and read the corpus into raw variable
raw = unicode(raw.lower(), 'utf-8') # make it lowercase and unicode
len(raw) # how many characters does it contain?
raw[:2000] # first 2000 characters
```

We actually already downloaded this file when we first cloned the ResBaz GitHub
repository. It's in our *corpora* folder. We can access it like this:

```python
f = open('../corpora/oz_politics/ozpol.txt')
raw = f.read()
raw = unicode(raw.lower(), 'utf-8') # make it lowercase and unicode
len(raw)
raw[:2000] 
```

## Regular Expressions

Before we go any further, we need to talk about Regular Expressions. Regular
Expressions (regexes) are ways of searching for complex patterns in strings.
Regexes are standardised across many programming languages, and can also be used
in GUI text editors and word processers.

```python
import nltk # just in case (you should only need to import a library once per session, 
    # but nothing bad will happen if you do it again)
import re # import this before using regexes!
```

If only using alphanumeric characters and spaces, regexes work like any normal
search.

```python
# print only match
regex = re.compile(r"government")
re.findall(regex, raw)
```

Hmm ... not very useful.

```python
# print whole line
regex = re.compile(r'government')
for line in raw.splitlines():
    if regex.search(line) is not None:
        print line
```

```python
# or, we can do it by adding to our regex:
regex = re.compile(r".*government.*")
re.findall(regex, raw)
```

... but regex can be much more powerful than that. Certain characters have
special meanings:

* . (period): any character
* \* (asterisk): any number of times
* \b: word boundary
* ^ (carat): start of a line
* $ (dollar sign): end of a line
* \+ (plus): one or more times
* [xy]: either x or y
* o{5}: five os in a row
* o{5,}: at least five os in a row
* o{,5}: max five os in a row
* (any|of|these|words)

```python
# [a-z] means any letter
# the plus means ''one or more of the thing before''
regex = re.compile(r"[a-z]+ment")
# find all instances of regex in raw
results = re.findall(regex, raw)
sorted(set(results)) # sort and print only unique results
```

```python
# word that ends in ation:
regex = re.compile(r"[a-z]+ation\b")
# find all instances of regex in raw
results = re.findall(regex, raw)
sorted(set(results)) # sort and print only unique results
```

Sometimes, we want match the special characters:

```python
string = 'What!? :) U fink im flirtin wit *u*!? LOL'
# define a regex:
regex = re.compile(r':)')
re.findall(regex, string)
```

What happened? Well, if you want to search for any special character, it must be
'escaped' by a backslash:

```python
string = 'What!? :) U fink im flirtin wit *u*!? LOL'
# define a regex:
regex = re.compile(r':\)')
re.findall(regex, string)
```

Regular expressions take a while to master, but have great benefits for
searching large texts for very specific things. Here are some additional regex
resources:

* [Regex Info](http://www.regular-expressions.info/): tutorials etc.
* [Regexr](http://www.regexr.com/): a place to build and test out your regex
* [Regex crosswords](http://regexcrossword.com/): exactly what you think it is!

The code below will get any word over *minlength* alphabetic characters by
passing an integer into a regex.

```python
minlength = 5  # minimum number of letters as integer 
# gets passed into the building of the regex
sentence = 'We, the democratically-elected leaders of our people, hereby declare Kosovo to be an independent and sovereign state'

# any letter, minlength times:
pattern = re.compile(r'[A-Za-z]{' + str(minlength) + ',}') 
# What happens if you add a hyphen after 'a-z' inside the square brackets above?
re.findall(pattern, sentence)
```

## Sentence segmentation

So, with a basic understanding of regex, we can now start to turn our corpus
into a structured resource. At present, we have 'raw', a very, very long string
of text.

 We should break the string into segments. First, we'll split the corpus into
sentences. This task is a pretty boring one, and it's tough for us to improve on
existing resources. We'll try, though.

Let's define a sentence as any string ending with (one or more) newline, full
stop, question mark, or exclamation mark. A regex can be written to find these,
and the split() function can  be used to break the string into a list of
strings.

```python
import re
sentences = re.split("(\n|\.|\?|!)+", raw)
print 'Sentence: ' + '\nSentence: '.join(sentences[:10]) # print the first ten sentences
```

Well, it worked, sort of. What problems do we have?

1. sh.thole was split
2. Empty matches/punctuation only matches

So, to fix our first problem, we have to make an intpretative decision. Is
*sh.thole* a word? We could:

1. Clean the corpus and remove this false positive
2. Define sentence differently

We'll go with the second option, for better or worse. The regex in the code
below makes sure that any sentence final character must not be followed by a
letter.

```python
sentences = re.split("(?:\n|\.|\?|!)+([^a-zA-Z]|$)", raw)
print 'Sentence: ' + '\nSentence: '.join(sentences[:10])
```

Problem 1 is solved: *sh.thole* is now a single token. To fix Problem 2, we will
remove list items not containing a letter, as any sentence must contain at least
one by definition.

```python
regex = re.compile('[a-zA-Z]')
no_blanklines = [sentence for sentence in sentences if regex.match(sentence)]
# or, an alternative approach using filter function:
# no_blanklines = filter(regex.match, sentences)
print 'Sentence: ' + '\nSentence: '.join(no_blanklines[:20]) # this should be better!
```

The code below turns out sentence segmenter into a function.

```python
def sent_seg(string):
    allmatches = re.split("(?:\n|\.|\?|!)+([^a-zA-Z]|$)", string)
    regex = re.compile('[a-zA-Z]')
    sentences = [sentence for sentence in allmatches if regex.match(sentence)]
    length = len(sentences)
    return sentences
```

```python
# Call the segmenter on our raw text here. Store it in a variable called 'sents'
sents = sent_seg(raw)
```

It's all academic, anyway: NLTK actually has a sentence segmenter built in that
works better than ours (we didn't deal with quotation marks, or brackets, for
example).

```python
sent_tokenizer=nltk.data.load('tokenizers/punkt/english.pickle')
sents = sent_tokenizer.tokenize(raw)
sents[101:111]
```

Alright, we have sentences. Now what?

## Tokenisation

Tokenisation is simply the process of breaking texts down into words. We already
did a little bit of this in Session 1. We won't build our own tokenizer, because
it's not much fun. NLTK has one we can rely on.

Keep in mind that definitions of tokens are not standardised, especially for
languages other than English. Serious problems arise when comparing two corpora
that have been tokenised differently.

> **Note:** It is also possible to use NLTK to break tokens into morphemes,
syllables, or phonemes. We're not going to go down those roads, though.

```python
tokenized_sents = [nltk.word_tokenize(i) for i in sents]
tokenized_sents[:3]
# another view:
# print tokenized_sents[:10]
```

## Stemming

Stemming is the task of finding the stem of a word. So, *cats --> cat*, or
*taking --> take*. It is an important task when counting words, as often the
counting each inflection seperately is not particuarly helpful: forms of the
verb 'to be' might seem under-represented if we could *is, are, were, was, am,
be, being, been* separately.

NLTK has pre-programmed stemmers, but we can build our own using some of the
skills we've already learned.

A stemmer is the kind of thing that would make a good function, so let's do
that.

```python
def stem(word):
    for suffix in ['ing', 'ly', 'ed', 'ious', 'ies', 'ive', 'es', 's', 'ment']: # list of suffixes
        if word.endswith(suffix):
            return word[:-len(suffix)] # delete the suffix
    return word
```

Let's run it over some text and see how it performs.

```python
# empty list for our output
stemmed_sents = []
for sent in tokenized_sents:
    # empty list for stemmed sentence:
    stemmed = []
    for word in sent:
        # append the stem of every word
        stemmed.append(stem(word))
    # append the stemmed sentence to the list of sentences
    stemmed_sents.append(stemmed)
# pretty print the output
stemmed_sents[:10]
```

Looking at the output, we can see that the stemmer works: *wingers* becomes
*winger*, and *tearing* becomes *tear*. But, sometimes it does things we don't
want: *Nothing* becomes *noth*, and *mate* becomes *mat*. Even so, for the
learns, let's rewrite our function with a regex:

```python
def stem(word):
    import re
    # define a regex to get word, and common suffixes
    regex = r'^(.*?)(ing|ly|ed|ious|ies|ive|es|s|ment)?$'
    stem, suffix = re.findall(regex, word)[0]
    return stem
```

Because we just redefined the *stem()* function, we can run the previous code
and get different results.

Here's a very quick implementation of our stemmer on our raw tokens:

```python
tokens = nltk.word_tokenize(raw)
stemmed = [stem(t) for t in tokens]
print stemmed[:50]
```

We can see that this approach has obvious limitations. So, let's rely on a
purpose-built stemmer. These rely in part on dictionaries. Note the subtle
differences between the two possible stemmers:

```python
# define stemmers
lancaster = nltk.LancasterStemmer()
porter = nltk.PorterStemmer()
# stem each word in tokens
stems = [lancaster.stem(t) for t in tokens]  # replace lancaster with porter here
print stems[:100]
```

Notice that both stemmers handle some things rather poorly. The main reason for
this is that they are not aware of the *word class* of any particular word:
*nothing* is a noun, and nouns ending in *ing* should not have *ing* removed by
the stemmer (swing, bling, ring...). Later in the course, we'll start annotating
corpora with grammatical information. This improves the accuracy of stemmers a
lot.

> Note: stemming is not *always* the best thing to do: though *thing* is the
stem of *things*, things has a unique meaning, as in *things will improve*. If
we are interested in vague language, we may not want to collapse things -->
thing.

## Keywording: 'the aboutness of a text'

Keywording is the process of generating a list of words that are unusually
frequent in the corpus of interest. To do it, you need a *reference corpus*, or
at least a *reference wordlist* to which your *target corpus* can be compared.
Often, *reference corpora* take the form of very large collections of language
drawn from a variety of spoken and written sources.

Keywording is what generates word-clouds beside online news stories, blog posts,
and the like. In combination with speech-to-text, it's used in Oxford
University's [Spindle Project](http://openspires.oucs.ox.ac.uk/spindle/) to
automatically archive recorded lectures with useful tags.

In fact, the keywording part of the Spindle Project is written in Python, and in
open source.

Spindle has sensible defaults for keyword calculation. Let's download their code
and use it to generate keywords from our corpus.

```python
import sys
 # download spindle from github
#!wget https://github.com/sgrau/spindle-code/archive/master.zip
#!unzip master.zip # unzip it
#!rm master.zip # remove the zip file
# put the keyworder directory in python's path, so we can call it easily
sys.path.insert(0, '../spindle-code-master/keywords')
# import the function
from keywords import keywords_and_ngrams # import keywords function
```

```python
# this tool works with raw text, not tokens!
keywords_and_ngrams(raw.encode("UTF-8"), nBigrams = 0)
```

Success! We have keywords.

> Keep in mind, the BNC reference corpus was created before ISIS and ISIL
existed. *Moslem/moslems* is a dispreferred spelling of Muslim, used more
frequently in anti-Islamic discourse. Also, it's unlikely that a transcriber of
the spoken BNC would choose the Moslem spelling. *Having an inappropriate
reference corpus is a common methodological problem in discourse analytic work*.

Our keywords would perhaps be better if they were stemmed. That shouldn't be too
hard for us:

```python
keywords_and_ngrams(stems, nBigrams = 0) # this will use our corpus of stems, as defined earlier.
```

Rats! Keywording with stems actually revealed a list of incorrect stems.

What re really need to do is improve our stemmer, and then come back and try
again.

## A return to stemming

Keywords and ngram searches actually work by comparing a corpus to a reference
corpus. In our case, we have been using a dictionary of words in the 100 million
word *British National Corpus*. We could use this same dictionary to make sure
our stemmer does not create non-words when it stems.

First, let's get a list of common words in the BNC from the *pickle* provided by
SPINDLE (*pickle* is a kind of list compression).

```python
import pickle
import os
# unpack the pickled list
bncwordlist = pickle.load(open('../spindle-code-master/keywords/bnc.p', 'rb')) 
bnc_commonwords = [] # empty list
for word in bncwordlist:
    getval = bncwordlist[word] # find out number of occurrences of word
    if getval > 20: # if more than 20
        bnc_commonwords.append(word) # add to common word list
print bnc_commonwords[:200] # what are our results?
```

So, this gives us a list of any word appearing more than twenty times in the
BNC. We could build this function into our stemmer:

```python
# Now, let's use this as the dict for our stemmer
# The third variable sets a default threshold, but also allows us to enter one.
def newstemmer(words, stemmer, threshold = 20):
    """A stemmer that uses Lancaster/porter stemmer plus a dictionary."""
    import pickle
    import os
    import nltk
    bncwordlist = pickle.load(open('../spindle-code-master/keywords/bnc.p', 'rb'))
    bnc_commonwords = {k for (k,v) in bncwordlist.iteritems() if v > threshold}
    # if words is a raw string, tokenise it
    if type(words) == unicode or type(words) == string:
        tokens = nltk.word_tokenize(words)
    # or, if list of tokens, duplicate the list
    else:
        tokens = words
        # define stemmer based on the argument we passed in
    if stemmer == 'Lancaster':
        stemmertouse = nltk.LancasterStemmer()
    if stemmer == 'Porter':
        stemmertouse = nltk.PorterStemmer()
    # empty list of stems
    stems = []
    for w in tokens:
        # stem each word
        stem = stemmertouse.stem(w)
        # if the stem is in the bnc list
        if stem in bnc_commonwords:
            # add the stem
            stems.append(stem)
        else:
            # or else, just add the word as it was
            stems.append(w)
    return stems
```

Now, we can fiddle with the stemmer and BNC frequency to get different keyword
lists.

```python
# try raising the threshold if there are still bad spellings!
stemmed = newstemmer(raw, 'Lancaster', 10)
```

```python
keys, ngrams = keywords_and_ngrams(stemmed)
keys # only keywords
 # only n-grams
```

## Collocation

> *You shall know a word by the company it keeps.* - J.R. Firth, 1957

Collocation is a very common area of interest in corpus linguistics. Words
pattern together in both expected and unexpected ways. In some contexts, *drug*
and *medication* are synonymous, but it would be very rare to hear about
*illicit* or *street medication*. Similarly, doctors are unlikely to prescribe
the *correct* or *appropriate drug*.

This kind of information may be useful to lexicographers, discourse analysts, or
advanced language learners.

In NLTK, collocation works from ordered lists of tokens. Let's put out tokenised
sents into a single, huge list of tokens:

```python
allwords = []
# for each sentence,
for sent in tokenized_sents:
    # for each word,
    for word in sent:
        # make a list of all words
        allwords.append(word)
print allwords[:20]
# small challenge: can you think of any other ways to do this?
```

Now, let's feed these to an NLTK function for measuring collocations:

```python
# get all the functions needed for collocation work
from nltk.collocations import *
# define statistical tests for bigrams
bigram_measures = nltk.collocations.BigramAssocMeasures()
# go and find bigrams
finder = BigramCollocationFinder.from_words(allwords)
# measure which bigrams are important and print the top 30
sorted(finder.nbest(bigram_measures.raw_freq, 30))
```

So, that tells us a little: we can see that terrorists, Muslims and the Middle
East are commonly collocating in the text. At present, we are only looking for
immediately adjacent words. So, let's expand out search to a window of *five
words either side*

''window size'' specifies the distance at which
two tokens can still be considered collocates
finder = BigramCollocationFinder.from_words(allwords, window_size=5)
sorted(finder.nbest(bigram_measures.raw_freq, 30))

Now we have the appearance of very common words! Let's use NLTK's stopwords list
to remove entries containing these:

```python
finder = BigramCollocationFinder.from_words(allwords, window_size=5)
# get a list of stopwords from nltk
ignored_words = nltk.corpus.stopwords.words('english')
# make sure no part of the bigram is in stopwords
finder.apply_word_filter(lambda w: len(w) < 2 or w.lower() in ignored_words)
finder.apply_freq_filter(2)
#print the sorted collocations
sorted(finder.nbest(bigram_measures.raw_freq, 30))
```

There! Now we have some interesting collocates. Finally, let's remove
punctuation-only entries, or entries that are *n't*, as this is caused by
different tokenisers:

```python
finder = BigramCollocationFinder.from_words(allwords, window_size=5)
ignored_words = nltk.corpus.stopwords.words('english')
# anything containing letter or number
regex = r'[A-Za-z0-9]'
# the n't token
nonot = r'n\'t'
# lots of conditions!
finder.apply_word_filter(lambda w: len(w) < 2 or w.lower() in ignored_words or not re.match(regex, w) or re.match(nonot, w))
finder.apply_freq_filter(2)
sorted(finder.nbest(bigram_measures.raw_freq, 30))
```

You can get a lot more info on collocation at the [NLTK
homepage](http://www.nltk.org/howto/collocations.html).

## Clustering/n-grams

Clustering is the task of finding words that are commonly **immediately**
adjacent (as opposed to collocates, which may just be nearby). This is also
often called n-grams: bigrams are two tokens that appear together, trigrams are
three, etc.

Clusters/n-grams have a spooky ability to tell us what a text is about.

We can use *Spindle* for bigram searching as well:

```python
# an argument here to stop keywords from being produced.
keywords_and_ngrams(raw.encode("UTF-8"), nKeywords=0)
```

There's also a method for n-gram production in NLTK. We can use this to
understand how n-gramming works.

Below, we get lists of any ten adjacent tokens:

```python
from nltk.util import ngrams
# define a sentence
sentence = 'give a man a fish and you feed him for a day; teach a man to fish and you feed him for a lifetime'
# length of ngram
n = 10
# use builtin tokeniser (but we could use a different one)
tengrams = ngrams(sentence.split(), n)
for gram in tengrams:
  print gram
```

So, there are plenty of tengrams in there! What we're interested in, however, is
duplicated n-grams:

```python
# arguments: a text, ngram size, and minimum occurrences
def ngrammer(text, gramsize, threshold = 4):
    """Get any repeating ngram containing gramsize tokens"""
    # we need to import this in order to find the duplicates:
    from collections import defaultdict
    from nltk.util import ngrams
    # a subdefinition to get duplicate lists in a list
    def list_duplicates(seq):
        tally = defaultdict(list)
        for i,item in enumerate(seq):
            tally[item].append(i)
            # return to us the index and the ngram itself:
        return ((len(locs),key) for key,locs in tally.items() 
               if len(locs) > threshold)
    # get ngrams of gramsize    
    raw_grams = ngrams(text.split(), gramsize)
    # use our duplication detector to find duplicates
    dupes = list_duplicates(raw_grams)
    # return them, sorted by most frequent
    return sorted(dupes, reverse = True)
```

Now that it's defined, let's run it, looking for trigrams

```python
ngrammer(raw, 3)
```

Too many results? Let's set a higher threshold than the default.

```python
ngrammer(raw, 3, threshold = 10)
```

## Concordancing with regular expressions

We've already done a bit of concordancing. In discourse-analytic research,
concordancing is often used to perform thematic categorisation.

```python
text = nltk.Text(tokens)  # formats our tokens for concordancing
text.concordance("muslims")
```

We could even our stemmed corpus here:
text = nltk.Text(stemmed)
text.concordance("muslims")

You get no matches in the latter case, because all instances of *muslims* were
stemmed to *muslim*.

A problem with the NLTK concordancer is that it only works with individual
tokens. What if we want to find words that end with **ment*, or words beginning
with *poli**?

We already searched text with Regular Expressions. It's not much more work to
build regex functionality into our own concordancer.

From running the code below, you can see that bracketting sections of our regex
causes results to split into lists:

```python
# define a regex for different aussie words
aussie = r'(aussie|australia)'
searchpattern = re.compile(r"(.*)" + aussie + r"(.*)")
search = re.findall(searchpattern, raw)
search[:5]
```

Well, it's ugly, but it works. We can see five bracketted results, each
containing three strings. The first and third strings are the left-context and
right-context. The second of the three strings is the search term.

These three sections are, with a bit of tweaking, the same as the output given
by a concordancer.

Let's go ahead and turn our regex seacher into a concordancer:

```python
def concordancer(text, regex):
    """Concordance using regular expressions"""
    import re
    # limit context to 30 characters max
    searchpattern = re.compile(r"(.{,30})(\b" + regex + r"\b)(.{,30})")
    # find all instances of our regex
    search = re.findall(searchpattern, raw)
    for result in search:
        #join each result with a tab, and print
        print("\t".join(result).expandtabs(20))
        # expand tabs helps align results
```

```python
concordancer(raw, r'aus.*?')
```

Great! With six lines of code, we've officially created a function that improves
on the one provided by NLTK! And think how easy it would be to add more
functionality: an argument dictating the size of the window (currently 30
characters), or printing line numbers beside matches, would be pretty easy to
add, as well.

> Adding too much functionality is known as *feature creep*. It's often best to
keep your functions simple and more varied. An old adage in programming is to
*make each program do one thing well*.

In the cells below, try concordancing a few things. Also try creating variables
with concordance results, and then manipulate the lists. If you encounter
problems with the way the concordancer runs, alter the function and redefine it.
If you want, try implementing the window size variable!

> **Tip:** If you wanted to get really creative, you could try stemming
concordance or n-gram results!

```python
#
```

```python
#
```

```python
#
```

```python
#
```

```python
#
```

## Summary

That's the end of session three! Great work.

So, some of these tasks are a little dry---seeing results as lists of words and
scores isn't always a lot of fun. But ultimately, they're pretty important
things to know if you want to avoid the 'black box approach', where you simply
dump words into a machine and analyse what the machine spits out.

Remember that almost every task in corpus linguistics/distance reading depends
on how we segment our data into sentences, clauses, words, etc.

Building a stemmer from scratch taught us how to use regular expressions, and
their power. But, we also saw that they weren't perfect for the task. In later
lessons, we'll use more advanced methods to normalise our data.

*See you tomorrow!*

# Bibliography

<a id="ref:baker"></a>
Baker, P., Gabrielatos, C., Khosravinik, M., Krzyzanowski, M., McEnery, T., &
Wodak, R. (2008). A useful methodological synergy? Combining critical discourse
analysis and corpus linguistics to examine discourses of refugees and asylum
seekers in the UK press. Discourse & Society, 19(3), 273-306.

<a id="firth"></a>
Firth, J. (1957).  *A Synopsis of Linguistic Theory 1930-1955*. In: Studies in
Linguistic Analysis, Philological Society, Oxford; reprinted in Palmer, F. (ed.)
1968 Selected Papers of J. R. Firth, Longman, Harlow.

<a id="ref:hymes"></a>
Hymes, D. (1972). On communicative competence. In J. Pride & J. Holmes (Eds.),
Sociolinguistics (pp. 269-293). Harmondsworth: Penguin Books. Retrieved from [ht
tp://humanidades.uprrp.edu/smjeg/reserva/Estudios%20Hispanicos/espa3246/Prof%20S
unny%20Cabrera/ESPA%203246%20-%20On%20Communicative%20Competence%20p%2053-73.pdf
](http://humanidades.uprrp.edu/smjeg/reserva/Estudios%20Hispanicos/espa3246/Prof
%20Sunny%20Cabrera/ESPA%203246%20-%20On%20Communicative%20Competence%20p%2053-73
.pdf)

<a id="ref:widdowson"></a>
Widdowson, H. G. (2000). On the limitations of linguistics applied. Applied
Linguistics, 21(1), 3. Available at [http://applij.oxfordjournals.org/content/21
/1/3.short](http://applij.oxfordjournals.org/content/21/1/3.short).

### Workspace

Here are a few blank cells, in case you need them for anything:

```python
# 
```

```python
#
```

```python
#
```

```python
#
```

```python
#
```

```python
#
```

```python
#
```

```python
#
```

```python
#
```

```python
#
```

```python
# 
```
