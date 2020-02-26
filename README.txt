
GCE (genomic charactor estimator) is a bayes model based method to estimate the 
genome size ,genomic repeat content and the heterozygsis rate of the sequencing
sample. The estimated result can be used to design the sequencing strategy.


INSTALLATION
Download the package and run

tar -xzvf gce.tar.gz 
make (to build the executable file "gce")

in the compiled version, you can use the gce directly.

USAGE
gce -f test.freq [-g total_kmer_num]

test.freq is a list file containing at least two lines, the first line
is depth and the second line is frequency(not the ratio) of the depth, other
line is not recognized in the program. 

Options:
-g 	total kmer number counted from the reads. It is suggested to set this
	value. If not, the total kmer number will be calculated using data in
	kmer_depth_file.
-c	unqiue coverage depth. It is suggested to be set when there is no
	clear peak or there is clear un-unique peaks, especially when the
	heterozygous ratio is high.
-H	when the heterozygous caused peak is clear, it is suggested to use
	hybrid mode.
-b	when there is sequencing bias, you need to set the value.

-m	estiation mode, there are standard discrete model(default) and continuous model. You can
	set 1 to use continuous model, but its stability is not well.

-D	set the raw distance for continuous model, which decide the peak
	number.
	
-h: display help information.

OUTPUT
when you run: gce -f test.freq >gce.table 2>gce.error

Estimation result file: gce.table:
there are two tables, one is ai table and the other is frequency table.
#ai table:
showing the estimated ci and ai for kmer species and Ci and bi for kmer individuals. 
the range of i is from 1 to max peak.
#i      c[i]    a[i]    C[i]    b[i]

#frequency table:
showing the raw depth distribution of kmer species(real_P(x)), the raw depth distribution
of kmer individuals(real_F(x)), the estimated depth distribution of kmer species(est_P(x)) 
and the estimated depth distribution of kmer individuals(est_F(x)).
#depth  real_P(x)       real_F(x)       est_P(x)        est_F(x)

For more details about kmer species and kmer individuals, please read the manuscript.

Estimation log file: gce.error
we use the Final estimation table:
raw_peak        now_node        low_kmer        now_kmer        cvg genome_size     a[1]    b[1]
20      101211427       0       2442901555      20.9675 1.16509e+08 0.915339        0.799342
the genome size estimated here can be used in practise.

PERFORMANCE
gce is a extremely fast tool for estimating the genomic charactor. For the
standard discrete model, the max memory is about 1.5MB, taking less than one
second. For the continuous model, when setting -D 8, the max memeroy is about
1.5MB, taking about 5 seconds. The memory and time cost is only related to the
max kmer depth and the depth distribution, not related with K size.

COMMENTS/QUESTIONS/REQUESTS
Please send an e-mail to liubinghang@genomics.cn

