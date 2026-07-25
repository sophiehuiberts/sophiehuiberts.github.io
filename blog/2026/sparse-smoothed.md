+++
title = "The Failure of Sparse Smoothed Analysis"
hasmath = true
hascode = false

date = Date(2026, 7, 25)
tags = ["blog", "theory"]

rss_title = title
rss_description = "A proof that Dantzig's and Bland's pivot rules have exponential sparse smoothed complexity."
rss_pubdate = date
+++

Earlier this week I posted a list of arguments why smoothed analysis is a poor model to study the running time of the simplex method.
(Hence why we introduced [by-the-book analysis](https://arxiv.org/abs/2510.21613) which has none of those problems.)

One thing I mentioned was that the standard Klee-Minty cube is stable under zero-preserving perturbations.
Someone asked me for a proof. Since there is currently no written source for this lore, I will write a quick proof here.

Let's start with some traditional Klee-Minty cubes:
\begin{align}
\operatorname{maximize} \quad &x_d \\
    \operatorname{subject~to} \quad & 0 \leq x_1 \leq 1 \\
    & 0.2 x_{j-1} \leq x_j \leq 1-0.2 x_{j-1} \qquad \forall j \in \{2,\dots,d\}.
\end{align}
Assume the inequalities are ordered as shown: first we list both inequalities for $j=2$ and then both inequalities for $j=3$ and so on.

We shift the means slightly and then perturb all non-zero coefficients.
This yields a linear program
\begin{align}
\operatorname{maximize} \quad x_d \hphantom{\leq}& \\
    \operatorname{subject~to} \quad  0.1 + \alpha_1 \leq& (1+\gamma_1) x_1 \\
    & (1+\delta_1) x_1 \leq 0.9+\eps_1 \\
     0.1 + \alpha_j + (0.2 + \beta_j)  x_{j-1} \leq& (1+\gamma_j) x_j \\
    & (1+\delta_j) x_j \leq (0.9+\eps_j) - (0.2 + \eta_j) x_{j-1} \qquad \forall j \in \{2,\dots,d\}.
\end{align}

**Theorem.** If $\alpha,\beta,\gamma,\delta,\eps,\eta \in [-0.05,0.05]^d$ then Bland's rule takes exponentially many pivot steps.

**Proof.** We go by induction. We prove that there are exactly $2^d$ bases, and Bland's rule will visit all bases when it starts at the basis minimizing $x_d$ and works to maximize $x_d$. All basic feasible solutions $x$ satisfy $x \in [0,1]^d$. The Bland path is reversible: if we start at the basis maximizing $x_d$ and we use Bland's rule to minimize $x_d$ then the same bases are visited but in reverse order.

We can check the base case $d=1$ easily. The starting basis consists of the constraint $0.1 + \alpha_1 \leq (1+\gamma_1)x_1$ and the end basis consists of the constraint $(1+\delta_1)x_1 \leq 0.9 + \eps_1$.
The two basic feasible solutions $\frac{0.1 + \alpha_1}{1+\gamma_1}$ and $\frac{0.9+\eps_1}{1+\delta_1}$ are easily verified to be in $[0,1]$.

When $d \geq 2$, assume that the induction statement has been proven for $d-1$.
The starting basis for $d$ consists of all lower-bounding constraints: $0.1 + \alpha_1 \leq (1+\gamma_1)x_1$ and $0.1 + \alpha_j + (0.2+\beta_j)x_{j-1}\leq(1+\gamma_j)x_j$ for all $j \in \{2,\dots,d\}$.

Take the Bland path for $d-1$, and append to every basis in it the constraint $0.1 + \alpha_j + (0.2+\beta_j)x_{j-1} \leq (1+\gamma_j)x_j$.
These bases are now bases for the KM-cube in $d$ variables. The associated basic solutions are identical on coordinates $1,\dots,d-1$ and have $x_d = \frac{0.1 + \alpha_j + (0.2+\beta_j)x_{d-1}}{1+\gamma_j}$.
The Bland path for $d-1$ contains $2^{d-1}$ bases and increases the value of $x_{d-1}$ in every step.
Since $1+\gamma_d \in [0.95,1.05]$ and $0.1 + \alpha_d \in [0.05,0.15]$ and $0.2 + \beta_d \in [0.15,0.25]$ and $x_{d-1} \in [0,1]$
we get $x_d \in [0, 0.43]$.

When we consider the constraint $(1+\delta_d)x_d \leq (0.9+\eps_d)-(0.2+\eta_d)x_{d-1}$, we see that the left-hand side is at most $1.05 \cdot 0.43 \leq 0.46$ and the right-hand side is at least $0.85-0.25 = 0.6$.
That is all we needed to check in order to verify that the bases constructed so far are feasible.

Every step on our path so far is improving. Due to our chosen ordering of the inequalities, the constraint $0.1 + \alpha_j + (0.2+\beta_j)x_{j-1} \leq (1+\gamma_j)x_j$ will not leave the basis until there is no other possible improving direction left. Hence our path so far is all part of the Bland path for our KM-cube in $d$ variables.

At the end of our path so far, Bland's rule will pivot out the constraint
$0.1 + \alpha_j + (0.2+\beta_j)x_{j-1} \leq (1+\gamma_j)x_j$
and pivot in
$(1+\delta_d)x_d \leq (0.9+\eps_d)-(0.2+\eta_d)x_{d-1}$,
since it is the only available improving move.

Now take again the Bland path for $d-1$, but reverse its order and append to every basis in it the constraint
$(1+\delta_d)x_d \leq (0.9+\eps_d)-(0.2+\eta_d)x_{d-1}$.
This yields $2^{d-1}$ more bases for the $d$-variable KM-cube.
Because we reversed the order, the Bland path is decreasing $x_{d-1}$.
Since $0.2+\eta_d > 0$ and $1+\delta_d > 0$, the path is thus increasing the value of $x_d$.
We have now constructed our full Bland path of $2^d$ bases.
It remains to show that these final $2^{d-1}$ bases are feasible and in $[0,1]^d$.
Note that $1+\delta_d \in [0.95,1.05]$, $0.9+\eps_d \in [0.85,0.95]$ and $0.2+\eta_d \in [0.15,0.25]$.
Since $x_{d-1} \in [0,1]$ we get $x_d = \frac{0.9+\eps_d - (0.2+\eta_d)x_{d-1}}{1+\delta_d} \in [0.57, 1]$.
The only constraint that can lead to infeasiblity is
$0.1 + \alpha_j + (0.2+\beta_j)x_{j-1} \leq (1+\gamma_j)x_j$
but its left-hand side is at most $0.4$ and its right-hand side is at least $0.57$, hence the bases are all feasible. *QED*

## Interpretation

This theorem directly shows us that Bland's rule has exponential complexity under both relative and zero-preserving smoothed analysis.
When we rescale the inequalities by large numbers in the standard way, it follows directly that Dantzig's most-negative reduced cost rule has exponential complexity for relative smoothed analysis.

In my [previous post](/blog/2026/beyond-worst-case/) I interpreted the above theorem by stating that zero-preserving smoothed analysis is a dead end.
While I stand by that statement, I do want to give a second possible interpretation.

Namely, you can conclude that Bland's rule is just bad.[^agree]
More specifically: Bland's rule is purely combinatorial in nature.
Perturbations are (stochastic) geometry.
It is no wonder that Bland's pivoting decisions fail to benefit from perturbations.
Dantzig's rule [is also bad](https://www.youtube.com/watch?v=tbOZvbpZp44) of course.
In this case, specifically because it fails to be scale-invariant with respect to rescaling the inequalities.
Dantzig thus similarly fails to be a geometrically-steered pivot rule.

When your pivot rule does incorporate the ambient geometry, you can do significantly better than zero-preserving smoothed analysis.
In fact, in the [by-the-book paper](https://arxiv.org/abs/2510.21613) we bound the running time of a simplex method in a much weaker probabilistic model: only the right-hand side is perturbed.[^btba]


[^agree]: Every practitioner will agree.

[^btba]: We use the semi-random shadow vertex pivot rule, which generally prefers steeper edges over shallower edges. Hence it incorporates the ambient geometry. The analysis also needs some additional minor assumptions to make the definitions make sense: RHS perturbations alone lack scale.
