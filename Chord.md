# Data Visualization in Bioinformatics

Chord Plot
Chord plot在生物資訊學的應用上雖然沒有heatmap這麼廣泛，但基本上我看過適用的地方就包含了gene fusion以及copy number variation(CNV)的呈現。而下面的範例則是我之前的研究，關於T cell receptor模型的建立。

T細胞受體在基因體層次上包含了 Variable, Diversity 以及 Joint Region的小塊exon分佈。
這三種gene exon之間目前尚未被發現有無特殊的連接狀況，因此藉由定序資料分析得到他們的組合機率分佈，資料前處理完之後就可以用Chord Plot呈現。

目前R package - Circlize可以做到把很多二維呈現的資料拉成圓形可以看他的[範例](http://zuguang.de/circlize/)


```R
library('circlize')
head(data)
        V_seg     J_seg
1  TRAV1-1*00 TRAJ23*00
2  TRAV1-2*00 TRAJ23*00
3  TRAV1-2*00 TRAJ28*00
4  TRAV1-2*00 TRAJ33*00
5   TRAV10*00 TRAJ20*00
6 TRAV12-1*00 TRAJ36*00

# Pair的組合表列在dataframe格式中，我這次的範例有2236個結果
# 只取出現次數大於4次的資料，方便示範而已

df <- as.data.frame(table(data))
df <- df[df$Freq>4,]
chordDiagram(df)

# VDJ的 segments combination 的統計不太需要 track 的刻度
chordDiagram(df, annotationTrack = c('name','grid'))


```
![pic](https://gist.githubusercontent.com/KestindotC/7f8caa8be6b33cbe06dbdc5b98d10d6a/raw/z_chordplot_r.png)


> 可以看到在text labeling的調整上可能還是要透過base package(circos)   
> 建議需要產生高解析的圖的話還是用illustrator重篇編輯文字或已legend的方式呈現   
> 缺點：當類別項目太複雜的時候，這樣的呈現需要考量到最重要的pair們怎麼Hightlight

結論：其實只要是有pair特性的資料，然後資料量不要太過複雜，以chord plot來呈現是一件很美好的事情。
