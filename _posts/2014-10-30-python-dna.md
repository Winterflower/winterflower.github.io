---
---

<script type="text/javascript"
  src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML">
</script>

#Analyzing DNA sequences using Python and Scikit learn

DNA sequencing has been marching forward with giant steps.
Advances in sequencing technologies means that more and more genomes
have been sequenced and assembled. While this is no doubt exciting for
biologists and geneticists, the prospect of so much 'biological code'
should also inspire any data hacker.

While completing my MSc at Edinburgh, I had a lot of fun working with
Python and various DNA sequences I downloaded from public databases. This small
tutorial is meant to give you an example of how to analyze a DNA sequence of
interest using Python's machine learning libraries.

#Creating an "adapter" between raw data and Scikit learn
In supervised machine learning, we are interested in training a model, which
can classify unseen examples into classes. For simplicity, let us assume we have
a binary classification task, that is we have some DNA sequences, which
represent belong to the 'positive' class and others, which belong
to the 'negative' class. These positive and negative DNA sequences are
stored in the standard FASTA format.

Our first challenge is to convert DNA strings such as "TTGTTGGTGTCG" into
a feature representation. We want to take the string "TTGTTGGTGTCG"
and produce a vector of attribute-value pairs. This is where the "machine
learning as an art and experimental science" part comes in.
One, though usually quite uninformative way to represent a DNA sequence
as a feature vector is to count the occurrence of each nucleotide. The feature
vector, <span> \( v_{DNA} \) </span> for the DNA sequence "TTGTTGGTGTCG" is then

<div> \[v_{DNA}=(count(T), count(A), count(C), count(G))^T=(6,0,1,4)^T\] </div>

Note that we are ignoring the fact that DNA is double stranded for now. In real
applications of machine learning to DNA, it would be
imperative to taken into account the double strandedness.

Instead of counting each nucleotide, we might be able to capture
more of the 'nature' of the DNA code in the fragment by representing
the DNA sequence as a vector of k-mer counts. The features in the k-mer count
vector are all of the possible k-mers of length k. For small values of k, it
is possible to generate all of the permutations by hand, but for larger
values of k this gets tedious quickly. The *preprocess.py* module
in the [dna-adapter](https://github.com/Winterflower/dna-adapter) project contains
a method which generates all of the possible k-mers.

```python
# A code snippet from the preprocess.py class

def generate_kmers(kmerlen):
        """
        Returns a list of all possible kmers of length kmerlen

        :author Dongwon Lee (2011)

        Arguments:
        kmerlen -- integer, length of k-mer

        Return:
        a list of the full set of k-mers

        >>> preprocess.generate_kmers(2)
        ['AA', 'AC', 'AG', 'AT', 'CA', 'CC', 'CG', 'CT', 'GA', 'GC', 'GG', 'GT', 'TA', 'TC', 'TG', 'TT']

        """

        nts = ['A', 'C', 'G', 'T']
        kmers = []
        kmers.append('')
        l = 0
        while l < kmerlen:
                imers = []
                for imer in kmers:
                        for nt in nts:
                                imers.append(imer+nt)
                kmers = imers
                l += 1

        return kmers

```
To use this method in your own scripts, simply import the module and call
the generate_kmers method.

```python
import preprocess
#generate all possible k-mers of length 2
kmers=preprocess.generate_kmers(2)
```
