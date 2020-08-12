# filter_ngs
Filter out abnormally repeated reads in the second-generation sequencing platform (Illumina\MGISEQ).

mgi (MGISEQ) sequencing will cause the following error reads:

(1) A large number of N bases appear in reads.
<pre><code>
@V300015177L3C001R0341008922/1
ATTTCTAAATACCGTACCGTTTAATACCATTGTGAGGTATAAAAAAGAGAGAGAGAAGGGGTGTGACGGGACAGAAGAACTACTGGCTATGCATCCGTGTTTTATTCTATATTAACACCAGTATCAAAGCCATCGACATTCGNNNNNNNN
+
FFFFFFFEFFFF:F9FFFF<FFGFFFEFFFF@FFDFEFFFFFFFFFFFFFDFFFFFFFFFEFFFF=BFEFFFFFFFFFFFFFFFFFFF5FEFFFFFFFFFFEF9FFFFBFFFEFEFFFFFAFGFFFFF=9FFFFF4FF:EF;!FFFF<DE
</code></pre>
(2) A large number of single base repeats in reads.
<pre><code>
@V300015177L3C001R020690553/2
GTCAATGGATTTGGGATATTGAGAGTATTTTCATCTTCGTGGCCGTGTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTTACTCCTTGGGAAAGAAGATGGAAACTTTGGGTTGCAAGGAGAGGGGGGGGCGGGGGGGGGG
+
99BEE=??8:4?<1:/E=8;>6@7=?=;EEE?5E9AEE66:>E:AGD4E>E&5E>8CC;F<77E9D<B85;;31F=??9E5=9E6;76)D3>51:;'0&2;?3E=>)2&//B>(&EB2883(0&C,DA+E,:>,1C.=6)*/<5E)C@E8
</code></pre>
(3)A large number of multi-base repeats in reads.
<pre><code>
@V300015177L3C001R009061026/1
ATGATGATGATGATGATGATGATGATGATGATGATGATGATGATGATGATGATGATGATGATGATGATGATGATGATGATGATGATGATGATGATGATGATGATGATGATGATGATGATGATGATGATGATGATGATGATGATGATGATG
+
FFFFFGFF>GFGEFFFFGFFFFGFFEGFFE@FFFFFFFFFFFFGFFBFFFFFFFFGFFFAFFFGGFFFFFGDFF?FFEFGFFFFFFGFGFGCFFFGBFEFCFFEEFGEFGFFFFCDF1FG?FBG=G:GGFAFGBEGFED9E?GCD=GFF>
</code></pre>

Illumina sequencing will cause the following error reads:
(1) A large number of single base repeats in reads.
<pre><code>
GGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGG
ATCCATTAATTGCGACAGGTGGCGTAGGCGCAACATGTGACCGACTGCAGGTCGGTGGCAACAGTAGGAAGTCATACATTTGAGATCGGAAGAGCGTCGTGTAGGGAAAGAGTGTCGTGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGG
GGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGTGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGTGGGGGGGGGGGGGGGGGGGGGGGGGGG
GGGCGGGGGGGGGGGGGGGTGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGTGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGTGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGG
GGGGGGGGGGGGGGGGGGGGGGTGGGGGGGGGGGCGGGGGGGGGGGGGGGGGGGGGGGGCGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGGG
</code></pre>

filter_ngs mainly filters the MGI data by the partial characteristics of the kmer and the CG content characteristics.

## Installation
Installation method 1
<pre><code>
git clone https://github.com/zxgsy520/filter_ngs.git
cd filter_ngs
chmod 755 filter_ngs
</code></pre>

Installation method 2
<pre><code>
wget https://github.com/zxgsy520/filter_ngs/archive/v1.1.0.tar.gz
tar -zxvf v1.1.0.tar.gz
cd filter_ngs-1.1.0
chmod 755 filter_ngs
</code></pre>

## Instructions
<pre><code>
usage: filter_ngs [-h] -i FILE -I FILE [-kl INT] [-kc INT] [-kv INT] [-mb INT]
                  [-rl INT] [-o STR] [-O STR]

name:
    filter_ngs.py --The filter sequence contains N or kmer and gc have abnormal reads.

attention:
    filter_ngs.py --input1 R1.fastq.gz --input2 R2.fastq.gz
version: 1.1.0
contact:  Xingguo Zhang <113178210@qq.com>    

optional arguments:
  -h, --help            show this help message and exit
  -i FILE, --input1 FILE
                        Input file R1, fastq or fastq.gz.
  -I FILE, --input2 FILE
                        Input file R2, fastq or fastq.gz.
  -kl INT, --klen INT   Set the length of kmer,default=4.
  -kc INT, --kscore INT
                        Set kmer score(0-100), default=40.
  -kv INT, --kvari INT  Set the variance of kmer(>=0), default=50
  -mb INT, --maxbase INT
                        Set the percentage of the largest base(0-100),
                        default=40
  -rl INT, --rlen INT   Maximum single base repeat number(5-150), default=12
  -o STR, --output1 STR
                        Output file R1.
  -O STR, --output2 STR
                        Output file R2.
 </code></pre>

## Treatment effect
MGI data is first processed with filter_ngs (default parameter).Use flye to assemble, use racon correction to correct 3 times using third-generation data, use pilon correction to use second-generation data twice, and use nextpolish (default parameter) to use second-generation data to correct once.
### Comparison of sequencing depth and coverage
The correction effect of MGI data processed with filter_ngs (default parameter) and Illumina data.
<pre><code>
Data Type	MGI	Illumina
Depth	Base number	Coverage ratio(%)	Base number	Coverage ratio(%)
1	3,124,148	99.99	3,124,219	99.99
5	3,123,647	99.97	3,123,957	99.98
10	3,123,345	99.96	3,123,877	99.98
20	3,122,809	99.94	3,123,601	99.97
Coverage depth(X)	514.98	516.8
 </code></pre>
 ### Single base accuracy assessment
 <pre><code>
Data Type	Depth	Hetero SNP	Hetero Indel	Homo SNP	Error rate by Homo SNP(%)	Homo Indel	Error rate by Homo Indel(%)	Error rate by homo variants(%)	Accuracy genome(%)
MGI	depth>=1x	1	0	0	0	7	0	0.000224	99.999776
depth>=5x	1	0	0	0	5	0	0.00016	99.99984
depth>=10x	1	0	0	0	2	0	0.000064	99.999936
Illumina	depth>=1x	0	0	0	0	6	0.000192	0.000192	99.999808
depth>=5x	0	0	0	0	3	0.000096	0.000096	99.999904
depth>=10x	0	0	0	0	0	0	0	100
 </code></pre>

