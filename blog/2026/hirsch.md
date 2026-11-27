+++
title = "The poly Hirsch conjecture is solved! And strongly poly LP!"
hasmath = false
hascode = false

date = Date(2026, 11, 27)
tags = ["blog","theory", "simplex"]

rss_title = title
rss_description = "The inevitable has dropped."
rss_pubdate = date
+++

This week OpenAI claimed solutions to the two big open problems in optimization.
It felt inevitable this would happen, the companies' steady march through the field was bound to reach us eventually.
Still, it marks the end of an era.

## Hirsch conjecture
For convex polyhedra $P = \{x \in \mathbb{R}^d : Ax \leq b\}$ with $n$ facets, i.e., $A \in \mathbb{R}^{n \times d}, b \in \mathbb{R}^n$, its graph consists of all vertices of the polyhedron and all edges of the polyhedron.
This graph has a diameter: the minimum number of edges that suffices to connect any given pair of vertices.
In the early days of linear programming, Hirsch asked if the diameter is always at most $n-d$.
This became known as the Hirsch conjecture.
The Hirsch conjecture was resolved in the negative by Francisco Santos Leal in 2010.
He did that by constructing a polyhedron of diameter $1.05(n-d)$.
Through simple products, one example becomes an infinite family of them.

Ever since then, attention has shifted to the polynomial Hirsch conjecture: is there a polynomial $f(n,d)$ such that the diameter of any polyhedron is at most $f(n,d)$?
Researchers in the field had different beliefs about the poly Hirsch conjecture. Some expected it to be true, others to be false, most would not wager on either option.

This is an important question for complexity of the simplex method.
As traditionally thought of in theory, the simplex method moves from vertex to vertex along the edges of the feasible region.
Considered this way, the simplex method can only have polynomial running time only if the polynomial Hirsch conjecture is true.

According to OpenAI, [the polynomial Hirsch conjecture is false](https://github.com/openai/math/tree/main/preprints/Superpolynomial-graph-diameter-of-simple-polytopes-November-24-2026).
I have not looked at the proof, but I see they have a Lean certificate so I believe the claim.
What they show is remarkably strong: for any sub-polynomial function $s(n)=n^{o(1)}$, consider the graph of vertices of the polyhedron where two vertices are connected if they lie on the same $s(n)$-dimensional face of the polyhedron.
There is a family of bounded simple polyhedra $P_n$ with $n$ facets, in subpolynomial dimension $d=n^{o(1)}$, such that this new graph has superpolynomial diameter $n^{\omega(1)}$.
The polynomial Hirsch conjecture is the statement you get when $s(n)=1$.

This implies that all hopes are dead for a polynomial time simplex method IF we restrict ourselves to the traditional way of thinking where the algorithm only visits exact basic feasible solutions.

The practical relevance of this result is limited.
The robot tells me that the smallest polytope they construct is in dimension $10^{10^{19}}$.
But more importantly, simplex methods do not work like that.
Real-world implementations of the simplex method are full of special tricks and dark floating-point magic.
The polyhedron diameter model is a gross oversimplification, and it is that simplification which makes the superpolynomial lower bound possible.

One of the tricks used in practice is *perturbation* of the bounds or costs.
In [recent work](https://arxiv.org/abs/2510.21613) we already showed that simplex methods with bound perturbations run in polynomial time.
If we learn anything from the poly Hirsch refutation, it is that bound perturbations now *provably* speed up the simplex method.

## Strongly-polynomial time linear programming
The other big question that OpenAI resolved is the complexity of linear programming.
Ever since Khachiyan's result on the ellipsoid method, we know that LP can be solved in weakly-polynomial time.
That is, the worst-case running time is bounded as poly$(n, d, L)$ where $L$ is the number of bits necessary to write down the LP data $A,b,c$.
If the numbers in $A,b,c$ require more bits to write down, then the ellipsoid method is allowed to search for longer: not only are the numerical operations more work, but the number of numerical operations performed will grow.
Interior-point methods also admit weakly-polynomial time upper bounds, as do an assortment of other algorithms such as [this funny paper of Kelner and Spielman](https://doi.org/10.1145/1132516.1132524).

What was open for decades, is whether there exists a strongly-polynomial time algorithm for LP.
For an algorithm to run in strongly-polynomial time, the total number of operations has to be bounded by poly$(n,d)$ and the bit-complexity of any intermediate quantities in the algorithm should be poly$(n,d,L)$.
In the same drop as the Hirsch result, [OpenAI claims a strongly-polynomial time algorithm for LP](https://github.com/openai/math/tree/main/preprints/Strongly-polynomial-linear-programming-November-24-2026).
Once again it seems to be accompanied by a Lean certificate.

We do not understand yet what exactly they are doing.
As is well-known, OpenAI's pdfs are very poorly written.
They use new and unwieldly notation, and even new and unwieldly names, for many well-established concepts.

It will take a while to figure out exactly what they do.
I did notice some form of multiplicative weights or expert learning being used to learn the complementary pair $[n] = B \cup N$.
This is an idea which I believe we have not seen before.

I believe OpenAI's result also solves the Linear Complementary Problem (LCP), LP's annoying cousin.
With that, it also solves convex quadratic programming and turn-based mean-payoff games in strongly-polynomial time.
A big deal for people who care about that.
One consequence of that generality is that it makes the paper even harder to read.
I expect that the first step to understanding OpenAI's method will be to ask a robot to extract and simplify a result for LP only.

The practical relevance of this result are limited.
For linear programming, the robot tells me that the algorithm involves solving many different LPs of bounded bit-complexity.
This means thousands of LPs, each the same size as the original.
These solves can be sped up somewhat using a warm-started simplex method, but still the algorithm is slow.
Early vibe-experiments suggest that OpenAI's algorithm requires 300x more pivot steps than simply solving the LP directly.
Robot says that there are no useful ideas that could help improve existing solvers.

For parity games, Tom van Dijk [tried including](https://www.linkedin.com/posts/trolando_goodbye-beloved-research-project-just-share-7531485131027644416-OFoK/) OpenAI's algorithm in his solver Oink.
It happens to fit well with his existing frameworks, and hence OpenAI's method is only an order of magnitude slower than the state of the art.
This is much better than the situation in LP but still very slow.

## Reflection

I would not have guessed these outcomes.
For all I knew, poly Hirsch could have been true but strongly poly LP could have been impossible.

Having these answer, though, changes little.
Together with my collaborators we have been on the empiricist's track: the interesting research questions are all centered around 'why are existing LP/MILP solvers so fast?'
Such questions are unaffected by the resolution of these open questions, except insofar as that they indicate that the empiricist lens is necessary for any polynomial-time bound on simplex methods.
Vindication, of a sorts.
