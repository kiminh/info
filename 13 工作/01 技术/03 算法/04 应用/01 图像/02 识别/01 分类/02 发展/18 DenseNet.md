# DenseNet


CVPR2017


卷积神经网络结构的设计主要朝着两个方向发展，一个是更宽的网络（代表：GoogleNet、VGG），一个是更深的网络（代表：ResNet）。但是随着层数的加深会出现一个问题——梯度消失，这将会导致网络停止训练。到目前为止解决这个问题的思路基本都是在前后层之间加一个 identity connections(short path)。

<p align="center">
    <img width="70%" height="70%" src="http://images.iterate.site/blog/image/20190722/oOsnvYg86Dcg.png?imageslim">
</p>



由上图中可知 Resnet 是做值的相加（也就是 add 操作），通道数是不变的。而 DenseNet 是做通道的合并（也就是 Concatenation 操作），就像 Inception 那样。从这两个公式就可以看出这两个网络的本质不同。此外 DensetNet 的前面一层输出也是后面所有层的输入，这也不同于 ResNet 残差网络。


<p align="center">
    <img width="70%" height="70%" src="http://images.iterate.site/blog/image/20190722/gUMekvoicV3X.png?imageslim">
</p>



DenseNet的 Block 结构如上图所示。


1*1卷积核的目的：减少输入的特征图数量，这样既能降维减少计算量，又能融合各个通道的特征。我们将使用 BottleNeck Layers的 DenseNet 表示为 DenseNet-B。(在论文的实验里，将 1×1×n小卷积里的 n 设置为 4k，k为每个 H 产生的特征图数量)

<p align="center">
    <img width="70%" height="70%" src="http://images.iterate.site/blog/image/20190722/oTMVGzFJxqUn.png?imageslim">
</p>



上图是 DenseNet 网络的整体网络结构示意图。其中 1*1卷积核的目的是进一步压缩参数，并且在 Transition Layer层有个参数 Reduction（范围是 0 到 1），表示将这些输出缩小到原来的多少倍，默认是 0.5，这样传给下一个 Dense Block的时候 channel 数量就会减少一半。当 Reduction 的值小于 1 的时候，我们就把带有这种层的网络称为 DenseNet-C。


DenseNet网络的优点包括：

- 减轻了梯度消失
- 加强了 feature 的传递
- 更有效地利用了 feature 
- 一定程度上较少了参数数量
- 一定程度上减轻了过拟合

