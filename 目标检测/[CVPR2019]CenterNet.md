# CenterNet:Objects as Points

## 边界框的表示形式
- 目标的中心点以及宽高
- 与FCOS相似

## 正负样本分配
- CenterNet引入高斯热图确定正负样本
- 将GT坐标通过高斯核分布到热图上
- 正样本：高斯核的中心点；困难样本：高斯核中心附近；负样本：不在高斯核内

## 损失函数
1. 分类Loss ： Focal Loss
2. 回归Loss ： 包括offest回归，和wh回归。L1 Loss。


## 总结
Center + 高斯热图