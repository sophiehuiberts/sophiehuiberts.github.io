---
title: "A Friendly Smoothed Analysis of the Simplex Method"
collection: publications
permalink: /publication/2018-6-20-A-Friendly-Smoothed-Analysis-of-the-Simplex-Method
excerpt: >-
  Updated June 2019 to include a new algorithm,
  improving running time when σ is big.
#  Explaining the excellent practical performance of the
#  simplex method for linear programming has been a major
#  topic of research for over 50 years. One of the most
#  successful frameworks for understanding the simplex
#  method was given by Spielman and Teng (JACM ‘04), who
#  developed the notion of smoothed analysis. Starting
#  from an arbitrary linear program with d variables and
#  n constraints, Spielman and Teng analyzed the expected
#  runtime over random perturbations of the LP (smoothed LP),
#  where variance σ<sup>2</sup> Gaussian noise is added to
#  the LP data and proved a running time bound
#  polynomial in n, d and σ<sup>−1</sup>.   
#
#
#  Their result has since been substantially improved by Deshpande
#  and Spielman (FOCS ‘05) and later Vershynin (SICOMP ‘09). The
#  fastest current algorithm, due to Vershynin, solves the
#  smoothed LP using an expected O(d<sup>3</sup> log<sup>3</sup>
#  n σ<sup>−4</sup> + d<sup>9</sup> log<sup>7</sup> n) number of
#  pivots.   
#
#
#  In this work, we prove a better complexity bound using
#  a much simpler analysis. Our main algorithm requires
#  an expected O(d<sup>2</sup> √logn σ<sup>−2</sup> + d<sup>3</sup>
#  log<sup>3/2</sup> n) number of simplex pivots. We obtain our
#  results via an improved shadow bound, key to earlier analyses
#  as well, combined with an improvement on algorithmic
#  techniques of Vershynin. As an added bonus, our analysis is
#  completely modular, allowing us to obtain non-trivial bounds
#  for perturbations beyond Gaussians, such as Laplace perturbations.    
#
#
#  Last updated June 2019.
date: 2018-6-20
venue: '50th Annual ACM SIGACT Symposium on Theory of Computing'
paperurl: 'https://arxiv.org/abs/1711.05667'
coauthors: 'Daniel Dadush'
slidesurl: 'http://sophie.huiberts.me/files/slides-smoothed-simplex.pdf'
---
