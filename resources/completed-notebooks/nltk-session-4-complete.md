<br>
<img style="float:left" src="http://ipython.org/_static/IPy_header.png" />
<br>

# Session 4: Getting the most out of what we've learned

So, now you know Python and NLTK! The main things we still have to do are:

1. Address some specific questions
2. Manage resources and results
3. Brainstorm some other uses for NLTK
4. Integrate IPython into your existing workflow
5. Have an open discussion about what we've done
6. Summarise and say goodbye!

This lesson is pretty light on content and structure. Please do jump in at any
point, and tell us about your research, and whether or not what you've learned
here will be of much use.

Or, ask us if Python can do a certain thing. Maybe we have some tips!

```python
import nltk
from IPython.display import (display, clear_output, Image, display_pretty, 
                 display_html, display_jpeg, display_png, display_svg, HTML)
%matplotlib inline
```

## Mining the web

Let's have a look at [Project
Gutenberg](https://www.gutenberg.org/wiki/Technology_%28Bookshelf%29). Let's
check out *Food processing*.

Using the skills we've learned, it should be possible to extract texts from
Project Gutenberg.

```python
booknums = ['24510', '19073', '21592']
```

```python
def gutenberger(list_of_nums):
    from urllib import urlopen
    text = []
    for num in list_of_nums:
        num = str(num)
        url = 'https://www.gutenberg.org/cache/epub/' + num + '/pg' + num + '.txt'
        raw = urlopen(url).read()
        raw = unicode(raw, 'utf-8')
        # title = 'No title'
        title = next(line for line in raw.splitlines() if line.startswith('Title:'))
        print title
        title = title.replace('Title: ', '')
        text.append([title, raw])
    return text
```

```python
# call our function!
books = gutenberger(booknums)
```

The thing to remember here is that the web is well-structured. URLs are just
strings, and you can hack them very easily.

### Getting my data into NLTK

The key here is to get your work into **clean, plain text**

So, let's save the Gutenberg data to disk:

```python
def saver(book_data):
    import os
    # a path to our soon-to-be corpus
    newpath = 'corpora/gutenberg'
    os.makedirs(newpath)
    for title, text in book_data:
        title = title.replace(' ', '-')
        filename = ''.join([c for c in title if c.isalnum() or c == '-']) + '.txt'
        fo = open(os.path.join(newpath, filename),"w")
        fo.write(text.encode("UTF-8"))
        fo.close()
        print 'File created: ' + filename
```

```python
saver(books)
```

### Challenge

**Combine the two functions into `book_saver()`**.

## Adding information to our text

```python
from nltk.corpus import brown
print brown.words() 
print brown.tagged_words()
```

Some other things are annotated in the Brown Corpus, too. Head here for more
info:

```python
HTML('<iframe src=http://en.wikipedia.org/wiki/Brown_Corpus#Part-of-speech_tags_used width=700 height=350></iframe>')
```

So, we can pretty easily make lists containing all words of a given type. Below,
we'll print the first 50 adverbs. Try changing the 'RB' to another kind of tag
(in the list above), and see what results turn up.

> JJ and RB are shorthand for adjective and adverb. It's linguistics jargon from
the 50s that we're stuck with now.

```python
from nltk.corpus import brown
adverbs = []
for word, tag in brown.tagged_words():
    # get any word whose tag is adverb
    if tag == 'RB':
        adverbs.append(word)
adverbs[:50]
```

It's easy to grasp the potential power of annotation: think how difficult it
would be to write regular expressions that locate all adverbs!

> **Note:** John Sinclair, an early proponent of corpus linguistics generally,
was famously resistant to the use of annotation and parsing. He felt that the
corpus alone should be used to build theory, rather than using existing theories
(grammars) to annotate data (e.g. [2004](#ref:sinclair)). Though this is an
uncommon viewpoint today, it is still useful to remember that the process of
'value-adding' is never free of theory or interpretation.

## Part-of-speech tagging

Part-of-speech (POS) tagging is the process of assigning each token a label.
Often, these labels are similar to what was used to tag the Brown Corpus.

> **Note:** It is generally considered good practice to train your tagger by
exposing it to well-annotated language of a similar variety. For reasons of
scope, however, training taggers and parsers is not covered in these sessions.

```python
books[1][1][9793:9968]
```

```python
sent = books[1][1][9793:9968]
text = nltk.word_tokenize(sent)
tagged = nltk.pos_tag(text)
print tagged
```

We could use this to search text by part of speech:

```python
for word, tag in tagged:
    if tag == 'NN':
        print word
```

And even do really complicated stuff if we want:

```python
for index, tup in enumerate(tagged):
    if tup[1] == 'DT':
        print tagged[index + 1][0] 
```

### Challenge!

**Use three nested conditional statements to find a single word. Be creative!**

## Getting your data into Python/NLTK

### Scenario 1: You have some old books.

* Are they machine readable?
* OCR options&mdash;institutional or DIY?
* Structure them in a meaningful way&mdash;by author, by year, by language ...
* Start querying!

### Scenario 2: You're interested in an online community.

* Explore the site. Sign up for it, maybe.
* Download it: *Wget*, *curl*, *crawlers, spiders* ...
* Extract relevant data and metadata: Python's *Beautiful Soup* library.
* **Structure your data!**
* Annotate your data, save these annotations
* Start querying!

### Scenario 3: Something of interest breaks in the news

* It will start being discussed all over the web.
* You can use the Twitter API to harvest tweets containing a term or hashtag of
interest.
* You can get a list of RSS feeds and mine news articles
* You can use something like *WebBootCat* to harvest search engine results and
make a plain text corpus
* Process these into a manageable form
* Structure them
* *Start querying!

## Managing resources and results

You generate huge amounts of code, data and findings. Often, it's hard to know
what to do with it all. In this section, we'll provide some suggestions designed
to keep your work:

1. Reproducible
2. Reusable
3. Comprehensible

### Your code

1. Most importantly, **write comments on your code**. You **will** forget what
bits of code are supposed to do. Using others' code is much easier if it's
commented up.
2. A related point is to name your variables meaningfully: *variablexxy* does
not tell us much about what it will contain. *For image in images:*  is a very
comprehensible line.
3. Also, write docstrings for your functions. Help messages come in very handy
for not only others, but yourself. Simply stating what
2. **Version control**. When editing your code, you may sometimes break it.
[Here](https://drclimate.wordpress.com/2012/11/16/version-control/)'s a write-up
about version control from Damien Irving.
3. **Share your code**. You are often doing novel things when you code, and
sharing what you've done can save somebody else a lot of work. *GitHub* is free
for open-source projects. GitHub provides version control, which is especially
useful when you are working with a team.

#### Developing as a programmer

We've only scratched the surface of Python, to be honest. In fact, we've only
been treating Python as a programming language. Many of its users, however, see
it as more than just a programming language: it is an ideology and culture, as
well.

You'll notice on Stack Overflow, people will remark that some solutions are more
'pythonic' than others. By this, they typically mean that the code is easy to
read and broken into discrete functions. More broadly, *pythonic* refers to code
that adheres to the *Zen of Python*:

```python
import this
```

So, as you explore Python more and more, you learn not only new ways to get
tasks done, but also what ways are better to others. While at first you'll be
content with making code that works, you'll later want to make sure your code is
elegant as well. Fixing up your old code becomes a form of procrastination from
thesis writing. Luckily, of all the kinds of procrastination, it's one of the
better kinds.

Another change you might notice is a switch toward *defensive programming*,
where you write code to handle potential errors, and to provide useful messages
when people do something wrong. This is a really awesome thing to do.

Some code authors also try to use *test-driven development*. From [the wikipedia
article](http://en.wikipedia.org/wiki/Test-driven_development):

> First the developer writes an (initially failing) automated test case that
defines a desired improvement or new function, then produces the minimum amount
of code to pass that test, and finally refactors the new code to acceptable
standards.

This helps stop feature-creep, builds your confidence, and encourages the
division of long code into well-defined functions.

Oh, and you'll probably start dreaming in code. *Not* a joke.

### Your data

It should now be clear to you that you have data!
Think about how you structure it. Without necessarily becoming an archivist, do
think about your metadata. It will help you to manage your data later.
*Cloud computing* offers you access to more storage and compute-power than you
might want to own. Plus you're unlikely to spill coffee on it.

### Your findings

[*Figshare*](http://www.figshare.com) is a site for storing tables and figures.
It's particularly useful for working with large datasets, as we often generate
far more raw tables and statistics than we can possibly publish.

It's becoming more and more common to link journal publications to additional
online resources such as GitHub code or Figshares. It's also more and more
common to cite GitHub and Figshare&mdash;always nice to bump up your citation
count!

## Other uses of NLTK

What other things might we use NLTK for? A few examples, and possible workflows.

## Integrating IPython into your workflow

What you've learned here isn't much good unless you can pull things out of it
and put them into your own research workflow.

It's important to remember that IPython code may be a little different from
vanilla Python, as it can contain Magics, shell commands, and the like.

Perhaps the coolest thing about programming is you are simultaneously
researching and developing. The functions that you write can be uploaded to the
web and used by others who encounter the problem that necessitated your writing
the function in the first place.

In reality, NLTK is nothing more than a lot of Python functions, coupled with
some datasets (corpora, stopword lists, etc.). You can even visit NLTK on
GitHub, fork their repository, and start playing around with the code! If you
find bugs in the code, or if you think documentation is lacking, you can either
write directly to the people who maintain the code, or fix the problem yourself
and request that they review your fix and integrate it into NLTK.

### Using IPython locally

We've done everything on the cloud so far, and it's been pretty good to us. You
may also want to use IPython locally. To do this, you need to install it. There
are many ways to install it, and these vary depending on your OS and what you
already have installed. See the [IPython website](http://ipython.org/ipython-
doc/2/install/install.html#installnotebook) for detailed instructions.

> *[Anaconda](http://continuum.io/downloads)* is a large package of Python-based
tools (including IPython and Matplotlib) that is easy to install.

Once you have IPython installed, it's very easy to start using it. All you need
to do is open up Terminal, navigate to the notebook directory and type:

     ipython notebook filename.ipynb

This will open up a blank notebook, exactly the same as the kind of notebook
we've been using on the cloud. The only difference will be that if you enter:

     os.listdir('.')

you'll get a list of files in the directory of your notebook file, rather than a
directory of your part of the cloud.

## Next steps - keep going!

```python
Image(url='http://starecat.com/content/wp-content/uploads/two-states-of-every-programmer-i-am-god-i-have-no-idea-what-im-doing.jpg')
```

We hope you've learned enough in these two days to be excited about what NLTK
can add to your work and you're feeling confident to start working on your own.
Code breaks. Often. Be patient and try not to get discouraged.
The good thing about code breaking so often is that you can find help. Try:
* Coming back to these notebooks and refreshing your memory
* Checking the NLTK book
* Googling your error messages. This will often lead you to Stack Overflow, the
major online community for sharing coding questions.
* NLTK also has a Google group where people share their experiences and ask for
help
* Keep in touch! Your community is a wonderful resource.

## Summaries and goodbye

Before we go, we should summarise what we've learned. Add all this to your CV!

* Navigating the IPython notebook
* Python commands - defining a variable; building a function
* Using Python to perform basic quantitative analysis of text
* Tagging and parsing to perform more sophisticated analysis of language
* A crash course in corpus linguistics!
* An appreciation of clean vs messy data and data structure
* Data management practices

## Bragging rights

The work you have been doing today on the Fraser corpus is actually pretty
cutting edge. Very little analysis like this has been undertaken on an
Australian political corpus.

You have produced publishable work today. Really. Be proud. And if you feel like
writing up your findings, do it!

## Thanks!

That's the end of of course. Thank you to everybody for your participation.

Please let us know how you found the course.

Also, [submit a pull request](https://github.com/resbaz/lessons) and improve our
teaching materials!

## Bibliography

<a id="ref:chomsky"></a>
Chomsky, N. (1965). Aspects of the Theory of Syntax (Vol. 11). The MIT press.

Eggins, S. (2004). Introduction to systemic functional linguistics. Continuum
International Publishing Group.

Halliday, M., & Matthiessen, C. (2004). An Introduction to Functional Grammar.
Routledge.

<a id="ref:sinclair"></a>
Sinclair, J. (2004). Trust the text: Language, corpus and discourse. Routledge.
Available at
[http://books.google.com.au/books/about/Trust_the_Text.html?id=n6xU2lyVoeQC&redi
r_esc=y](http://books.google.com.au/books/about/Trust_the_Text.html?id=n6xU2lyVo
eQC&redir_esc=y).

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
