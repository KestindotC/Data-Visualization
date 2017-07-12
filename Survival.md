# Data Visualization in Bioinformatics

在流行病學研究上很容易看到 Kaplan Meier Plot，它的作用是為了將我們給定病人條件的分類下視覺化病人群之間存活率是否有差異，
R package有非常多survival專用的package，**surv**就是一個計算時用到最大宗的套件，被包含在許多視覺套件內，我自己使用的是[survminer](http://www.sthda.com/english/rpkgs/survminer/index.html)，那麼病人的存活資料準備好之後就沒什麼好擔心的了。
         
 
|    Sample    | Censor | Time | TIL_level |
|:------------:|:------:|:----:|:---------:|
| TCGA-G4-6294 |    1   |  858 |     0     |
| TCGA-AA-3814 |    0   |   0  |     1     |
| TCGA-AA-3672 |    0   |  28  |     1     |
| TCGA-D5-6541 |    0   |  474 |     1     |
| TCGA-AA-3861 |    0   |  914 |     1     |
| TCGA-CM-6165 |    0   |  488 |     1     |


```R
# data 的資料如上表
# Censor: 1 for dead 0 for alive
fit <- survfit(Surv(Time/30,event=Censor) ~ TIL_level, data = data)
ggsurvplot(fit,data = data,
           size=0.7,
           font.main = 10, font.x=8,font.y=8,
           palette=c('#E8B800','#2E9FDF'),
           xlab = "Time (Month)",
           pval = T, # Log-rank P value
           legend.labs = c('Low','High'),ggtheme = theme_minimal())

```

![pic](https://gist.githubusercontent.com/KestindotC/7f8caa8be6b33cbe06dbdc5b98d10d6a/raw/92dbde4648322c15ec13e76c0038297e8fe7d485/z_surv.png)

如果你需要更多功能像是加入Confidence interval或是Risk table的話可以參考官方網站特別整理出的Cheat sheet