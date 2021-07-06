
# 

> Backbone、 Matcher 和 positional encoding 的实现和 DETR是一样的，主要的修改在 deformable_detr.py 和 deformable_transformer.py 中。真正有效的应该主要在于 multi-scale deformable attention，毕竟 multi-scale / pyramid representation， deformable 和 attention 本身就是 CV 里最 work 的几类 idea。
 
> Deformable DETR主要是将之前DETR直接使用特征图进行训练变为注意力之后的特征图。将之前DETR花费时间学习到的Sparse Att map快速得到。

>self-attention的最大意义是在于建立长距离的相互关系，并且能够避免CNN中存在的归纳偏好问题。Deformable attention操作降低self-attention的复杂度，同时保留了self-attention构建长距离相互关系的优点。

## 概述
- Deformable 借鉴了DCN(Deformable Convolutional NetWorks) 将其应用在注意力机制上
- 相对于DETR全局&密集的注意力机制，提出了一种每个参考点仅关注领域的一组采样点，这些采样点的位置并非固定，同DCN一样是可学习的
  
## DETR
### 优点
- 第一个端到端目标检测器
- 不需要手工设计：anchor、标签分配策略、NMS后处理
- 方法论的存在
### 缺点
-  DETR收敛慢，且能够处理的特征分辨率有限
    - Transformer在初始化时 分配给特征像素的注意力权重几乎相等
    - Transformer在计算注意力权重是，伴随着高计算量与空间复杂度
### 思考
-   每个像素特征不必与所有特征像素交互计算，只需要与部分基于采样获得的其他像素(交互)即可

## Multi-Scale Feartures & Scale-Level Embedding
- backbone提取不同层级的特征，共四层,C3~C5分别对应下采样8,16,32;C6通过C5经过步长为2的3×3卷积获得
- 在多尺度特征中，位于不同特征层的特征点可能拥有相同的坐标。为此提出了“Scale-Level Embedding”

## Deformable Attention
  对于每个query,仅在全局位置中采样部分位置的key，并且value也是基于这些位置进行采样插值得到