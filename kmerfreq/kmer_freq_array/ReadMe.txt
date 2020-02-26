1. Brief introduction

This is a k-mer counter program (version 1.0), it counts k-mer frequence of both strand from sequencing data and uses the array index to represent the codes of k-mers. Program requires 4^K byte computer memory for any data, for example: if KmerSize=17, then the memory usage is 4^17 = 16G, (KmerSize=15 Memory=1G; KmerSize=19 Memory=256G). 


2. There are 3 executable files under this package:

(1). kmer_freq
   The single thread version of k-mer frequence counter program.

(2). kmer_freq_pfile
   The multiple thread version of k-mer frequence counter program.
   Each thread process a file, this can parallel both IO and CPU.

(3). kmer_freq_pread
   Another multiple thread version of k-mer frequence counter program.
   Each thread process a read, this can only parallel CPU, but not IO.
   This version also works better when the IO is limited;
   

3. Usage:

 kmer_freq_pread [options]

   -k <int>  set the k-mer size, default=17
   note: when k=17£¬k-mer theoretic max number is 4^17 = 2^34 = 16G, store 1 byte for a k-mer frequence(<=255), the k-mer frequence table will use 16G memory.
   
   -l <string>  set the read file list, no default vaule
   note: the absolute paths list of reads file£¬each reads files take a line.
   
   -c <double>  set min accuracy rate of k-mer, set between 0~0.99, or -1 for unrestrained, default=-1
   note: set high accuracy rate to filter low quality k-mers, but make sure the sequencing coverage is adequate. The accuracy rate is calculated by the Phred quality score.
   
   -q <int>   set Quality value ascii shift, default=64
   note: generally 64 or 33 for illumina sequencing data.
   
   -m <int>   set whether to output k-mers frequence file, 1:yes, 0:no, default=1
   note: if you don't need to get each k-mer frequence, set to 0.
   
   -b <int>   set total base number used to get k-mers, default: all the bases of read files
   note: when the sequencing coverage is too large, you can set the total bases number used to get suitable k-mers.

   -t <int>   set the thread number, default=1
   note: the more thread number, the higher running speed, and more resources occupation, so it should be less than the number of CPU cores.

   -p <string>  set the output prefix, default=output
   note: often use species name as prefix

4. A quick start example: (the frequently-used parameters are showing)

	kmer_freq_pread -k 17 -l yeast_reads.lst -t 6 -p yeast_k17  2>kmerfreq.log
  
  This will generate 3 result file£º
  (1)k-mer frequence file: in *.freq.gz compressed format, each line represent "k-mer_sequence	frequence".
  (2)k-mer frequence statistic file(*.freq.stat): each lin represent "frequence	k-mer_species_number". The max frequence is 255, when k-mer frequence larger than 255, it will count as 255.
  (3)kmerfreq.log: the program running log and analysis result.

