# Introduction
不平衡问题主要分为四大类：1）Class Imbalance 2）Scale Imbalance 3）Spatial Imbalance 4）Obejective Imbalance

## 1.Class Imbalance
### 1.1 前景-背景 不均衡
#### 硬采样 Hard Sample Methods
 直接选择更具有意义的样本，抛弃简单/常见/无用的样本
- Random Sampling
- Hard Exapmle Mining : SSD、IoU-based Sampling、Bootstraping
- Limit Search Space
#### 软采样 Soft Sample Methods
根据样本的重要性给予样本不同的权重，具有高loss值/高IoU值的样本，在训练中起较大的作用
- Focal Loss
- Gradient Harmonizing Mechanism
- Prime Sample Attention
#### 生成式方法 Generative Method
在使用GAN生成数据集时，1)直接的生成 2)利用
- Adversarial Faster-RCNN
- PSIS
- pRoI Generation
### 1.2 前景-前景 不均衡
- 通过取batch时，样本量少的类别尽可能被选中
- gan生成样本

## 2.Scale Imbalance
### 2.1 object/box level 
样本中目标的大小，标定框的尺寸
- 在每一层都做推理，如SSD
- 特征金字塔：不同层的特征融合后做推理，如FPN
- 图形金字塔： 对图像进行缩放
- 图像金字塔&特征金字塔结合，如Scale Aware Trident Network
### 2.2 feature level
- 以特征金字塔为基础的方法：使用额外的操作改善FPN对特征的集成（PANet）
- 以骨干网络为基础的方法,以最后一层为基础做尺度变换，得到多尺寸的特征图
1. PANet
2. LIbra FPN
2. Scale-Transferrable Detection Network
3. Parallel FPN
4. Deep Feature Pyramid Reconfiguration 
5. zoom Out-and-In Network
6. Multi-Level FPN
7. NAS-FPN

## 3.Spatial Imbalance
此类问题主要存在与监测网络中的检测框回归函数、IoU分布不均衡、目标位置不均衡

### 3.1 Regression Loss
L1范数相比L2范数对回归误差较小时不稳定，但对较远时较为友好，而L2范数相反。

Smooth L1损失函数：将L1和L2结合
Loss Function | 介绍
---|---
L2 Loss | 对小误差稳定，对大误差惩罚严重
L1 Loss | 对小误差不稳定，对大误差友好
Smooth L1 Loss | 与L1相比 对小误差更加稳定
Balance L1 Loss | 与Smooth L1相比increase inliers compare
Kullback-Leibler Loss | 基于KL散度
Iou Loss | 使用IoU
Bounded IoU Loss | 
GIou Loss | GIOU = IOU -(Ac-U)/Ac Ac:最小包闭区域
DIou Loss | DIOU = IOU - 欧式距离(预测框中心点,GT框中心点)/最小包闭框对角线距离
CIous Loss | 加入长宽比

### 3.2 IOU分布
Cascade RCNN 通过级联的方式不断优化IoU的分布

### 3.3 目标位置

## 4. Objective Imbalance
检测任务需要定位目标的位置并对其分类，这两个损失函数分支并不是均衡的
在COCO上，分类的损失是高于回归的损失

CARL（Classification-Aware Regression Loss）

