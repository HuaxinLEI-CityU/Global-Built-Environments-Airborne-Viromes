# Running on a PBS system cluster

#!/bin/bash
#PBS -l nodes=5:ppn=8
#PBS -o /disk/rdisk10/huaxinlei2/air_sample2/un_subsample_coverage/coverm_o
#PBS -e /disk/rdisk10/huaxinlei2/air_sample2/un_subsample_coverage/coverm_e
#PBS -N coverm
#PBS -l walltime=96:00:00
#PBS -m bea
#PBS -M huaxinlei2-c@my.cityu.edu.hk

source deactivate
conda deactivate
conda activate bowtie

#samples list
sample='
SL470203
SL470205
SL470206
'

mkdir coverage

#build index
bowtie2-build all_votus.fna vOTU_index

#coverm
for i in $sample
do

bowtie2 -p 100 --very-sensitive -x vOTU_index -1 ${i}_trim_hg_neg_decontm_paired_1.fastq.gz -2 ${i}_trim_hg_neg_decontm_paired_2.fastq.gz | samtools view -b --threads 100 > coverage/${i}.bam
#
coverm filter -b /disk/rdisk10/huaxinlei2/air_sample2/un_subsample_coverage/bam_files2coverage/${i}.bam -o coverage/${i}_filtered.bam --min-read-percent-identity 95 --threads 100
#
samtools sort -@ 32 -o coverage/${i}_filtered_sorted.bam coverage/${i}_filtered.bam
#
coverm genome --bam-files coverage/${i}_filtered_sorted.bam --genome-fasta-directory votu_folder -m rpkm -t 100 --min-covered-fraction 0.7 > coverage/${i}.rpkm
#
#rm bam_files2/${i}.bam
#rm bam_files2/${i}_filtered.bam

done
