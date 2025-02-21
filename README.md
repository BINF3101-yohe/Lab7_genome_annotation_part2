# Lab_7_genome_annotation_part2

&ensp;
**OUTLINE**

[Step 1 - Lab setup](#step-1---lab-setup)

[Step 2 - Prepare files for annotation](#step-2---prepare-files-for-annotation)

[Step 3 - Download your files](#step-3---download-your-files)

[Step 4 - Upload files to GenomeQC](#step-4---upload-files-to-genomeqc)

[Step 5 - Submit your job](#step-5---submit-your-job)

[Step 6 - Record your Annotation Metrics results](#step-6---record-your-annotation-metrics-results)

[Lab Question 1](#lq-1)

[Step 7 - Generate your DNA coding sequences](#step-7---generate-your-dna-coding-sequences)

[Lab Question 2](#lq-2)

[Step 8 - Filter your CDS file](#step-8---filter-your-cds-file)

[Lab Question 3](#lq-3)

[Step 9 - Generate your protein-coding sequences](#step-9---generate-your-protein-coding-sequences)

[Lab Question 4](#lq-4)

[Step 10 - BUSCO results](#step-10---busco-results)

[LAb Question 5](#lq-5)


We have now annotated our genomes. This means we have identified locations in the genome where we believe genes are located. 
This week in the lab we will be analyzing our genome annotations. 

We will address questions such as: How many genes did we annotate? How well did our annotation capture the genes we expect to see? 


&ensp;
## Step 1 - Lab setup

We will be using several files from lab 4 this week. 

&ensp;
&ensp;

### Step 1a
First, create a new folder in your home directory for this week's lab. 

```bash
#go to home directory
cd

#make a new directory
mkdir lab_7
```
&ensp;
&ensp;

### Step 1b
Now we need to copy several files from our braker results into the new directory. 
First, find your way into the braker directory 

```bash
#move into the braker folder
cd lab_6/braker/
#look at the files
ls
```

Our results are stored in two files 
**braker.gtf**
**braker.gff3**

These are _not named well_ so we are going to copy _and_ rename them at the same time. You will need your SRR number to replace the SRR number below. 

**IF THESE FILES DO NOT EXIST FOLLOW COPYING INSTRUCTIONS #2**

### Copying instructions 1 - you have braker files

```bash
cp braker.gtf ~/lab_7/SRRXXXXXX.gtf
cp braker.gff3 ~/lab_7/SRRXXXXXX.gff3
```

**Note** the ~ symbol means starting from your home directory. 

We also need a copy of our genome. We are going to save it as a new version because we are going to edit it slightly

```bash
#go back to our lab_6 directory
cd ~/lab_6

cp SRRXXXXXX-contigs.v2.fa ~/lab_7/SRRXXXXXX-contigs.v3.fa
cp SRRXXXXXX-contigs.v2.fa.masked ~/lab_7/SRRXXXXXX-contigs.v3.fa.masked
```


### Copying instructions 2 - you do not have braker files; you need my permission to use this; I am assuming you have completed Lab6

You will copy braker files that I generated in the background from your assemblies

```bash
cd ~/lab_7
cp /projects/class/binf3101_001/braker_results/SRRXXXXXX.gff3 .
cp /projects/class/binf3101_001/braker_results/SRRXXXXXX.gtf .
cp /projects/class/binf3101_001/braker_results/SRRXXXXXX-contigs.v3.fa.masked .
````

&ensp;
&ensp;
&ensp;

## Step 2 - Prepare files for annotation 

We are going to use a web browser called GenomeQC to analyze the quality of our genome annotation. We need to prepare the files before we download them

First, we need to move back into the lab_7 directory

```bash
cd ~/lab_7
```

&ensp;
&ensp;

### Step 2a - Compress the gtf file

We need to compress the gtf file for the analysis. We will do this with gzip

```bash
gzip -k SRRXXXXXX.gtf
# the -k option means keep the original file

ls
#look at files
```

You should now see an additional file called SRRXXXXXX.gtf.gz

&ensp;
&ensp;

### Step 2b - Edit our genome file

The genome file (SRR-contigs.v3.fa) has the contig names in this format:

```bash
>3464 97477 4960737 1433+,...,412+
>3465 39585 1955840 2387-,...,1611+
>3466 69657 3516060 2720+,...,2533-
```

The gtf file has them like this:
```bash
3465_39585_1955840_2387-,...,1611+      AUGUSTUS        stop_codon      11874   11876   .       -       0       transcript_id "file_1_file_1_g2087.t1"; gene_id "file_1_file_1_g2087";
3465_39585_1955840_2387-,...,1611+      AUGUSTUS        CDS     11874   12905   0.76    -       0       transcript_id "file_1_file_1_g2087.t1"; gene_id "file_1_file_1_g2087";
3466_69657_3516060_2720+,...,2533-      AUGUSTUS        transcript      38970   39335   0.29    -       .       g1088.t1
3466_69657_3516060_2720+,...,2533-      AUGUSTUS        start_codon     53135   53137   .       +       0       transcript_id "file_1_file_1_g1093.t1"; gene_id "file_1_file_1_g1093";
```

What is the difference between these two identifiers?

3465 39585 1955840 2387-
3465_39585_1955840_2387-

One has spaces! and the other has _. Spaces are bad form in bioinformatics. So we want to remove them from our genome file! Or they won't match

We will use a very powerful program called sed. If you are curious how sed works to find and replace things in your file see this great tutorial https://catonmat.net/worlds-best-introduction-to-sed

We will use sed to replace " " with "_".

```bash
sed -i "s/ /_/g" SRRXXXXX-contigs.v3.fa.masked
```

You can read this command as "call sed, overwrite our file and replace in line (-i). Substitute (s/) the first item " " for the second item "_" and do it globally (g) in this file."


Take a look at your genome file and see what changes

```bash
head SRR-contigs.v3.fa.masked
```

&ensp;
&ensp;

### Step 2c - Compress our genome file

We also need to compress the genome file

```bash
gzip -k SRR-contigs.v3.fa.masked
```

&ensp;
&ensp;

## Step 3 - Download your files

To upload our files to the web server we need to download the files we need. 

The two files we need are
- SRRXXXXXXXX.gtf.gz
- SRRXXXXXXXX-contigs.v3..fa.masked.gz


### Download on Mac
Using scp. This will download it to your default directory. You can change the ```.``` to a different direcoty. 

```scp username@hpc-student.charlotte.edu:lab_5/SRRXXXXXXXX.gtf.gz .```
```scp username@hpc-student.charlotte.edu:lab_5/SRRXXXXXXXX-contigs.v3.fa.masked.gz .```

### Download on Ubuntu for Windows

```scp username@hpc-student.charlotte.edu:lab_5/SRRXXXXXXXX.gtf.gz /mnt/c/Users/local/laptop/directory```


```scp username@hpc-student.charlotte.edu:lab_5/SRRXXXXXXXX-contigs.v3.fa.masked.gz /mnt/c/Users/local/laptop/directory```

### Download using PSFTP

After connecting move to a safe location on your computer by changing the local directory (LCD)

```bash
lcd C:\users\lyohe1
```

```get lab_5/SRRXXXXXXXX-contigs.v3.fa.gz```
```get lab_5/SRRXXXXXXXX.gtf.gz```

If you get an "Unable to open (file)" error. That means that your _local_ directory is preventing you from downloading the file. You need to change your local directory. 

&ensp;
&ensp;
&ensp;

## Step 4 - Upload files to GenomeQC

Navigate to the genomeQC website here: https://genomeqc.maizegdb.org/

NOTE - if you run into issues, it is likely because the entire class is using the website. Give it a few minutes and try again. 

Then click on **Analyze your genome annotation**

![image](https://github.com/BINF-3101/Lab_5_genome_annotation_part2/assets/47755288/1c364f49-b3f5-4b03-823e-e546e84a34c1)



You will see all the options on the left-hand side. Enter the following items in each of these fields

- Reference genome - select nothing
- Email address - enter your email address
- Name of your genome annotation - I suggest naming it your SRR number
- BUSCO Datasets (select one) - Use **saccharomycetales_odb9 (Fungi)**
- Upload Genome - Upload your SRRXXXXXXXX-contigs.v3.fa.gz
- Upload Structure Annotation file - upload your SRR1234.gtf.gz file
- Upload Transcripts Fasta File - upload nothing

It should look like this

![image](https://github.com/BINF-3101/Lab_5_genome_annotation_part2/assets/47755288/13ece9ac-fe87-4c4a-af9e-b78af572d982)


&ensp;
&ensp;
&ensp;

## Step 5 - Submit your job

Click **Click To Submit Your Job**

Before moving on to looking at your metrics 
**YOU MUST CLICK THE ANNOTATION BUSCO PLOT TAB**

![image](https://github.com/BINF-3101/Lab_5_genome_annotation_part2/assets/47755288/4bedbee2-6ebd-4d91-9a55-6eced6e47461)


This will submit your BUSCO job and you will see confirmation it has been submitted

![image](https://github.com/BINF-3101/Lab_5_genome_annotation_part2/assets/47755288/5b308548-c38b-48e5-96d5-165929a4a961)


We will come back to this at the end

&ensp;
&ensp;
&ensp;

## Step 6 - Record your Annotation Metrics results

You can write down the results in the Annotation Metrics Table or click the "Download annotation metrics table" under the Click to Submit. 

&ensp;

# LQ 1

Report all 10 metrics in a screenshot or copied table. 

&ensp;
&ensp;
&ensp;

## Step 7 - Generate your DNA coding sequences

Our gtf and gff3 files are like a road map, showing us where the genes are in our genome. We need to extract the coding sequences (CDS) and the amino acid sequences so we can analyze them down the road. 

### Step7a - Edit the gtf file to contain only the CDS

In the GTF file you will see that each coding sequence (CDS) has a start_codon, a CDS, an exon, any introns, and the stop_codon. 

To view the GTF file use a command like `head -50 SRRXXXXX.gtf`

We want to analyze _only_ the CDS sequences

To do that we will extract only the lines that contain the CDS

```bash
grep "CDS" SRRXXXXX.gtf > SRRXXXXX.cds.gtf
````

We will use a tool called **bedtools** to do this. 

```bash
#load bedtools
module load bedtools2
bedtools getfasta -s -fi SRRXXXXX-contigs.v3.fa.masked -bed SRRXXXXX.cds.gtf -fo SRRXXXXX.cds.v1.fasta

#look at your files to see what was generated
ls

#take a look at your sequences
head SRRXXXXX.cds.v1.fasta
```
You should see a new file SRRXXXXXX.cds.fasta. This contains the coding sequences for each gene predicted in our genome!

&ensp;

# LQ 2 
How many coding sequences are in your new cds file? _hint - we have done this multiple times with grep_

&ensp;
&ensp;
&ensp;

## Step 8 - Filter your CDS file 

Take a look at your new cds file using ```head```, it should look something like this:

```bash
>3271_18188_862880_324+,...,334-:5515-6385(-)
ATGAGTTTAGATGGGCCTGGAGATGACCTTAAAAAGGTCGCTCAGGATTTATTGACCGGGATACTGTGGACAGAATCTGTTTCGAACGCAAATGA
GACAAGACGACCAGCCTATGATCATATAAATCGATTATTAGCCCATTATACTGGGAATGGCTACGGCAAAACCTCGTTCCGTAGGTTGTCCGCCT
GCGGCGAGTGGTTATTGGCGGGGGTTCAGCTCGACACTGGAGATTTAGAGTGTGACTACTGGGTACCATGGAGCGATTCAATTGAGCGGGCAGAA
ATAATCGAGCTCCGTCAGACGCTTCATACACTACTGGGTCATCCCAATCGTGATCATTCTCTAGGTGACTCGGCCACTCAAAATCCTTATGCATT
CACTGTCGGATCCTCAGCATCTACTCCAAATACTATTCTTAGTGAGGACTCAGAGGTTTTCCCAATCTCCGGCTATGAGAACGATTGTTCGTTCG
GGGATCGGTATGACTACGAACTCAGTGAATGTTAG
>3271_18188_862880_324+,...,334-:5515-6385(-)
ATGAGTTTAGATGGGCCTGGAGATGACCTTAAAAAGGTCGCTCAGGATTTATTGACCGGGATACTGTGGACAGAATCTGTTTCGAACGCAAATGA
GACAAGACGACCAGCCTATGATCATATAAATCGATTATTAGCCCATTATACTGGGAATGGCTACGGCAAAACCTCGTTCCGTAGGTTGTCCGCCT
GCGGCGAGTGGTTATTGGCGGGGGTTCAGCTCGACACTGGAGATTTAGAGTGTGACTACTGGGTACCATGGAGCGATTCAATTGAGCGGGCAGAA
ATAATCGAGCTCCGTCAGACGCTTCATACACTACTGGGTCATCCCAATCGTGATCATTCTCTAGGTGACTCGGCCACTCAAAATCCTTATGCATT
CACTGTCGGATCCTCAGCATCTACTCCAAATACTATTCTTAGTGAGGACTCAGAGGTTTTCCCAATCTCCGGCTATGAGAACGATTGTTCGTTCG
GGGATCGGTATGACTACGAACTCAGTGAATGTTAG
```
You'll notice we have "coding sequences" that are just ATG (start codon) or TAG/TAA/TGA (stop codon)

Emboss thinks that the start and stop codons are transcripts. We can easily remove them using the length filter we used before. 

```bash
#copy the script into this directory
cp /projects/class/binf3101_001/filter_by_length.py .

#load our necessary modules
ml anaconda3

python filter_by_length.py SRRXXXXXXX.cds.v1.fasta SRRXXXXXXX.cds.fasta 30 100000000

```

We are filtering coding sequences between 30 and 100,000,000 base pairs. The upper limit is arbitrary because we want to keep all our long sequences. The lower limit (30 base pairs or 10 amino acids) because the smallest known protein is 11 amino acids (or 33 base pairs)

&ensp;

# LQ 3
How many coding sequences were removed during the filtering step? You will need to do some subtraction.

&ensp;
&ensp;
&ensp;

## Step 9 - Generate your protein-coding sequences

To translate our DNA into Protein we use the codon table. This will predict the protein-coding sequences for all of our genes. 

In class we discussed that the genetic code (the codon table) is _nearly_ universal. In the budding yeast, there are **three** different genetic codes in the nuclear DNA. 

&ensp;
&ensp;

## Step 9a - Determine your genetic code

Go to this table and determine which Order your species belongs to 

https://docs.google.com/spreadsheets/d/1AIT-GHvhBJoh7BDy-Ee5fLxVRKuSfp8hqoU4H4CEA9s/edit?usp=sharing


Based on the order your species is in select the correct genetic code
|Order	|Codon Table|
|------|------|
|Alaninales|	12|
|Dipodascales| 1|
|Lipomycetales| 1|
|Phaffomycetales|	1|
|Pichiales|	1|
|Saccharomycetales|	1|
|Saccharomycodales|	1|
|Serinales|	12|
|Sporopachydermiales|	1|


A description of each code can be found here: https://www.ncbi.nlm.nih.gov/Taxonomy/Utils/wprintgc.cgi

&ensp;

# LQ 4
What is the **name** of your genetic code listed on the NCBI website?

&ensp;
&ensp;

## Step 9b - Translate your coding sequences

To translate our DNA into amino acid sequences we will use emboss again. We will use the ```transeq``` 

The number you should fill in is the number of your translation table

```bash
ml emboss
#load emboss

transeq SRR1235.cds.fasta SRRXXXXXXXX.prot.fasta -table FILLIN

#look at your files
ls
```

This will translate our DNA into Protein using the translation table you found above. 

&ensp;
&ensp;
&ensp;

## Step 10 - BUSCO results

After an hour or so you should have received two emails about your BUSCO results

![image](https://github.com/BINF-3101/Lab_5_genome_annotation_part2/assets/47755288/d643efc9-12dd-498b-b4e8-ff97c42a8926)

One will have an attached PNG describing the results and the other will have an interactive HTML document. 

&ensp;
&ensp;

### Step 10a - Download and open HTML file

Download and open your HTML file. When you **scroll over the plot** you should see the percentage next to each bar. 

![image](https://github.com/BINF-3101/Lab_5_genome_annotation_part2/assets/47755288/747aa2f3-1521-4957-ae5b-d0e2fc618f71)

This will tell us how "complete" our genome annotation is. 

**Make sure to save the png and HTML somewhere for later**

&ensp;

# LQ 5
What percent of BUSCO annotations were 
- Missing (M)
- Fragmented (F)
- Duplicated (D)
- Complete & Single Copy (C)








