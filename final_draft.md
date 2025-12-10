# Chapter:  Molecular Evolutionary Clock

## Overview

This chapter introduces the concept of the molecular evolutionary clock, a foundational tool in evolutionary biology for estimating when species diverged. The chapter explains why the molecular clock is used, how it works, the types of clocks, the data pipeline, and the limitations of the molecular clock method. 

By the end of the chapter, you should understand both the theory and the practical workflow underlying molecular clock analysis.

Created by: Luigi Bartulo, Mitra Sutar, Zihan (Isabella) Xie 

The markdown file (and a better view of the paper!) is viewable at https://github.com/kuigi/BENG-183-Final-Project/blob/main/final_draft.md . There is an image in 4.3 that appears skewed in the PDF, and an image in 5.5 that simply does not show up in the PDF, so please consider using this view when looking at the paper! 

---

## Contents

1\. What Is a Molecular Evolutionary Clock?

> 1.1. Definition

> 1.2. Concept of using mutation rates to estimate divergence time

2\. Why Do We Use Molecular Clocks?

> 2.1. Comparing evolutionary rates across species

> 2.2. Using genetic distance to infer timing

3\. How Molecular Clocks Work

> 3.1. Measuring genetic differences

> 3.2. Mutation rate assumption

> 3.3. Time estimation formula

> 3.4. Phylogenetic inference tools

4\. Types of Molecular Clocks

> 4.1 Strict Clock

> 4.2 Relaxed Clock

> 4.3 Local Clock 

5\. Data Analysis Pipeline

> 5.1 Obtaining Sequence Data

> 5.2 Multiple Sequence Alignment

> 5.3 Inference of Phylogenetic Tree

> 5.4 Selection and Placement of Molecular Clock Model

> 5.5 Final Time-Scaled Ultrametric Phylogenetic Tree 

6\. Historical Significance

> 6.1 Neutral Evolutionary Theory

> 6.2 COVID-19 Sequencing 

7\. Limitations of the Molecular Evolutionary Clock

> 7.1 Inconsistent Mutation Rates

> 7.2 Difficult Calibration 

> 7.3 Effect of Selection


## 1\. What Is a Molecular Evolutionary Clock?

1.1  Definition

A **molecular evolutionary clock** uses DNA or protein mutation rates to estimate the time when two species or populations diverged from a common ancestor.

1.2  Using mutation rates to estimate divergence time

Mutations occur spontaneously during DNA replication and accumulate gradually across generations.

If the mutation rate for a particular gene or genome region is known or can be estimated, researchers can calculate how long two lineages have been evolving separately.

## 2\. Why Do We Use a Molecular Clock?

2.1  Comparing evolutionary rates across species

One major reason to use molecular clocks is to compare whether different species or lineages evolve at similar or different mutation rates. By examining genetic differences among species that share a common ancestor, researchers can determine:

* whether evolutionary change has occurred uniformly,

* whether some groups evolve more quickly,

* or whether certain genes change at predictable speeds.

Such comparisons help biologists understand the dynamics of evolution and the forces shaping genomic change.

2.2 Inferring Divergence Timing from Genetic Distance

Molecular clocks are widely used because genetic distance becomes a proxy for time. When two species share a distant ancestor, more substitutions accumulate than between species that diverged recently. 

This creates a measurable pattern:

**more differences → more time has passed**

This approach is especially valuable when fossil records are sparse or ambiguous. While fossils provide direct physical snapshots, molecular clocks offer continuous, quantifiable information about lineage history.

## 3\. How Does the Molecular Clock Work?

**Basic formula to estimate divergence time:**

TimeNumber of substitutionsMutation rate

| Steps to Estimate Divergence Times | Description |
| :---- | :---- |
| 3.1  Measure genetic differences | Count substitutions in DNA/protein sequences between species |
| 3.2  Assume constant mutation rate | mutation rates tend to be predictable enough over evolutionary timescales |
| 3.3 Converting Substitutions to Time | TimeNumber of substitutionsMutation rate |
| 3.4  Phylogenetic inference tools | Examples: BEAST, MrBayes, MEGA  |

Phylogenetic Tool Examples

- BEAST: Bayesian Evolutionary Analysis by Sampling Trees

* Inputs: DNA sequences, substitution model, molecular clock model

* Outputs: Time-scaled phylogenetic tree

* Purpose: Converts genetic distances into precise divergence times.

- MrBayes

A Bayesian phylogenetics tool that can incorporate clock assumptions.

- MEGA

  A widely used platform for molecular evolution analysis, suitable for simpler clock models.


## 4. Types of Molecular Clock Models 
The primary objective of molecular clock analysis is to look at how over time, genetic sequences accumulate substitutions according to some statistical process. From these substitutions or sequences changes, we can place a clock model with a statistical model that best fits the sequence's evolution. From this, we can estimate things like evolutionary rates and divergence times. 
In our implementation of the molecular clock model onto the phylogenic tree that we choose, we have to select the type of molecular clock model that best fits our data. We make this decision by identifying the model that provides the best balance between model complexity and goodness of fit, especially regarding the biological features of the data. [Ho, Duchene 2014](https://onlinelibrary.wiley.com/doi/10.1111/mec.12953)

4.1 Strict Clocks

A strict molecular clock, also called a global clock, assumes that the phylogenetic tree maintains the same rate of substutions per unit time throughout all branches. These clocks are computationally simpler, only needing 1 or a few parameters, and best used when data is limited or we know the rate is close to constant. With this clock, once a single global rate has been estimated, you can convert substitutions per site directly into absolute times once you provide a calibration point (ex. fossil age or known divergence time). 

However, the usage of strict clocks with many datasets often results in an overdispersion of rates as we see variance in rates that ends up being larger than what we expect with a strict clock, ending up in biased time estimates in the final phylogeny.  Thus, strict clocks are more oftne used as a baseline model which is compared to more flexible models (which we will see now) using comparison tools like likelihood ratio tests, Bayes factors, or other model comparison metrics. 
 [Robinson 2006](https://pmc.ncbi.nlm.nih.gov/articles/PMC1395352/)

[<img width="600" height="400" alt="image" src="https://github.com/user-attachments/assets/fb0627de-b1ce-44fa-9fe8-d0e6daaef5ff" />](https://beast.community/images/clocks/strictClock.png)


4.2 Relaxed Clocks

As opposed to strict clocks, relaxed clocks were created with the goal to deal explicitly with rate heterogeneity among lineages. This is especially useful now with our current undertanding of biological variation showing a wide array of sources for variation that fall into the categories of gene effects, lineage effects, and a mix known as gene by lineage effects. The classic relaxed-clock model (one of two often used models but most relevant for this paper), was created by Alexei Drummond and their team in 2006 and allows for each branch to have its own rate and evolving the single global constant model into a diverse colection of random variables on each branch. 
[Drummond et al. 2006](https://journals.plos.org/plosbiology/article?id=10.1371%2Fjournal.pbio.0040088)

<img src="https://beast.community/images/clocks/uncorrelatedClock.png" width="600" height="400">

The more simpler and prevalent version of relaxed clocks are called uncorrelated relaxed clocks. In these relaxed clocks, we independently draw each branch rate from a shared probability distribution (lognormal, gamma, exponential are the models supported in BEAST). In BEAST, each branch is assigned a unique evolutionary rate from a fixed number of discrete rate which are created from a categorization of the distribution's rates. [BEASTdoc 2017](https://beast.community/clocks.html)

4.3 (Fixed) Local Clocks 

Local clocks serve as a good middle ground between strict and relaxed clocks. These clocks assume that different groups of branches share different but internally constant rates, and they allow the model to avoid over or underfitting of rate variation to the phylogeny. This is useful in cases when specific groups or clades of the tree are suspected to evolve faster or slower than the rest of the tree. In these cases, having a fully relaxed clock may overparameterize the model, but a fully strict clock will not properly account for the variation in these clades. [BEASTdoc 2017](https://beast.community/clocks.html) 


<img src="https://beast.community/images/news/EBOV_Reference_Set_15_LC1.MCC.tree.png" width="600" height="600" >

Here, we can see that the different colored branch groups indicate different rates of mutation (in the context of this study, blue indicated a lower mutation rate). [BEAST Community, 2019](https://beast.community/ebov_local_clocks.html) 



## 5\. Data and Analysis Pipeline for Molecular Clocks

A molecular clock analysis generally follows the same pipeline that starts with raw sequence data and finalizes with a time-scaled phylogenetic tree. 


5.1 Obtaining Sequences and Molecular Data

Of course, to get started we first have to obtain the homologous DNA/RNA/amino acid sequences sampled from different species, populations, or time points that we will be analyzing for sequence change and create a phylogenetic tree. Molecular clock analysis can use whole genomes, single genes, and concatenated loci, so whatever part of the genome we need to utilize we can use. We can obtain through our public DNA/RNA databases such as GenBank, ENA, and GISAID. We can also use the sequences we obtain from sequencing in our own labs, provided we ensure that they pass our respective quality control filtering. 

<img src="https://s3.amazonaws.com/libapps/accounts/5290/images/NC_record_CDS_2.jpg" width="600" height="400">

We also want to make sure that we have the relevant **metadata** that we will use later in our phylogenetic tree. This is composed mainly of elements that provide our sequences with more context, providing our sequences with information about the species it was obtained from, sampling location, and (arguably most importantly) sampling *date*. By utilizing the sampling date, BEAST can convert sequence changes to dates, and dates to years on our phylogenetic tree. 
![image](tipdates1.png)

*Here in BEAST, import of tip dates (sampling dates for each sample) is crucial for the creation of the phylogeny in the next steps.* 

[Berkeley 2024](https://guides.lib.berkeley.edu/ncbi/sequences) 

Lastly, we want to ensure that the sequences that we use contain homologous regions across all samples. This way, in the next step (Multiple Sequence Alignment), the alignment of sequences will be successful and meaningful, and the creation of the phylogenetic tree later on will be accurate. 


5.2 Multiple Sequence Alignment

A crucial step in many data pipelines in bioinformatics tools, multiple sequence alignment (MSA) arranges sequences so that columns correspond to homologous sites and gaps represent insertions or deletions. In the molecular clock analysis pipeline, MSA is crucial as the phylogeny is derived from looking at substitution counts when looking at column-wise comparisons. Bad alignment can construe false substitutions or hide real substitutions, resulting in a biased creation of the phylogeny and clock fitting. 

<img src="https://biohub.org/rapid-response/wp-content/uploads/sites/5/2024/11/MSA_before-n-after_example.png" width="600" height="400">

Of course, there are a lot of common MSA tools that have been developed, and some have been specifically useful in alignment for the purpose of the creation of a phylogeny. These include tools like MAFFT, a widely used, fast, and accurate tool for nucleotide and protein sequences, and MUSCLE/MUSCLE5, a tool with high-accuracy alignments and tools to diagnose alignment uncertainty.


[Geneious Academy](https://www.geneious.com/guides/understanding-sequence-alignment)

5.3 Inference of Phylogenetic Tree

<img src="https://biohub.org/rapid-response/wp-content/uploads/sites/5/2024/10/MSA-Tree_Schematic.png" width="600" height="400"> 

[Biohub](https://biohub.org/rapid-response/genomic-epidemiology/sequence-alignments-molecular-clocks/)


After MSA, we can now infer a phylogenetic tree under a substitution model which we will use to describe how nucleotides or amino acids change over time. Major computational frameworks for this step include: 

Maximum likelihood: finds tree and model parameters that maximize the likelihood of the observed alignment, used in tools like IQ-TREE, RAxML, PhyML

[Minh et. al 2020](https://pmc.ncbi.nlm.nih.gov/articles/PMC7182206) 

Bayesian inference: samples trees and parameters from the posterior distribution using Markov chain Monte Carlo (MCMC), tools include our primary program BEAST, MrBayes, RevBayes 

[Ho, Duchene 2014](https://onlinelibrary.wiley.com/doi/10.1111/mec.12953)

The output of this inference is a phylogenetic tree which has branch lengths corresponding to subsitutions per site, and in the next step we will place the clock model on this tree to convert substituions per site into units of time. 


5.4 Selection and Placement of Molecular Clock Model

Once the substitution tree is created, a clock model is place onto the tree to convert substitutions per site into time units. However, to do this, we must perform calibration using a time constraint in order to position the times/years we obtain into actual real time, or else all we have are relative ages. 

![image](m_rsbl20150194f01.jpeg)

[Ho et. al 2015](https://royalsocietypublishing.org/rsbl/article/11/9/20150194/34621/Biogeographic-calibrations-for-the-molecular) 




Common calibration strategies include: fossil calibrations, which construct node ages from fossil ages using min/max bounds or probablistic priors (lognormal, uniform, etc.); biogeographic/historical calibrations, which construct timing constraints from known geographic/historical events (continental drift, island colonizations); tip dating and heterochronous sampling, which uses sampling dates of sequences that act as calibrations directly on the tips, allowing simultaneous estimation of rates and times. 




Once calibrated, we use frameworks like BEAST to place our choice of clock onto the phylogeny to produce date estimates as well as check model fit using analysis from our statistical inferences (Bayesian inferences, posterior/prior predictive checks) of choice in order to verify the model fit is ideal. 

[Drummond et. al 2006](https://journals.plos.org/plosbiology/article?id=10.1371%2Fjournal.pbio.0040088) 


5.5 Final Output: Time-Scaled, Ultrametic Phylogenic Tree

Our final product is usually an ultrametric, time-scaled phylogenetic tree. All tips align either at present time or at their respective uniform sampling time. The x-axis should be in units of absolute time (years or some magnitude of years).

![image](https://github.com/kuigi/beng-183-final-project/blob/main/rescaleTree-1.png) 

*This visual provides a simple yet compelling visual of how we go from substitutions per site to the final strcuture of the phylogenic tree using our clock model and calibrations.* 

[Yu 2022](https://yulab-smu.top/treedata-book/chapter4.html) 

From these trees, we can read off many important pieces of data and information (some examples we will cover in depth in the next section). Common use cases include: 

*Time to most recent common ancestor (tMRCA)*: Analyzing where particular clades or branches originate, such as detecting the origin of the lineage of a virus, or finding out where humans divereged from chimpanzees. 

*Speed and method of diversification*: Similar to tMRCA, determined by comparing branching patterns and branch ages across clades 

*Comparison of divergence times to historical reccords*: Utilizing the model's estimated divergence times to previous understandings and records of events (fossil records, geological events, historical data) 



## 6\. Historical Significance

6.1 Neutral Evolutionary Theory

"This neutral theory claims that the overwhelming majority of evolutionary changes at the molecular level are not caused by selection acting on advantageous mutants, but by random fixation of selectively neutral or very nearly neutral mutants through the cumulative effect of sampling drift (due to finite population number) under continued input of new mutations" (Kimura, 1991)

Neutral Evolutionary Theory was developed by the Japanese scientist Motoo Kimura in 1968, proposing that most variation at the molecular level is not a result of natural selection and does not affect fitness, and so the majority of genetic variation is explained via randomness.

Pseudogenes and nonfunctional areas of the genomes mutate at a far faster rate than active genes, and synonymous mutations occur at a much higher rate than non-synonymous ones. Actively beneficial mutations are rare, meaning the remaining mutations do not have strong effects on the population. The majority of mutations found in the genome are evolutionarily neutral. Kimura’s model shows that the rate of neutral mutations is a consequence of the mutation rate, not selection. His calculated mutation rate gave credibility to the concept of the molecular evolutionary clock, revolutionizing genetic research, as by calculating the mutation rate and number of mutations present, the genetic age of a species can be estimated.

6.2 COVID-19 Sequencing

During COVID-19, BEAST (as well as other bioinformatics tools MEGA-X and TempEst) was used to track the introduction and movement of SARS-CoV-2.

Using these tools, scientists built the virus’s genomic evolutionary tree, deduced how it changed over time, found its Time to Most Recent Common Ancestor, or TMRCA, and analyzed the virus’s selection pressures (Analysis of variation and evolution of SARS-CoV-2 genome)

They also found the spread path of the virus—originating in China before spreading to other Asian countries before moving to the Americas and Europe. (Evaluation of global evolutionary variations in the early stage of SARS-CoV-2 pandemic)

This research helped identify COVID-19’s global variant patterns and potential evolutionary directions, which supported the development of vaccines and other preventative measures like predictive outbreak models.

## 7. Limitations of the Molecular Evolutionary Clock

Molecular clocks are powerful tools, but they have several important limitations. Inconsistent mutation rates, lack of fossil record, natural selection, and substitutional saturation can all introduce uncertainties into the final results.

7.1 Inconsistent Mutation Rates

Mutation rates are not constant. Although molecular clocks often assume a steady rate of mutation, real biological systems show substantial variation. Mutation rates differ across species due to factors such as generation time, DNA repair efficiency, and environmental pressures. Even within a single organism, different genes evolve at different rates. While careful gene selection can reduce some of this variability, it remains an important limitation of molecular clock models.

7.2 Difficult Calibration

Fossil calibration is inherently uncertain. Molecular clock estimates often rely on fossils to anchor divergence times. However, the fossil record is incomplete and sometimes difficult to interpret, which introduces uncertainty into calibration points. Missing or ambiguous fossils can therefore cascade error down the whole timescale.

7.3 Effect of Selection

Selection affects apparent mutation rates. Molecular clocks work best when most mutations are neutral and accumulate at a roughly consistent pace. However, natural selection disrupts this process. Purifying (negative) selection removes deleterious mutations, making evolutionary rates appear slower, while positive selection accelerates the fixation of advantageous mutations. These selective pressures can distort rate estimates and complicate clock-based inferences.

7.4 Substitutional Saturation

Multiple substitutions at the same nucleotide site obscure true evolutionary change. A single site in a sequence may mutate more than once over long time periods. Additionally, depending on how densely sequences are sampled through time, some mutations may go undetected. This phenomenon—often called substitutional saturation—causes observed genetic differences to underestimate the actual number of mutations, reducing the accuracy of molecular clock estimates.
