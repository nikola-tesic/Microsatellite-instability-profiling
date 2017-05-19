# Microsatellite-instability-profiling
This workflow enables profiling microsatellite instability using WXS or WGS data. It uses BWA-MEM (https://github.com/lh3/bwa) for read alignment, lobSTR (https://github.com/mgymrek/lobstr-code) for calling microsatellite variants, and MSIsensor (https://github.com/ding-lab/msisensor) for quantifying microsatellite instability. It also usees FastQC (https://github.com/csf-ngs/fastqc) to check fastq quality. It can be executed in two modes:

1. Paired tumor normal sample mode - that enables differentiating somatic changes from germline variants at microsatellite loci; this mode leverages MSIsensor to obtain a measure of microsatellite instability [1]. VCFs containing results of lobSTR allelotyping [2] at a set of reference microsatellite loci, for both the normal and tumor samples is also outputted.

2. Tumor sample only mode - in which case only the output of lobSTR allelotyping will be obtained.

Required inputs into the workflow include:

*   Reference genome assembly in FASTA format, or a TAR bundle produced by BWA-MEM Index, which will be used in read alignment. If a TAR bundle is provided, the reference will not be (re)indexed, resulting in shorter execution times. 
*   lobSTR reference and index bundle, available for UCSC HG19 and GRCh37 genome builds. This is a TAR bundle available on the Seven Bridges platform, prebuilt and distributed with the lobSTR package. The chosen bundle must match the reference genome assembly used for read alignment.
*   Stutter and step PCR noise model files, appropriate for the particular library prep protocol used for samples under analysis. These files can be obtained by running the lobSTR Train Model tool.
*   Tumor sample reads in FASTQ format (gzipped input accepted). If providing paired-end reads in separate files, make sure to set the appropriate metadata fields accordingly.

If performing paired tumor normal analysis, besides the normal sample reads, it is necessary to provide a list of microsatellites that MSIsensor will use. A prebuilt list for HG19 is available on the platform, and lists for other genome builds can be generated using MSIsensor scan tool. 

Note that it is necessary to explicitly list chromosome names that should be treated as haploid by lobSTR Allelotype. For example, if processing reads from a male sample, aligned to HG19, 'chrX,chrY' should be inputted in the appropriate field under the App Settings tab.

The output VCFs will contain a list of all microsatellite loci that were allelotyped by lobSTR, with the ALT allele field left blank ('.') if no microsatellite variations were reported.

[1] Niu et al. (2014) Bioinformatics 30(7): 1015-1016.
[2] Gymrek et al. (2012) Genome Res. 22(6): 1154-1162.
