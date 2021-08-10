# ExremeNet: Bottom-up Object Detecion by Grouping Extreme and Center Points

## 边界框的表示形式
- 四个极值点 + 中心点
- GT值 由于没有直接标记四个边缘点的数据，作者使用COCO的segmentation标签，根据分割mask的到边缘点
## 网络
- 与CornerNet相同使用两个连接的Hourglass Network
- 输出为5×C heatmaps 和 4×2 offset maps (中心点没有offset)
  

## 配对
- 每个类别输出5个heatmaps, 通过检测峰值提取出5个heatmaps的关键点.
- 通过4个极值点，计算几何中心.
- 如果几何中心在center map 上高效应，则4个极值点为有效检测.
- 作者通过暴力枚举方式，得到所有有效机制点对.


## 与CornetNet比较
1. CornetNet：两个角点，ExtremeNet：四个极值点+中心点
2. CornetNet使用角点的embedding配对，ExtremeNet使用暴力枚举极值点，通过中心点判断4个极值点是否为一组 














