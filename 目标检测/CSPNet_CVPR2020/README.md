> 该track被YOLOV4使用

- CSPNet ：Cross Stage Partial Network，从网络结构设计的角度解决以往推理过程中大计算量的问题。

- 使该体系结构在减少计算量的同时实现更丰富的梯度组合。通过将基层的baselayer 分为两个部分，通过一个跨级段的层级结构将他们合并。
- 通过分裂梯度流，使梯度流通过不同的网络途径传播。

![img](./CSP.png)