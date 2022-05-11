#parse_medaka_variants.py
Add _rough_ base counts for ALT alleles in a vcf file from `medaka` using the vcf file and a bam file as input. Useful for a superficial look at the basic read evidence for an ALT allele, _but_ there are issues with this approach given the many intricacies of variant calling. See usage below for further information on the assumptions/issues.

##Installation
`mamba env create -f environment.yaml`
`conda activate medaka-parse`

Installs `medaka` (and its dependencies), as well as `cyvcf2`. Otherwise uses default python libraries.

##Usage
```
usage: python3 parse_medaka_variants.py [options]

Use the medaka API to get base counts & frequencies for variants

optional arguments:
  -h, --help            show this help message and exit
  -b BAM, --bam BAM     bam file for sample
  -c CHROM, --chrom CHROM
                        CHROM in vcf (based on reference genome). Default: MN908947.3
  -l LENGTH, --length LENGTH
                        Length of reference genome. Default: 29903
  -o OUTFILE, --outfile OUTFILE
                        Output vcf name. Default: /path/to/input_vcf_directory/edited.vcf
  -v VCF, --vcf VCF     vcf file for sample

        This approach is very rough and should not be treated as rock-solid evidence.
        The purpose is to give a rough idea of the underlying number of reads that
        support a variant, as well as the allele frequency (count/depth). Having
        these data can be useful in cases where a given sample has unexpected results,
        e.g. unassigned lineage, and can reveal sites that have (e.g.) a roughly
        50/50 split of reads supporting different bases, which can _sometimes_
        indicate a QC issue or a mixed infection.

        The read depths and ALT counts for simple SNPs is calculated fairly
        easily, but these metrics are more difficult to calculate for indels using
        the approach in this script. As a compromise, the final metrics for indels
        are averaged across the span of the variant.

        However, there are problems associated with purely taking the raw base counts as
        evidence for variants, especially with Nanopore data. For example,
        "counts of bases and evidence for variants are not synonymous".

        See the following posts:
        - https://community.nanoporetech.com/posts/calculating-variant-allele
        - https://community.nanoporetech.com/posts/what-is-the-best-approach
        - https://labs.epi2me.io/notebooks/Introduction_to_how_ONT's_medaka_works.html

        Again, treat results from this script as a rough guide for further investigation.
```
