---
layout: post
title:  "Are there publications in Mathematics written only by women?"
date:   2019-06-26
comments: True
math: true
categories: articles
---

<p style='text-align: justify;'> 
Earlier this month I attended the <a href="https://eller.arizona.edu/events/icsd19">18<sup>th</sup> International Conference on Social Dilemmas</a>.
The conference was a great experience and I would highly recommend it if you
are interested in the social dilemmas' research field. During the 2<sup>nd</sup>
day of the conference and after having sat in many of the talks there was something
I started to notice. That was that all the speakers, including female speakers, had mainly
only male collaborators and even if a speaker had a female collaborator then it would
be just one.
</p>

<p style='text-align: justify;'> 
I am an example of this. At the conference I presented a piece of work
that I conducted with my collaborators, and all three of them are men.
So I started to wonder: Do female researchers publish alone? And more specifically
do female Mathematicians publish together?

So in this post I am going to try and answer the following question:
</p>

<p style='text-align: center;'>
<span style="font-weight:bold">Are there publications in Mathematics written only by women?</span>
</p>

**Collecting Data**

<p style='text-align: justify;'>
To explore the question the first thing that is needed is data. In order to collect
meta data on publications I am using an open source package, to which I am a
maintainer, called <a href="https://github.com/ArcasProject/Arcas">Arcas</a>.
</p>

<p style='text-align: justify;'>
Using the library I can collect data from <a href="https://arxiv.org/">arXiv</a>,
<a href="https://www.springer.com/gp">Springer</a> and <a href="https://www.ieee.org/">IEEE</a>
on the topic of <strong>Mathematics</strong>. If you are interested in the code for collecting
the data it can be found <a href="https://github.com/Nikoleta-v3/Nikoleta-v3.github.io/blob/master/assets/code/Women%20in%20Mathematics%20Blog%20data%20collection%20source.ipynb">here</a>
(in the form of a Jupyter Notebook).
</p>

**Determining the Authors' Gender**

<p style='text-align: justify;'>
Using Arcas a total of 19280 Math articles have been collected.
Arcas returns a series of meta data but most of them are ignored for the purpose
of this post. The meta data
of interest here are the <strong>unique key</strong> (which will allows us to differentiate
between articles), the <strong>year</strong> of publication, the names of the
<strong>authors</strong>, the <strong>title</strong> of the article and the
<strong>source</strong> (arXiv, Springer or IEEE).
</p>

<p style='text-align: justify;'>
An author's name consists of the author's forename and surname. To determine the
gender of the author I am using a Python package called <a href="https://pypi.org/project/gender-guesser/">
<code>gender_guesser</code></a>.
<code>Gender_guesser</code> is an open source package which verifies gender based on the forename!
The package it's based on the program "gender" by <a href="https://autohotkey.com/board/topic/20260-gender-verification-by-forename-cmd-line-tool-db/">Jorg Michael</a> and its
use is pretty straightforward:
</p>

{% highlight python %}
>>> import gender_guesser.detector as gender
>>> d = gender.Detector(case_sensitive=False)

>>> d.get_gender('nikoleta')
'female'
{% endhighlight %}

The result of `d.get_gender` can be one the following:

- `unknown` (name not found),
- `andy` (androgynous),
- `male`,
- `female`,
- `mostly_male`,
- `mostly_female`.

Now that the gender of the authors has been "determined" there are 3 extra measures
left to introduce. The **number of authors** per article, the **ratio of female authors**
\\(\frac{\text{#female} + \text{#mostly_female}}{\text{#authors}}\\) per article
and the ratio of male authors \\(\frac{\text{#male} + \text{#mostly_male}}{\text{#authors}}\\).

Now that all the measures have been presented we can proceed to the analysis.
Note that the data is archived and available <a href="https://zenodo.org/record/3257436#.XRNq_VVKjZI">
here</a>.

**Analysis**

<p style='text-align: justify;'>
There are a total of 28404 different authors within the data set. There are
authors with multiple entries/papers. Out of the 28404 authors 10991 of them have
been identified as males and 482 as mostly males. A total of 2168 have been identified
as females and 186 as mostly females.
There are 10144 authors whose gender is unknown and it will be left as it is.
I would like to avoid making any further assumptions regarding the authors' gender.
</p>
<br>

|&nbsp; Gender &nbsp; | &nbsp;Count in data &nbsp; |
|:------------:       |:-------------:|
|mostly_female        |186             |
|mostly_male          |482             |
|female               |2168            |
|andy                 |3584            |
|unknown              |10144           |
|male                 |10991           |


<br>

<p style='text-align: justify;'>
Thus, approximately 11473 authors within the data set are men and 2354 are
women. This indicates a substantial gender gap in mathematics publications.
<strong>For every female author there are 5 male ones.</strong>
</p>

<p style='text-align: justify;'>
Mathematics is a field known to be suffering from gender
inequality. For example only one woman,
<a href="https://en.wikipedia.org/wiki/Maryam_Mirzakhani">Maryam Mirzakhani</a>,
has won the <a href="https://en.wikipedia.org/wiki/Fields_Medal">Fields Medal</a>
in the field and less than 30% of all U.S. doctoral degrees in mathematics and
statistics are awarded to women (<a href="https://ncses.nsf.gov/pubs/nsf19304/digest/field-of-degree-women#mathematics-and-statistics">source</a>).
It is important to note that actions are being taken to encourage women employment
in higher education and research positions in Mathematics. One example of this is
<a href="https://www.ecu.ac.uk/equality-charters/athena-swan/">Athena SWAN</a>
in the UK.
</p>

<p style='text-align: justify;'>
Figure 1 shows the mean number of female authors over the years. Though there
were not many female authors before 2010 there seems to be an increase in
numbers since then.
</p>

<figure align="center">
  <img src="/assets/images/number_of_authors_per_year.png" style='height: 20%; width: 50%; object-fit: contain'>
  <figcaption>Figure 1 - Mean number of authors over time.</figcaption>
</figure>

<p style='text-align: justify;'>
Let's explore the gender ratios and more specifically their distributions. The
summary statistics of the distributions are given by the table below, and the
normalised distributions are also shown in Figure 2.
</p>
<br>

|&nbsp;Gender&nbsp;|&nbsp;Female Ratio&nbsp;|&nbsp;Male Ratio&nbsp;|
|:------------:|:-------------:|:-------------:|
|mean          |**0.072**      |**0.497**      |
|std           |0.210          |0.447          |
|min           |0.000          |0.000          |
|25%           |0.000          |0.000          |
|50%           |0.000          |0.500          |
|75%           |**0.000**      |1.000          |
|max           |**1.000**      |1.000          |

<br>

There is a significant difference (p value\\(=0.0\\)) between the two distributions
as can be seen in the plots of the distributions.

<p style='text-align: justify;'>
The are two
main conclusions that can be made from the ratio distributions. These are that
<strong>50% of the publications have a male ratio of 0.5 or above
and, 75% of the publications have no female authors</strong>.
</p>

<figure align="center">
<div class="row">
  <div class="column">
    <img src="/assets/images/female_ratio_distribution.png" alt="Females" style="width:40">
  </div>
  <div class="column">
    <img src="/assets/images/male_ratio_distribution.png" alt="Males" style="width:40">
  </div>
</div>
  <figcaption>Figure 2 - Ratio Distribution.</figcaption>
</figure>

<p style='text-align: justify;'>
There are however publications that include women authors 🎉. To gain a
better understanding of the papers that do have female authors the female ratio
distribution is plotted once again, but this time the values of 0 have been excluded.
This is shown in Figure 3.
</p>

So **there are indeed articles written only by women**. It is most common
however, that if a paper has women authors that, on average,
**they make up the half of the authors**.

<figure align="center">
    <img src="/assets/images/female_ratio_distribution_excl_zeros.png" alt="Females" style="width:45%">
  <figcaption>Figure 3 - Female Ratio Distribution excluding zero values.</figcaption>
</figure>

<br>

The papers that have been written by **only** female authors can also be further
examined. The number of authors per paper, written only by women, is shown
in the following table:
<br>

|&nbsp;Number of Authors of papers written by women (female ratio = 1) &nbsp;|&nbsp;Count&nbsp;|&nbsp;Percentage&nbsp;|
|:------------:|:-------------:|:-------------:|
|1             |457            |77.72          |
|2             |117            |19.90          |
|3             |12             | 2.04          |
|4             |1              | 0.17          |
|7             |1              | 0.17          |

<br>

<p style='text-align: justify;'>
78% of the papers written only by women are written only by a single woman.
117 articles have each been written by 2 women and 12 by 3.
There is however a paper with 7 women authors and that is the article
<a href="https://link.springer.com/article/10.1007/s40993-018-0146-6">"Modular invariants for genus
3 hyperelliptic curves"</a> written by Sorina Ionica, Pinar Kilicer, Kristin
Lauter, Elisa Lorenzo Garcia, Maike Massierer, Adelina Manzateanu
and Christelle Vincent.
</p>

<p style='text-align: justify;'>
For comparison, the same table is given for male authors. Though similarly
a big percentage of papers written only by men have been written by a single man,
approx 60%, the interesting difference <strong>appears to be in papers with 3 to 5 authors</strong>.
Almost 8% of the publications written by only men, have been written by 3 authors.
On the other hand only 2% of the publications written by women have 3 authors. That
is 12 articles compared to 589. Similar observations can be made for papers
with 4 and 5 authors. <strong>There are hardly any with only female authors</strong>.
</p>

<br>

|&nbsp;Number of Authors of papers written by men (male ratio = 1) &nbsp;|&nbsp;Count&nbsp;|&nbsp;Percentage&nbsp;|
|:------------:|:-------------:|:-------------:|
|1             |4545           |60.40          |
|2             |2244           |29.82          |
|3             |589            | 7.82          |
|4             | 125           | 1.66          |
|5             |  14           | 0.19          |
|6             |   7           | 0.09          |
|8             |   1           | 0.01          |

<br>

**Summary**

<p style='text-align: justify;'>
In summary, in this post I have tried to present some data that allow us to
understand publications in the field of Mathematics, and more specifically
the inequality between the authors' gender.
</p>

<p style='text-align: justify;'>
<strong>There are significantly less women publishing in Mathematics than men</strong>. More
specifically every woman author corresponds to 5 males ones and 75% of the articles
we have gathered (automatically) do not have a single female author.
Women mathematicians do however exist, I am also a proof!, and they do publish.
</p>

<p style='text-align: justify;'>
If a papers has female authors then they are more likely to make up half of the
authors, and there are also publications that are solitarily by women.
However, there are significantly less publications made by teams of 3 to 5 women
compared to men.
</p>

<p style='text-align: justify;'>
The difference between gender publications has also been examined over the different
sources used in this post, arXiv, Springer and IEEE, and there appears to be
no significant effect based on the source.
</p>

<p style='text-align: justify;'>
Though the number of women in Mathematics in research positions are increasing
and publications are made with other collaborators, there appears to be a boundary that
makes collaborations of a large group of only women hard. There
are several causes that could be influencing this effect, though I can only
make speculations to this point,
</p>

- The low ratio of women to men. Mathematics is an enormous field and with
  such a low ratio of women to men finding women in your field could be a task.
- Journals can be biased.
  - Anyone is more likely to publish if one of the named authors is "famous".
    The probability of an author being famous is higher if they are a man.
  - [Men reviewers are more likely to accept articles written by men](https://www.wiley.com/network/researchers/being-a-peer-reviewer/gender-bias-and-the-peer-review-process) and most reviewers
    are men. Thus you are more likely as a woman to publish if you have another
    man co author.

**Comparison to Psychology**

<p style='text-align: justify;'>
So there is clearly an issue with gender equality in Mathematical publications.
Though I have not yet identified the cause of the effect, that might end up being a
future post, many might wonder: are not all fields similar?
So let's compare the gender ratios to another field.
</p>

<p style='text-align: justify;'>
I originally came up with the idea while sitting in a series of talks given
by Psychologists. So it only seems fair to collect data in Psychology.
Note that arXiv does not have papers in Psychology so data have been collected only
from IEEE and Springer. There a total of 10169 articles within the new data,
and a total of 23740 unique authors. The gender ratio distributions are given
by Figure 4.
</p>

<figure align="center">
<div class="row">
  <div class="column">
    <img src="/assets/images/female_ratio_distribution_psychology.png" alt="Females" style="width:40">
  </div>
  <div class="column">
    <img src="/assets/images/male_ratio_distribution_psychology.png" alt="Males" style="width:40">
  </div>
</div>
  <figcaption>Figure 4 - Ratio Distribution in Psychology papers.</figcaption>
</figure>

**It's hard to tell the difference between them right? Hopefully the ratios for**
**Math publications will be the same in the near future.**

If you are interested in the topic of gender inequality, gender stereotypes and how
questionable science has been fueling the gender gap I highly recommend the book
<a href="http://www.angelasaini.co.uk/inferior">Inferior</a>
written by, one of my favourite authors, Angela Saini.

Finally, the source code used for this post is available on GitHub:

- <a href="https://github.com/Nikoleta-v3/Nikoleta-v3.github.io/blob/master/assets/code/Women%20in%20Mathematics%20Blog%20cleaning%20data.ipynb"> Cleaning data notebook</a>
- <a href="https://github.com/Nikoleta-v3/Nikoleta-v3.github.io/blob/master/assets/code/Women%20in%20Mathematics%20blog%20post.ipynb"> Analysis notebook</a>
- <a href="https://github.com/Nikoleta-v3/Nikoleta-v3.github.io/blob/master/assets/code/Women%20in%20Mathematics%20Blog%20post%20Psycology%20data.ipynb"> Psychology analysis</a>
