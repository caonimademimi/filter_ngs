# filter_ngs
Filtering erroneous data from mgi (MGISEQ) sequencing

mgi (MGISEQ) sequencing will cause the following error reads:
(1) A large number of N bases appear in reads.
(2) A large number of single base repeats in reads.
(3)A large number of multi-base repeats in reads.

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
cd filter_ngs
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


