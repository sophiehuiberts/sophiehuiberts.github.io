+++
title = "Smoothed Analysis is Bullshit*"
hasmath = false
hascode = false

date = Date(2026, 7, 21)
tags = ["blog", "theory"]

rss_title = title
rss_description = "Honesty posting."
rss_pubdate = date
+++

# Smoothed Analysis is Bullshit*

Last week we organized a [workshop at Schloss Dagstuhl](https://www.dagstuhl.de/en/seminars/seminar-calendar/seminar-details/26292) on analysis of algorithms beyond the worst case.
Here, beyond worst-case[^BWCAbook] refers to a pressing tension in the scientific study of algorithms: theory is often useless shit.[^spicy]
I mean this with love.

Theoretical computer science has given us many great pieces of theory, predominantly following the classic modeling paradigm called 'worst-case analysis'.
Under this paradigm, we specify a single algorithm (or class of algorithms) with mathematical precision and rigor, along with the set of valid inputs.
A worst-case analysis then attempts to find the worst-possible performance of the algorithm (in time, memory, or output solution quality).
We describe that worst-case performance as a function of the size of the input, often counted as the number of bits or number of numbers.
In some parts of computation, whether modern or undergrad curriculum, worst-case analysis is a useful paradigm for understanding your algorithm's performance.

Other parts of computation spit in the face of theory. Paging, SAT, graph coloring, linear programming, mixed-integer linear programming, clustering, traveling salesperson.
Each of these works better, is solveable faster, or returns better quality solutions than what worst-case analysis would predict.
SAT and MILP are NP-hard in theory.
In practice they are easy.[^explanatorycommaSATMIP]
The simplex method for linear programming runs in exponential time under worst-case analysis.
In practice its blazingly fast.[^explanatorycommaLP]

We need different approaches, new modeling strategies.
And the problem is, mathematicians like us are poorly equipped to think critically about what we do.
Nobody believes in average-case analysis.
But also smoothed analysis is kinda bullshit,[^smoothedistrash]
even if people still believe in it.[^godel]

So let me describe you how I got radicalized against smoothed analysis, so that you too can stop falling for its cursed allure.
These arguments mostly come from discussions with my PhD student [Eleon Bach](https://eleonbach.github.io/).
They were published in [this paper](https://arxiv.org/abs/2510.21613) but here on the blog I can write with more zest.

## Hypersparsity

Real-world linear program constraint matrices have a property called hypersparsity.
For one, that means that the matrix is very sparse: 99.99% of matrix entries are zero (and thus not even stored in memory). Only 0.01% is non-zero.
This is important for real-world codes: the simplex method is [memory-bound](https://www.mixedinteger.org/EUROMIP/2025/slides/laurent-porrier.pdf), so if you destroy all zeroes then your running time blows up.
The instances we study in smoothed analysis have no sparsity: with probability 1, all entries of the matrix are non-zero.
This is bad.

The second part of hypersparsity is the structure of the non-zeroes: most non-zero matrix entries share a handful of values.
Most entries are probably equal to 1, but even among the remaining entries there are only a limited number of unique values.
This is a problem for smoothed analysis: almost surely, all entries have distinct values.
This is bad.

## Worst-case instances are sparse and stable

So smoothed instances do not look like real-world instances for sparsity reasons.
This is unlike worst-case instances: the Klee-Minty cubes are hypersparse.
Looking purely at the patterns in the constraint matrix, one would assume that KM-cubes are more like real-world instances than smoothed analysis instances are.

You may think "could we do smoothed analysis in a sparse way? We could only perturb the non-zero entries in the input data".
This thought puts you in good company: Spielman and Teng [propose](https://arxiv.org/abs/cs/0111050) the same thing.
Too bad its a dead end.
If you choose the parameters right, then the behavior of the KM-cube is stable under constant-magnitude zero-preserving perturbations.
That is, the zero-preserving smoothed complexity of the simplex method (with the most-negative reduced cost pivot rule) is exponential.

## Even imprecise numbers are precise

Next issue. Some linear programs do contain numbers that 'look inaccurate': numbers like 15.79081 or 43.15593.
Maybe those numbers contain some type of independent Gaussian noise to make them look like that?
I don't think so.
If you allow yourself to change such 'ugly' numbers by as little as 0.01% then the previously-optimal solution will violate some constraints by as much as 50%.
This holds for 13 out of 90 NETLIB instances, quite a large amount.[^robust]
Feel free to disagree with my interpretation, but I personally think that means that the 'ugly random-looking numbers' have important non-trivial relations between eachother.
Hence they cannot be changed in isolation.
This would violate a key assumption of smoothed analysis: that all random noise entries are independently distributed.

## Singular matrices should stay singular

In a typical LP, not every maximal square submatrix is a basis. Some submatrices are singular.
Singular submatrices are good: the simplex method will never pivot to them, so they don't cost any extra time.
If you add small random Gaussian noise to your constraint matrix, then every submatrix will be basic.
This is a disaster: what used to be a singular submatrix (good) is now a basic submatrix with high condition number (very bad).

If you read your favorite solver's user manual or talk to their helpdesk, you will learn this:
please write your input data in maximum precision. Use integer numbers if you can, or 64-bit floats if you must.
Do not use 32-bit floats, for they are too imprecise and that imprecision hurts solver performance.
One reason that is given for this advice is what I say above: low precision may cause nominally singular submatrices to pass the threshold and become basic.
A basis with high condition number will ruin your numerical stability and your simplex method performance along with it.

So: smoothed analysis says 'more noise (less precision) is good'.
The user manual says 'more precision is good'.
Smoothed analysis is wrong, the user manual is correct.

## Philosophical incoherence

Final issue. In smoothed analysis, we assume that there is an idealized piece of input data which gets perturbed by random noise added after its formulation.
What is this noise? Where does it come from? What does it model?

Some people suggest the noise as modeling floating-point inaccuracy, implicitly advocating for a value of $\sigma = 2^{-53} \approx 10^{-16}$ as per IEEE 754.
Other people speak of the noise as modeling measurement error, which would endorse a value of $\sigma \approx 10^{-2}$.
This disagreement is ridiculous.
Imagine admitting this to a real scientist, like a physicist: "yeah we have a theoretical understanding, except we don't agree what our model models and our opinions about the central parameter's value differ by 14 order of magnitude."
Not a good look.

## Smoothed analysis is trash

So that's the deal, those are the arguments that radicalized me against smoothed analysis for LP.
Its instances don't pass the most basic sanity checks, its conclusions seem to contradict the user manual ground truth, and the whole theory lacks a coherent philosophical grounding.

This all leaves a crucial question: how should one analyze an algorithm when worst-case analysis fails?
At the workshop we had talks from different subfields, and people had different partial answers to this question.
Some people went deeper into the theory, other people proposed to engage more with computational experiments in one form or another. Some people thought that a good analysis method would be broadly applicable, others were happy to exploit more problem-specific features.
This is an exciting set of questions and a lively research field.

For the simplex method, by far the best current analysis framework is [by-the-book analysis](https://arxiv.org/abs/2510.21613), our new baby (STOC '26). It is not perfect but it is a big step up from all that came before.

I am curious to see where all the workshop's topics and participants will go next.
Although none of us has all the answers, we are all making progress.


[^BWCAbook]: For an overview of items falling under this umbrella, take a look at [this book edited by Tim Roughgarden](https://www.cambridge.org/9781108494311#resources) and use password 'BWCA\_CUP' to open it.

[^spicy]: I am not pulling any punches today. The literature doesn't capture people's feelings, and normally you can only observe those by attending an IRL event. However, events like this workshop, broadly scoped on BWCA in its entirety, happen only once every 12 years and only permit 25 attendees. As such, you probably don't often hear the spicier takes. This blog post collects a few more critical notes, some mine and some taken from others. Don't get discouraged by any of this criticism: it applies to everyone's work, including my own. I built my career on doing smoothed analysis of the simplex method, so mostly I am throwing in my own windows here. The point is to reflect critically, and sometimes that is easier when we don't mince our words. Anyway, see for yourself what you can get out of this blog post: use it to help develop your own sense of critical evaluation. Nobody trains us mathematicians how to do this, and we gotta learn somehow.

[^explanatorycommaSATMIP]: Crafting hard instances is not difficult. But somehow, we have reams and reams of practical real-world instances which are easy to solve. Electronics designs get verified bug-free by showing that huge boolean formulas are UNSAT. Swathes of global shipping and manufacturing get planned by solving MIPs. 10k variables is tiny for these solvers, but should be huge for any true believer in NP-hardness.

[^explanatorycommaLP]: For the simplex method, crafting hard instances is difficult. What I mean by that is, crafting numerically unstable instances is easy and your solver will struggle with those. But instances without numerical problems, but which do exhibit super-polynomial running times on real physical IEEE 754 compliant computer hardware? Constructing those is an open research problem.

[^smoothedistrash]: At least it is trash for the context of studying linear programming, as I will lay out in this blog post. For other algorithms and applications areas, these arguments may not apply. Maybe for your problem, smoothed analysis is fine. I invite you to consider this matter critically in the context of the algorithm you study.

[^godel]: People really did believe in this theory. Spielman and Teng's [Gödel Prize citation](https://www.sigact.org/prizes/g%C3%B6del/2008.html) claims that smoothed analysis "provides a new rigorous framework for explaining the practical success of algorithms". Those are big claims: not only is it rigorous (mathematically? scientifically? who knows), but it provides actual explanation!

[^robust]: These facts are taken from [this book](https://web.archive.org/web/20251211052055/https://www2.isye.gatech.edu/~nemirovs/FullBookDec11.pdf), although the authors give a very different interpretation of what these same facts mean.
