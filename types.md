# Types of Molecular Clock Models 
The primary objective of molecular clock analysis is to look at how over time, genetic sequences accumulate substitutions according to some statistical process. From these substitutions or sequences changes, we can place a clock model with a statistical model that best fits the sequence's evolution. From this, we can estimate things like evolutionary rates and divergence times. 
In our implementation of the molecular clock model onto the phylogenic tree that we choose, we have to select the type of molecular clock model that best fits our data. We make this decision by identifying the model that provides the best balance between model complexity and goodness of fit, especially regarding the biological features of the data. [Ho, Duchene 2014](https://onlinelibrary.wiley.com/doi/10.1111/mec.12953)

## Strict Clocks

A strict molecular clock, also called a global clock, assumes that the phylogenetic tree maintains the same rate of substutions per unit time throughout all branches. These clocks are computationally simpler, only needing 1 or a few parameters, and best used when data is limited or we know the rate is close to constant. With this clock, once a single global rate has been estimated, you can convert substitutions per site directly into absolute times once you provide a calibration point (ex. fossil age or known divergence time). 

However, the usage of strict clocks with many datasets often results in an overdispersion of rates as we see variance in rates that ends up being larger than what we expect with a strict clock, ending up in biased time estimates in the final phylogeny.  Thus, strict clocks are more oftne used as a baseline model which is compared to more flexible models (which we will see now) using comparison tools like likelihood ratio tests, Bayes factors, or other model comparison metrics. 
 [Robinson 2006](https://pmc.ncbi.nlm.nih.gov/articles/PMC1395352/)

[<img width="600" height="400" alt="image" src="https://github.com/user-attachments/assets/fb0627de-b1ce-44fa-9fe8-d0e6daaef5ff" />](https://beast.community/images/clocks/strictClock.png)


## Relaxed Clocks

As opposed to strict clocks, relaxed clocks were created with the goal to deal explicitly with rate heterogeneity among lineages. This is especially useful now with our current undertanding of biological variation showing a wide array of sources for variation that fall into the categories of gene effects, lineage effects, and a mix known as gene by lineage effects. The classic relaxed-clock model (one of two often used models but most relevant for this paper), was created by Alexei Drummond and their team in 2006 and allows for each branch to have its own rate and evolving the single global constant model into a diverse colection of random variables on each branch. 
[Drummond et al. 2006](https://journals.plos.org/plosbiology/article?id=10.1371%2Fjournal.pbio.0040088)

<img src="https://beast.community/images/clocks/uncorrelatedClock.png" width="600" height="400">

The more simpler and prevalent version of relaxed clocks are called uncorrelated relaxed clocks. In these relaxed clocks, we independently draw each branch rate from a shared probability distribution (lognormal, gamma, exponential are the models supported in BEAST). In BEAST, each branch is assigned a unique evolutionary rate from a fixed number of discrete rate which are created from a categorization of the distribution's rates. [BEASTdoc 2017](https://beast.community/clocks.html)

## (Fixed) Local Clocks 

Local clocks serve as a good middle ground between strict and relaxed clocks. These clocks assume that different groups of branches share different but internally constant rates, and they allow the model to avoid over or underfitting of rate variation to the phylogeny. This is useful in cases when specific groups or clades of the tree are suspected to evolve faster or slower than the rest of the tree. In these cases, having a fully relaxed clock may overparameterize the model, but a fully strict clock will not properly account for the variation in these clades. [BEASTdoc 2017](https://beast.community/clocks.html) 


<img src="https://beast.community/images/news/EBOV_Reference_Set_15_LC1.MCC.tree.png" width="1000" height="1000" >

Here, we can see that the different colored branch groups indicate different rates of mutation (in the context of this study, blue indicated a lower mutation rate). [BEAST Community, 2019](https://beast.community/ebov_local_clocks.html) 



