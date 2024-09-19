# 步骤
https://github.com/singleron-RD/CeleScope/blob/master/methods/snp_v.1.5.1.md

variant注释用的[Annovar](https://annovar.openbioinformatics.org/en/latest/).

# 将support matrix读入Seurat

处理输出文件，生成可以读入Seurat的support_mtx目录。
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

- 为了方便记录，每个细胞barcode对应1个ID（CID）, 每个变异也对应1个ID(VID)。

- 在基因组学和变异检测中，ref 和 alt 分别代表：

ref（reference）：参考等位基因，表示在参考基因组中存在的标准序列。例如，在人类基因组项目中，使用的参考基因组是由代表人类基因组的序列构成的，其中每个位置的碱基称为参考碱基。

alt（alternative）：替代等位基因，表示与参考基因组序列不同的变异序列。它代表在样本中检测到的碱基替换、插入或缺失。

举例：
假设基因组的某个位置在参考序列中是碱基A，但在一个个体的序列中，这个位置是碱基G。

ref: A（参考基因组中的碱基）
alt: G（在样本中发现的替代碱基）
这表明这个个体在该位置有一个碱基替换变异。

ref_count和alt_count是指某个位点，支持ref和alt的read数目。

- {sample}_support.mtx：
以Matrix Market Exchange格式输出的支持矩阵。行表示变异（VID），列表示细胞（CID）。值可以是1、2或3：
1：该位置上的所有read支持ref。
2：该位置上的所有read支持alt。
3：该位置上既有支持ref，也有支持alt的read。