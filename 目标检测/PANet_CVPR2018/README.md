## 整体描述
PANet是基于Mask-RCNN进行改进的，改进点如下：
1. Bottom-Up Path Augmentation
 - 原始的Mask-RCNN没有很好的利用低层信息。
 - 低层FeatureMap关注物体的整体，高层FeatureMap关注物体的纹理图案，因此低层的信息可以用于对物体进行更好的定位。
 - PANet 增加了Bottom-Up Path Augmentation,将低层信息又传导到高层之中，同事减少了高层到低层信息流通需要穿过的卷积层数
2. Adaptive Feature Pooling
 - 原ROI POOLING只在最后一层上提取信息
 - PANet使用Adaptive Feature Pooling同时对多层级进行ROI POOLING 
3. Mask预测分支融合FCN式的预测和Fully-Connected式的预测。前者关注局部，后者关注整体Context信息

## 特征融合演变
- 无融合。  利用多尺度特征分别负责不同scale大小物体的检测。如SSD;
- 自上而下单向融合 FPN。仍是当前主流的融合模式，常见的YOLOV3、RetinaNet、FRCNN等
- 简单双向融合。 PANet第一个提出从下向上二次融合
- 复杂双向融合。 ASFF、BiFPN
- - ASFF。 不同stage特征的融合，采用了注意力机制，控制其他stage对本stage特征的贡献度
- - BiFPN。 在FPN中寻找一个有效的block，然后重复叠加，弹性的控制FPN的大小。