# Classification 接口

??? info "版本信息"
	Created by 孙姣姣 2019-07-31 20:14 Version 1.0.1    

	Last Update：2019-08-01 08:37  

	* 修改文档格式;
	* 将图片更换为代码;



[Alexnet 代码下载](/2019_summer/deepLearning/alexnet.zip)

---



### DatalistGenerate 接口
从分好类的图片集(一类一个文件夹)，生成文件路径与对应标签列表。  

1. `DataFile`: 为类名，`imagePath`:  待生成图片集根目录（根目录下不要有其他文件和文件夹），`extensions`： 文件扩展名，`training_ratio`: 数据集中用于训练的样本比例，`validation_ratio`:  数据集中用于验证的样本比例，除训练集和验证集外，剩余的样本为测试集,训练集和验证集的总比例不能大于1。
```python
class DataFile:
	def __init__(self, imagePath, extensions, training_ratio, validation_ratio)
	...
```

2. 定义`resize_data`函数，用来读取训练集和测试集的label，以文件夹的名字为类别，在每个文件夹下面读取图片及其图片大小，扩展名。
 ```python
	...
	def resize_data(self, resize_savepath, shape):
	...
 ```

3. 在`rezised`文件夹下生成一个`.json`文件，用来记录训练数据、验证数据、测试数据、标签字典、训练集数量、验证集数量和测试集数量。（图片读取路径，标签，扩展名和识别后的类别）
```python
	...
	return print("Json file saved!")
```

---

### input接口
1. 定义一个类INPUT_DATA，类中定义了9个函数。
```python

class INPUT_DATA():
	...
```
2. `__init__`函数：输入路径、扩展名、训练比率、验证比率、图片尺寸、类型。		
```python
# INPUT_DATA构造函数
def __init__(self, filepath=None, extension=['.jpg','.png'], training_ratio=0.8, validation_ratio = 0.1, shape=(224, 224, 3), type='original')
```

3. `get_all_resized_data`函数和`get_all_original_data`函数分别用来获取原始数据集和经处理后图片大小为224*224的数据集信息（路径、扩展名、训练比率、验证比率和类型）。
```python
def get_all_resized_data(self):
	...
def get_all_original_data(self):
	...
```

4. `get_training_data`函数、`get_validation_data`函数和`get_testing_data`函数分别用来获取训练数据，验证数据和测试数据。
```python
def get_training_data(self):
	...
def get_validation_data(self):
	def __get_training_data(self):
		...
	def get_validation_data(self):
		...
	def get_testing_data(self)
		...

```
 
5. `get_label`函数：用来获取分类标签字典。
```python
def get_label(self):
	...
```


6. `cv_imread`函数：用来读取图片。
 ```python
 def cv_imread(filePath)
 ```

7. `input_original`函数和`input_resized`函数分别用来输入原始数据集和经处理后图片大小为224*224的数据集信息（路径、扩展名、训练比率、验证比率和类型）。
``` python
def input_original(filepath,extension,traning_ratio,validation_ratio,shape):
	...
def input_resized(filepath,extension,training_ratio,validation_ratio,shape):
	...
```

!!! 注释
	`cv2.imread`亦可实现图像读取功能, 但文件路径不能包含中文字符.此处采用自定义的`cv_imread`函数,目的是支持对包含中文路径文件的读入.


---

### loss接口
1. 定义一个类：`LossHistory`，`Callback`:回调函数，保存每个`batch`的`loss`、`acc`、`val_loss`、`val_acc`的回调函数,保存每个`epoch`的`loss`、`acc`、`val_loss`、`val_acc`的回调函数。


2. 在每个`epoch`的结尾处（on_epoch_end），`logs`将包含训练的正确率和误差率,`acc`和`loss`，如果指定了验证集，还会包含验证集正确率和误差率`val_acc`, `val_loss`，`val_acc`还额外需要在`.compile`中启用`metrics=['accuracy']`
```python 
def on_epoch_end(self, batch, log+{}):
	...
```
3. 在每个`batch`的结尾处（on_batch_end）：`logs`包含`loss`，若启用`accuracy`则还包含`acc`.
```python
def on_batch_end(self, batch, logs+{}):
	...
```
4. 定义一个函数`training_vis`：用来描绘训练过程。
```python
def training_vis(hist, savename):
	...
```

5. 分别用来绘制`loss`（`trainig_loss`和`val_loss`）和`acc`（`training_loss`和`val_loss`）值。
```python
ax1 = fig.add_subplot(121)
ax2 = fig.add_subplot(122)
```

---

### test_performance接口
1. 生成一个`.txt`文件，用来记录`precision_score`:准确率，`recall_score`：召回率，`f1_score`：统计学中用来衡量二分类模型精确度的一种指标，`classification_report`：分类报告，`confusion_matrix`:混淆矩阵。
```python
recorder_name = info_dict["savepath"]+"/record_"+part_string+".txt"
```

2. 生成一个`.png`文件，将训练结果用一个混淆矩阵来表示。
```python
confusion_matrix_name = info_dict["savepath"]+"/test"+"/confusion_matrix_"+part_string+".png"
```

3. 定义一个函数：`plot_confusion_matrix`，用来绘制一个混淆矩阵，包含类别、矩阵和保存名。
```python
def plot_confusion_matrix(classes,matrix,savename):
	...
```

---

### optimizers_settings接口
1. 定义一个函数：`get_optimizer`，对优化器算法进行设定。
```python
def get_optimizer(info_dict = {}):
	...
```

2. `SGD`:随机梯度下降法，`RMSProp`: 能够解决Adagrad学习率急速下降问题;  `Adagrad`:自适应梯度算法;  `Adadelta`:对Adagrad的扩展；`Adam`:自适应矩估计法;  `Adamax`：（Adam的一种变体）;  `Nadam`:（类似于带有Nesterov动量项的Adam）。
```python 
from keras.optimzers import SGD,RMSprop,Adagrad,Adadelta,Adam,Adamax,Nadam
```

3. `lr`:学习率，`decay`:衰减率，`momentum`:动量。
```python
opt = SGD(lr=1e-3,decay=1e-6,momentum=0.9,nesterov=True)
```

4. `lr`:学习率，`rho`：移动平均系数，`epsilon`:该参数是非常小的数，其为了防止在实现中除以0，阈值，用来确定是否进入检测值的“平原区”。
```python
opt = RMSprop(lr=1e-3,rho=0.9,epsilon=1e-06)
opt = Adagrad(lr=0.01,rho=0.9,epsilon=1e-06)
opt = Adadelta(lr=0.01,rho=0.9,epsilon=1e-6)
```

5. `lr`:学习率，`beta_1`：一阶矩估计的指数衰减率，`beta_2`: 二阶矩估计的指数衰减率， `decay`:衰减率。
```python
opt = Adam(lr=0.001,beta_1=0.9,beta_2=0.999,epsilon=None,decay=0.0,amsgrad=False)
opt = Adamax(lr=0.002,beta_1=0.9,beta_2=0.999,epsilon=None,decay=0.0)
opt = Nadam(lr=0.002,beta_1=0.9,beta_2=0.999,epsilon=None,schedule_decay=0.004)
```
