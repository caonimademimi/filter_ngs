# filter_ngs
Filtering erroneous data from mgi (MGISEQ) sequencing

mgi (MGISEQ) sequencing will cause the following error reads:
(1) A large number of N bases appear in reads.
(2) A large number of single base repeats in reads.
(3)A large number of multi-base repeats in reads.

filter_ngs mainly filters the MGI data by the partial characteristics of the kmer and the CG content characteristics.



