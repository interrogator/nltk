<br>
<img style="float:left" src="http://ipython.org/_static/IPy_header.png" />
<br>

# Session 3: Charting change in Fraser's speeches

In this lesson, we investigate a fully-parsed version of the Fraser Corpus. We
do this using a set of purpose-built tools called `corpkit`.

In the first part of the session, we will go through how to use each of the
tools. Later, you'll be able to use the tools to navigate the data and visualise
results in any way you like.

The Fraser Speeches have been parsed for part of speech and grammatical
structure by [*Stanford
CoreNLP*](http://nlp.stanford.edu/software/corenlp.shtml), a parser that can be
loaded within NLTK. We rely on
[*Tregex*](http://nlp.stanford.edu/~manning/courses/ling289/Tregex.html) to
interrogate the parse trees. Tregex allows very complex searching of parsed
trees, in combination with [Java Regular Expressions](http://docs.oracle.com/jav
ase/7/docs/api/java/util/regex/Pattern.html), which are very similar to the
regexes we've been using thus far.

If you plan to work more with parsed corpora later, it's definitely worthwhile
to learn the Tregex syntax in detail. For now, though, we'll use simple queries,
and explain the query construction syntax as we go.

Before we get started, we let's import some of the usual suspects:

```python
import sys
import nltk
from IPython.display import display, clear_output
sys.path.append("/usr/lib/python2.7/site-packages/")
%matplotlib inline
```

**Welcome back!**

So, what did we learn yesterday? A brief recap:

* The **IPython** Notebook
* **Python**: syntax, variables, functions, etc.
* **NLTK**: manipulating linguistic data
* **Corpus linguistic tasks**: tokenisation, keywords, collocation, stemming,
concordances

Today's focus will be on **developing more advanced NLTK skills** and using
these skills to **investigate the Fraser Speeches Corpus**. In the final
session, we will discuss **how to use what you have learned here in your own
research**.

*Any questions or anything before we dive in?*

## Malcolm Fraser and his speeches

So, for much of the next two sessions, we are going to be working with a corpus
of speeches made by Malcolm Fraser.

```python
# this code allows us to display images and webpages in our notebook
from IPython.display import display
from IPython.display import display_pretty, display_html, display_jpeg, display_png, display_svg
from IPython.display import Image
from IPython.display import HTML
import nltk
```

```python
Image(url='http://www.unimelb.edu.au/malcolmfraser/photographs/family/105~36fam6p9.jpg')
```

Because our project here is *corpus driven*, we don't necessarily need to know
about Malcolm Fraser and his speeches in order to analyse the data: we may be
happy to let things emerge from the data themselves. Even so, it's nice to know
a bit about him.

Malcolm Fraser was a member of Australian parliament between 1955 and 1983,
holding the seat of Wannon in western Victoria. He held a number of ministries,
including Education and Science, and Defence.

He became leader of the Liberal Party in March 1975 and Prime Minister of
Australia in December 1975, following the dismissal of the Whitlam government in
November 1975.

He retired from parliament following the defeat of the Liberal party at the 1983
election and in 2009 resigned from the Liberal party after becoming increasingly
critical of some of its policies.

He can now be found on Twitter as **@MalcolmFraser12**

```python
HTML('<iframe src=http://en.wikipedia.org/wiki/Malcolm_Fraser width=700 height=350></iframe>')
```

In 2004, Malcolm Fraser made the University of Melbourne the official custodian
of his personal papers. The collection consists of a large number of
photographs, speeches and personal papers, including Neville Fraser's WWI
diaries and materials relating to CARE Australia, which Mr Fraser helped to
found in 1987.

```python
HTML('<iframe src=http://www.unimelb.edu.au/malcolmfraser/ width=700 height=350></iframe>')
```

Every week, between 1954 until 1983, Malcolm Fraser made a talk to his
electorate that was broadcast on Sunday evening on local radio.

The speeches were transcribed years ago. Optical Character Recognition (OCR) was
used to digitise the transcripts. This means that the texts are not of perfect
quality.

Some have been manually corrected, which has removed extraneous characters and
mangled words, but even so there are still some quirks in the formatting.

For much of this session, we are going to manipulate the corpus data, and use
the data to restructure the corpus.

## Cleaning the corpus

A common part of corpus building is corpus cleaning. Reasons for cleaning
include:

1. Not break the code with unexpected input
2. Ensure that searches match as many examples as possible
3. Increasing readability, the accuracy of taggers, stemmers, parsers, etc.

The level of kind of cleaning depends on your data and the aims of your project.
In the case of very clean data (lucky you!), there may be little that needs to
be done. With messy data, you may need to go as far as to correct variant
spellings (online conversation, very old books).

### Discussion

*What are the characteristics of clean and messy data? Any personal experiences?
Discuss with your neighbours.*

It will be important to bear these characteristics in mind once you start
building your own datasets and corpora.

## Exploring the corpus

First of all, let's load in our text.

Via file management, open and inspect one file in
*corpora/UMA_Fraser_Radio_Talks*. What do you see? Are there any potential
problems?

We can also look at file contents within the IPython Notebook itself:

```python
import os
```

```python
# import tokenizers
# from nltk import word_tokenize
# from nltk.text import Text
```

```python
# make a list of files in the directory 'UMA_Fraser_Radio_Talks'
files = os.listdir('corpora/UMA_Fraser_Radio_Talks')
print files[:3]
```

Actually, since we'll be referring to this path quite a bit, let's make it into
a variable. This makes our code easier to use on other projects (and saves
typing)

```python
corpus_path = 'corpora/UMA_Fraser_Radio_Talks'
```

We can now tell Python to get the contents of a file in the file list and print
it:

```python
# print file contents
# change zero to something else to print a different file
f = open(os.path.join(corpus_path, files[0]), "r")
text = f.read()
print text
```

### Exploring further: splitting up text

We've had a look at one file, but the real strength of NLTK is to be able to
explore large bodies of text.

When we manually inspected the first file, we saw that it contained a metadata
section, before the body of the text.

We can ask Python to show us just the start of the file. For analysing the text,
it is useful to split the metadata section off, so that we can interrogate it
separately but also so that it won't distort our results when we analyse the
text.

```python
# new:
data = text.split("<!--end metadata-->")
```

```python
# old, maybe not needed:
# open the first file, read it and then split it into two parts, metadata and body
data = open(os.path.join(corpus_path, os.listdir(corpus_path)[0])).read().split("<!--end metadata-->")
# notice that many different commands can be strung together in one line!
```

```python
# view the first part
print data[0]
```

```python
# split into lines, add '*' to the start of each line
# \r is a carriage return, like on a typewriter.
# \n is a newline character
for line in data[0].split('\r\n'):
    print '*', line
```

```python
# skip empty lines and any line that starts with '<'
for line in data[0].split('\r\n'):
    if not line:
        continue
    if line[0] == '<':
        continue
    print '*', line
```

```python
# split the metadata items on ':' so that we can interrogate each one
for line in data[0].split('\r\n'):
    if not line:
        continue
    if line[0] == '<':
        continue
    element = line.split(':')
    print '*', element
```

```python
# actually, only split on the first colon
for line in data[0].split('\r\n'):
    if not line:
        continue
    if line[0] == '<':
        continue
    element = line.split(':', 1)
    print '*', element
```

### **Challenge**: Building a Dictionary

We've already worked with strings, integers, and lists. Another kind of data
structure in Python is a *dictionary*.

Here is how a simple dictionary works:

```python
# create a dictionary
commonwords = {'the': 4023, 'of': 3809, 'a': 3098}
# search the dictionary for 'of'
commonwords['of']
```

```python
type(commonwords)
```

The point of dictionaries is to store a *key* (the word) and a *value* (the
count). When you ask for the key, you get its value.

Notice that you use curly braces for dictionaries, but square brackets for
lists.

Dictionaries are a great way to work with the metadata in our corpus. Let's
build a dictionary called *metadata*:

Your first line will look like this:

      metadata = {}

```python
metadata = {}
for line in data[0].split('\r\n'):
    if not line:
        continue
    if line[0] == '<':
        continue
    element = line.split(':', 1)
    metadata[element[0]] = element[-1]
print metadata
```

```python
# look up the date
print metadata['Date']
```

### Building functions

**Challenge**: define a function that creates a dictionary of the metadata for
each file and gets rid of the whitespace at the start of each element

**Hint**: to get rid of the whitespace use the *.strip()* command.

```python
# open the first file, read it and then split it into two parts, metadata and body
data = open(os.path.join(corpus_path, 'UDS2013680-100-full.txt'))
data = data.read().split("<!--end metadata-->")
```

```python
def parse_metadata(text):
    metadata = {}
    for line in text.split('\r\n'):
        if not line:
            continue
        if line[0] == '<':
            continue
        element = line.split(':', 1)
        metadata[element[0]] = element[-1].strip(' ')
    return metadata
```

Test it out!

```python
parse_metadata(data[0])
```

Now that we're confident that the function works, let's find out a bit about the
corpus.
As a start, it would be useful to know which years the texts are from. Are they
evenly distributed over time? A graph will tell us!

```python
#import conditional frequency distribution
from nltk.probability import ConditionalFreqDist
import matplotlib
% matplotlib inline
cfdist = ConditionalFreqDist()
for filename in os.listdir(corpus_path):
    text = open(os.path.join(corpus_path, filename)).read()
    #split text of file on 'end metadata'
    text = text.split("<!--end metadata-->")
    #parse metadata using previously defined function "parse_metadata"
    metadata = parse_metadata(text[0])
    #skip all speeches for which there is no exact date
    if metadata['Date'][0] == 'c':
        continue
    #build a frequency distribution graph by year, that is, take the final bit of the 'Date' string after '/'
    cfdist['count'][metadata['Date'].split('/')[-1]] += 1
cfdist.plot()
```

Now let's build another graph, but this time by the 'Description' field:

```python
cfdist2 = ConditionalFreqDist()
for filename in os.listdir(corpus_path):
    text = open(os.path.join(corpus_path, filename)).read()
    text = text.split("<!--end metadata-->")
    metadata = parse_metadata(text[0])
    if metadata['Date'][0] == 'c':
        continue
    cfdist2['count'][metadata['Description']] += 1
cfdist2.plot()
```

#### Discussion

We've got messy data! What's the lesson here?
<br>

**Bonus chellenge**: Build a frequency distribution graph that includes speeches
without an exact date.
Hint: you'll need to tell Python to ignore the 'c' and just take the digits

```python
cfdist3 = ConditionalFreqDist()
for filename in os.listdir(corpus_path):
    text = open(os.path.join(corpus_path, filename)).read()
    text = text.split("<!--end metadata-->")
    metadata = parse_metadata(text[0])
    date = metadata['Date']
    if date[0] == 'c':
        year = date[1:]
    elif date[0] != 'c':
        year = date.split('/')[-1]
    cfdist3['count'][year] += 1
cfdist3.plot()
```

### Structuring our data by metadata feature

Because our data samples span a long stretch of time, we thought we'd
investigate the ways in which Malcolm Fraser's language changes over time. This
will be the key focus of the next session.

In order to study this, it is helpful to structure our data according to the
year of the sample. This simply means creating folders for each sample year, and
moving each text into the correct one.

We can use our metadata parser to help with this task. Then, after structuring
our corpus by date, we want the metadata gone, so that when we count language
features in the files, we are not also counting the metadata.

So, let's try this:

```python
import re
# a path to our soonwordso-be organised corpus
newpath = 'corpora/fraser-structured'
#if not os.path.exists(newpath):
    #os.makedirs(newpath)
files = os.listdir(corpus_path)
# define a regex to match year portion of date
yearfinder = re.compile('[0-9]{4}')
for filename in files:
    # split file contents at end of metadata
    text = open(os.path.join(corpus_path, filename))
    data = text.read().split("<!--end metadata-->")
    # get date from data[0]
    # use our metadata parser to get metadata
    metadata = parse_metadata(data[0])
    #look up date field of dict entry
    date = metadata.get('Date')
    # search date for year
    yearmatch = re.search(yearfinder, str(date))
    #get the year as a string
    year = str(yearmatch.group())
    # make a directory with the year name
    if not os.path.exists(os.path.join(newpath, year)):
        os.makedirs(os.path.join(newpath, year))
    # make a new file with the same name as the old one in the new dir
    fo = open(os.path.join(newpath, year, filename),"w")
    # write the content portion, without metadata
    fo.write(data[1])
    fo.close()
```

Did it work? How can we check?

```python
# print os.listdir(newpath)
# print os.listdir(newpath + '/1981')
```

## Using `corpkit` to analyse the Fraser Corpus

Next, we have to install Java, as some of the `corpkit` tools rely on Java code.
You'll very likely have Java installed on your local machine, but we need it on
the cloud. To make it work, run the following:

```python
! yum -y install java
clear_output()
```

Now, let's download and install `corpkit`, if we haven't already:

```python
# ! pip install corpkit
```

OK, that's out of the way. Next, let's import the functions we'll be using to
investigate the corpus. These functions have been designed specifically for our
investigation, but they will work with any parsed dataset.

We'll take a look at the code used in this session a little later on, if there's
time. Much of the code is derived from things we've learned here, combined with
a lot of Google and Stack Overflow searching. All our code is on GitHub too,
remember. It's open-source, so you can do whatever you like with it.

Here's an overview of the functions we'll be using, and their purpose:

| **Function name** | Purpose                            | |
| ----------------- | ---------------------------------- | |
| *searchtree()*  | find things in a parse tree         | |
| *interrogator()*  | interrogate parsed corpora         | |
| *plot()*       | visualise *interrogator()* results | |
| *quickview()*     | view *interrogator()* results      | |
| *tally()*       | get total frequencies for *interrogator()* results      | |
| *surgeon()*       | edit *interrogator()* results      | |
| *merger()*       | merge *interrogator()* results      | |
| *conc()*          | complex concordancing of subcopora | |

```python
import corpkit
from corpkit import (
    interrogator, plotter, table, quickview, 
    tally, surgeon, merger, conc, keywords, 
    collocates, quicktree, searchtree
                    )
```

We also need to set the path to our corpus as a variable. If you were using this
interface for your own corpora, you would change this to the path to your data.

```python
path = 'corpora/fraser-corpus-annotated' # path to corpora from our current working directory.
```

### Interrogating the corpus

To interrogate the corpus, we need a crash course in parse labels and Tregex
syntax. Let's define a tree (from the Fraser Corpus, 1956), and have a look at
its visual representation.

     Melbourne has been transformed over the let 18 months in preparation for
the visitors.

```python
melbtree = (r'(ROOT (S (NP (NNP Melbourne)) (VP (VBZ has) (VP (VBN been) (VP (VBN transformed) '
           r'(PP (IN over) (NP (NP (DT the) (VBN let) (CD 18) (NNS months)) (PP (IN in) (NP (NP (NN preparation)) '
           r'(PP (IN for) (NP (DT the) (NNS visitors)))))))))) (. .)))')
```

Notice that an OCR error caused a parsing error. Oh well. Here's a visual
representation, drawn with NLTK:

<br>
<img style="float:left" src="https://raw.githubusercontent.com/resbaz/nltk/resou
rces/images/melbtree.png" />
<br>

The data is annotated at word, phrase and clause level. Embedded here is an
elaboration of the meanings of tags *(ask Daniel if you need some
clarification!)*:

```python
HTML('<iframe src=http://www.surdeanu.info/mihai/teaching/ista555-fall13/readings/PennTreebankConstituents.html width=700 height=350></iframe>')
```

Note that the tags are a little bit different from the last parser we were
using:

```python
quicktree("Melbourne has been transformed over the let 18 months in preparation for the visitors")
```

Neither parse is perfect, but the one we just generated has a major flaw:
*Melbourne* is parsed as an adverb! Stanford CoreNLP correctly identifies it as
a proper noun, and also, did a better job of handling the 'let' mistake.

*searchtree()* is a tiny function that searches a syntax tree. We'll use the
sample sentence and *searchtree()* to practice our Tregex queries. We can feed
it either *tags* (S, NP, VBZ, DT, etc.) or *tokens* enclosed in forward slashes.

```python
# any plural noun
query = r'NNS'
searchtree(melbtree, query)
```

```python
# A token matching the regex *Melb.?\**
query = r'/Melb.?/'
searchtree(melbtree, query)
```

```python
query = r'NP'
searchtree(melbtree, query)
```

To make things more specific, we can create queries with multiple criteria to
match, and specify the relationship between each criterion we want to match.
Tregex will print everything matching **the leftmost criterion**.

```python
# NP with 18 as a descendent
query = r'NP << /18/'
searchtree(melbtree, query)
```

Using an exclamation mark negates the relationship. Try producing a query for a
*noun phrase* (NP) without a *Melb* descendent:

```python
query = r'NP !<< /Melb.?/'
searchtree(melbtree, query)
```

The dollar specifies a sibling relationship between two parts of the tree--
wordshat is, two words or tags that are horizontally aligned.

```python
# NP with a sister VP
# This corresponds to 'subject' in many grammars
query = r'NP $ VP'
searchtree(melbtree, query)
```

Try changing the **more than** symbols to **less than**, and see how it affects
the results.

```python
# Prepositional phrase in other prepositional phrases
query = r'PP >> PP'
searchtree(melbtree, query)
```

There is also a double underscore, which functions as a wildcard.

```python
# anything with any kind of noun tag
query = r'__ > /NN.?/'
searchtree(melbtree, query)
```

Using brackets, it's possible to create very verbose queries, though this goes
well beyond our scope. Just know that it can be done!

```python
# particle verb in verb phrase with np sister headed by Melb.
# the particle verb must also be in a verb phrase with a child preposition phrase
# and this child preposition phrase must be headed by the preposition 'over'.
query = r'VBN >> (VP $ (NP <<# /Melb.?/)) > (VP < (PP <<# (IN < /over/)))'
searchtree(melbtree, query)
```

Here are two more trees for you to query, from 1969 and 1973.

     We continue to place a high value on economic aid through the Colombo Plan,
involving considerable aid to Asian students in Australia.

<br>
<img style="float:left" src="https://raw.githubusercontent.com/resbaz/nltk/resou
rces/images/colombotree.png" />
<br>

```python
colombotree = r'(ROOT (S (NP (PRP We)) (VP (VBP continue) (S (VP (TO to) (VP (VB place) (NP (NP (DT a) (JJ high) '
    r'(NN value)) (PP (IN on) (NP (JJ economic) (NN aid)))) (PP (IN through) (NP (DT the) (NNP Colombo) (NNP Plan))) '
    r'(, ,) (S (VP (VBG involving) (NP (JJ considerable) (NN aid)) (PP (TO to) (NP (NP (JJ Asian) (NNS students)) 
        r'(PP (IN in) (NP (NNP Australia))))))))))) (. .)))'
```

     As a result, wool industry and the research bodies are in a state of wonder
and doubt about the future.

<br>
<img style="float:left" src="https://raw.githubusercontent.com/resbaz/nltk/resou
rces/images/wooltree.png" />
<br>

```python
wooltree = r'(ROOT (S (PP (IN As) (NP (DT a) (NN result))) (, ,) (NP (NP (NN wool) (NN industry)) (CC and) '
                 r'(NP (DT the) (NN research) (NNS bodies))) (VP (VBP are) (PP (IN in) (NP (NP (DT a) (NN state)) '
                    r'(PP (IN of) (NP (NN wonder) (CC and) (NN doubt))))) (PP (IN about) (NP (DT the) (NN future)))) (. .)))'
```

Try a few queries in the cells below.

> If you need help constructing a Tregex query, ask Daniel. He writes them all
day long for fun.

```python
query = '?'
searchtree(colombotree, query)
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

So, now we understand the basics of a Tregex query (don't worry---many queries
have already been written for you. We can start our investigation of the Fraser
Corpus by generating some general information about it. First, let's define a
query to find every word in the corpus. Run the cell below to define the
*allwords_query* as the Tregex query.

> *When writing Tregex queries or Regular Expressions, remember to always use
**r'...'** quotes!*

```python
# any token containing letters or numbers (i.e. no punctuation):
# we specify here that it cannot have any descendants,
# just to be sure we only get tokens, not tags.
allwords_query = r'/[A-Za-z0-9]/ !< __' 
```

Next, we perform interrogations with *interrogator()*. Its most important
arguments are:

1. **path to corpus** (the *path* variable)

2. Tregex **options**:
  * **'t'**: return only words
  * **'c'**: return a count of matches

3. the **Tregex query**

We only need to count tokens, so we can use the **count** option (it's often
faster than getting lists of matching tokens). The cell below will run
*interrogator()* over each annual subcorpus and count the number of matches for
the query.

```python
allwords = interrogator(path, 'count', allwords_query) 
```

When the interrogation has finished, we can view the total counts by getting the
*totals* branch of the *allwords* interrogation:

```python
# from the allwords results, print the totals
print allwords.totals
```

If you want to see the query and options that created the results, you can print
the *query* branch.

```python
print allwords.query
```

### Plotting results

Lists of years and totals are pretty dry. Luckily, we can use the *plot()*
function to visualise our results. At minimum, *plot()* needs two arguments:

1. a title (in quotation marks)
2. a list of results to plot

```python
plot('Word counts in each subcorpus', allwords.totals)
```

Great! So, we can see that the number of words per year varies quite a lot.
That's worth keeping in mind.

Next, let's plot something more specific, using the **words** option.

```python
query = r'/(?i)\baustral.?/' # australia, australian, australians, etc.
aust = interrogator(path, 'words', query) # words option to get matching words, not just count
```

We now have a list of words matching the query stores in the *aust* variable's
*results* branch:

```python
aust.results[:3] # just the first few entries
```

*Your turn!* Try this exercise again with a different term.

We can use a *fract_of* argument to plot our results as a percentage of
something else. This helps us deal with the issue of different amounts of data
per year.

```python
# as a percentage of all aust* words:
plot('Austral*', aust.results, fract_of = aust.totals)
# as a percentage of all words (using our previous interrogation)
plot('Austral*', aust.results, fract_of = allwords.totals)
```

Great! So, we now have a basic understanding of the *interrogator()* and
*plot()* functions.

### Customising visualisations

By default, *plot()* plots the absolute frequency of the seven most frequent
results.

 We can use other *plot()* arguments to customise what our chart shows.
*plot()*'s possible arguments are:

 | plot() argument | Mandatory/default?       |  Use          | Type  |
 | :------|:------- |:-------------|:-----|
 | *title* | **mandatory**      | A title for your plot | string |
 | *results* | **mandatory**      | the results you want to plot |
*interrogator()* total |
 | *fract_of* | None      | results for plotting relative frequencies/ratios
etc. | list (interrogator(count) form) |
 | *num_to_plot* | 7     | number of top results to display     |   integer |
 | *multiplier* | 100     | result * multiplier / total: use 1 for ratios |
integer |
 | *x_label* | False    | custom label for the x-axis     |  string |
 | *y_label* | False    | custom label for the y-axis     |  string |
 | *yearspan* | False    | plot a span of years |  a list of two int years |
 | *justyears* | False    | plot specific years |  a list of int years |
 | *csvmake* | False    | make csvmake the title of csv output file    |  string
|

You can easily use these to get different kinds of output. Try changing some
parameters below:

```python
# maybe we want to get rid of all those non-words?
plot('Austral*', aust.results, fract_of = allwords.totals, num_to_plot = 3, y_label = 'Percentage of all words')
```

```python
# or see only the 1960s?
plot('Austral*', aust.results, fract_of = allwords.totals, num_to_plot = 3, yearspan = [1960,1969])
```

**Your Turn**: mess with these variables, and see what you can plot. Try using
some really infrequent results, if you like!

```python
#
```

```python
#
```

### Viewing and editing results

Aside from *interrogator()* and *plot()*, there are also a few simple functions
for viewing and editing results.

#### quickview()

*quickview()* is a function that quickly shows the n most frequent items in a
list. Its arguments are:

1. an *interrogator()* result
2. number of results to show (default = 50)

We can see the full glory of bad OCR here:

```python
quickview(aust.results, n = 20)
```

The number shown next to the item is its index. You can use this number to refer
to an entry when editing results.

#### tally()

*tally()* displays the total occurrences of results. Its first argument is the
list you want tallies from. For its second argument, you can use:

* a list of indices for results you want to tally
* a single integer, which will be interpreted as the index of the item you want
* a string, 'all', which will tally every result. This could be very many
results, so it may be worth limiting the number of items you pass to it with
[:n],

```python
tally(aust.results, [0, 3])
```

**Your turn**: Use 'all' to tally the result for the first 11 items in
aust.results

```python
tally(aust.results[:10], 'all')
```

#### surgeon()

Results lists can be edited quickly with *surgeon()*. *surgeon()*'s arguments
are:

1. an *interrogator()* results list
2. *criteria*: either a regex or a list of indices.
3. *remove = True/False*

By default, *surgeon()* removes anything matching the regex/indices criteria,
but this can be inverted with a *remove = False* argument. Because you are
duplicating the original list, you don't have to worry about deleting
*interrogator()* results.

We can use it to remove some obvious non-words.

```python
non_words_removed = surgeon(aust.results, [5, 9], remove = True)
plot('Some non-words removed', non_words_removed, fract_of = allwords.totals)
```

Note that you do not access surgeon lists with *aust.non_words_removed* syntax,
but simply with *non_words_removed*.

#### merger()

*merger()* is for merging items in a list. Like *surgeon()*, it duplicates the
old list. Its arguments are:

1. the list you want to modify
2. the indices of results you want to merge, or a regex to match
3. newname = *str/int/False*:
  * if string, the string becomes the merged item name.
  * if integer, the merged entry takes the name of the item indexed with the
integer.
  * if not specified/False, the most most frequent item in the list becomes the
name.

In our case, we might want to collapse *Australian* and *Australians*, because
the latter is simply the plural of the former.

```python
# before:
plot('Before merging Australian and Australians', aust.results, num_to_plot = 3)
# after:
merged = merger(aust.results, [1, 2],  newname = 'australian(s)')
plot('After merging Australian and Australians', merged, num_to_plot = 2)
```

#### conc()

The final function is *conc()*, which produces concordances of a subcorpus based
on a Tregex query. Its main arguments are:

1. A subcorpus to search *(remember to put it in quotation marks!)*
2. A Tregex query

```python
# here, we use a subcorpus of politics articles,
# rather than the total annual editions.
conc(os.path.join(path,'1966'), r'/(?i)\baustral.?/') # adj containing a risk word
```

You can set *conc()* to print *n* random concordances with the *random = n*
parameter. You can also store the output to a variable for further searching.

```python
randoms = conc(os.path.join(path,'1963'), r'/(?i)\baustral.?/', random = 5)
randoms
```

*conc()* takes another argument, window, which alters the amount of cowordsext
appearing either side of the match.

```python
conc(os.path.join(path,'1981'), r'/(?i)\baustral.?/', random = 5, window = 50)
```

*conc()* also allows you to view parse trees. By default, it's false:

```python
conc(os.path.join(path,'1954'), r'/(?i)\baustral.?/', random = 5, window = 30, trees = True)

# Now you're familiar with the corpus and functions, it's time to explore the corpus in a more structured way. To do this, we need a little bit of linguistic knowledge, however.
```

### Some linguistics...

*Functional linguistics* is a research area concerned with how *realised
language* (lexis and grammar) work to achieve meaningful social functions.

One functional linguistic theory is *Systemic Functional Linguistics*, developed
by Michael Halliday (Prof. Emeritus at University of Sydney).

Central to the theory is a division between **experiential meanings** and
**interpersonal meanings**.

* Experiential meanings communicate what happened to whom, under what
circumstances.
* Interpersonal meanings negotiate identities and role relationships between
speakers

Halliday argues that these two kinds of meaning are realised **simultaneously**
through different parts of English grammar.

* Experiential meanings are made through **transitivity choices**.
* Interpersonal meanings are made through **mood choices**

Here's one visualisation of it. We're concerned with the two left-hand columns.
Each level is an abstraction of the one below it.

<br>
<img style="float:left" src="https://raw.githubusercontent.com/resbaz/nltk/resou
rces/images/egginsfixed.jpg" />
<br>

Transitivity choices include fitting together configurations of:

* Participants (*a man, green bikes*)
* Processes (*sleep, has always been, is considering*)
* Circumstances (*on the weekend*, *in Australia*)

Mood features of a language include:

* Mood types (*declarative, interrogative, imperative*)
* Modality (*would, can, might*)
* Lexical density--wordshe number of words per clause, the number of content to
non-content words, etc.

Lexical density is usually a good indicator of the general tone of texts. The
language of academia, for example, often has a huge number of nouns to verbs. We
can approximate an academic tone simply by making nominally dense clauses:

      The consideration of interest is the potential for a participant of a
certain demographic to be in Group A or Group B*.

Notice how not only are there many nouns (*consideration*, *interest*,
*potential*, etc.), but that the verbs are very simple (*is*, *to be*).

In comparison, informal speech is characterised by smaller clauses, and thus
more verbs.

      A: Did you feel like dropping by?
      B: I thought I did, but now I don't think I want to

Here, we have only a few, simple nouns (*you*, *I*), with more expressive verbs
(*feel*, *dropping by*, *think*, *want*)

> **Note**: SFL argues that through *grammatical metaphor*, one linguistic
feature can stand in for another. *Would you please shut the door?* is an
interrogative, but it functions as a command. *invitation* is a nominalisation
of a process, *invite*. We don't have time to deal with these kinds of
realisations, unfortunately.

### Fraser's speeches and linguistic theory

So, from an SFL perspective, when Malcolm Fraser gives a speech, he is
simultaneously making meaning about events in the real world (through
transitivity choices) and about his role and identity (through mood and modality
choices).

With this basic theory of language, we can create two research questions:

1. **How does Malcolm Fraser's tone change over time?**
2. **What are the major things being spoken about in Fraser's speeches, and how
do they change?**

As our corpus is well-structured and parsed, we can create queries to answer
these questions, and then visualise the results.

#### Interpersonal features

We'll start with interpersonal features of language in the corpus. First, we can
devise a couple of simple metrics that can teach us about the interpersonal tone
of Fraser's speeches over time. We don't have time to run all of these queries
right now, but there should be some time later to explore the parts of this
material that interest

```python
# number of content words per clause
openwords = r'/\b(JJ|NN|VB|RB)+.?\b/'
clauses = r'S < __'
opencount = interrogator(path, 'count', openwords)
clausecount = interrogator(path, 'count', clauses)
```

```python
plot('Lexical density', opencount.totals, 
        fract_of = clausecount.totals, y_label = 'Lexical Density Score', multiplier = 1)
```

We can also look at the use of modals auxiliaries (*would could, may, etc.*)
over time. This can be interesting, as modality is responsible for communicating
certainty, probability, obligation, etc.

Modals are very easily and accurately located, as there are only a few possible
words, and they occur in predicable places within clauses.

Most grammars tag them with 'MD'.

If modality interests you, later, it could be a good set of results to
manipulate and plot.

```python
query = r'MD < __'
modals = interrogator(path, 'words', query)
plot('Modals', modals.results, fract_of = modals.totals)
```

```python
# percentage of tokens that are I/me
query = r'/PRP.?/ < /(?i)^(i|me|my)$/'
firstperson = interrogator(path, 'count', query)
```

```python
plot('First person', firstperson.totals, fract_of = allwords.totals)
```

```python
# percentage of questions
query = r'ROOT <<- /.?\?.?/'
questions = interrogator(path, 'count', query)
```

```python
plot('Questions/all clauses', questions.totals, fract_of = clausecount.totals)
```

```python
# ratio of open/closed class words
closedwords = r'/\b(DT|IN|CC|EX|W|MD|TO|PRP)+.?\b/'
closedcount = interrogator(path, 'count', closedwords)
```

```python
plot('Open/closed word classes', opencount.totals, 
        fract_of = closedcount.totals, y_label = 'Open/closed ratio', multiplier = 1)
```

```python
# ratio of nouns/verbs
nouns = r'/NN.?/ < __'
verbs = r'/VB.?/ < __'
nouncount = interrogator(path, 'count', nouns)
verbcount = interrogator(path, 'count', verbs)
```

```python
plot('Noun/verb ratio', nouncount.totals, fract_of = verbcount.totals, multiplier = 1)
```

#### Experiential features of Fraser's speech

We now turn our attention to what is being spoken about in the corpus. First, we
can get the heads of grammatical participants:

```python
# heads of participants (heads of NPS not in prepositional phrases)
query = r'/NN.?/ >># (NP !> PP)'
participants = interrogator(path, 'words', query, lemmatise = True)
```

```python
plot('Participants', participants.results, fract_of = allwords.totals)
```

Next, we can get the most common processes. That is, the rightmost verb in a
verbal group (take a look at the visualised tree!)

> *Be careful not to confuse grammatical labels (predicator, verb), with
semantic labels (participant, process) ... *

```python
# most common processes
query = r'/VB.?/ >># VP >+(VP) VP'
processes = interrogator(path, 'words', query, lemmatise = True)
```

```python
plot('Processes', processes.results[2:], fract_of = processes.totals)
```

It seems that the verb *believe* is a common process in 1973. Try to run
*conc()* in the cell below to look at the way the word behaves.

```python
# write a call to conc() that gets concordances for r'/VB.?/ < /believe/ in 1973
# conc('fraser-corpus-annotated/1973', r'/VB.?/ < /believe/)
#
```

For discussion: what events are being discussed when *believe* is the process?
Why use *believe* here?
<br>

Next, let's chart noun phrases headed by a proper noun (*the Prime Minister*,
*Sydney*, *John Howard*, etc.). We can define them like this:

```python
# any noun phrase headed by a proper noun
pn_query = r'NP <# NNP'
```

To make for more accurate results the *interrogator()* function has an option,
*titlefilter*, which uses a regular expression to strip determiners (*a*, *an*,
*the*, etc.), titles (*Mr*, *Mrs*, *Dr*, etc.) and first names from the results.
This will ensure that the results for *Prime Minister* also include *the Prime
Minister*, and *Fraser* results will include the *Malcolm* variety. The option
is turned on in the cell below:

```python
# Proper noun groups
propernouns = interrogator(path, 'words', pn_query, titlefilter = True)
```

```python
plot('Proper noun groups', propernouns.results, fract_of = propernouns.totals, num_to_plot = 15)
```

Proper nouns are a really good category to investigate further, as it is through
proper nouns that we can track discussion of particular people, places or
things. So, let's look at the top 100 results:

```python
quickview(propernouns.results, n = 100)
```

 You can now use the *merger()* and *surgeon()* options to make new lists to
plot. Here's one example: we'll use *merger()* to merge places in Victoria, and
then *surgeon()* to create a list of places in Australia.

```python
merged = merger(propernouns.results, [9, 13, 27, 36, 78, 93], newname = 'places in victoria')
quickview(merged, n = 100)

ausparts = surgeon(merged, [7, 9, 23, 25, 33, 41, 49], remove = False)
plot('Places in Australia', ausparts, fract_of = propernouns.totals)
```

Neat, eh? Well, that concludes the structured part of the lesson. You now have a
bit of time to explore the corpus, using the tools provided. Below, for your
convenience, is a table of the functions and their arguments.

Particularly rewarding can be playing more with the proper nouns section, as in
the cells above. Shout out if you find something interesting!

<br>
<img style="float:left"
src="https://raw.githubusercontent.com/resbaz/nltk/resources/images/options.png"
/>
<br>

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
