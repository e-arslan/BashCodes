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
for bam in $(ls *.sorted.bam); do samtools index $bam; done
```

# Total Number of Reads
```bash
for file in *.sorted.bam; do samtools idxstats "${file%%.*}".sorted.bam | cut -f3 | awk 'BEGIN {total=0} {total += $1} END {print total}'; done 
```

# Removing Duplicates & No reads
sambamba markdup -r -t 3 -p E119-H3K27me3.sorted.bam E119-H3K27me3.f1.sorted.bam
samtools view -F 0x04 -b E119-H3K27me3.f1.sorted.bam > E119-H3K27me3.f2.sorted.bam
```bash
for bam in $(ls *.sorted.bam); do samtools index $bam; done
```

# Creating Subsampling Ratios and Sorted Bam Files Names
```bash
for file in *.sorted.bam; do samtools idxstats "${file%%.*}".sorted.bam | cut -f3 | awk 'BEGIN {total=0} {total += $1} END {print 15000000/total}'; done > subsampling_ratio.txt
for file in *.sorted.bam; do echo "${file%%.*}"; done > sorted_list.txt
```

# Subsampling Each sorted.bam files
```bash
nlines=$(wc -l subsampling_ratio.txt| awk '{ print $1 }')
for i in $(seq 1 $nlines)
do 
	perc=$(tail -n+$i subsampling_ratio.txt| head -1)
	sample=$(tail -n+$i sorted_list.txt| head -1)
	samtools view -s $perc -b $sample.sorted.bam -o $sample.subsampled.bam
done
```

# Indexing subsampled.bam files

```bash
for bam in $(ls *.subsampled.bam); do samtools index $bam; done
```


#Check Total Number of Reads of Subsampled Versions
```bash
for file in *.subsampled.bam; do samtools idxstats "${file%%.*}".subsampled.bam | cut -f3 | awk 'BEGIN {total=0} {total += $1} END {print total}'; done 
```


tail -n+1 subsampling_ratio.txt| head -1

for file in *.sorted.bam; do {print "${file%%.*}".sorted.bam}; done



for file in *.bam; do cat "${file%%.*}"; done


samtools view -s 0.757599 -b E119-H3K27ac.sorted.bam -o E119-H3K27ac.subsampled.bam
samtools view -s 0.957529 -b E119-H3K27me3.bam -o E119-H3K27me3.subsampled.bam
samtools view -s 0.5 -b E119-H3K4me1.bam -o E119-H3K4me1.subsampled.bam
samtools view -s 0.563724 -b E119-H3K4me3.bam -o E119-H3K4me3.subsampled.bam
samtools view -s 0.86532 -b E119-H3K9me3.bam -o E119-H3K9me3.subsampled.bam
samtools view -s 0.838561 -b E119-Input.bam -o E119-Input.subsampled.bam

sambamba view -s 1.3 -t 10 -f bam  --subsampling-seed=34223  E119-H3K27me3.sorted.bam -o E119-H3K27me3.subsampled.bam

sambamba view -f bam -t 10 --subsampling-seed=3 -s 0.6 E119-H3K27me3.f2.sorted.bam -o subsample.bam



wc -l E119-H3K27me3.sorted.bam| awk '{ print $1 }'
wc -l E119-H3K27me3.subsampled.bam| awk '{ print $1 }'


wc -l subsampled.bam| awk '{ print $1 }'
samtools idxstats E119-H3K27me3.bam | cut -f3 | awk 'BEGIN {total=0} {total += $1} END {print total}'
samtools idxstats subsampled.bam | cut -f3 | awk 'BEGIN {total=0} {total += $1} END {print total}'

samtools flagstat E119-H3K27me3.sorted.bam 
samtools flagstat BRCA-B001_A-downsample.sorted.bam 



sambamba view -s 0.7 -t 3 -f bam  --subsampling-seed=34223  E119-H3K27me3.sorted.bam -o subsampled1.bam
sambamba view -s 0.7 -t 3 -f bam  --subsampling-seed=34223  E119-H3K27me3.f1.sorted.bam -o subsampled2.bam
sambamba view -s 0.7 -t 3 -f bam  --subsampling-seed=34223  E119-H3K27me3.f2.sorted.bam -o subsampled3.bam


samtools idxstats subsampled1.bam | cut -f3 | awk 'BEGIN {total=0} {total += $1} END {print total}'
samtools idxstats subsampled2.bam | cut -f3 | awk 'BEGIN {total=0} {total += $1} END {print total}'
samtools idxstats subsampled3.bam | cut -f3 | awk 'BEGIN {total=0} {total += $1} END {print total}'


bedtools bedtobam -i E119-H3K27ac.tagAlign -g hg19chrom.sizes > E119-H3K27ac.bam
bedtools bedtobam -i E119-H3K27ac.tagAlign -g hg19chrom.sizes | samtools sort - E119-H3K27ac
