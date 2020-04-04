# yolov3训练自己的数据集(权重)

## 原理

在吴恩达卷积网络介绍中理解了**yolov1**的原理，比**yolov3**的原理简单，这样理解**yolov3**相对不难。

**yolov1**原理是把图片分成A*A规格的网格，然后每个网格单元进入**卷积网络**和**yolo层**检测，在某个网格单元中检测出**中心点**，每个网格单元预测B个**bounding box**(先验框)**和置信度**(confidence score)，以及C个不同类别的概率。**归一化bbox**的信息(**x,y,w,h**--->中心相对于网格的**坐标**,网格**宽度**,网格**高度**)，置信度为**Pr(Object)×IoU(叉乘)**，Pr(Object)意思为是否检测到物体，取值为0或1。之后通过置信度的非极大值抑制，筛选出**置信度最高的bbox**。**yolov3**相比于**yolov1**具有**多尺度预测**和更好的**基础分类网络**以及**分类器**。



------

## 数据集准备

#### 准备了300张图片

![018](https://github.com/010404/yolov3/blob/master/picture/018.png)



### 用**labelImg**进行每个图像标注

![020](https://github.com/010404/yolov3/blob/master/picture/020.png)



#### 生成xml标注文件

![019](https://github.com/010404/yolov3/blob/master/picture/019.png)

在**yolov3/data**目录下新建**Image**、**JEPGImages**和**Annotations**<u>文件夹</u>把图片放入**Images**和**JEPGImages**中，把标注文件放入**Annotations**中。

![021](https://github.com/010404/yolov3/blob/master/picture/021.png)



------



## 效果实现

<u>**采用的是yolov3-tiny，对不具有GPU加速的电脑比较友好**</u>

##### 总体上是代码复现，然后用自己查找的图片和xml进行训练

**weight**压缩包下载：

```
http://home.linglian.ml:82/index.php/s/PPJJQc2yi8Z9qxJ
```

把网盘下载解压好的weights文件夹添加到yolov3目录中

依次运行**makeTxt.py**、**voc_label.py**、**train.py**(weights中有训练好的best.pt，这步可以省略)、**detect.py**



#### **yolov3总览**

![013](https://github.com/010404/yolov3/blob/master/picture/013.png)



### [yolov3整体(点击查看)](https://github.com/010404/yolov3/tree/master/%E4%BB%A3%E7%A0%81%E6%A8%A1%E5%9E%8B)

------



### 修改cfg文件

**修改yolov3/cfg/yolov3-tiny.cfg**

每个[yolo]的classes需要修改

![012](https://github.com/010404/yolov3/blob/master/picture/012.png)



每个[yolo]上的[convolutional]的**filters(过滤器/卷积核)**需要修改：

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

![001](https://github.com/010404/yolov3/blob/master/picture/001.png)



其中**train.txt**:

![002](https://github.com/010404/yolov3/blob/master/picture/002.png)

![004](https://github.com/010404/yolov3/blob/master/picture/004.png)

------



### [voc_label.py](https://github.com/010404/yolov3/blob/master/voc_label.py)

**yolov3**文件夹中的**voc_label.py**是把已有的标注文件(xml)转化为txt文件

​	在**labels**中得到内容

![003](https://github.com/010404/yolov3/blob/master/picture/003.png)



不仅仅有**文件名**，还有**文件的具体路径**

![004](https://github.com/010404/yolov3/blob/master/picture/004.png)



------





### [train.py](https://github.com/010404/yolov3/blob/master/train.py)

用于训练自己的数据集

其中cfg文件(神经网络各层的配置)使用**yolov3-tiny.cfg**，权重使用**yolov3-tiny.conv.15**预训练权重

![006](https://github.com/010404/yolov3/blob/master/picture/006.png)

**epochs=30**(迭代次数)，**mAp**为0.9左右

#### （最佳训练epochs时未能截图，因为按照很多不同的epochs训练）

#### **（下面这个是其中的epochs=40，最佳为20）**

![016](https://github.com/010404/yolov3/blob/master/picture/016.png)



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



![015](https://github.com/010404/yolov3/blob/master/picture/017.png)



![009](https://github.com/010404/yolov3/blob/master/picture/009.png)



![010](https://github.com/010404/yolov3/blob/master/picture/010.png)



#### **detect.py**使用的是**best.pt**权重

![014](https://github.com/010404/yolov3/blob/master/picture/014.png)

------



## 总结

其实这次考核项目是对大佬算法的复现，但实际操作上遇到了很大困难，比如标注文件的软件不同、标注图片源文件名是中文、一些图片不符合要求、需要大小一致的图片(后来明白了它是resize的)、权重的缺失比如在**yolov3/test.py**中需要**yolov3-spp-ultralytics.pt**，而训练需要预训练权重**yolov3-tiny.conv.15**。最难的是代码理解上，能做到只是把英文标注逐个翻译成中文理解，整体看下来理解了一点，其中的**yolov3/utils/parse_config.py**是用于解析cfg配置文件，**model.py**是整个darknet的结构以及一些函数。这个算法实现步骤也许看着很简单，但过程很久，都是在报错中度过，慢慢去校正，也积累了一些报错的经验以及解决方法，最大的帮助是在于让我学会了通过多个编码软件和平台渠道去获取有关信息，复现这个算法也是我进步了不少。

