+++
banner = "banners/placeholder.png"
categories = ["学习"]
date = "2021年7月18日"
menu = ""
tags = []
title = "GO注释绘图"

+++

# Go注释绘图

| GO                 | Pathway                          | Pvalue   | Count |
| ------------------ | -------------------------------- | -------- | ----- |
| Molecular Funciton | oxidoreductase activity          | 3.1E-06  | 135   |
| Biological Process | photosynthesis                   | 3.07E-08 | 28    |
| Biological Process | photosynthesis, light harvesting | 7.55E-07 | 11    |



~~~
#GO注释，数据格式如上图

library(readxl)
library('ggplot2')

pathway=read_xlsx('GO_down.xlsx')
Class<-pathway$GO
p = ggplot(pathway,aes(-1*log10(Pvalue),Pathway,shape=Class))
p=p + geom_point()
# 修稿点的大小
p=p + geom_point(aes(size=Count))
# 展示三维数据
pbubble = p+ geom_point(aes(size=Count,color=-1*log10(Pvalue)))
# 设置渐变色
pr = pbubble+scale_color_gradient(low="green",high = "red")
# 绘制p气泡图
pr = pr+labs(color=expression(-log[10](Pvalue)),size="Count",  
             x="-log10(Pvalue)",y="Pathway name",title="Pathway enrichment")
pr + theme_bw()
pr+  theme(axis.text.x = element_text(size = 10,color="black"),axis.text.y = element_text(size = 10,color="black"))+theme(axis.title.x = element_text(size = 10),axis.title.y = element_text(size = 10))
#t
ggsave("MSLT_down.pdf")# 保存为pdf格式
ggsave("MSLT_down.png",width=4,height=4)


library('eoffice')
topptx(pr,filename = 'mslt_down.pptx',height = 6,width = 8)
~~~

