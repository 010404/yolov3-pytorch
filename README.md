# yolov3训练自己的数据集(权重)

##### 总体上是代码复现，然后用自己查找的图片和xml进行训练

------



### [makeTxt.py](https://github.com/010404/yolov3/blob/master/makeTxt.py)

**yolov3**中的**makeTxt.py**是将数据集分为训练集(training data)和测试集(test data)

​		比例分别为0.9和0.1

```
trainval_percent=0.1
train_percent=0.9
```



在ImageSets中得到四个文件：

![](https://github.com/010404/yolov3/tree/master/picture/001.png)



