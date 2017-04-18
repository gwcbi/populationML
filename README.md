# populationML
Learn population parameters from NGS data

## Hypothesis/question:

Can we estimate genetic diversity and other population genetics parameters from sequencing reads without assembly?

## Approach:

Create a model to infer genetic diversity from the kmer distribution of the sequencing reads.

1. **Simulate populations** with known parameters
2. **Simulate sequencing** of these populations
3. **Calculate the kmer distribution** of the simulated datasets. 
4. **Perform supervised learning** using the simulated kmer distributions and known diversity parameters as training data

## Methods:

The supervised learning technique chosen should find a function that maps a kmer distribution (*X*) to the population parameters (*Y*).

Here, we define the kmer distribution (or kmer spectra) to be the set of all the kmers that appear in the dataset and their frequencies. Let *X: X(k<sub>j</sub>) = c<sub>j</sub>* where *k<sub>j</sub>* is a word of length *k* and *c<sub>j</sub>* is the number of times *k<sub>j</sub>* is observed in the dataset. Since the kmer space is potentially very large, and because the kmer distribution is not directly comparable between populations with different sequences, we must summarize the kmer distribution using relationships among the *k<sub>j</sub>* and/or *c<sub>j</sub>*. I can think of two possible ways to do this that we can explore:

Reduce the kmer distribution *X* to a distribution of kmer counts *x*. The kmer count distribution *x* is *x(c<sub>i</sub>) -> n<sub>c_i</sub>* where *c<sub>i</sub>* is a frequency in *X* and *n<sub>c_i</sub>* is the number of kmers with frequency equal to *c<sub>i</sub>*. Thus the kmer count distribution is  the number of kmers that appear once, twice, etc. Intuitively, the shape of this distribution should reflect the number and frequency of mutations (SNPs) in the population. For example, given a population with zero diversity and perfect sequencing, every kmer will have the same number of observations, with the number of observations equal to the sequencing depth. If one mutation occurs in the population, this will create *k* new kmers, and the frequency of the new kmers will be a function of the frequency of that SNP in the population. Thus, given the kmer count distribution, there may be a way to extract the number of SNPs and their frequencies. This idea could be extended to many SNPs, but it becomes but with added complexity when SNPs occur within *k* nucleotides of each other.

The other way to look at this problem is looking at the sequences of the kmers. Each kmer is related to each other kmer by an edit distance (could be [Hamming distance](https://en.wikipedia.org/wiki/Hamming_distance), maybe [Levenshtein](https://en.wikipedia.org/wiki/Levenshtein_distance)) that describes how similar the sequences are. Kmers that are the result of mutations will be very closely related. By clustering kmers by distance and examining the distances and sizes among clusters, we may be able to learn about the diversity of the dataset. Another parallel is nucleotide diversity (π) as a measure of genetic diversity. The nucleotide diversity is the average number of nucleotide differences between all pairs of samples. The idea would be to learn a function relating the pairwise distances among all kmers to the nucleotide diversity or other parameters.


