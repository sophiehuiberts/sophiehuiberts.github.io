+++
title = "AI-assisted Proofs Come with Additional Responsibilities"
hasmath = true
hascode = false

date = Date(2026, 11, 23)
tags = ["blog", "theory", "ai"]

rss_title = title
rss_description = "."
rss_pubdate = date
+++

Recently, Marcin Polkowski announced `popso`: a vibe coded MILP solver.
He advertised it as 'written from scratch' and 'already solving more problems than HiGHS and SCIP'.
On the process, all he writes is this:
> Does anyone know how long it takes to write a competitive MILP solver? [...] The textbook answer is "a team, about a decade". My answer, as of this week, is "hold my Coke Zero".
Despite being reductive, I do like this question.

`popso` is not a well-executed project.
For one, it is clearly tuned on the test set.
Performance variability[^variability] in MIP is high, and so tuning on your test set is problematic.
Much of that variability is within families.
That is, when you write an instance generator and you tune on one set of samples, then even if you draw fresh samples from those generators you are still overtuned.
This invalidates all Polkowski's benchmark results.

Benjamin Müller of COPT additionally [pointed out](https://lnkd.in/p/eGEpckaU) various parts of the code come from HiGHS and SCIP, and that 'one could be tempted to say that the AI just created a combination of the two solvers'.
Indeed, Claude's comments in the code reveal that the other solvers were used as a direct reference.

So far, this is a story like so many others.
An LLM creates a large piece of bad work.
The machine operator fails the basic responsibilities that we have towards each other.
Not meaningfully different from what OpenAI and Anthropic have been doing.

But not all LLM-involved projects hit the same failures.
I have seen plenty of non-examples.
Colleagues who prompted to solve mathematical problems within their expertise and wrote up the result as a well-readable paper with adequate exposition and citations.
These are pieces of work that, had they been produced last year, I would have called 'nice' or even 'good'.
Nevertheless, I do think many of these papers fail their responsibilities.
Specifically: they fail their specific responsibilities as LLM-involved projects.

We must not forget: these papers required a different (and less) work from their authors.
A significant chunk of the work was performed by the machine, with the authors taking up roles as machine operators.
This is a methodological novelty.

In other fields of science, papers report their methodology.
Mathematicians have not traditionally needed to do so, because every paper's methods were the same.
You read the literature, walk through a forest, discuss at the whiteboard, and take a long shower.
At some point you have a strategy, and then you spend however long working it out and writing it up.

Now we have more options in terms of different methods.
That means we need more substantive methodological reporting.
The 'AI disclosures' that people are writing are far too meagre.
It is not enough to say "GPT-6 pro was used to prove Lemma 1, 2, and 3".
That is distributing credit, not a description of methods.[^STOC]
The methods section needs to be a full section of the paper, able to cover a page with little issue.

When you study an object, you always study the method as well.
You cannot learn about the planets without learning about your telescope, and you cannot study medicines without learning about mice.
Similarly, I don't think we can learn about math from LLMs without learning about the LLMs.
That means the specific usage of the LLMs must be reported in papers.

Here are just a few examples of things that I would like to see reported:
1. We tested LLMs A, B, and C. Each was prompted n times for this problem, and respectively x%, y% and z% of sessions outputted the key ideas that were needed for this theorem.
2. We used a \$N/month subscription from vendor D over the course of M days. The session used K tokens and, if one were to pay API prices for that, would have cost \$L.
3. We prompted the machine with the problem like this, after a while we suggested such-and-so strategy and told it to look at these papers for ideas.
4. When we asked the robot post-hoc to identify where the ideas came from, it said UVW99. This answer was true/false.
5. The machine gave extraneous details O, P, Q, and left out important steps R, S, T.
6. All raw chat transcripts.
What unifies these example questions is that they can only be answered by the original machine operators.
Some are about how the operator used the machine, others have been could be prompted by anyone but became impossible once the paper was publically available and assimilated into the borg.

Reporting on your machine use is very common in other computational fields.
I often hear people say that writing computational MIP papers consists of much more effort writing than effort in computation.
That is because they (or their reviewers..) know that methodological transparency is valuable. It is just as important a piece of the science as the actual results.
The change I ask for has long been normal in other fields.

Let's wrap back to Polkowski.
Not only is it not good, he is missing out on answering all the methodological questions that came across him.
Most visibly, 'how long does it take to write a competitive MILP solver?'.
He is aware of the question, asks it himself, but refuses to attempt a serious answer.
That makes his effort a waste of everyone's attention.

I want to highlight one positive example.
After 3SUM, [Josh Alman](https://youtu.be/hN9-BwaZCfw?t=3935) and [Virginia Vassilevska Williams](https://people.csail.mit.edu/virgi/thoughts.txt) each provided an examplary amount of information.
This is key scientific information, and I would have loved to see it all included in the paper.
Skipping that is a missed opportunity, and I believe that going forward we should start demanding papers to include a substantial section of this exact nature.

[^variability]: Performance variability is the phenomenon where small changes in the code can cause a lot of variation in running time. A change which improves your geometric mean time by 1% may increase the running time on some instances by 2-3x and decrease others by 0.5x. These changes can be algorithmic, but can also come from changing the random seed or permuting the rows and columns of a model.

[^STOC]: The [STOC 2027 CFP](https://acm-stoc.org/stoc2027/stoc2027-cfp.html) is one example of a document that is confused about this difference. The CFP mandates 'disclose of substantive use' but the template they provide reads 'AI methodology: We used [Tool Name] to assist with [Brief Description of Use]. The tool materially affected [Sections X and Y]. More details can be found in [Section Z]. The authors verified the correctness and originality of all content, including references.' This is merely a disclosure of usage but it is titled as methodology.
