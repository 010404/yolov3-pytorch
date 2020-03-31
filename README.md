# yolov3训练自己的数据集(权重)

##### 总体上是代码复现，然后用自己查找的图片和xml进行训练

**weight**压缩包下载：

```
http://home.linglian.ml:82/index.php/s/PPJJQc2yi8Z9qxJ
```

把网盘下载解压好的weights文件夹添加到yolov3目录中

依次运行**makeTxt.py**、**voc_label.py**、**train.py**、**detect.py**

------



### 修改cfg文件

**修改yolov3/cfg/yolov3-tiny.cfg**

每个**[yolo]**的**classes**需要修改

![012](https://github.com/010404/yolov3/blob/master/picture/012.png)



每个**[yolo]**上的**[convolutional]**的**filters(过滤器/卷积核)**需要修改：

![011](https://github.com/010404/yolov3/blob/master/picture/011.png)

------



### [makeTxt.py](https://github.com/010404/yolov3/blob/master/makeTxt.py)

**yolov3**中的**makeTxt.py**是将数据集分为训练集(training data)和测试集(test data)

​		比例分别为0.9和0.1

```
trainval_percent=0.1
train_percent=0.9
```



在**ImageSets**中得到四个文件：

![**001**](https://github.com/010404/yolov3/blob/master/picture/001.png)



其中**train.txt**:

![002](https://github.com/010404/yolov3/blob/master/picture/002.png)

![004](https://github.com/010404/yolov3/blob/master/picture/004.png)

------



### [voc_label.py](https://github.com/010404/yolov3/blob/master/voc_label.py)

**yolov3**文件夹中的**voc_label.py**是把已有的标注文件(xml)转化为txt文件

​	在**labels**中得到内容

![003](https://github.com/010404/yolov3/blob/master/picture/003.png)



不仅仅有**文件名**，还有**文件的具体路径**

![](https://github.com/010404/yolov3/blob/master/picture/004.png)



------





### [train.py](https://github.com/010404/yolov3/blob/master/train.py)

用于训练自己的数据集

其中cfg文件(神经网络各层的配置)使用**yolov3-tiny.cfg**，权重使用**yolov3-tiny.conv.15**预训练权重

![006](https://github.com/010404/yolov3/blob/master/picture/006.png)

**epochs=30**(迭代次数)，**mAp**为0.9左右

#### （训练时未能截图）



在**data**目录下：

![005](https://github.com/010404/yolov3/blob/master/picture/005.png)

其中**rbc.data**是新建的：

```
classes=1
train=data/train.txt
valid=data/test.txt
names=data/rbc.names
backup=backup/
eval=coco
```

同样，**rbc.names**也是新建的：

```
computer
```

用于检测一个类(笔记本电脑)



**train.py**训练完之后，在**weights**目录下生成**best.pt**和**last.pt**，即最好的权重以及最后一次训练的权重，二者都是**pytorch形式**的权重

![007](https://github.com/010404/yolov3/blob/master/picture/007.png)

------



### [detect.py](https://github.com/010404/yolov3/blob/master/detect.py)

用于检测目标，在**data/samples**中放入待测图片，图片运行后在**yolov3/output**中输出检测的结果。

![008](https://github.com/010404/yolov3/blob/master/picture/008.png)



![009](https://github.com/010404/yolov3/blob/master/picture/009.png)



![010](https://github.com/010404/yolov3/blob/master/picture/010.png)



#### **detect.py**使用的是**best.pt**权重

![013](https://github.com/010404/yolov3/blob/master/picture/013.png)

------



## 总结

其实这次考核项目是对大佬算法的复现，但实际操作上遇到了很大困难，比如标注文件的软件不同、标注图片源文件名是中文、一些图片不符合要求、需要大小一致的图片(后来明白了它是resize的)、权重的缺失比如在**yolov3/test.py**中需要**yolov3-spp-ultralytics.pt**，而训练需要预训练权重**yolov3-tiny.conv.15**。最难的是代码理解上，能做到只是把英文标注逐个翻译成中文理解，整体看下来理解了一点，其中的**yolov3/utils/parse_config.py**是用于解析cfg配置文件，**model.py**是整个darknet的结构以及一些函数。这个算法实现步骤也许看着很简单，但过程很久，都是在报错中度过，慢慢去校正，也积累了一些报错的经验以及解决方法，最大的帮助是在于让我学会了通过多个编码软件和平台渠道去获取有关信息，复现这个算法也是我进步了不少。

