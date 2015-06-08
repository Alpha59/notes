# MEGAN5 Program Review
**John Ailor**

## Introduction 
Megan, or  MEtaGenome  ANalyzer  is a stand-alone analysis tool, developed for the metagenomic analysis of short-read data. MEGAN uses a database search based analysis of very large data sets, much like BLAST, known as DIAMOND to parse genometric sequences before processing the alignments.  MEGAN currently on the fifth iteration of its software, with the most recent publication known as MEGAN5. MEGAN5 offers a number of significant improvements over MEGAN4, many of which are aesthetic- but also a number of algorithmic improvements. These improvements have led to faster processing, better BIOM extraction, and the ability to work with multiple sample sets simultaneously.  

## Usage 
In order to use MEGAN, users must first use either the software BLAST, or DIAMOND. Although BLAST offers better, more accurate results, DIAMOND offers significant improvements on speed. The developers of MEGAN hope to offer a joint package known as MALT (MEGAN alignment tool), which can be used in place of BLAST. The basic flow of data is shown below:

```flow
start=>start: Acquire Sample data
end=>end: Interactive Analysis and Visualization
op1=>operation: DNA Reads
op2=>operation: Sequence Comparison w/ BLAST
op3=>operation: Comparison Data
op4=>operation: Reference Database 

start->op1->op2->op3->end
op4->op2
```

MEGAN will then allow users to interactively explore the dataset. MEGAN provides a variety of different ways to view the dataset, depending on if the dataset is a SEED or KEGG classification. MEGAN can also (now available in version 5) calculate the distance between nodes using either a PCoA, UPGMA tree, or NJ tree calculation. Available in both version 4 and 5 is the non-hierarchical clustering using the Neighbor-net method. 

The output from MEGAN has been improved to provide new plotting tools, including co-occurrence plots, space-filling radial trees, bubble charts, and wordclouds. These can be used interchangeably depending on the research needs. 

### Installation
MEGAN5 installation is simple and is done through a setup wizard for both windows and *nix OS. Both setup wizards can be acquired on the [downloads](http://ab.inf.uni-tuebingen.de/data/software/megan5/download/welcome.html) page of the website. Windows installation is through a direct download, and *nix installation can be done using the `wget` command line tool.  

## Data set 
The data set that will be analysed will be as stated in the original project proposal list

>Metagenomic dataset from mice that were healthy vs. mice that were injected with DEN (a toxin) that induces fatty liver disease

The goal is this experiment will be to see if MEGAN can identify any changes in the mice DNA sequences pre/post the introduction of the DEN toxin. MEGAN should easily be able to find where the DNA sequences split from each other. 

## Review of MEGAN5
After downloading and exploring the MEGAN5[^1] tool set, the user interface seems to be somewhat use able, and the program installed without any problems on both Linux and Windows 7 operating systems. On the Linux OS, MEGAN5 offers a command line tool- although you are expected to have GUI availability in order to perform any graphing since the software is based off several GUI based Java libraries. 

Extensive and comprehensive documentation is available through both the [manual](http://ab.inf.uni-tuebingen.de/data/software/megan5/download/manual.pdf) and the [website](http://ab.inf.uni-tuebingen.de/software/megan5/), which makes use of MEGAN5 very operable. Out of the requested features listed in the project description, MEGAN5 offers a large tool set to choose from. This includes

* Taxonomic composition through NCBI Taxonomy (BLASTn)
* Functional Analysis through SEED analysis
* Multidimensional Scaling and Diversity Metrics through PCoA analysis
* Differential Metabolic pathways through KEGG trees
* Statistical Comparison using Bonferroni correction to highlight differences on the trees
* As well as some other graphical output formats, including
	* Word Clouds of Genus Ranks
	* 16S rRNA analysis
	* COG (Clusters of Orthologous Groups of proteins) analysis
	* co-occurrence plot
	* space-filling radial trees

The main focus of MEGAN5 is on the Taxonomic composition, which does expect that a BLAST output file be provided. The BLAST output file, as well as associated FASTA files used can be imported through an easy to use dialog, although a license is required in order to process the results[^1]. The Taxonomic composition uses a easy to read logarithmic scale to display the number of hits for each output. Overall the software seems to provide a good interface for analysis and study of files post BLAST. There are also methods available for importing pure BIOM files or csv formatted files, however these do not support the full functionality that is provided by importing a BLAST file. 

## Results
Due to computing limitations on my windows 7 machine- I was only able to run sequencing on a single set of the files provided- about 750 sequences out of several hundred million. 

The files originally provided were in FASTQ format, and needed to be manually converted using a small bash/awk program into FASTA. 

```bash
#!/bin/bash
# The original file was not split into multiple lines, however for readability
# purposes the script was broken up at appropriate points. 
ls ./Sample_GRWS002*/* | 
gunzip -c | 
awk '
	{
		if(NR%4==1){ # if this is the first line of the sequence data
			printf(">%s\n",substr($0,2));# print the header 
								#information prepended by a carrot (>)
		}else if(NR%4 == 2){ # if this is the second line of the sequence data
			print $0; # print the sequence
		}
	}'
```
which was adapted from the wiki pages for [FASTAQ](http://en.wikipedia.org/wiki/FASTQ_format) and [FASTA](http://en.wikipedia.org/wiki/FASTA_format). This was run on a copy of the data which had been moved to the user directory. Using a subset of this data, I was able to run the web version of BLAST on the files, and use that output to run MEGAN5, Taxonomy, KEGG, SEED, and COG[^2] analysis. These reports seem similar to the reports generated by BLAST, showing most significantly that 100% of the samples contained mus musculus (Mouse) sequences. 

A bash script was developed to run the full sequence list through BLAST, saving the results and then running them through MEGAN5's command line tools on Proteus, however due to an unknown error the command line blast was unable to find any sequence matches for any of the samples. Since a subset of the same samples were successfully run through the BLAST online tool, and found an abundance of sequence matches using the same settings, these results are most likely caused by some error in configuration. 

Purely for completeness, the bash program is provided below. In order to lesson the load, the intention was to run each sample set separately- although a similar attempt was made to run all sequences at once. According to quick command line counts, there were approximately 350 million sequences on file. Since the BLAST of ~750 sequences took approximately 2 hours, it is unlikely that the complete data set would have been analysed, even with Proteus's many cores,  given the 72 hour time limit. 

MEGAN.sh
```bash
#!/bin/bash
importA=" import blastFile="
importB=" fastaFile="
importC=" meganFile=./execution.blastn.rma blastFormat=BlastN;"
endA=".blastn"
endB=".faa"

ls ./fasta/Rosen_Bouchard_FLD_mice/*/* | grep -v '.faa' | xargs rm -f -v

for name in $( ls ./fasta/Rosen_Bouchard_FLD_mice/*/* | grep '.faa' | xargs )
do
        ./blast-2.2.24/bin/blastall -i $name -p blastn -o ${name:0:75}$endA -b 5 -v 5 #-m 8
        echo "BLASTED : "$name"    Into : "${name:0:75}$endA
done

for name in $( ls ./fasta/Rosen_Bouchard_FLD_mice/*/* | grep '.faa' | xargs )
do
		#xvfb-run --auto-servernum --server-num=1 
		# would have allowed non-gui interaction, 
		# however this was not available on Proteus
        ../megan/MEGAN -g true -v true -x $importA${name:0:75}$endA$importB$name$importC
done
# simply used to output the BLAST files and ensure they 
# Found some sequences. 
tail -n4 fasta/Rosen_Bouchard_FLD_mice/*/*.blastn
```

and the Proteus submission script

```bash
#!/bin/bash
# tell SGE to use bash for this script
#$ -S /bin/bash
# execute the job from the current working directory, i.e. the directory in which the qsub command is given
#$ -cwd
# set email address for sending job status
#$ -M jra59@drexel.edu
# project - basically, your research group name with "Grp" replaced by "Prj"
#$ -P nsftuesPrj
# select parallel environment, and number of job slots
#$ -pe openmpi_ib 256
# request 15 min of wall clock time "h_rt" = "hard real time" (format is HH:MM:SS, or integer seconds)
#$ -l h_rt=72:00:00
# a hard limit 8 GB of memory per slot - if the job grows beyond this, the job is killed
#$ -l h_vmem=8G
# want at least 6 GB of free memory
#$ -l mem_free=6G
# select the queue all.q, using hostgroup @intelhosts
#$ -q all.q@@intelhosts

. /etc/profile.d/modules.sh
module load shared
module load proteus
module load sge/univa

bash MEGAN.sh
```

The MEGAN5 Command line interface would have provided a very strong way of processing the output from the bash files, however with the limitations of Proteus's GUI through X-term Server (not available for clustered use), and the the error in running BLAST, the MEGAN5 command line interface was not explored in further depth.

On the ~750 sequence sample subset, MEGAN5 performed analysis in approximately 5 minutes- utilizing only the resources available on a desktop PC. Several output graphs are available, and screenshots are provided of outputs from both Species and Family level genomic analysis. All of the graphs provided by MEGAN5 are scale able and interactive- which makes them ideal analysis tools for visual output. All graphic outputs can be exported to image or data formats- which allows them to be used outside of the MEGAN5 software package. Overall MEGAN5 appears to be a strong tool for Geometric analysis, although limitations of computational power prevented full exploration of it's capabilities. 

Unfortunately, although MEGAN5 does offer analysis of KEGG, SEED, and COG trees an error in the program[^2] prevented them from being successfully rendered. Similarly, MEGAN5 is capable of using Bonferroni correction to find differences between two sets, which would have been ideal in the situation of Mice sample genomes being compared, I was unable to complete this analysis since the sequence subset was only available for one sample. This means there were not two available sequence samples to compare.  

[^1]:The license for MEGAN5 is available for purchase, or online for academic use. 

[^2]: An error occurred while generating KEGG, SEED, and COG analyis, MEGAN5 timed out while waiting for connections to online database's and was unable to complete the analysis of those data sets. 

## Screenshots Appendix
#### Family 
![Word Cloud of Families](https://lh3.googleusercontent.com/-eEaNB6P19aY/VWu0NTrTZjI/AAAAAAAAAIM/ZfDlER15uNI/s800/wordF.JPG "wordF.JPG") Word Cloud of Families

![Tree of Families](https://lh3.googleusercontent.com/-SYo81gwAooo/VWu0exdetCI/AAAAAAAAAIY/ErIZTYnQHu8/s800/treeF.JPG "treeF.JPG") Tree of Families

![Rarefication curve of Families](https://lh3.googleusercontent.com/-DtkDHxMRgrc/VWu0m5JJ0NI/AAAAAAAAAIk/gMIzCCy7DMA/s800/rareF.JPG "rareF.JPG") Rarefication curve of Families

![Pie Chart of Families](https://lh3.googleusercontent.com/-ZYk4YKuwZrU/VWu0tuURLhI/AAAAAAAAAIw/yNhOg-ZOATg/s800/pieF.JPG "pieF.JPG") Pie Chart of Families

![Annoted Pie Chart of Families](https://lh3.googleusercontent.com/-qowIyP2aVwk/VWu01-AQ4bI/AAAAAAAAAI8/oeBX0OTU4BI/s800/pie2F.JPG "pie2F.JPG") Annoted Pie Chart of Families

![Bar Chart of Families](https://lh3.googleusercontent.com/-Ymemnd_NoCc/VWu0-Hxze-I/AAAAAAAAAJI/zoQF5AmXGpw/s800/barF.JPG "barF.JPG") Bar Chart of Families

#### Species 

![Word Cloud of Species](https://lh3.googleusercontent.com/-7-fLx8PNu34/VWu1G23QSAI/AAAAAAAAAJU/GY-7ZxueULY/s800/word.JPG "word.JPG") Word Cloud of Species

![Tree of Species](https://lh3.googleusercontent.com/-F8S9gSmFEV0/VWu1PFLd7UI/AAAAAAAAAJg/GW1sERHEQbM/s800/tree.JPG "tree.JPG") Tree of Species

![Rarefication Curve of Species ](https://lh3.googleusercontent.com/-T0dl9ZnBV2o/VWu1YJoZeZI/AAAAAAAAAJw/Vm8Jnld63-U/s800/rare.JPG "rare.JPG") Rarefication Curve of Species

![Pie Chart of Species](https://lh3.googleusercontent.com/-67clhWZlbXY/VWu1kvjuiwI/AAAAAAAAAJ8/64I4P0Wpbqg/s800/pie.JPG "pie.JPG") Pie Chart of Species

![Annotated Pie chart of Species](https://lh3.googleusercontent.com/-w675fn2cNPw/VWu1suzqWuI/AAAAAAAAAKI/TmW56om2rV4/s800/pie2.JPG "pie2.JPG") Annotated Pie chart of Species

![Bar Chart of Species](https://lh3.googleusercontent.com/-XPl4IhbBJoI/VWu14BoiD4I/AAAAAAAAAKY/awg_dpUAQuo/s800/bar.JPG "bar.JPG") Bar Chart of Species

