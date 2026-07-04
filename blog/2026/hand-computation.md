+++
title = "Solving LP by Hand"
hasmath = false
hascode = false

date = Date(2026, 7, 04)
tags = ["blog", "history"]

rss_title = title
rss_description = "Historical anecdotes."
rss_pubdate = date
+++

# Solving LP by Hand

As narrated by Dantzig in many retrospective articles, he invented linear programming in the summer of 1947 with helpful suggestions of Leonid Hurwicz and Tjalling Koopmans.
The first computational experiment with the simplex method finished April 10th,[^NBS] 1948.
This was the famous diet problem computation, described from Dantzig's point of view in *Linear Programming and Extensions* and described from the computer's point of view in *When Computers Were Human* (David Alan Grier).
Notably, this computation was performed by hand: 5 people worked on this for 21 days.[^letters]

![Black and white photograph. Twelve humans sit, quite packed, in rows at small desks. Each person has a large calculator in front of them. In terms of size and button shape they look like Comptometers, but they also seem to have rounded corners and cables leading from the back. Ten of the humans I would describe as white women, one I think is a white man, and the last one is a black man. The desks have enough space for the calculator, plus maybe two pieces of paper side by side without overlap.](/blog/2026/computers-mathtables.jpg)

*Human computers of the Mathematical Tables Project, where the 1948 diet LP computation was performed.*

Even after this demonstration, a number of real applications of LP were performed by hand.
Lets look a bit of a timeline for electronic computers first.
The first specialized simplex method for transportation problems[^transportation] was coded in 1950 for the SEAC.
In 1952, Orchard-Hays got the first simplex code working on the IBM Card Programmed Calculator, and later on the more recognizeable IBM 701 machines.
These early hardwares and softwares were very error-prone and needed a lot of highly-skilled babysitting, but Orchard-Hays' later codes were commercially useful.

Charnes, Cooper and Mellon famously presented the first work on LP in fossil fuel blending in 1951. They do not directly specify, but we can be fairly sure that their computation was done by hand. Although the LP had no workable structure that aided computation, it was fairly small problem: maybe 9 inputs, 6 outputs.

Dantzig writes in 1954[^largescale] that often, when studying a particular structure of LP, human hands with a specialized algorithm were faster than an electronic computer with a general LP code.
For example, a transportation problem[^transportation] with a hundred combined rows (sources) and columns (sinks) can be 'handled nicely by clerks'.
Dantzig writes that already in 1953, Heinz[^advances] used LP to ship ketchup from 6 plants to 70 warehouses.
We can be fairly sure that a small transportation problem like this was solved by hand.
And this is presumably without the 1956 Ford-Fulkerson algorithm.

One other application of a transportation problem solved by hand is described in 1955[^advances].
The RAND Corperation employed a large number of computers,[^facilities] as well as many researchers who had computations they wanted to see performed.
In order to form a schedule that takes both priority and urgency into account, a transportation problem was formulated. Every week has a number of computer-hours available, every tasks needs a certain hours before it is completed.
Dantzig gives an example where one has 18 projects and 10 weeks, resulting in a problem with 28 equations in 180 unknowns. He adds upper bounds of 40 hours on the individual edges to indicate that only one *person* can work on a project at a time.

This is perhaps the ultimate showcase that human computers remained important well into the mid-1950s: humans were solving an LPs, by hand, to decide in what order the different computing tasks should be solved (also by humans).

[^NBS]: Date is from the [NBS report](https://nvlpubs.nist.gov/nistpubs/Legacy/RPT/nbsreportApr-Jun1948.pdf). Note the involvement of famous matrix theorist Olga Taussky-Todd. I am not aware of any other document crediting her involvement with LP or computation. I asked David Alan Grier about this, he speculated that perhaps Taussky-Todd did not want to be credited: computation was seen as low-skilled (women's) work, and Taussky-Todd may not have felt comfortable being associated with that.

[^letters]: [Correspondence from Dantzig to Von Neumann](https://sophie.huiberts.me/files/dantzig-von-neumann-1948.pdf). The 5 computers were supervised for 4 hours per day by Jack Laderman. Note that only Laderman is credited by name, not the computers and not Taussky-Todd.

[^transportation]: Given a bipartite graph, with supplies of a commodity available on the one side and demands on the other side, how to route the goods at minimal cost?

[^advances]: [Recent Advances in Linear Programming](https://www.rand.org/pubs/research_memoranda/RM1475.html), Dantzig, April 12th, 1955.

[^facilities]: Dantzig writes "The RAND Corporation has extensive computing facilities that are in constant use by the research personnel." Despite how he phrases it, he is talking about human being here.

[^largescale]: [Status of Solution of Large-Scale Linear Programming Problems](https://apps.dtic.mil/sti/tr/pdf/ADA596199.pdf), Dantzig, November 30th, 1954.
