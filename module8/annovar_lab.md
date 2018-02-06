---
layout: tutorial_page
permalink: /bicg_2018_lab_8_annovar
title: BiCG Module 8
header1: Bioinformatics for Cancer Genomics 2018
header2: Lab Module 8 - Annovar
image: /site_images/CBW_cancerDNA_icon-16.jpg
home: https://bioinformaticsdotca.github.io/bicg_2018
---

# Lab Module 8 - Annovar


This tutorial will take you through using [Annovar](http://www.openbioinformatics.org/annovar/annovar_download_form.php).  Instructions to install Annovar on your own computer can be found [here](https://raw.githubusercontent.com/bioinformaticsdotca/BiCG_2017/master/module8/Reimand_CBW_may2017_lab8A_Annovar_installation_optional.txt).

## Login

```
ssh -i CBWCG.pem ubuntu@cbw##.dyndns.info
```

where ## is your student number.

## Create a New Directory 

In your workspace, make a working directory for Module8

```
cd /home/ubuntu/workspace
mkdir Module8
```

## Copy the Exercise File to Workspace

```
curl https://raw.githubusercontent.com/bioinformaticsdotca/BiCG_2017/master/module8/Passed.somatic.snvs.vcf > Passed.somatic.snvs.vcf
```

## Convert the VCF File for Annovar

```
convert2annovar.pl --includeinfo -format vcf4old Passed.somatic.snvs.vcf > Passed.somatic.snvs.vcf.annovar.in.txt
```

Standard outputs you'll get:

```
# NOTICE: Read 121 lines and wrote 0 different variants at 8 genomic positions (8 SNPs and 0 indels)
# NOTICE: Among 8 different variants at 8 positions, 0 are heterozygotes, 0 are homozygotes
# NOTICE: Among 8 SNPs, 1 are transitions, 7 are transversions (ratio=0.14)
```

## Create Annovar Output

```
table_annovar.pl --buildver hg19 Passed.somatic.snvs.vcf.annovar.in.txt /usr/local/annovar/humandb/ -protocol refGene,cytoBand,genomicSuperDups,esp6500siv2_all,1000g2015aug_all,1000g2015aug_eur,exac03,avsnp147,dbnsfp30a -operation g,r,r,f,f,f,f,f,f --nastring NA --outfile passed.somatic.snvs.vcf.annovar.out.txt
```


*Notes:*

* --buildver hg19 specifies the human genome reference build  
* Passed.somatic.snvs.vcf.annovar.in.txt is the input file
* /media/cbwdata/software/annovar/annovar/humandb/ is the Annovar database path

* --protocol refGene,ljb26_all,1000g2014oct_all,caddgt10,cg69,clinvar_20150330,cosmic70,esp6500siv2_all,exac02,snp138,genomicSuperDups,phastConsElements46way is the list of Annovar annotation modules to be executed, corresponding to specific databases
* --operation g,f,f,f,f,f,f,f,f,f,r,r is the type of operation to be executed by Annovar annotation modules where g = gene (only for the gene database), f = filter (exact match by coordinates, ref, alt), r = regional (coordinate overlap); --protocol and --operation need the same number of comma-separated items
* --nastring NA is the encoding of NA values, "NA" is good for R post-processing, use "." for VCF output
* --outfile passed.somatic.snvs.vcf.annovar.out.txt is the output file name prefix

The output file looks like this:

```
# Passed.somatic.snvs.vcf.annovar.out.txt.hg19_multianno.txt

# std outputs you'll get:
# -----------------------------------------------------------------
# NOTICE: Processing operation=g protocol=refGene

# NOTICE: Running with system command <annotate_variation.pl -geneanno -buildver hg19 -dbtype refGene -outfile passed.somatic.snvs.vcf.annovar.out.txt.refGene -exonsort passed.somatic.snvs.vcf.annovar.in.txt /usr/local/annovar/humandb/>
# NOTICE: Reading gene annotation from /usr/local/annovar/humandb/hg19_refGene.txt ... Done with 51039 transcripts (including 11569 without coding sequence annotation) for 26311 unique genes
# NOTICE: Reading FASTA sequences from /usr/local/annovar/humandb/hg19_refGeneMrna.fa ... Done with 21 sequences
# WARNING: A total of 345 sequences will be ignored due to lack of correct ORF annotation
# NOTICE: Finished gene-based annotation on 8 genetic variants in passed.somatic.snvs.vcf.annovar.in.txt
# NOTICE: Output files were written to passed.somatic.snvs.vcf.annovar.out.txt.refGene.variant_function, passed.somatic.snvs.vcf.annovar.out.txt.refGene.exonic_variant_function
# -----------------------------------------------------------------
# NOTICE: Processing operation=f protocol=ljb26_all
# NOTICE: Finished reading 25 column headers for '-dbtype ljb26_all'

# NOTICE: Running system command <annotate_variation.pl -filter -dbtype ljb26_all -buildver hg19 -outfile passed.somatic.snvs.vcf.annovar.out.txt passed.somatic.snvs.vcf.annovar.in.txt /usr/local/annovar/humandb/ -otherinfo>
# NOTICE: the --dbtype ljb26_all is assumed to be in generic ANNOVAR database format
# NOTICE: Variants matching filtering criteria are written to passed.somatic.snvs.vcf.annovar.out.txt.hg19_ljb26_all_dropped, other variants are written to passed.somatic.snvs.vcf.annovar.out.txt.hg19_ljb26_all_filtered
# NOTICE: Processing next batch with 8 unique variants in 8 input lines
# NOTICE: Database index loaded. Total number of bins is 557362 and the number of bins to be scanned is 7
# NOTICE: Scanning filter database /usr/local/annovar/humandb/hg19_ljb26_all.txt...Done
# -----------------------------------------------------------------
# NOTICE: Processing operation=f protocol=1000g2014oct_all

# NOTICE: Running system command <annotate_variation.pl -filter -dbtype 1000g2014oct_all -buildver hg19 -outfile passed.somatic.snvs.vcf.annovar.out.txt passed.somatic.snvs.vcf.annovar.in.txt /usr/local/annovar/humandb/>
# NOTICE: Variants matching filtering criteria are written to passed.somatic.snvs.vcf.annovar.out.txt.hg19_ALL.sites.2014_10_dropped, other variants are written to passed.somatic.snvs.vcf.annovar.out.txt.hg19_ALL.sites.2014_10_filtered
# NOTICE: Processing next batch with 8 unique variants in 8 input lines
# NOTICE: Database index loaded. Total number of bins is 2824642 and the number of bins to be scanned is 6
# NOTICE: Scanning filter database /usr/local/annovar/humandb/hg19_ALL.sites.2014_10.txt...Done
# -----------------------------------------------------------------
# NOTICE: Processing operation=f protocol=caddgt10

# NOTICE: Running system command <annotate_variation.pl -filter -dbtype caddgt10 -buildver hg19 -outfile passed.somatic.snvs.vcf.annovar.out.txt passed.somatic.snvs.vcf.annovar.in.txt /usr/local/annovar/humandb/>
# NOTICE: the --dbtype caddgt10 is assumed to be in generic ANNOVAR database format
# NOTICE: Variants matching filtering criteria are written to passed.somatic.snvs.vcf.annovar.out.txt.hg19_caddgt10_dropped, other variants are written to passed.somatic.snvs.vcf.annovar.out.txt.hg19_caddgt10_filtered
# NOTICE: Processing next batch with 8 unique variants in 8 input lines
# NOTICE: Database index loaded. Total number of bins is 2625942 and the number of bins to be scanned is 6
# NOTICE: Scanning filter database /usr/local/annovar/humandb/hg19_caddgt10.txt...Done
# -----------------------------------------------------------------
# NOTICE: Processing operation=f protocol=cg69

# NOTICE: Running system command <annotate_variation.pl -filter -dbtype cg69 -buildver hg19 -outfile passed.somatic.snvs.vcf.annovar.out.txt passed.somatic.snvs.vcf.annovar.in.txt /usr/local/annovar/humandb/>
# NOTICE: the --dbtype cg69 is assumed to be in generic ANNOVAR database format
# NOTICE: Variants matching filtering criteria are written to passed.somatic.snvs.vcf.annovar.out.txt.hg19_cg69_dropped, other variants are written to passed.somatic.snvs.vcf.annovar.out.txt.hg19_cg69_filtered
# NOTICE: Processing next batch with 8 unique variants in 8 input lines
# NOTICE: Database index loaded. Total number of bins is 2789339 and the number of bins to be scanned is 6
# NOTICE: Scanning filter database /usr/local/annovar/humandb/hg19_cg69.txt...Done
# -----------------------------------------------------------------
# NOTICE: Processing operation=f protocol=clinvar_20140929

# NOTICE: Running system command <annotate_variation.pl -filter -dbtype clinvar_20140929 -buildver hg19 -outfile passed.somatic.snvs.vcf.annovar.out.txt passed.somatic.snvs.vcf.annovar.in.txt /usr/local/annovar/humandb/>
# NOTICE: the --dbtype clinvar_20140929 is assumed to be in generic ANNOVAR database format
# NOTICE: Variants matching filtering criteria are written to passed.somatic.snvs.vcf.annovar.out.txt.hg19_clinvar_20140929_dropped, other variants are written to passed.somatic.snvs.vcf.annovar.out.txt.hg19_clinvar_20140929_filtered
# NOTICE: Processing next batch with 8 unique variants in 8 input lines
# NOTICE: Database index loaded. Total number of bins is 44738 and the number of bins to be scanned is 1
# NOTICE: Scanning filter database /usr/local/annovar/humandb/hg19_clinvar_20140929.txt...Done
# -----------------------------------------------------------------
# NOTICE: Processing operation=f protocol=cosmic70

# NOTICE: Running system command <annotate_variation.pl -filter -dbtype cosmic70 -buildver hg19 -outfile passed.somatic.snvs.vcf.annovar.out.txt passed.somatic.snvs.vcf.annovar.in.txt /usr/local/annovar/humandb/>
# NOTICE: the --dbtype cosmic70 is assumed to be in generic ANNOVAR database format
# NOTICE: Variants matching filtering criteria are written to passed.somatic.snvs.vcf.annovar.out.txt.hg19_cosmic70_dropped, other variants are written to passed.somatic.snvs.vcf.annovar.out.txt.hg19_cosmic70_filtered
# NOTICE: Processing next batch with 8 unique variants in 8 input lines
# NOTICE: Database index loaded. Total number of bins is 232279 and the number of bins to be scanned is 5
# NOTICE: Scanning filter database /usr/local/annovar/humandb/hg19_cosmic70.txt...Done
# -----------------------------------------------------------------
# NOTICE: Processing operation=f protocol=esp6500siv2_all

# NOTICE: Running system command <annotate_variation.pl -filter -dbtype esp6500siv2_all -buildver hg19 -outfile passed.somatic.snvs.vcf.annovar.out.txt passed.somatic.snvs.vcf.annovar.in.txt /usr/local/annovar/humandb/>
# NOTICE: the --dbtype esp6500siv2_all is assumed to be in generic ANNOVAR database format
# NOTICE: Variants matching filtering criteria are written to passed.somatic.snvs.vcf.annovar.out.txt.hg19_esp6500siv2_all_dropped, other variants are written to passed.somatic.snvs.vcf.annovar.out.txt.hg19_esp6500siv2_all_filtered
# NOTICE: Processing next batch with 8 unique variants in 8 input lines
# NOTICE: Database index loaded. Total number of bins is 594771 and the number of bins to be scanned is 7
# NOTICE: Scanning filter database /usr/local/annovar/humandb/hg19_esp6500siv2_all.txt...Done
# -----------------------------------------------------------------
# NOTICE: Processing operation=f protocol=exac02

# NOTICE: Running system command <annotate_variation.pl -filter -dbtype exac02 -buildver hg19 -outfile passed.somatic.snvs.vcf.annovar.out.txt passed.somatic.snvs.vcf.annovar.in.txt /usr/local/annovar/humandb/>
# NOTICE: the --dbtype exac02 is assumed to be in generic ANNOVAR database format
# NOTICE: Variants matching filtering criteria are written to passed.somatic.snvs.vcf.annovar.out.txt.hg19_exac02_dropped, other variants are written to passed.somatic.snvs.vcf.annovar.out.txt.hg19_exac02_filtered
# NOTICE: Processing next batch with 8 unique variants in 8 input lines
# NOTICE: Database index loaded. Total number of bins is 750585 and the number of bins to be scanned is 7
# NOTICE: Scanning filter database /usr/local/annovar/humandb/hg19_exac02.txt...Done
# -----------------------------------------------------------------
# NOTICE: Processing operation=f protocol=snp138

# NOTICE: Running system command <annotate_variation.pl -filter -dbtype snp138 -buildver hg19 -outfile passed.somatic.snvs.vcf.annovar.out.txt passed.somatic.snvs.vcf.annovar.in.txt /usr/local/annovar/humandb/>
# NOTICE: Variants matching filtering criteria are written to passed.somatic.snvs.vcf.annovar.out.txt.hg19_snp138_dropped, other variants are written to passed.somatic.snvs.vcf.annovar.out.txt.hg19_snp138_filtered
# NOTICE: Processing next batch with 8 unique variants in 8 input lines
# NOTICE: Database index loaded. Total number of bins is 2894320 and the number of bins to be scanned is 6
# NOTICE: Scanning filter database /usr/local/annovar/humandb/hg19_snp138.txt...Done
# -----------------------------------------------------------------
# NOTICE: Processing operation=r protocol=genomicSuperDups

# NOTICE: Running with system command <annotate_variation.pl -regionanno -dbtype genomicSuperDups -buildver hg19 -outfile passed.somatic.snvs.vcf.annovar.out.txt passed.somatic.snvs.vcf.annovar.in.txt /usr/local/annovar/humandb/>
# NOTICE: Reading annotation database /usr/local/annovar/humandb/hg19_genomicSuperDups.txt ... Done with 51599 regions
# NOTICE: Finished region-based annotation on 8 genetic variants in passed.somatic.snvs.vcf.annovar.in.txt
# NOTICE: Output file is written to passed.somatic.snvs.vcf.annovar.out.txt.hg19_genomicSuperDups
# -----------------------------------------------------------------
# NOTICE: Processing operation=r protocol=phastConsElements46wayPlacental

# NOTICE: Running with system command <annotate_variation.pl -regionanno -dbtype phastConsElements46wayPlacental -buildver hg19 -outfile passed.somatic.snvs.vcf.annovar.out.txt passed.somatic.snvs.vcf.annovar.in.txt /usr/local/annovar/humandb/>
# NOTICE: Reading annotation database /usr/local/annovar/humandb/hg19_phastConsElements46wayPlacental.txt ... Done with 3743478 regions
# NOTICE: Finished region-based annotation on 8 genetic variants in passed.somatic.snvs.vcf.annovar.in.txt
# NOTICE: Output file is written to passed.somatic.snvs.vcf.annovar.out.txt.hg19_phastConsElements46wayPlacental
# -----------------------------------------------------------------
# NOTICE: Multianno output file is written to passed.somatic.snvs.vcf.annovar.out.txt.hg19_multianno.txt
```
## Copy the file back to your local machine

In your browser URL, type `http://cbw##.dyndns.info/` where ## is your student number.  Navigate to the Module8 directory and download `Passed.somatic.snvs.vcf.annovar.out.txt.hg19_multianno.txt`.
