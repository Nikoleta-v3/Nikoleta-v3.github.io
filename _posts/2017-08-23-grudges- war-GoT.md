---
layout: post
title:  "Grudges, War and Game of Thrones"
comments: true
math: true
categories: articles
---

<p align="justify">
You will often see me talking about <a href="https://en.wikipedia.org/wiki/Game_theory">Game Theory</a>.
But what is Game Theory and more importantly what does it allow us to 
comprehend? 
</p>

<p align="justify">
The right answer is many things. For example should people hold a grudge? In a 
war of many different parties what strategy should one use to stand 
victorious? Should the North and South join hands against the army of the dead?
</p>

<p align="justify">
All these questions can be answered by studying the emergence of cooperation. 
A topic that I am concerned with in my PhD study. 
The game commonly used is the famous <a href="https://en.wikipedia.org/wiki/Prisoner%27s_dilemma">Prisoner’s 
Dilemma (PD)</a>. The PD is a two player game where both players 
simultaneously can choose to either <b>Cooperate</b> or <b>Defect</b>. Though 
both sides are better off <b>Cooperating (3)</b> there will always be a 
temptation for an individual to deviate and <b>Defect (5)</b>. The following 
matrix matches the players moves to payoffs.

<p align="center">
  <img src="/assets/images/matrix.png" style='height: 20%; width: 30%; object-fit: contain'>
</p>

<p align="justify">
Simulating the PD interaction is made possible thanks to an open 
source library called the <a href="https://github.com/Axelrod-Python/Axelrod">
Axelrod Python Library</a>. The library is alive thanks to a league of 
extraordinary people, it is documented and tested to an extraordinarily high 
degree of coverage. This ensures the correctness of our results.
Now that we have the basics down let us explore some scenarios! 
</p>

<p align="justify">
Imagine that you are repeatedly interacting with a friend who is quite 
<code>Sneaky</code>. Your friend is cooperating with you but will also try 
to defect once and will repent only if punished. <b> Should you hold a grudge 
against that friend?</b> Let us create this scenario using the Axelrod 
library. Your friend will be playing a strategy called <code>SneakyTitForTat
</code> and  you can choose between two strategies: either hold a grudge by 
defecting all the way once your friend defects ( <code>Grudger</code> ) or 
after punishing your friend you can try to cooperate once again ( <code>
TitForTat</code> ).
</p>

{% highlight python %}

>>> import axelrod as axl

>>> first_match = axl.Match([axl.SneakyTitForTat(), axl.Grudger()], 
... 			    turns=20)
>>> first_match.play()[:6]
[('C', 'C'), ('C', 'C'), ('D', 'C'), ('D', 'D'), ('C', 'D'), ('C', 'D')]
>>> first_match.final_score()
(20, 55)
>>> second_match = axl.Match([axl.SneakyTitForTat(), axl.TitForTat()], 
... 			     turns=20)
>>> _ = second_match.play()
>>> second_match.final_score()
(57, 57)

{% endhighlight %}

<p align="justify">
In a simple match of the Iterated Prisoner's Dilemma the strategy <code>
TitForTat</code> can be seen to score higher than <code>Grudger</code>. Thus,
when playing against <code>SneakyTitForTat</code> you would be better off not holding a
grudge. 
</p>

<p align="justify">
Another feature of the library is simulating simplified <b>war scenarios</b>. In a 
war several players with different strategies collide in the battlefield. One 
can choose from over 200 strategies that are implemented within the library. 
Once the strategies are selected they can be placed into a tournament. Each 
player interacts with the rest of the players and the one with 
the highest average score is the winner. In the following example, six very 
basic strategies have been chosen and a tournament of 200 turns is repeated 5 
times.
</p>

{% highlight python %}
>>> import axelrod as axl

>>> axl.seed(0)
>>> players = [axl.Cooperator(), axl.Random(), axl.TitForTat(), axl.Grudger(),
...            axl.Defector()]

>>> tournament = axl.Tournament(players)
>>> results = tournament.play()
>>> results.ranked_names
['Grudger', 'Defector', 'Tit For Tat', 'Cooperator', 'Random: 0.5']
{% endhighlight %}

<p align="justify">
The library comes with a results summary where several measures can be
explored and visualised. Such as the winner and the winning score.
</p>


{% highlight python %}
>>> plot = axl.Plot(results)
>>> p = plot.boxplot()
>>> p.show()
{% endhighlight %}

<p align="center">
  <img src="/assets/images/boxplot.png" style='height: 40%; width: 70%; object-fit: contain'>
</p>

<p align="center">
<i>may contain spoiler for Game of Thrones season 7</i>
</p>

<p align="justify">
Lastly regarding the recent events of <a href="https://en.wikipedia.org/wiki/Game_of_Thrones">Game of Thrones</a> 
season 7, should the North alliance work with the South to defeat the Night 
King? <a href="https://en.wikipedia.org/wiki/Evolutionary_game_theory">
Evolutionary Game Theory</a>, allows the study of such population dynamics. In 
this example <a href="https://en.wikipedia.org/wiki/Moran_process">the Moran
process</a> will be used as an evolutionary algorithm. The Moran process is 
a birth day process where the weakest member of one generation is replaced by 
a stronger individual. How are the individuals compared? They are 
compared based on their normalized score in a tournament against the entire 
population. 
</p>

{% highlight python %}
>>> import axelrod as axl
>>> import random

>>> N = 5
>>> players = []
>>> axl.seed(5)
>>> for _ in range(N):
...     player = random.choice([axl.Defector, axl.Cooperator])
...     players.append(player())

>>> mp = axl.MoranProcess(players=players, turns=200)
>>> mp.play()
[Counter({'Cooperator': 3, 'Defector': 2}),
 Counter({'Cooperator': 3, 'Defector': 2}),
 Counter({'Cooperator': 3, 'Defector': 2}),
 Counter({'Cooperator': 2, 'Defector': 3}),
 Counter({'Cooperator': 2, 'Defector': 3}),
 Counter({'Cooperator': 1, 'Defector': 4}),
 Counter({'Cooperator': 1, 'Defector': 4}),
 Counter({'Cooperator': 1, 'Defector': 4}),
 Counter({'Defector': 5})]
>>> p.show()
{% endhighlight %}

<p align="center">
  <img src="/assets/images/evolution_results.png" style='height: 20%; width: 40%; object-fit: contain'>
</p>

<p align="justify">
In population dynamics we do not concern ourselves with players anymore but 
with strategies. In our example, if  two of the five lords in Westeros would 
always chose to defect then the rest of the lords can be seen to adapt the same
behavior. 
</p>

<p align="justify">
These are just simple examples of how Game Theory and the Axelrod Python 
Library can be used to comprehend everyday questions. There are various other 
scenarios that can be explored. Please find all details about the Axelord 
Python Library here: <a href="https://github.com/Axelrod-Python/Axelrod">
https://github.com/Axelrod-Python/Axelrod</a>.
</p>

<p align="justify">
 This blog post accompanies a poster which can be found here: <a href="https://nikoleta-v3.github.io/talks/talks//2017-08-28-Euroscipy/poster.pdf">
 https://nikoleta-v3.github.io/talks/talks//2017-08-28-Euroscipy/poster.pdf</a>
 which was presented at <a href="https://www.euroscipy.org/2017/">Euroscipy 2017</a>, in Erlangen Germany.
</p>