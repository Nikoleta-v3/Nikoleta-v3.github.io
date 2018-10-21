---
layout: post
title:  "An Evolutionary Game Theoretic Model of Rhino Horn Devaluation."
date:   2017-12-22
comments: false
---

Τhe name rhinoceros means `nose horn’ which is often shortened to rhino. The name
comes from the Greek words rhino (nose) and ceros (horn). There are five species
and 11 subspecies of rhino; some have two horns, while others have one
([source](https://www.savetherhino.org/rhino_info/for_kids/everything_rhino)).

Unfortunately, rhino populations are at critical level today due to the demand
for rhino horn and subsequent poaching. The current rhino populations are mainly
gathered in large protected areas. Wildlife managers, the people in charge of these
protected areas, have been using several approaches to secure the life of the animals.
One of these approaches includes removing the horn itself; dehorning. Namibia was
the first country to use dehorning to protect rhinos from poaching
([source](https://www.savetherhino.org/rhino_info/issues_for_debate/de-horning)).

There are numerous cases where dehorning has proved insufficient to prevent rhinos
from falling victim to poachers. The efficacy is dependent on the behaviour of
the poachers:

- There can be `selective poachers': will not hunt dehorned rhinos or
- `indiscriminate poachers': will kill any rhino, even dehorned rhinos have some
horn remaining and so have a small value.

The interaction between poachers and wildlife managers can be described using a
[game theoretic model](https://en.wikipedia.org/wiki/Game_theory). This work was
done in 2016 by [Tamsin Lee and David Roberts](https://ora.ox.ac.uk/objects/uuid:d6c01110-1f53-4efe-88c7-10fc35efb3ac)
and their game is given by the following matrix,

<p align="center">
  <img src="/assets/images/RhinoPic.png" style='height: 40%; width: 60%; object-fit: contain'>
</p>

The game is a two players game (manager and poacher). Both player have a set of
two strategies. To dehorn or not and to either be selective or indiscriminate.
Assuming that both players will behave to maximise their payoff, there are three
possible Nash equilibriums of the game. In the 2016’s work the authors studied
the following two equilibriums:

- either all rhinos are devalued and all poachers are selective;
- or all horns are intact and all poachers are indiscriminate.

Their work concludes that poachers will always choose to behave indiscriminately,
and thus the game settles to the bottom right quadrant, i.e., the poachers win!

Their work though did not take into the population dynamic effect of these strategies.
For example in a population full of indiscriminate poachers would there be a benefit
for a poacher to behave selectively? In 2017, alongside one of the 2016’s work
authors Tamsin Lee, we further enhanced that model to allow for cross dependencies
across multiple poachers. The game we consider is no longer a two player game but
now the players are an infinite population of poachers. The dynamics of these
populations have been explored using [evolutionary game](https://en.wikipedia.org/wiki/Evolutionary_game_theory).

There are three possible population that could be stable. These are,

- a population where everyone behave selectively;
- a population where everyone behave indiscriminately;
- a mixed population of poachers.

This is shown in figure below:

<p align="center">
  <img src="/assets/images/evolutionary.png" style='height: 80%; width: 100%; object-fit: contain'>
</p>

Using a realistic and generic utility model we tested the evolutionary stability
of these populations. The first set of results, both analytical and numerical,
showed that only the population of indiscriminate poachers is stable and further
more evolutionary stable. The figure below illustrates some of the numerical
experiments. It is shown that for any given starting population everyone converges
in an indiscriminately behaviour.

<p align="center">
  <img src="/assets/images/IndiscriminateESS.png" style='height: 100%; width: 100%; object-fit: contain'>
</p>

However, our results reveal that it is possible for a population of selective
poachers to exist, but for this to occur a disincentive must be applied to the
utility of indiscriminate poachers. The disincentive factor can have several
interpretations; such as engaging the rural communities that live with wildlife.

<p align="center">
  <img src="/assets/images/ESS-new-utility.png" style='height: 100%; width: 100%; object-fit: contain'>
</p>

Essentially, our results for these model have shown that selective behaviour amongst
poachers is unstable unless there is a big incentive.

The work described in this post has managed to be, in more details, developed
into an academic article. The article is now on a preprint server and under review
in order to be published. The preprint can be found on arXiv: <https://arxiv.org/abs/1712.07640>.

The source code for all the numerical experiments held are online and available
on Github: <https://github.com/drvinceknight/Evolutionary-game-theoretic-Model-of-Rhino-poaching>.
Note that all source code has been developed using best practice, thus
the code has been properly documented and automatically tested.

To finish off I would like to thank the co-authors for their support and explaining
to me that alpha is not a:

- [Tamsin E. Lee](https://twitter.com/t_e_lee)
- [Vincent Knight](https://twitter.com/drvinceknight)