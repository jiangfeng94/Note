# FCOS:Fully Convolutional One-Stage Object Detection

## 背景
### anchor-base检测器的缺点
- anchor的大小，宽高比，不同框的数量都会影响检测器的效果；
- 小目标难以检测
- 为了提高召回率，需要更多的anchor防止漏检，而其中大部分是负样本
- 计算IOU时大量的anchor box 计算量大
### anchor-free检测器的缺点
- YOLOV1:只检测中心点附近的目标（YOLOV2提到了该缺点，故YOLOV2使用了anchor-based） 
- ConerNet 需要比较复杂的后处理

## FCOS
### 问题与解决
1. 问题1: 只检测中心点附近目标   
  FCOS不再学习预测目标的中心点，对每个位置学习它到GT BOX的上、下、左、右的距离（t,b,l,r）
2. 问题2: 重叠    
  使用FPN的思路解决重叠问题
    - FCOS有5个feature map（P3、P4、P5、P6、P7）设定五组阈值分别是(0,64)(64,128)(128,256)(256,512)(512,∞)
    - 计算出(t,b,l,r) 取其中最大值m = max(t,b,l,r)
    - 如果m在对应的阈值范围内，则为正样本，反之不在该feature map的范围为负样本

### LOSS
1. Center-ness
   - 加大中心点附近的贡献（可以理解为YOLOV1中心点即正样本的soft版本）
   - 公式如下，使用根号的目的是为了降低centerness的衰减速度
   - 因为centerness范围是(0,1)所以使用BCE Loss
```math
centerness = \sqrt{\frac{min(l,r)}{max(l,r)} × \frac{min(t,b)}{max(t,b)}}
```

2. 类别loss: Focal Loss
3. box loss: IOU Loss