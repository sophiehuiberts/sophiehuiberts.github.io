+++
title = "Principles for Teaching Linear Programming"
hasmath = false
hascode = false

date = Date(2026, 9, 28)
tags = ["blog"]

rss_title = title
rss_description = "Philosophy about what to teach when teaching LP"
rss_pubdate = date
+++

This semester I am teaching a class together with Alantha Newman at ENS Lyon. My half of the course is on the simplex method and linear programming.
In this blog post, I will outline the principles of how _I_ think linear programming should be taught.

## No Tableau
It is my firm belief that nobody has ever learned anything from a simplex tableau.
Personally, despite being a well-recognized researcher in LP, I do not understand them.
I could not pivot a step if you gave me a thousand dollars.
As such, I would never ask a student to learn how to do this either.

## Inequality Form
The only linear program worth writing down is of the form
\begin{align}
\operatorname{maximize} \quad &c^T x \\
    \operatorname{subject~to} \quad & A x \leq b.
\end{align}
In particular: standard form ($Ax = b, x \geq 0$) and canonical form ($Ax \leq b, x \geq 0$) are banned.
When you permit variable bounds as part of your notation, you commit a type error.
An index should be _either_ a variable index _or_ a constraint index. Variable bounds like $x\geq 0$ demand you use a single index both to indicate a variable $x_i$ and a constraint $x_i \geq 0$.
This is a terrible situation that must be avoided at all costs.

### Pros and Cons
Sticking to inequality form has many other benefits including:
- you can draw pictures of your LPs, where a 3D picture is for an LP with 3 variables. In equality form this is is impossible
- Newtonian mechanics' provides a clean intuition for deriving duality. The law $F=ma$ provides a clean analog for Farkas' Lemma, and in turn complementary slackness becomes the most intuitive fact in the world.
- The simplex method is much easier to represent in inequalty than in equality form. In particular the expression $b-Ax$ gets to be reserved for slacks (which makes sense) instead of being forced into the role of reduced cost (which does not make sense).

The only meaningful downside is if you want to cover interior-point methods.
Something like a log-barrier IPM is much simpler in equality form.

## No Theoretical Nonsense

As Harris so eloquently puts it: "Linear programming is a practical technique and not a mathematical
exercise.'
Therefore, we shall not waste any class time on nonsense that only exists in theory.
- Degenerate pivot steps do not exist.
- We only mention Bland's pivot rule in order to make fun of it.
- We only mention Dantzig's pivot rule in order to have a healthy discussion about scale-invariance.

The matter of degeneracy is a complicated one.
Everything from its definitions, its causes, implications and remedies are vastly different in theory and in practice.
I am not aware of any comprehensive academic publications describing degeneracy in practice,
and I do not respect the theoretical approaches to it.
Thus, ignoring its existence is the only way forward.
This way we have time to talk about the more important things in life.

## Lecture Notes
I post [my lecture notes](/files/lyon2026.pdf) online, and typically are only a week or two behind class schedule haha.
If you have any comments, do give me a shout.
I love to hear from you.
