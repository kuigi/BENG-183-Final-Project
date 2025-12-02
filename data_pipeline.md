# Data and Analysis Pipeline for Molecular Clocks




## 1) Obtaining Sequences and Molecular Data

Of course, to get started we first have to obtain the sequences that we will be analyzing for sequence change and create a phylogenetic tree. Molecular clock analysis can use whole genomes, single genes, and concatenated loci, so whatever part of the genome we need to utilize we can use. We can obtain through our public DNA/RNA databases such as GenBank, ENA, and GISAID. We can also use the sequences we obtain from sequencing in our own labs, provided we ensure that they pass our respective quality control filtering. 

We also want to make sure that we have the relevant **metadata** that we will use later in our phylogenetic tree. This is composed mainly of elements that provide our sequences with more context, providing our sequences with information about the species it was obtained from, sampling location, and (arguably most importantly) sampling *date*. By utilizing the sampling date, BEAST can convert sequence changes to dates, and dates to years on our phylogenetic tree. 

Lastly, we want to ensure that the sequences that we use contain homologous regions across all samples. This way, in the next step (Multiple Sequence Alignment), the alignment of sequences will be successful and meaningful, and the creation of the phylogenetic tree later on will be accurate. 

---
## 2) Multiple Sequence Alignment



---
## 3) Inference of Phylogenetic Tree



---
## 4) Selection and Placement of Molecular Clock Model



---
## 5) Final Output: Time-Scaled, Ultrametic Phylogenic Tree
