# Genomic Variant Training – Answers

---

### Q1: How many positions are found in the region 1:1105411–44137860 in the VCF file?

```bash
# Extract the specified region and create a new file
tabix CEU.exon.2010_03.genotypes.vcf.gz 1:1105411-44137860 > region.vcf

# Count the number of variant positions (exclude header lines)
grep -v '^#' region.vcf | wc -l

```
A: 69

---

### Q2: How many samples are included in the VCF file?

```bash
#count number of samples with addition of | wc -l 
bcftools query -l CEU.exon.2010_03.genotypes.vcf.gz | wc -l

```
A:90 

---

### Q3: How many positions are there total in the VCF file?

```bash
#print every variant's position in single line and count them 
bcftools query -f '%POS\n' CEU.exon.2010_03.genotypes.vcf.gz | wc -l

```
A: 3489

---

### Q4: How many positions are there with AC=1? Note that you cannot simply count lines since the output of bcftools filter includes the VCF header lines. You will need to use bcftools query to get this number.

```bash
#using query instead of filter to avoid header lines 
#-1 to select variants with AC=1 and -f to print only the position 
bcftools query -i 'AC=1' -f '%POS\n' CEU.exon.2010_03.genotypes.vcf.gz | wc -l

```
A: 1075 

---

### Q5: What is the ratio of transitions to transversions (ts/tv) in this file?

```bash
#create stats about the file and store into new file 
bcftools stats CEU.exon.2010_03.genotypes.vcf.gz > vcf_stats.txt
#find the line that contains tstv ratio 
grep ^TSTV vcf_stats.txt
#output 
TSTV	0	2708	781	3.47	2708	781	3.47

```
A: 3.47 

---

### Rename Chromosomes in VCF 

```bash
#create chr_map.txt then paste provided map file 
nano chr_map.txt
#use bcftools annotate and ---rename-chrs flag
bcftools annotate --rename-chrs chr_map.txt \
  -O z \   #compress file 
  -o CEU.exon.2010_03.genotypes.chr_conv.vcf.gz \
  CEU.exon.2010_03.genotypes.vcf.gz




