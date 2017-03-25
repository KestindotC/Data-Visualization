# Data Visualization in Bioinformatics

DNA拷貝變異數的呈現通常會需要整個基因組的位置資訊，這時候在繪圖的時候其中一軸會很常使用到多塊小圖合併的原理。
以R-ggplot的名詞來說的話就是`facet_grid`，而我們今天拿到的資料範例如下表：

| Sample | Chromosome | Start   | End     | Segment_Mean | length  |
|--------|------------|---------|---------|--------------|---------|
| S1     | 1          | 61735   | 258978  | -0.1625 | 197243  |
| S1     | 1          | 239324  | 259407  | -1.8405 | 83      |
| S1     | 1          | 259422  | 1892483 | -0.1967 | 1633061 |
| S1     | 1          | 1892631 | 1892631 | -0.2738 | 1774063 |
| S1     | 1          | 3668655 | 3893197 | -0.1861 | 224542  |

> 這種資料格式是由常見的Copy Number Variation Detection演算法(ex.CBS or HMM)計算之後report。   
> **Sample**的名字以及**Chromosome**染色體位置
> 在哪些區間裡面他的拷貝變異數轉換數值**Segment_Mean**，以及我額外加上去的區間長度**length**。    
> 當樣本數只有少數，可以單純對每一個樣本畫出 CNV genomic profile，或者是每個CNV區間出現的樣本比例。

不是做生物資訊的人也或許會用到，在某個時間序列或空間序列當中，觀察數值呈現正負的擺動，藉由這種圖表來呈現
以ggplot2的指令做如下圖的範例，概念上是將每個Chromosome區分成不同facet然後facet之間不要有空隙就可以了




```R
ggplot(data=data) + 
  facet_grid(~Chromosome, space="free_x", scales="free_x") + 
  theme(axis.text.x=element_blank(), axis.ticks=element_blank(), axis.title.x=element_blank(),
        panel.spacing = unit(0, "lines")) +
  geom_bar(data = data[data$Segment_Mean>0,],aes(x=Start, y=Segment_Mean, width=length, fill = "gain"), stat="identity",position="identity") +
  geom_bar(data = data[data$Segment_Mean<0,],aes(x=Start, y=Segment_Mean, width=length, fill = "loss"), stat="identity",position="identity") +
  geom_hline(yintercept=0.2016, colour="grey", size=0.8, linetype="dashed") +
  geom_hline(yintercept=-0.2344, colour="grey", size=0.8, linetype="dashed") +
  ylim(c(-1.5,1.5))+
  scale_fill_manual(values = c("loss" = "darkblue", "gain" = "red"))

```

[pic](https://gist.githubusercontent.com/KestindotC/7f8caa8be6b33cbe06dbdc5b98d10d6a/raw/e950633f191f928e39251707be0888e557a591b7/z_CNV_first_gainloss.png)


> 以圖來呈現非常廣泛的Genomic Range會造成當距離不是很長的資訓，會呈現不出Bar圖，以上圖來說10Mb的區間才呈現出一條細細的線
(畢竟3Gb的genome被壓成這樣很難看出有什麼區域是真的有gain或loss)   
除此之外，事實上每一條Chromosome的極限尾端並不一定有被CNV caller report的前提下，根本不存在於圖上。   你說有沒有解決方法？目前建議的方式就是把有興趣的region抓出來看或者是每一條Chromosome抽出來個別視覺化   


而個別的Sample看完了，想要進一步看所有Sample並且比較他們的Whole Genome Scale的分佈狀況就可以使用前面提過的Heatmap，差別在於其中一軸會變成Genomic Scale，另一軸維持Sample，並且利用`facet_grid	`跟`geom_tile`結合讓圖像是Heatmap的樣子


```R
# 這裡的data是如同上面的資料格式，然後Sample有很多個，並且對於Sample2的Segment_Mean做了小幅度的調整

ggplot(data=data) + 
  facet_grid(~Chromosome, space="free", scales="free") +theme_minimal() +
  geom_tile(aes(x=Start,y=Sample,fill=Segment_Mean,width=length),stat="identity") +
  theme(axis.text.x=element_blank(),axis.ticks=element_blank(), 
        axis.title.x=element_blank(),axis.text.y=element_blank(),
        panel.spacing = unit(0, "lines"),legend.position="top") +
  scale_fill_gradient2(low = 'steelblue',mid='white',high = 'maroon',limits=c(-2,2),na.value = "white")

```

[pic](https://gist.githubusercontent.com/KestindotC/7f8caa8be6b33cbe06dbdc5b98d10d6a/raw/e950633f191f928e39251707be0888e557a591b7/z_CNV_first_gainloss.png)
