```R
library(pheatmap)

# 数据读取
O <- "J:/BBYFY/2nd/summary_amr.txt"
OTU <- read.delim(O,header=T,row.names=1)
# 统计
OTU$sum <- rowSums(OTU)
# OTU_filter = OTU[OTU$sum != 54,]
#排序
OTU_order <- OTU[order(OTU$sum, decreasing=TRUE), ]
#去重sum列
mat <- OTU_order[,-ncol(OTU)]

#绘图,注意加上width和height参数.
pheatmap(mat=mat,width = 9,height = 18,filename = "J:/BBYFY/2nd/result_amr.pdf")
```

