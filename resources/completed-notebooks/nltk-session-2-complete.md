<br>
<img style="float:left" src="http://ipython.org/_static/IPy_header.png" />
<br>

# Session 2: Common NLTK tasks

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
GitHub](https://github.com/resbaz/nltk). We can ask Python to get it for us.

> Later in the course, we'll discuss how to extract data from the Web and turn
this data into a corpus.

```python
from urllib import urlopen # a library for working with urls
url = "https://raw.githubusercontent.com/resbaz/nltk/master/corpora/oz_politics/ozpol.txt" # define the url
raw = urlopen(url).read() # download and read the corpus into raw variable
raw = unicode(raw.lower(), 'utf-8') # make it lowercase and unicode
len(raw) # how many characters does it contain?
raw[:2000] # first 2000 characters
```

So that just got one file. Next, let's have a look at [Project
Gutenberg](https://www.gutenberg.org/wiki/Technology_%28Bookshelf%29). Let's
check out *Food processing*.

We can find the URL of a txt file and download it just like above.

 We could use a loop to get more, however. Let's also write a function to get
the texts of books we want. Let's use the book number section to do that.

```python
booknums = ['24510', '19073', '21592']
```

```python
def gutenberger(list_of_nums):
    text = []
    from urllib import urlopen
    for num in list_of_nums:
        num = str(num)
        url = 'https://www.gutenberg.org/cache/epub/' + num + '/pg' + num + '.txt'
        raw = urlopen(url).read()
        raw = unicode(raw, 'utf-8')
        title = [line for line in raw.splitlines() if line.startswith('Title:')]
        if title:
            title = title[0]
            print title
        text.append([title, raw])
    return text
```

Let's call our function:

```python
foodbooks = gutenberger(booknums)
```

We could then use indexing:

```python
book[0][0]
```

Or we could then use another loop to start playing with our data:

```python
for title, text in foodbooks:
    print title.upper()
    print text[10000:10500]
    print '\n\n'
```

For this session, we'll work with the forum corpus.

We actually already downloaded a version of this file when we first cloned the
ResBaz GitHub repository. It's in our *corpora* folder. We can access it like
this:

```python
f = open('corpora/oz_politics/ozpol.txt')
raw = f.read()
raw = unicode(raw.lower(), 'utf-8') # make it lowercase and unicode
len(raw)
raw[:2000]
```

## Sentence segmentation

So, with a basic understanding of regex, we can now start to turn our corpus
into a structured resource. At present, we have 'raw', a very, very long string
of text.

 We should break the string into segments. First, we'll split the corpus into
sentences. This task is a pretty boring one, and it's tough for us to improve on
existing resources. We'll try, though.

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
print tokenized_sents[:10]
# another view:
# tokenized_sents[:10]
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

Give it a word to stem!

```python
stem('friends')
```

Let's run it over some text and see how it performs.

```python
# empty list for our output

for sent in tokenized_sents[:5]:
    for token in sent:
        print stem(token)
```

Looking at the output, we can see that the stemmer works: *wingers* becomes
*winger*, and *tearing* becomes *tear*. But, sometimes it does things we don't
want: *Nothing* becomes *noth*, and *mate* becomes *mat*.

We can see that this approach has obvious limitations. So, let's rely on a
purpose-built stemmer. These rely in part on dictionaries. Note the subtle
differences between the two possible stemmers.

Currently, we have a list of sentences, and each sentence is a list of words. We
need to flatten this list:

```python
tokens = []
for sent in tokenized_sents:
    for token in sent:
        tokens.append(token)
```

Now we can try our NLTK's stemmers!

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

We'll use corpkit, which relies on Spindle.

```python
! pip install corpkit
import corpkit
from corpkit import keywords
```

```python
# this tool works with raw text, not tokens!
keys, ngrams = keywords(raw.encode("UTF-8"))
for key in keys[:20]:
    print key
```

Success! We have keywords.

> Keep in mind, the BNC reference corpus was created before ISIS and ISIL
existed. *Moslem/moslems* is a dispreferred spelling of Muslim, used more
frequently in anti-Islamic discourse. Also, it's unlikely that a transcriber of
the spoken BNC would choose the Moslem spelling. *Having an inappropriate
reference corpus is a common methodological problem in discourse analytic work*.

## Collocation

> *You shall know a word by the company it keeps.* - J.R. Firth, 1957

Collocation is a very common area of interest in corpus linguistics. Words
pattern together in both expected and unexpected ways. In some contexts, *drug*
and *medication* are synonymous, but it would be very rare to hear about
*illicit* or *street medication*. Similarly, doctors are unlikely to prescribe
the *correct* or *appropriate drug*.

This kind of information may be useful to lexicographers, discourse analysts, or
advanced language learners.

In NLTK, collocation works from ordered lists of tokens. We made this earlier as
tokens, didn't we:

```python
print tokens[:50]
```

If not, here:

```python
tokens = []
for sent in tokenized_sents:
    for token in sent:
        tokens.append(token)
```

Now, let's feed these to an NLTK function for measuring collocations:

```python
# get all the functions needed for collocation work
from nltk.collocations import *
# define statistical tests for bigrams
bigram_measures = nltk.collocations.BigramAssocMeasures()
# go and find bigrams
finder = BigramCollocationFinder.from_words(tokens)
# measure which bigrams are important and print the top 30
print sorted(finder.nbest(bigram_measures.raw_freq, 30))
```

So, that tells us a little: we can see that terrorists, Muslims and the Middle
East are commonly collocating in the text. At present, we are only looking for
immediately adjacent words. So, let's expand out search to a window of *five
words either side*

```python
# ''window size'' specifies the distance at which 
# two tokens can still be considered collocates
finder = BigramCollocationFinder.from_words(tokens, window_size=5)
```

Now we have the appearance of very common words! Let's use NLTK's stopwords list
to remove entries containing these:

```python
ignored_words = nltk.corpus.stopwords.words('english')
finder.apply_word_filter(lambda w: w.lower() in ignored_words)
```

There! Now we have some interesting collocates. Finally, let's remove
punctuation-only entries, or entries that are *n't*, as this is caused by
different tokenisers:

```python
finder.apply_word_filter(lambda w: w.lower() in ignored_words or not w.isalnum())
```

You can get a lot more info on collocation at the [NLTK
homepage](http://www.nltk.org/howto/collocations.html).

Completed bigrams code:

```python
# get all the functions needed for collocation work
from nltk.collocations import *
# define statistical tests for bigrams
bigram_measures = nltk.collocations.BigramAssocMeasures()
# go and find bigrams
finder = BigramCollocationFinder.from_words(tokens, window_size=5)
ignored_words = nltk.corpus.stopwords.words('english')
finder.apply_word_filter(lambda w: w.lower() in ignored_words or not w.isalnum())
# measure which bigrams are important and print the top 30
result = sorted(finder.nbest(bigram_measures.raw_freq, 30))
for bigram in result:
    print bigram
```

## Clustering/n-grams

Clustering is the task of finding words that are commonly **immediately**
adjacent (as opposed to collocates, which may just be nearby). This is also
often called n-grams: bigrams are two tokens that appear together, trigrams are
three, etc.

Clusters/n-grams have a spooky ability to tell us what a text is about.

There's also a method for n-gram production in NLTK. We can use this to
understand how n-gramming works.

Below, we get lists of any ten adjacent tokens:

```python
from nltk.util import ngrams
# define a sentence
sentence = 'give a man a fish and you feed him for a day; teach a man to fish and you feed him for a lifetime'  
tokenised = nltk.word_tokenize(sentence)
# length of ngram
n = 10
# use builtin tokeniser (but we could use a different one)
tengrams = ngrams(tokenised, n)
for gram in tengrams:
    print gram
```

So, there are plenty of tengrams in there! What we're interested in, however, is
duplicated n-grams:

```python
# arguments: a text, ngram size, and minimum occurrences
def ngrammer(text, gramsize = 3, threshold = 4):
    """Get any repeating ngram containing gramsize tokens"""
    # we need to import this in order to find the duplicates:
    import nltk
    from nltk.util import ngrams
    from collections import defaultdict
    # get ngrams of gramsize    
    if type(text) != list:
        text = tokenised = nltk.word_tokenize(text)
    text = [token for token in text if token.isalnum()]
    # get ngrams of gramsize    
    raw_grams = ngrams(text, gramsize)
    
    # a subdefinition to get duplicate lists in a list
    def list_duplicates(seq):
        tally = defaultdict(list)
        for i,item in enumerate(seq):
            tally[item].append(i)
            # return to us the index and the ngram itself:
        return ((len(locs),key) for key,locs in tally.items() 
               if len(locs) > threshold)

    # use our duplication detector to find duplicates
    dupes = list_duplicates(raw_grams)
    # return them, sorted by most frequent
    return sorted(dupes, reverse = True)
```

Now that it's defined, let's run it, looking for trigrams

```python
ngrammer(raw, gramsize = 3)
```

Whoops, punctutation.

```python
# add me:
text = [token for token in text if token.isalnum()]
```

Too many results? Let's set a higher threshold than the default.

```python
ngrammer(raw, gramsize = 3, threshold = 10)
```

We can use *Spindle*/corpkit for bigram searching as well:

```python
keys, ngrams = keywords(raw.encode("UTF-8"))
for ngram in ngrams[:50]:
    print ngram
```

## Concordancing with regular expressions

We've already done a bit of concordancing. In discourse-analytic research,
concordancing is often used to perform thematic categorisation.

```python
text = nltk.Text(tokens)  # formats our tokens for concordancing
text.concordance("muslims")

# A problem with the NLTK concordancer is that it only works with individual tokens. What if we want to find words that end with **ment*, or words beginning with *poli**?

# We already searched text with Regular Expressions. It's not much more work to build regex functionality into our own concordancer.

# From running the code below, you can see that bracketting sections of our regex causes results to split into lists:
```

```python
# define a regex for different aussie words
```

Well, it's ugly, but it works. We can see five bracketted results, each
containing three strings. The first and third strings are the left-context and
right-context. The second of the three strings is the search term.

These three sections are, with a bit of tweaking, the same as the output given
by a concordancer.

Let's go ahead and turn our regex seacher into a concordancer:

```python
def concordancer(text, query):
    for line in text.splitlines():
        if query in line:
            start, end = line.split(query, 1)  
            concline = [start[-30:], query, end[:30]]
            print "\t".join(concline).expandtabs(35)
```

```python
concordancer(raw, 'australia')
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
