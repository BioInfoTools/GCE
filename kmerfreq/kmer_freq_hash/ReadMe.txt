1. Brief introduction

This is a k-mer counter program(version 1.0), it counts k-mer frequence of both strand from sequencing data and uses hash key to store the codes of k-mers.Program requires 8byte memory to store one k-mer, however the memory requirement is in line with the number of k-mer species in the given data.
   

2. Usage:

 kmer_freq_hash [options]

   -k <int>  set the k-mer size(9~27), default=17
   note: program store 56 bit for one k-mer sequence and 8 bit for frequence(<=255), so each k-mer entity in hash table use 64bit memory.
   
   -l <string>  set the read file list, no default vaule
   note: the absolute paths list of reads file£¬each reads files take a line.
   
   -c <double>  set min precision rate for k-mer, set between 0~0.99, or -1 for unrestrained, default=-1
   note: set high accuracy rate to filter low quality k-mers, but make sure the sequencing coverage is adequate. The accuracy rate is calculated by the Phred quality score. 
   
   -q <int>  set Quality value ascii shift, default=64
   note: generally 64 or 33 for illumina sequencing data.
   
   -r <int>  set read length used to get k-mers, default=read's real length
   
   -a <int>  ignored length of the beginning of read, default=0
   
   -d <int>  ignored length of the end of read, default=0
   
   -g <int>  set total bases number used to get k-mers, default: all the bases of read files
   note: when the sequencing coverage is too large, you can set the total bases number used to get suitable k-mers.
   
   -t <int>  set the thread number, default=1
   note: the more thread number, the higher running speed, and more resources occupation, so it should be less than the number of CPU cores.
   
   -i <int>  initial size of each hash table(hash table number equal to thread number), default=1048576
   
   -o <int>  set whether to output k-mers frequence file, 1:yes, 0:no, default=1
   note: if you don't need to get each k-mers frequence, set to 0.
   
   -L <int>  maximum read length, default=100

   -p <string>  set the output prefix, default=output
   note: often use species name as prefix
   
3. Attention

  (1)Parameter -i represent the initial size of each hash table, it will affect the running memory and speed, user is suggested to estimate the total k-mer species firstly. For example: suppose the genome size is 100M, the sequencing data coverage is 40X, the sequencing error rate is 1%, we use the kmer size is k-size = 21, thread number(-t) is T = 10, so the max number of real k-mer species is C = 100M, and the max number of error k-mer species is E = 40*21*1%*100M = 840M(here we just calculate the max number, suppose each error base generates k-size error k-mer species, but in practice, sequencing error general locate on the end of read, which will not alway generate k-size error k-mer species), and then, the total k-mer species number is A = C + E = 940M, we use F represent the hash load factor, in our program, it is 0.75. Now we can set the size of hash table : i = A/T/F = 125000000.

  (2)when the -i set reasonable, the peak size of running memory can be calculate by this formula: M = i * t * E, here E represent hash entity size(64bit). for example, suppose i = 125000000, t = 10, and then M = 125M*10*64bit = 1250M * 8Byte = 10G 
   

4. A quick start example: (the frequently-used parameters are showing)

	kmer_freq_hash -k 21 -l yeast_reads.lst -t 6 -p yeast_k17  2>kmerfreq.log
  
  This will generate 3 result file£º
  (1)k-mer frequence file: in *.freq.gz compressed format, each line represent "k-mer_sequence	frequence".
  (2)k-mer frequence statistic file(*.freq.stat): each lin represent "frequence	k-mer_species_number". The max frequence is 255, when k-mer frequence larger than 255, it will count as 255.
  (3)kmerfreq.log: the program running log and analysis result.

