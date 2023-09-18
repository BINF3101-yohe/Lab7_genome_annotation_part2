# Lab_5_genome_annotation_part2

We have now annotated our genomes. This means we have identified locations in the genome where we believe genes are located. 
This week in the lab we will be analyzing our genome annotations. 

We will address questions such as: How many genes did we annotate? How well did our annotation capture the genes we expect to see? 


## Step 1 - Lab setup

We will be using several files from lab 4 this week. 

### Step 1a
First, create a new folder in your home directory for this week's lab. 

```bash
#go to home directory
cd

#make a new directory
mkdir lab_5
```

### Step 1b
Now we need to copy several files from our braker results into the new directory. 
First, find your way into the braker directory 

```bash
#move into the braker folder
cd lab_4/braker/
#look at the files
ls
```

Our results are stored in two files
**braker.gtf**
**braker.gff3**

These are _not named well_ so we are going to copy _and_ rename them at the same time. You will need your SRR number to replace the SRR number below. 

```bash
cp braker.gtf ~/lab_5/SRR**6475892**.gtf
cp braker.gff3 ~/lab_5/SRR**6475892**.gff3
```

**Note** the ~ symbol means starting from your home directory. 

### Step 1c

We also need a copy of our genome. We are going to save it as a new version because we are going to edit it slightly

```bash
#go back to our lab_4 directory
cd ~/lab_4

cp SRR6475892-contigs.v2.fa ~/lab_5/SRR6475892-contigs.v3.fa

```

### Step 2 - Prepare files for annotation 

We are going to use a web browser called GenomeQC to analyze the quality of our genome annotation. We need to prepare the files before we download them

### Step 2a - Compress the gtf file

We need to compress the gtf file for the analysis. We will do this with gzip

```bash
gzip -k SRR**6475892**.gtf
# the -k option means keep the original file

ls
#look at files
```

You should now see an additional file called SRR6475892.gtf.gz


### Step 2b - Edit our genome file

The genome file (SRR-contigs.v2.fa) has the contig names in this format:

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
sed -i "s/ /_/g" SRR-contigs.v2.fa
```

You can read this command as "call sed, overwrite our file and replace in line (-i). Substitute (s/) the first item " " for the second item "_" and do it globally (g) in this file."


Take a look at your genome file and see what changes

```bash
head SRR-contigs.v2.fa
```



### Step 2c - Compress our genome file

We also need to compress the genome file

```bash
gzip -k


To do this you will need to ****







GTF analysis 
how many genes
average gene length

GFF3 to CDS
Find translation table
CDS to Protein
