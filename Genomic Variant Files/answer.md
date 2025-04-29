Q1: How many positions are found in the region 1:1105411-44137860 in the VCF file?
#extract specified region and create new file to store variants of the region 
tabix CEU.exon.2010_03.genotypes.vcf.gz 1:1105411-44137860 > region.vcf
#count number of variant positions 
grep -v '^#' region_with_header.vcf | wc -l
A: 69 

Q2: How many samples are included in the VCF file?
#count number of samples with addition of | wc -l 
bcftools query -l CEU.exon.2010_03.genotypes.vcf.gz | wc -l
A:90 

Q3: How many positions are there total in the VCF file?
#print every variant's position in single line and count them 
bcftools query -f '%POS\n' CEU.exon.2010_03.genotypes.vcf.gz | wc -l
A: 3489

Q4: How many positions are there with AC=1? Note that you cannot simply count lines since the output of bcftools filter includes the VCF header lines. You will need to use bcftools query to get this number.
#using query instead of filter to avoid header lines 
#-1 to select variants with AC=1 and -f to print only the position 
bcftools query -i 'AC=1' -f '%POS\n' CEU.exon.2010_03.genotypes.vcf.gz | wc -l
A: 1075 

Q5: What is the ratio of transitions to transversions (ts/tv) in this file?
#create stats about the file and store into new file 
bcftools stats CEU.exon.2010_03.genotypes.vcf.gz > vcf_stats.txt
#find the line that contains tstv ratio 
grep ^TSTV vcf_stats.txt
#output 
TSTV	0	2708	781	3.47	2708	781	3.47
A: 3.47 
