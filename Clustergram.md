Data Visualization in Bioinformatics
=========

In genetic profiling, we use clustergram to present the gene expression frequently.
Besides gene expression value, the distance between samples and genes were also concerned.
So, here I list few common packages in our lab to analyze gene(gene set) expression values.
Following packages were recommended, and few of them were shown in examples.


- pheatmap : R-package
- heatmap.2: R-package # Lastest version of heatmap.3   
- ggplot2  : R-package # Base package
- seaborn  : Python module


Example data set were genetic profiling with 31 genes and 600 samples approximately.


### pheatmap   
```R
library(pheatmap)
ggheat <- pheatmap(data,
                   color = , col=redgreen(75)
                   border_color = "white",cellwidth = 0.6, cellheight = 9, scale = "row",
                   cluster_rows = TRUE,cluster_cols = TRUE,
                   clustering_distance_rows = "euclidean",clustering_distance_cols = "euclidean",
                   clustering_method = "average",fontsize = 10,
                   cutree_rows = NA, cutree_cols = NA,  	# Do it latter.
                   legend = TRUE, legend_breaks = NA,
                   legend_labels = NA, annotation_row = NA, annotation_col = NA,
                   annotation = NA, annotation_colors = NA, annotation_legend = TRUE,
                   annotation_names_row = TRUE, annotation_names_col = TRUE,
                   drop_levels = TRUE, show_rownames = F, show_colnames = F, main = NA,
                   fontsize_row = 10, fontsize_col = 10,
                   display_numbers = F, number_format = "%.2f", number_color = "black",
                   fontsize_number = 0.8 * fontsize, gaps_row = NULL, gaps_col = NULL,
                   labels_row = NULL, labels_col = NULL, filename = NA, width = NA,
                   height = NA, silent = FALSE)

```


### heatmap.2

```R
library(gplots)
heatmap.2(as.matrix(data), col=redgreen, scale="row", key=T, keysize=1.3,breaks=c(seq(-3, 3,0.01)),density.info="none", trace="none",labCol = F,margins = c(3,12),cexRow = 0.8)

```
> Heatmap.2 有幾個比較需要自己調整的問題    
> 像是常見的包含在RStudio上會出現的下面的error可藉由調整 margin 以及 par參數來除錯
>
> Error in plot.new() : figure margins too large   
> Error in par(op) : invalid value specified for graphical parameter "pin"




### ggplot2

> ggplot2是最根本的方式，也是其他package內涵的基本方法.  
> 由於筆者趕著畢業，所以不使用這方式，待更新...

### seaborn

```python
import seaborn as sb
import pandas as pd
import matplotlib.pyplot as plt

# Please import your data as pandas dataframe first.
# The default clustering parameters in R were (complete and euclidean)
fig = sb.clustermap(data,standard_scale=0,method='complete', metric='euclidean')
plt.setp(fig.ax_heatmap.get_yticklabels(), rotation=0)
plt.setp(fig.ax_heatmap.set_xticklabels([]))     # X labels ommited
fig.savefig("heatmap_result.png")

```
