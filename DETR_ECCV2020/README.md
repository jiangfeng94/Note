# 0.OverView
- 文章的主要工作：将Transformer运用到目标检测中，取代现有需要手工设计的工作，如NMS、Anchor生成等；
- 主要贡献：
1. 用Transformer的encoder-decoder一次性生成N个pred boxes。（N为事先设定的值，论文中使用100）
2. 设计了**bipartite matching loss**,基于预测的boxes与GT boxes的二分图匹配计算loss的大小

- 与VIT相比，DETR不是一个完全有Transformer处理的架构，还是需要依赖于CNN作为backbone

# 1.Transformer
## 1.1 Transformer Encoder 
1. 使用BackBone（如ResNet）提取feature map (C=2048,H =H0/32,W=W0/32)
2. **维度压缩**：将BackBone输出的C×H×W维的feature map
3. 先使用1×1卷积处理，将通道数从C压缩到d，即得到d×H×W的 新feature map
4. **序列化**：将上一步得到的d×H×W维的feature map reshape 成 d×WH维的 feature map
5. 将上一步的feature map 与**positional encoding** 相加，输入 Transformer Encoder
6. Transformer Encoder 输出**image embedding** ，输入 Transformer Decoder

## 1.2 Transformer Decoder
Transformer Decoder的输入主要包括两部分：
- Transformer Encoder 输出的 image embedding 与 positional encoding 的和
- object queries

### object queries
- object queries 有N个(事先设定)，输入Transformer Decoder 会得到N个Decoder ouput embedding，经过后面FFN处理后得到N个预测的boxes以及其类别;
- 随机初始化，并在训练过程中学到的embedding？？
- 与原始的Transformer相比，DETR的Decoder一次性处理所有的object queries，而不像原始的Transformer是auot-regressive的，从左到右一个词一词输出。
- 
## 1.3 FFN (Feed-Forward Network)
有两种FFN一种用来预测bbox的中心位置、宽高，一种是预测class标签


# 2. LOSS
- 预测框数量为N，而实际GT框的数量为M，设定时N远大于M。如何匹配计算loss。
- 为了解决这一问题，人为构造了一个新的物体类别X(表示没有物体)，加入到image objects中，上述(N-M)个pred embedding会与此类别匹配。
- 定义好每一对pred与GT的匹配cost，使用匈牙利算法快速找到使得总cost最小的二分图匹配方案。
- 当配对的GT类别为X时，将loss设置为0，当配对的GT类别是真实目标，如果预测的pred与GT的类型相同的概率越大，或者box差距越小，则配对的cost越小。