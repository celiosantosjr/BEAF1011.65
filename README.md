# BEAF - Referenced Binning Engine for Autonomous Finding

This pipeline was created for exploratory analysis of binning using references of genome, proteins or gene families. It allows a posterior assembly and assessment of contigs. Briefly, first a FASTQ or FASTA file, in paired-end or interleaved format is added to pipeline after quality trimming. A format checking is performed and the indicated reference is used to generate a database. The query file is used to sort sequence buckets and speed up the process. A search by homologous sequences is made using Usearch algorithm. After this to genome option, a counting of hits is made and abundance is calculated using reads per million followed by assembly with Spades program and an assessment with QUAST. For proteins/gene families databases are generated according user's indicated references and the search continues using Blastx/n algorithm before hits counting, assembly and assessment per family.

Install all of the dependencies on Ubuntu: 
	
	Install Usearch in /bin path (Available in <http://www.drive5.com/usearch/download.html>)
	Install FastQC in /bin path (Available in <http://www.bioinformatics.babraham.ac.uk/projects/download.html>)

	After previously informed installations, type:

	$ ./install.sh

During installation it will be required your user password, since it should be done as superuser.

# Tests

During installation your BEAF will be tested out. If you detect some problem in outputs generated by program, please try to observe if all extensions referred bellow are installed and try to download new Reference_seqs and Test_sample folders. Decompress files inside Reference_seqs and carry out the test using the following commands. We strongly recommend to test out your machine and software functioning by using our provided genomes and references in Test_sample folder, before effective use of this pipeline. To this, first use our previously set up in config.file, and type in bash:

	$ ./BEAF.sh
	Option -b for full mode or -s for soft mode

It will spend by average in a machine of 16Gb-RAM and 4 CPUs almost 0.5-1h. Results should be consistent, enjoy getting to know the format of the outputs of this program.

# Usage

```

-Setting up config.file

Make a config.file file containing 7 columns, respectively: T1, T2, R1, R2, Ref, Subref and Out.
On the first column (T1), use 'G' to analyse genomes, 'P' to analyse proteins and 'N' with nucleotide sequences.
On the second column (T2), depending on the format of the metagnomic data, you may use 'R', for pair-end fastq files; 'I', for a interleaved fastq file; or 'F', for a interleaved fasta file. Regardless of the format you're using, all files must be compressed with gzip (.gz).
On the third column (R1), you must give the complete path to your file (R1 file in the case of pair-end, interleaved in the other two cases), from your main folder to the file itself (example: home/usr/Desktop/BEAF-master/R1_trimmed.fastq.gz).
On the fourth column (R2), in the case of paired end files, you must give the complete path to the second file (R2), as explained above. For interleaved files, use 'NA' instead.
On the fifth column (Ref), use the name of the reference genome file (not compressed in fasta format) or protein/gene databank (this first database should be less specific and be composed by a large number of sequences, helping program to identify besides highly homologous reads, less homologous ones too) file in the Reference folder. If there will be a Reference for protein/gene databanks, you can also enter instead just fasta format, other file formats, like: UDB or blastdb.
On the sixth column (SubRef), use the path to the folder containing subreferences for protein families, if any. If using genome, use 'NA' instead.
On the seventh column (Out), use the desired name for the output folder generated from that file (example: Metaspot1_Ecoli). Don't use spaces nor slashes in the name.
Be careful to not leave any empty lines in final file.

Example:

N	I	/home/usr/Desktop/BEAF1011.65_master/Test_sample/Alistipes_putredinis_DSM_17216.fna.fastq.gz	NA	DNA_pol.fasta	DNA_pol	1D

-Execute the batch script: 

	$ ./BEAF.sh

- Changing parameters of run

User can change every run parameter directly in shell script file, since blast/usearch identity or coverage parameters to keep or not keep formatted reference databases. You can choose between blastn and megablast when finding against gene families, it could in some cases speed up the process. All possible changes are indicated over script, but unless you have a good notion of shell language we strongly advice to not change any parameter. If some parameters are changed, indicates in your "Material and Methods" section the new parameters. If nothing was changed just cite these parameters as "default".

- Results 

Results are given inside OUTPUT folder. In each output, previously designed by user there will be following structure:

.....For any option:

/home/usr/PATHWAY_TO_HERE/BEAF-master
	File containing the summary of all runs: time.log

.....For genome binning:

/home/usr/PATHWAY_TO_HERE/BEAF-master/OUTPUT/$Out
	File containing the summary of analysis: Log.txt
	File containing the reads identified as hits: hits.fa.gz
	File contanining the contigs or scaffolds obtained in assembly of hits: scaffolds.fa.gz 
	File containing the assessment of assembly: assessment.tar.gz
	Folder containing the FASTQC results of trimming before files merging: FASTQCresults
	File containing identified orf's: ORFs.fa.gz

.....For protein/genes families binning:

/home/usr/PATHWAY_TO_HERE/BEAF-master/OUTPUT/$Out
	Folder containing the FASTQC results of trimming before files merging: FASTQCresults
	File containing the summary of analysis: Log.txt
	File containing the reads identified as hits: hits.fa.gz
	Folder containing the Blastx/n results: blast_hits
	Folder containing sequences after Blast-filtering: read_hits
	Folder containing contigs after assembly: contigs
	Folder containing identified orf's by contigs file: ORFs


```

# Speeding up the process

(1) To speed up all process, you can disabilitate some functions over the script, this will be done running script soft_BEAF10.11.65.sh instead BEAF.sh. - This pipeline does not make the fastq file trimming, quality test of fastq files, assembly retry (it would take small kmers scenario which can take more time instead for genomes binning), QUAST assessment or ORFs prediction;

	$ cd /PATHWAY_TO_HERE/BEAF-master
	$ ./BEAF.sh
	Option -s

(2) Alternativelly, you can run our helper script named optimizer.sh, already included. We strongly recommend run this script when running genomes binning function. It will request user password, since it works as a constant renicer device. It should be run in a new terminal, so your system will work with two independent terminals.

	$(1) cd /PATHWAY_TO_HERE/BEAF-master
	$(1) ./BEAF.sh

	In another terminal:

	$(2) cd /PATHWAY_TO_HERE/BEAF-master
	S(2) ./optimizer.sh

# Contact and License

All bugs should be reported to: celio.diasjunior@gmail.com. Please cite us: "Santos-Júnior, CD et al. BEAF: An automated pipeline to referenced genome and protein or gene families binning and assembly of next generation sequencing data. (Under publishing)". Rights between any third party software are not in claim, they continue under rights of original distribution.

```

    BEAF1011.65
    Copyright (C) 2016  Federal University São Carlos

    This program is free software: you can redistribute it and/or modify
    it under the terms of the GNU General Public License as published by
    the Free Software Foundation, either version 3 of the License, or
    (at your option) any later version.

    This program is distributed in the hope that it will be useful,
    but WITHOUT ANY WARRANTY; without even the implied warranty of
    MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
    GNU General Public License for more details.

    To get a copy of the GNU General Public License see <http://www.gnu.org/licenses/>.

```

# Third party softwares:

- Blast+ (National Center for Biotechnology Information, USA)
- HMMer3 (Sean R. Eddy, Travis J. Wheeler and HMMER development team)
- Python 2.7 (http://www.python.org)
- Biopython (http://www.biopython.org/)
- Ext.py (Emmanuel Prestat, HalloDx, Marseille / France)
- Quast 4.4 (Center for Algorithmic Biotechnology - CAB, St. Petersburg State University)
- Spades 3.9.1 (Nurk S. et al. Assembling Genomes and Mini-metagenomes from Highly Chimeric Reads. Res. in Comp. Mol. Biol. vol.7821, 158-170)
- Usearch (Edgar,RC (2010) Search and clustering orders of magnitude faster than BLAST, Bioinformatics 26(19), 2460-2461)
- Cutadapt (Martin, M. Cutadapt removes adapter sequences from high-throughput sequencing reads. DOI: http://dx.doi.org/10.14806/ej.17.1.200)
- FastQC (Simon Andrews, Babraham Bioinformatics, USA)
- bb.orffinder.pl (https://github.com/dsenalik/bb/blob/master/bb.orffinder)
