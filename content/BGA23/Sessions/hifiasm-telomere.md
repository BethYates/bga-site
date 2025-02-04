---
title: Scalable telomere-to-telomere assembly with hifiasm
aliases:
  - hifiasm-telomere
date_created: 18_30-04-2024
data_modified: 18_30-04-2024
search_name: HiFiasm on T2T assemblies
---
#BGA23/sessions #Assembly #HiFiasm #Tools #Workshop

> [!caution] This session may now be out of date so beware!!!
> This session is part of  [[tags/BGA23]]

## Session Leader(s)

Haoyu Cheng  
Research Fellow in Biomedical Informatics, Harvard Medical School

## Description
[![Open in Gitpod](https://gitpod.io/button/open-in-gitpod.svg)](https://gitpod.io/#https://github.com/BGAcademy23/hifiasm-telomere)
GitHub:: https://github.com/BGAcademy23/hifiasm-telomere

By the end of this session you will be able to:

1. Run hifiasm by yourself
2. Understand different types of assemblies
3. Understand how to improve the assembly quality when the results are not ideal

## Prerequisites

1. Understanding the terms genome assembly, reads, contigs, genome graph
2. Understanding of linux command line basics

> [!warning] !!! warning "Please make sure you MEET THE PREREQUISITES and READ THE DESCRIPTION above"
> 
> You will get the most out of this session if you meet the prerequisites above.
> 
> Please also read the description carefully to see if this session is relevant to you.
> 
> If you don't meet the prerequisites or change your mind based on the description or are no longer available at the session time, please email tol-training at sanger.ac.uk to cancel your slot so that someone else on the waitlist might attend.
> 
## Connecting to gitpod for the session

**[Click here to launch a Gitpod workspace with hifiasm](https://gitpod.io/#https://github.com/BGAcademy23/hifiasm-telomere)**


## Commands for the session

1. Let's assemble only with HiFi reads
   
```
cd /workspace/bin/HiFi

/usr/bin/time -v ../hifiasm -o HG002.chr11.10M -t4 ../HG002.HiFi.chr11.10M.fastq.gz &>>bp.asm.log

awk '/^S/{print ">"$2;print $3}' HG002.chr11.10M.bp.hap1.p_ctg.gfa > HG002.chr11.10M.bp.hap1.fa
awk '/^S/{print ">"$2;print $3}' HG002.chr11.10M.bp.hap2.p_ctg.gfa > HG002.chr11.10M.bp.hap2.fa

```

2. Let's assemble with HiFi reads and trio-binning 

```
cd /workspace/bin/HiFi

/usr/bin/time -v ../hifiasm -o HG002.chr11.10M -t4 -1 HG002.pat.chr11.10M.yak -2 HG002.mat.chr11.10M.yak ../HG002.HiFi.chr11.10M.fastq.gz &>>trio.asm.log

awk '/^S/{print ">"$2;print $3}' HG002.chr11.10M.dip.hap1.p_ctg.gfa > HG002.chr11.10M.trio.hap1.fa
awk '/^S/{print ">"$2;print $3}' HG002.chr11.10M.dip.hap2.p_ctg.gfa > HG002.chr11.10M.trio.hap2.fa

```

3. Let's assemble with HiFi and Hi-C reads

```
cd /workspace/bin/HiFi

/usr/bin/time -v ../hifiasm -o HG002.chr11.10M -t4 --h1 ../HG002.HiC.chr11.10M.R1.fastq.gz --h2 ../HG002.HiC.chr11.10M.R2.fastq.gz ../HG002.HiFi.chr11.10M.fastq.gz &>hic.asm.log

awk '/^S/{print ">"$2;print $3}' HG002.chr11.10M.hic.hap1.p_ctg.gfa > HG002.chr11.10M.hic.hap1.fa
awk '/^S/{print ">"$2;print $3}' HG002.chr11.10M.hic.hap2.p_ctg.gfa > HG002.chr11.10M.hic.hap2.fa

```

4. Evaluate phasing errors:

```
cd /workspace/bin/HiFi

../yak trioeval -t4 HG002.pat.chr11.10M.yak HG002.mat.chr11.10M.yak HG002.chr11.10M.bp.hap1.fa
../yak trioeval -t4 HG002.pat.chr11.10M.yak HG002.mat.chr11.10M.yak HG002.chr11.10M.bp.hap2.fa

../yak trioeval -t4 HG002.pat.chr11.10M.yak HG002.mat.chr11.10M.yak HG002.chr11.10M.trio.hap1.fa
../yak trioeval -t4 HG002.pat.chr11.10M.yak HG002.mat.chr11.10M.yak HG002.chr11.10M.trio.hap2.fa

../yak trioeval -t4 HG002.pat.chr11.10M.yak HG002.mat.chr11.10M.yak HG002.chr11.10M.hic.hap1.fa
../yak trioeval -t4 HG002.pat.chr11.10M.yak HG002.mat.chr11.10M.yak HG002.chr11.10M.hic.hap2.fa

```

5. Let's assemble with HiFi, UL and trio reads

```
cd /workspace/bin/HiFi_UL_trio

/usr/bin/time -v ../hifiasm -o HG002.chr11.10M.UL.trio -t4 -1 HG002.pat.chr11.10M.yak -2 HG002.mat.chr11.10M.yak --ul ../HG002.UL.chr11.10M.fastq.gz ../HG002.HiFi.chr11.10M.fastq.gz &>>trio.asm.log

```

6. Let's assemble with HiFi, UL and Hi-C reads

```
cd /workspace/bin/HiFi_UL_hic

/usr/bin/time -v ../hifiasm -o HG002.chr11.10M.UL.hic -t4 --h1 ../HG002.HiC.chr11.10M.R1.fastq.gz --h2 ../HG002.HiC.chr11.10M.R2.fastq.gz --ul ../HG002.UL.chr11.10M.fastq.gz ../HG002.HiFi.chr11.10M.fastq.gz &>>hic.asm.log

```
