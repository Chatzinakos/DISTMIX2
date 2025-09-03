# DISTMIX2

# Download DISTMIX2
The current release (Version 0.3.0) of DISTMIX2 is for a Linux user. The pre-compiled executables for other operating systems (e.g.,  Windows, MacOS) will be available soon. The latest source codes of DISTMIX2 are available upon request (christos.chatzinakos@downstate.edu). 

|**Direct download link**|**Versiom**|**Release Date**|**Note**|
|:---|:---|:---|:---|
|[DISTMIX2 for a Linux user](https://www.dropbox.com/scl/fi/ixvh93so5rvmy06ljlxlj/distmix2_v0.0.2?rlkey=0zlxuxxqksmn4k1to4l651nt1&st=81lwlz3p&dl=0|v0.3.0|09/03/2025|Includes info file for several scenarios|

# Download Reference Panels
|**Direct download link**|**Number of Samples**|**Number of populations**|**NCBI build**|**Release Date**|**Note**|
|:---|:---|:---|:---|:---|:---|
|[33Kg](https://www.dropbox.com/sh/sw03zojcwzpdeed/AACucpdpPMlbtVCp9zrwXLATa?dl=0)|32953|29|build 37 (hg19)|Sep. 3 2025|Includes chr1-chr22|

Here are the 33Kg population abbreviations used by DISTMIX2. AFR is an abbreviation for African; AMR for admixed American; ASN for East Asian; EUR for European and SAS for South Asian. 

|**Population Abbreviation**|**Number of Subjects**|**Super Population**|**Population Description**|
|:---|:---|:---|:---|
|ACB|164|AFR|African Caribbeans in Barbados|
|ASW|162|AFR|African Ancestry in Southwest US|
|BEB|86|SAS|Bengali from Bangladesh|
|CCE|3409|ASN|China Central East|
|CCS|2613|ASN|China Central South|
|CDX|95|ASN|Chinese Dai in Xishuangbanna, China|
|CEU|6360|EUR|Utah residents with Northern and Western European ancestry| 
|CLM|98|AMR|Colombians from Medellin, Colombia|
|CNE|2330|ASN|China North East|
|CSE|2020|ASN|China South-East|
|ESN|140|AFR|Esan in Nigeria|
|FIN|3529|EUR|Finnish in Finland|
|GBR|2020|EUR|British in England and Scotland|
|GIH|110|SAS|Gujarati Indian from Houston, Texas|
|GWD|113|AFR|Gambian in Western Divisions in the Gambia|
|IBS|1309|EUR|Iberian Population in Spain|
|ITU|95|SAS|Indian Telugu from the UK|
|JPT|107|ASN|Japanese in Tokyo, Japan|
|KHV|226|ASN|Kinh in Ho Chi Minh City, Vietnam|
|LWK|99|AFR|Luhya in Webuye, Kenya|
|MSL|87|AFR|Mende in Sierra Leone|
|MXL|187|AMR|Mexican Ancestry from Los Angeles, USA|
|ORK|5772|EUR|Orkney Island study|
|PEL|110|AMR|Peruvians from Lima, Peru|
|PJL|121|SAS|Punjabi from Lahore, Pakistan|
|PUR|138|AMR|Puerto Rican in Puerto Rico|
|STU|110|SAS|Sri Lankan Tamil from the UK|
|TSI|1291|EUR|Toscani in Italia|
|YRI|52|AFR|Yoruba in Ibadan, Nigeria|

# DISTMIX2 Input File Format
###DISTMIX2 takes as input a plain text file with rows and columns denoting SNPs and variables, respectively. The first line of the input file should be column names/headers. Data entries on each line should be separated by white space. 
The file should contain six columns, and optional another one: 1) rsid (SNP ID), 2) chr (chromosome number), 3) bp (base pair position), 4) a1 (reference allele), 5) a2 (alternative allele), 6) z (normally distributed GWAS/meta-analysis summary statistic, i.e. two-tailed Z-score) and 7) AF (Allele Frequency information, Optional)). DISTMIX2 does not require the input data to be sorted in ascending order by chromosome number and base pair position or SNP ID. Here is a sample DISTMIX2 input file.

```
rsid        chr  bp         a1  a2  z          AF    

rs1000109   9    117908733  C   T  -0.714464   0.89  

rs10001109  4    44904653   C   T   0.721919   0.40   

rs1000112   22   28273339   C   G  -0.583666   0.24 

rs10001127  4    130547376  T   C   0.069329   0.09    

rs1000113   5    150240076  T   C  -1.447288   0.33  
```
**Notification:** All SNPs in your input file are measured with Info=1.

**Note:** If the input summary data comes a different genome assembly (e.g. NCBI build 36 (hg18)) than NCBI build 37 (hg19), it needs to be converted by the user to NCBI build 37 (hg19) using a software, called [liftover](https://genome.ucsc.edu/cgi-bin/hgLiftOver), from UCSC.

**When study allele frequency information is not available**

Genome-wide association studies/meta-analyses typically do not provide study allele frequency information due to privacy issues. However, information about ethnic proportion of the study samples is usually available from the publications. By using the prior ethnic information, users can run DISTMIX2 for association summary data lacking allele frequency information. In this case, DISTMIX2 does not require study allele frequency information (the column ‘AF’) in the input file, but optionally DISTMIX2 needs one more input text file (super population weight file) specifying ethnic proportion of the study samples.

Here is a sample DISTMIX2 Super Population weight file, which is **Strongly recommended if your GWAS has not have AF column.** 


```
Super_Pop	 Wgt   
EUR       	0.4
ASN	        0.6
```
The first line of the super-population weight file should be column names/headers. The file should contain two columns: 1) SuperPop (Super population abbreviation) and 2) Wgt (Super population proportion). The columns of data should be separated by white space. The weight file name should be specified with the ‘--populationWeight’ or ‘-w’ option. The weights must be possitives and the summation of all weights must be 1. Practically you put the Super population proportion of the GWAS study.

# DISTMIX2 output file format for genes analysis

The DISTMIX2 output file, has nine columns: 1) rsid (SNP ID), 2) chr (chromosome number), 3) bp (base pair position), 4) a1 (reference allele), 5) a2 (alternative allele), 6) Zval (Distimix2 estimated z-value), 7) Pval (Distmix2 estimated p-value), 8) Info (DISTMIX2 estimated information), 9) W_AF1 (DISTMIX2 estimation of the weighted Allele Frequency weighted information) and IMPUTED_STATUS (Yes if the SNP was imputed; No if it was present in the original GWAS data). The first line of the output file is column names/headers.  Here is a sample DISTMIX2 output file, with 3 SNPS:

```
rsid        chr  bp   a1  a2     Zval     Pval    Info     W_AF1   IMPUTED_STATUS 

rs2462492   1   54676  T   C  -0.07594   0.9394   0.0375   0.4241   YES

rs2462492   1   52326  C   T   1.00863   0.31315  0.0242   0.0091   NO

rs74447903  1   57033  C   T  -1.04016   0.29826  0.8798   0.2345   YES

```

# Programm Options

|**Option**|**Short Flag**|**Parameter**|**Default**|**Description**|
|:---|:---|:---|:---|:---|
|--version|-v|none|none|Prints version information.|
|--help|-h|none|none|Outputs a full description of all DISTMIX2 options.|
|--reference|-r|filename|none|The filename of the reference population data.|
|--referenceIndex|-i|filename|none|The filename of the reference population index data.|
|--impute_out|-x|filename|out.impute.distmix2.txt|The filename of DISTMIX2 output.|
|--populationWeight|-w|filename|none|The filename of the super population weight data.|
|--windowSize|-n|decimal|0.5|The size of the DISTMIX2 prediction window (Mb).|
|--wingSize|-m|decimal|0.25|The size of the wing padded on the left and right of the DISTMIX2 prediction window (Mb).|
|--measured_snps|-j|integer|500|Number of measured Snps.|
|--chromosome|-c|none|none|The chromosome number (or the chromosome number and arm, e.g., -c 1q)|
