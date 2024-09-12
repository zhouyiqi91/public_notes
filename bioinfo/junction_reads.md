
## Adding junction tags to BAM

- jM:B:c,M1,M2,... : intron motifs for all junctions (i.e. N in CIGAR): 
0: non-canonical; 1: GT/AG, 2: CT/AC, 3: GC/AG, 4: CT/GC, 5: AT/AC, 6: GT/AT. 
If splice junctions database is used, and a junction is annotated, 20 is added to its motif value.

- jI:B:I,Start1,End1,Start2,End2,... : Start and End of introns for all junctions (1-based).

## Counts for annotated and novel splice junctions
`--soloFeatures SJ`

## celescope arguments

```
multi_rna \
 --SAM_attributes "jM jI" \
 --soloFeatures "GeneFull_Ex50pAS SJ" \
 ...
```

## reference
- https://github.com/alexdobin/STAR/blob/master/doc/STARmanual.pdf
- https://github.com/alexdobin/STAR/issues/771