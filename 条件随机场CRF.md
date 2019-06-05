## 条件随机场CRF

#### 1.对比逐帧softmax和CRF的异同

##### 1.1逐帧softmax

CRF主要用于序列标注问题，可以简单理解为是**给序列中的每一帧都进行分类**，很自然想到将这个序列用CNN或者RNN进行编码，接一个全连接层用softmax激活

![](https://github.com/Wupingyang/NLP-topic/blob/master/raw/softmax.png)

但是**逐帧softmax并没有直接考虑输出的上下文关联**

##### 1.2条件随机场

通常使用s(单字词),b(开始),m(中间),e(结尾)(参考序列标注问题)的4个标签来做字标注法的分词，目标输出序列本身会带有一些上下文关联，比如s后面就不能接m和e，等等。

逐帧softmax并没有考虑这种输出层面的上下文关联，意味着把这些关联放到了编码层面，希望模型自己学到这些内容。

而CRF则更直接，它**将输出层面的关联分离出来**，这使得模型在学习上更为从容：

![](https://github.com/Wupingyang/NLP-topic/blob/master/raw/crf.png)

CRF在输出端显示的考虑了上下文关联

#### 数学

1.模型概要

假设一个输入有n帧，每一帧的标签有k种可能，那么理论上就有<a href="https://www.codecogs.com/eqnedit.php?latex=k^n" target="_blank"><img src="https://latex.codecogs.com/png.latex?k^n" title="k^n" /></a>种不同的输入。

![](https://github.com/Wupingyang/NLP-topic/blob/master/raw/crf_path.png)

每个点代表一个标签的可能性，点之间的线表示标签之间的关联，而每一种标注结果，都对应图上的一条完整路径。

在序列标注任务中，我们研究的基本单位应该是路径，我们要做的事情是从<a href="https://www.codecogs.com/eqnedit.php?latex=k^n" target="_blank"><img src="https://latex.codecogs.com/png.latex?k^n" title="k^n" /></a>条路径总选出正确的一条。意味着将它视为一个分类问题，那么将是<a href="https://www.codecogs.com/eqnedit.php?latex=k^n" target="_blank"><img src="https://latex.codecogs.com/png.latex?k^n" title="k^n" /></a>类中选一类的**分类问题**。

逐帧softmax和CRF的根本不同：**前者将序列标注看成是n个k分类问题，后者将序列问题看成是1个<a href="https://www.codecogs.com/eqnedit.php?latex=k^n" target="_blank"><img src="https://latex.codecogs.com/png.latex?k^n" title="k^n" /></a>分类问题。**

具体分析：<https://www.jiqizhixin.com/articles/2018-05-23-3>

