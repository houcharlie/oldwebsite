---
layout: page
title: Projects
permalink: /projects/
---

Here are some of the projects/research I've been involved in.

### Interpolated peeling: new learning algorithms for vertex order recovery

This project is advised by [Professor Miklos Racz](http://mracz.princeton.edu/).  We
want to give a more precise characterization of the algorithm proposed [here](https://pdfs.semanticscholar.org/043a/4b15b8f563002e1d1e3ee8dea5eed9aa26ca.pdf) for vertex order recovery in random graphs.  A report containing
the progress we made in spring '18 is [here]({{site.url}}/pdfs/report.pdf), the code I used for the simulations is [here](https://github.com/houcharlie/peelingAnalysis) and a blog post giving a high level description is [here]({{site.url}}/blog/peeling.pdf).  The project is still ongoing, and we are excited about making more findings!


### "Acceleration" via depth in neural networks

Two of my friends, [Hrishikesh Khandeparkar](http://www.cs.princeton.edu/~hrk/index.html) and [Gene Li](https://gxli97.github.io/), and I worked on a project studying the effect of additional depth on deep learning as a follow-up to this [paper](https://arxiv.org/pdf/1802.06509.pdf).  The final report is [here]({{site.url}}/pdfs/depth-final-report.pdf).  We found that increasing depth in neural networks made the regularization loss decrease, but did not impact the "real loss".  We hypothesize this is because the (L2) regularization makes the loss surface behave more like an Lp surface.


### WallStreetBets

During HackPrinceton Fall '17, a few of my friends were working on implementing a trading idea that revolved around posts on a SubReddit called /r/WallStreetBets.  In the middle of their project, they asked me to whip up/implement a suitable ML algorithm for their strategy.  The results are in this [repository](https://github.com/houcharlie/WallStreetBets).  Surprisingly enough, it didn't do too badly in the time that we tested the strategy (the morning of presentations), managing to predict price direction better than 50% of the time.

