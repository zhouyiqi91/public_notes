V2相比V1, 可能会影响细胞数的改动

- 使用STARsolo代替featureCounts进行定量，运行时间减少, 同时也解决了以下问题：当基因组中两个基因的exon和intron重叠时，如果一个reads mapping到了这个区域，会被丢弃。现在这个reads会被分配到exon区域。
  
https://github.com/alexdobin/STAR/issues/890
https://github.com/alexdobin/STAR/issues/1460

- EmptyDrops_CR的FDR参数从0.01改为0.001。

https://github.com/alexdobin/STAR/blob/master/docs/STARsolo.md#emptydrop-like-filtering

- 将STAR版本从2.6.1更新到2.7.11a；由于STAR index文件在这两个版本间不兼容，基因组需要重新运行mkref。