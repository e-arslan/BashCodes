# Creating bam files from tagAligns with the same name

```bash
for file in *.tagAlign; do   bedtools bedtobam -i "${file%%.*}".tagAlign -g hg19chrom.sizes > "${file%%.*}".bam; done
```

# Total number of reads of bam files
```bash
for file in *.bam; do samtools idxstats "${file%%.*}".bam | cut -f3 | awk 'BEGIN {total=0} {total += $1} END {print total}'; done
```


for bam in $(ls *.bam); 
samtools sort bam bam;
done