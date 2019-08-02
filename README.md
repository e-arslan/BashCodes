# Creating bam files from tagAligns with the same name

```bash
for file in *.tagAlign; do   bedtools bedtobam -i "${file%%.*}".tagAlign -g hg19chrom.sizes > "${file%%.*}".bam; done
```