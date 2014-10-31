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
vector, <div> \(v_{DNA}\) </div> for the DNA sequence "TTGTTGGTGTCG" is then

<div> \[v_{DNA}=(count(T), count(A), count(C), count(G))^T=(6,0,1,4)^T\] </div>
