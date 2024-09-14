# CeleScope
https://github.com/singleron-RD/CeleScope

CeleScope is python package with a collection of bioinfomatics analysis pipelines to process data generated with Singleron products.

## single cell VDJ (sCircle) pipeline 

Use subcommand `multi_flv_trust4` of CeleScope or the nextflow pipeline
https://github.com/singleron-RD/scflvdj/

Steps

1. Extract barcode and UMI sequences.
2. Use TRUST4 to perform candidate reads extraction, de novo assembly and annotation.
3. CDR3 filtering.
- Filter nonfunctional CDR3(shown 'out_of_frame' in cdr3 report), or CDR3 sequences containing "N" in the nucleotide sequence.
- Filter CDR3aa not start with C.
- Filter CDR3aa length < 5.
- Filter stop codon in CDR3aa.
4. Keep the barcodes above an UMI threshold, similar to the knee filtering method used in single cell RNA.
5. Match the barcodes obtained from sCircle with the cell barcodes obtained from the single-cell RNA.


### alignment and annotation of TCR sequences

TRUST4 uses kmer count to avoid the time-consuming alignment in the candidate reads extraction algorithm. The details of the annotation can be found in the TRUST4 paper：https://www.nature.com/articles/s41592-021-01142-2

### cell-level matching to gene expression matrix

The high-confidence cell-barcode is determined by matching the barcode in the filtered annotations of sCircle with the barcodes in the single-cell RNA matrix.

### pairing of alpha and beta T cell receptor chains

We use the "Cells With Productive V-J Spanning Pair" metric to measure TCR receptor chain pairing. That is, the number of cells with at least one productive alpha contig and one productive beta contig.

## single cell RNA (GEXSCOPE) pipeline 
Use subcommand `multi_rna` of CeleScope or 
the nextflow pipeline: https://github.com/singleron-RD/scrna/
