#Data Visualization in Bioinformatics

Bar plot的使用上滿普遍的，本篇沒什麼其特點，用任何工具甚至是excel都可以達成，因人喜好而已。   
最近正好需要處理到Mutation Gene的profile，通常以binary的形式去紀錄gene的突變狀況，需要粗略的統計到有多少手邊的樣本產生突變，因此簡單地以一個表格統計gene以及mutation人數。

| gene  | SNP_samplenum | type |
|:---:|:---:|:---:|
|  ABCD1 |  12 | normal  |
| KRAS  |  11 |  onco |
|  BRCA1 |  8 |  onco |
|  BRCA2 |  2 |  onco |
|  TP53 |  18 |  onco |
|  CD3 |  1 | normal  |

> type是手動調整的，由於有些基因在某些疾病中比較容易突變，而且被report過多次
> 你的老闆或者是幫你看資料的人可能會想要知道他們長在哪些位置
> 所以把某些gene標註成oncogene使得ggplot自己去調整顏色

```R
ggplot(snp,aes(x=reorder(gene,SNPsummary),y=SNPsummary,fill=common))+
	geom_bar(stat="identity")+
	theme_classic()+  # Personal require
	theme(axis.text.x = element_text(angle = 90, hjust = 1))+  # Rotate the axis label
	scale_fill_manual(values=c("#9999CC","#CC6666"))
```

> 圖的範例為自己研究所需，當然有些參數已經過調整，所以非真實Data
 
![pic](https://gist.githubusercontent.com/KestindotC/7f8caa8be6b33cbe06dbdc5b98d10d6a/raw/e163fcf4f0ce64c22eaede481c5c0fb41932d8ff/z_barplot.png)