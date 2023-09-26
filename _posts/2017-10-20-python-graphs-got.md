---
layout: post
title:  "Python, Graphs and Game of Thrones"
date:   2017-10-19
comments: true
math: true
categories: articles
---


<p align="center">
<b>(The following blog contains spoilers for the TV series Game of Thrones)</b>
</p>

<p align="justify">
This blog post is based on a lightning  talk that I delivered earlier this
month in a PGR meeting at Cardiff University and it is about 3 things that
fascinate me! These are:
</p>

- the programming language Python,
- graph/network theory,
- the book series a song of Ice and Fire.

<p align="justify">
It is built upon the article
<a href="https://www.maa.org/sites/default/files/pdf/Mathhorizons/NetworkofThrones%20%281%29.pdf">
Networks of Thrones</a>, by Andrew Beveridge and Jie Shan. In their work
they created a network for each of the five books in the series of
<a href="http://www.georgerrmartin.com/book-category/">a song of Ice and Fire</a>.
Each node of the network corresponds to a character whose name (or nickname)
is mentioned within the book.  If two character names (or nicknames) appeared
within 15 words an edge is added to connect these two characters. An edge
weight was also applied based on the number of interactions of the characters.
The data is open and accessible on
<a href="https://github.com/mathbeveridge/asoiaf">Beveridge’s Github profile</a>.
</p>

<p align="justify">
Once I got my hands on these data sets I decided to go ahead and perform
a very brief analysis. For now I will consider only the first book of the
series, a <a href="https://en.wikipedia.org/wiki/A_Game_of_Thrones">Game of Thrones</a>.
Using the Python library <a href="http://pandas.pydata.org/">pandas</a>
I load the data of the first book using the following command:

{% highlight python %}
>>> import pandas as pd
>>> a_game_of_thrones = pd.read_csv('/data/asoiaf-book1-edges.csv')
>>> a_game_of_thrones.head()

    Source                          Target              Type	    weight	book
0   Addam-Marbrand                  Jaime-Lannister	Undirected	3	1
1   Addam-Marbrand                  Tywin-Lannister	Undirected	6	1
2   Aegon-I-Targaryen               Daenerys-Targaryen	Undirected	5	1
3   Aegon-I-Targaryen               Eddard-Stark	Undirected	4	1
4   Aemon-Targaryen-(Maester-Aemon) Alliser-Thorne	Undirected	4	1
{% endhighlight %}

<p align="justify">
Now that the data has been loaded all I have to do is create the network.
For that I will make use of another Python library specialising in the
creation and manipulation of networks, called <a href="https://networkx.github.io/">networkx</a>.
Thus, creating the graph becomes trivial:
</p>

{% highlight python %}
>>> import networkx as nx
>>> G = nx.Graph()
>>> for row in a_game_of_thrones.iterrows():
...     G.add_edge(row[1]['Source'],row[1]['Target'],
...                weight=row[1]['weight'], book=row[1]['book'])
{% endhighlight %}

<p align="justify">
Networkx allow us to study the network structure and provides several
analysis measures. The first question that arises is <b> who is the
central character of a Game of Thrones?</b>
</p>

<p align="justify">
In graph theory and network analysis, this can be thought as the most
central node. In order to verify the most central characters several
centrality measures, available within networkx, will be used.
<b> Degree centrality</b>, corresponds to the number of links connecting
it to another node.
</p>

{% highlight python %}
>>> central_characters = sorted(nx.degree_centrality(G).items(),
...         key=lambda x:x[1], reverse=True)[0:10] central_characters
{% endhighlight %}

<p align="justify">
The code above returns a list of tuples with the name of the ten most
central characters based on degree centrality. Thereupon, I will be using
the library <a ref="http://matplotlib.org/#">matplotlib</a> to illustrate
the results.
</p>

<p align="center">
  <img src="/assets/images/degree_centrality.jpg" style='height: 20%; width: 40%; object-fit: contain'>
</p>

<p align="justify">
Based on degree centrality, <a href="http://gameofthrones.wikia.com/wiki/Eddard_Stark">
Eddard Stark</a> appears to be the central character followed by
<a href="http://gameofthrones.wikia.com/wiki/Robert_Baratheon">Robert Baratheon
</a>, <a href="http://gameofthrones.wikia.com/wiki/Tyrion_Lannister">
Tyrion Lannister</a>, <a href="http://gameofthrones.wikia.com/wiki/Catelyn_Stark">
Catelyn Stark</a> and <a href="http://gameofthrones.wikia.com/wiki/Jon_Snow">
Jon Snow</a>. But several measures exist, what is the difference in
value between them?
</p>

<p align="justify">
Let us look at a second measure the <b>betweenness centrality</b>, both weighted
and unweighted. The betweenness centrality for each node is the number of
the shortest paths that pass through the node.
</p>

{% highlight python %}
>>> betweenness_unweighted = sorted(nx.betweenness_centrality(G).items(),
...                                 key=lambda x:x[1], reverse=True)[0:10]
>>> betweenness_weighted = sorted(nx.betweenness_centrality(G, weight='weight').items(),
...                               key=lambda x:x[1], reverse=True)[0:10]
{% endhighlight %}


<p align="justify">
The new bar plots with the importance of the top 10 characters are now given
below. Though the unweighted betweenness centrality seems to have no difference
to that of the degree centrality, several characters change rank based on
the weighted version of the measure. Now Robert Baratheon stands out to
be the most important character, <a href="http://gameofthrones.wikia.com/wiki/Robb_Stark">
Robb Stark</a> is suddenly found in fourth place and once again Tyrion
Lannister is third.
</p>

<p align="center">
  <img src="/assets/images/betweeness_unweighted.jpg" style='height: 20%; width: 40%; object-fit: contain'>
</p>

<p align="center">
  <img src="/assets/images/betweeness.jpg" style='height: 20%; width: 40%; object-fit: contain'>
</p>

<p align="justify">
Still not convinced about the characters' importance? A third measure is
examined, that is the <a href="https://www.sci.unich.it/~francesc/teaching/network/pagerank">
Pagerank centrality</a>, which is a centrality measured originally used by Google.
</p>

{% highlight python %}
>>> page_rank_unweighted = sorted(nx.pagerank_numpy(G, weight=None).items(),
...                               key=lambda x:x[1], reverse=True)[0:10]
>>> page_rank_weighted = sorted(nx.pagerank_numpy(G, weight='weight').items(),
...                             key=lambda x:x[1], reverse=True)[0:10]
{% endhighlight %}

<p align="center">
  <img src="/assets/images/pagerank_unweighted.jpg" style='height: 20%; width: 40%; object-fit: contain'>
</p>

<p align="center">
  <img src="/assets/images/pagerank.jpg" style='height: 20%; width: 40%; object-fit: contain'>
</p>

<p align="justify">
It can be seen that different measures may have a different story to tell.
The measure which is more appropriate always depends on the researcher
and the question that is being answered. In my research I often find
myself altering between these measures for different projects.
Thus unfortunately a general rule can not be applied. But based on the
three measures that have been examined a character that is frequently ranked
first is the honourable Eddard Stark! Having read the book and watched
the TV series it is safe to argue that Eddard Stark is in fact the most
central character of the book A Game of Thrones or season 1.
</p>

<p align="justify">
These insight though are just for a single book of the series and we know
that not many characters make it far in the series. Could we use the measures
to gain insights on <b>the progression of a character?</b> The answer is yes.
After loading the rest of the books the weighted betweenness centrality
is used to gain the centrality of each character for each book.
</p>

{% highlight python %}
>>> measures = [nx.betweenness_centrality(graph, weight='weight')
...             for graph in [G, G_book2, G_book3, G_book4, G_book5]]
>>> evol_df = pd.DataFrame.from_records(evolution).fillna(0)

{% endhighlight %}

<p align="justify">
This data frame contains more characters whose name we can remember and
a bunch of people do end up dead so let’s get ahead and look at the top
5 ranked characters and plot just their progress.
</p>

<p align="center">
  <img src="/assets/images/progress.jpg" style='height: 20%; width: 40%; object-fit: contain'>
</p>

<p align="justify">
Grumpy old man <a href="http://gameofthrones.wikia.com/wiki/Stannis_Baratheon">
Stannis Baratheon</a> is a very minor character in the first book but
that changed during the series, him claiming that he is the King and
going around setting people on fire. Two other characters that developed
over time have been Jon Snow and <a href="http://gameofthrones.wikia.com/wiki/Daenerys_Targaryen">
Daenerys Targaryen</a>, both characters appear less in the fourth book
where the minor character <a href="http://gameofthrones.wikia.com/wiki/Balon_Greyjoy">
Balon Greyjoy suddenly </a>
appears from the sea. This is because the fourth book focuses on the War
of the Five Kings, what’s left of it, and the <a href="http://gameofthrones.wikia.com/wiki/Iron_Islands">
Iron islands</a>.
</p>

<p align="justify">
An efficient way of illustrating all the connections between our characters
throughout the series is by drawing a comprehensive network.
Can networkx be used for visualization?
Well, no. At least not for such big networks as it can be seen here,
</p>

<p align="center">
  <img src="/assets/images/network.jpg" style='height: 40%; width: 60%; object-fit: contain'>
</p>

<p align="justify">
Networkx is a tool, for analysis and not for visualization, though it
works very well with software that specialise on visualising networks,
so no hard feelings.
</p>

<p align="justify">
This has been a very brief analysis, and mainly I would like to illustrate
how Python and mathematics can be used. A song of Ice and a Fire is a
book series that I enjoy but I am sure all the above are applicable to
other titles as well.
</p>

<p align="justify">
The authors of Network of Thrones have performed their own analysis which
can be found in their <a href="https://www.maa.org/sites/default/files/pdf/Mathhorizons/NetworkofThrones%20%281%29.pdf">
website</a>, and I would also like to give credits
to a <a href="https://www.youtube.com/watch?v=iTOC8TQrF_U">
NetworkX workshop</a> I attended recently at EurosciPy for the inspiration
for my lightning talk and blog post.
</p>