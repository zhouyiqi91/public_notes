# 步骤
https://github.com/singleron-RD/CeleScope/blob/master/methods/snp_v.1.5.1.md

variant注释用的[Annovar](https://annovar.openbioinformatics.org/en/latest/).

# 将support matrix读入Seurat

处理输出文件，生成可以读入Seurat的upport_mtx目录。
```shell
mkdir support_mtx
# barcode
tail -n +2 *_CID.tsv| awk '{print $1}'|gzip > support_mtx/barcodes.tsv.gz
# features
tail -n +2 *_VID.tsv|awk '{ $1="variant_"$1; print }'| gzip > support_mtx/features.tsv.gz
# matrix
gzip -c *_support.mtx > support_mtx/matrix.mtx.gz
```

```R
library(Seurat)
support_mtx = Seurat::Read10X('support_mtx',gene.column = 1)
```

```
support_mtx

  [[ suppressing 32 column names ‘AAACATCGAACAACCATGGTGGTA’, ‘AAACATCGAACGTGATATCATTCC’, ‘AAACATCGAAGGTACACATCAAGT’ ... ]]

1666 x 3573 sparse Matrix of class "dgCMatrix"
                                                                               
variant_1 1 632934 GA G                 . . . . . . . . . . . . . . . . . . . .
variant_2 1 632981 T C                  . . . . . . . . . . . . . . . . . . . .
variant_3 1 633039 T C                  . . . . . . . . . . . . . . . . . . . .
variant_4 1 633062 T C                  . . . . . . . . . . . . . . . . . . . .
variant_5 1 633083 A G                  . . . . . . . . . . . . . . . . . . . .
variant_6 1 633098 C T                  . . . . . . . . . . . . . . . . . . . .
variant_7 1 633105 A G                  . . . . . . . . . . . . . . . . . . . .
variant_8 1 633159 C T                  . . . . . . . . . . . . . . . . 1 . . .
variant_9 1 633172 A G                  . . . . . . . . . . . . . . . 1 1 . . .
variant_10 1 633192 G A                 . . . . . . . . . . . . . . . 0 . . . .
variant_11 1 633206 T G                 . . . . . . . . . . . . . . . 1 1 . . .
variant_12 1 633214 T A                 . . . . . . . . . . . . . . . . 1 . . .
variant_13 1 633236 A G                 . . . . . . . . . . . . . . . . 2 . . .
variant_14 1 633253 C T                 . . . 1 . . . . . . . . . . . . 1 . . .
variant_15 1 633263 C T                 . . . 1 . . . . . . . . . . . . 1 . . .
variant_16 1 633266 A T                 . . . 1 . . . . . . . . . . . . 1 . . .
variant_17 1 633281 T C                 . . . 1 . . . . . . . . . . . . 1 . . .
variant_18 1 633295 A T                 . . . 1 . . . . . . . . . . . . 1 . . .
variant_19 1 633297 C A                 . . . 1 . . . . . . . . . . . . 2 . . .
variant_20 1 633311 T C                 . . . . . . . . . . . . . . . . . . . .
variant_21 1 633323 A G                 . . . . . . . . . . . . . . . . . . . .
variant_22 1 633338 C T                 . . . . . . . . . . . . . . . . . . . .
variant_23 1 633372 T C                 . . . 2 . . . . . . . . . . . . 2 . . .
```

# 文件说明
{sample}_VID.tsv：
为每个变异分配的唯一数字ID（VID）。

{sample}_CID.tsv：
为每个细胞分配的唯一数字ID（CID）。

{sample}_RID.tsv（仅在提供--panel选项时生成）：
为每个目标区域分配的唯一数字ID（RID）。

{sample}_merged.vcf：
包含所有细胞的所有变异的VCF文件。VID和CID会被添加到INFO列中。

{sample}_filter.vcf：
经过过滤后的VCF文件。无效的CID会从INFO列中移除。

{sample}_variant_count.tsv：
记录参考和变异支持的读取/UMI（唯一分子标识）计数。

{sample}_filter_variant_count.tsv：
过滤后的参考和变异支持读取/UMI计数。

{sample}_support.mtx：
以Matrix Market Exchange格式输出的支持矩阵。行表示变异（VID），列表示细胞（CID）。值可以是1、2或3：
1：位置上的所有读取/UMI支持参考等位基因。
2：位置上的所有读取/UMI支持变异等位基因。
3：一个或多个读取/UMI同时支持参考和变异等位基因。