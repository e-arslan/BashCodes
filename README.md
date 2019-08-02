# Creating bam files from tagAligns with the same name

```bash
for file in *.tagAlign; do   bedtools bedtobam -i "${file%%.*}".tagAlign -g hg19chrom.sizes > "${file%%.*}".bam; done
```

# Sorting bam files

```bash
for file in *.bam; do samtools sort "${file%%.*}".bam -o "${file%%.*}".sorted.bam; done
```


# Indexing bam files

```bash
for bam in $(ls *.bam); do samtools index $bam; done
```


# Creating Subsampling Ratios
```bash
for file in *.bam; do samtools idxstats "${file%%.*}".bam | cut -f3 | awk 'BEGIN {total=0} {total += $1} END {print 15000000/total}'; done > subsampling_ratio.txt
```

for file in *.bam; do cat "${file%%.*}"; done


samtools view -s 0.757599 -b E119-H3K27ac.bam -o E119-H3K27ac.subsampled.bam
samtools view -s 0.957529 -b E119-H3K27me3.bam -o E119-H3K27me3.subsampled.bam
samtools view -s 0.5 -b E119-H3K4me1.bam -o E119-H3K4me1.subsampled.bam
samtools view -s 0.563724 -b E119-H3K4me3.bam -o E119-H3K4me3.subsampled.bam
samtools view -s 0.86532 -b E119-H3K9me3.bam -o E119-H3K9me3.subsampled.bam
samtools view -s 0.838561 -b E119-Input.bam -o E119-Input.subsampled.bam



samtools idxstats E119-H3K27ac.subsampled.bam | cut -f3 | awk 'BEGIN {total=0} {total += $1} END {print 15000000/total}'