??? info "版本信息"
	Created by 卓叶迪 2019-10-12 20:07 Version 1.0.1    

	Last Update：2019-10-13 16:31  by 孙铸

	* 修改文档内容;
	
### 使用densenet训练网络


	
1. 由于网络要求输入图片的尺寸是224*224，所以需要先使用Datalist程序对图片进行resized：

```python
 d = DataFile(imagePath=r'E:\output\data',extensions=['.jpg', '.png'], training_ratio=0.8, validation_ratio=0.1)
 #imagepath为修改的图片的路径
 #extensions为读取的文件类型拓展名，这里为图片类型‘png’.‘jpg’
 #training_ratio 训练数据的比例
 #validation_ratio 验证数据的比例
 #一般设置训练：测试：验证的比例为0.8：0.1：0.1
 d.resize_data('E:/output/resized', 224)
 #设置保存路径，resize尺寸为224
```

### 程序使用的网络接口
> densenet网络引用的函数  

1.  callbacks：Callbacks用于指定在每个epoch开始和结束的时候进行哪种特定操作。Callbacks中有一些设置好的接口，可以直接使用，如’acc’, 'val_acc’, ’loss’ 和 ’val_loss’等.
2. EarlyStopping：EarlyStopping是Callbacks的一种，是用于提前停止训练的callbacks。

```python
  earlystop = EarlyStopping(monitor='val_loss', min_delta=0.001, patience=20, verbose=0, mode='auto', baseline=None)
'''
  #monitor: 监控的数据接口，有’acc’,’val_acc’,’loss’,’val_loss’等等。
  min_delta：增大或减小的阈值，只有大于这个部分才算作improvement。
  patience：能够容忍多少个epoch内都没有improvement。
  mode: 就’auto’, ‘min’, ‘,max’三个可能。设置上升的监控数据，'acc',可设置mode='max'
  verbose：日志显示，0为不在标准输出流输出日志信息，1为输出进度条记录，2为每个epoch输出一行记录 
''' 
```


3. ZeroPadding2D函数
```python
 x = ZeroPadding2D(padding = (1, 1), name='pool1_zeropadding')
 #(1, 1)会在行和列的最前和最后都增加一行0
```

4. Dense（全连接层）
```python
x_fc = Dense(1000, name='fc6')(x_fc)
#1000代表1000个输出（densenet网络的定的参数）
``` 

5. Dropout（过拟合）
```python
 x = Dropout(dropout_rate)
 #为输入数据施加Dropout。Dropout将在训练过程中每次更新参数时随机断开一定百分比（dropout_rate(0~1))）的输入神经元连接，Dropout层用于防止过拟合。
``` 

6. Activation（激活函数）
```python
 x = Activation('relu', name='relu1')
 #第一个参数为激活函数，程序选择了'relu'激活函数。
```

7. Concatenate（连接函数）
```python
concat_feat = Concatenate(axis=concat_axis, name='concat_' + str(stage) + '_' + str(branch))([concat_feat, x])
#axis参数axis设置连接的维度，根据维度方式选择确定
见[注释]
```


8. Conv2D（卷积层）
```python
 x = Conv2D(nb_filter, (7, 7), strides=(2, 2), name='conv1', use_bias=False)
 #nb_filter为卷积核的数目
 # (7, 7)为卷积核的尺寸
 #use_bias=False代表不使用偏置
```


9. MaxPooling2D（池化层）
```python
x = MaxPooling2D((3, 3), strides=(2, 2), name='pool1')
#(3, 3)池化的尺寸
#strides=(2, 2)池化步长
```


10. BatchNormalization（正则化）
```python
x = BatchNormalization(epsilon=eps, axis=concat_axis, name='conv1_bn')
#epsilon为大于0的小浮点数，用于防止除0错误
#axis: 整数，指定要规范化的轴，通常为特征轴。
```

---

!!! info "注释"
	在7中提到了维度方式的选择，维度方式主要有'tf''th'两种，关于维度方式的介绍如下：
	
	
```python
K.image_dim_ordering() == 'tf'
#选择'tf'维度方式，参数顺序为(samples, height, width, channels)
#选择'th'维度方式,参数顺序为(samples , channels，height, width)
```

---

### gpu的设置  

1. gpu的选择

```python
 os.environ['CUDA_VISIBLE_DEVICES'] = '0,1,2,3'
#设置选用几号gpu，后输入对应的编码
```


2. gpu设置参数
```python
config = tf.ConfigProto()
config.gpu_options.per_process_gpu_memory_fraction = 0.888     
#设置使用88.8%的显存
config.gpu_options.allow_growth = True
#动态申请显存
session = tf.Session(config=config)
set_session(session)
```  


### densenet网络模型

1. 定义densenet169_model所需的参数:
	- nb_dense_block: number of dense blocks to add to end
	- growth_rate: number of filters to add per dense block
	- nb_filter: initial number of filters
	- reduction: reduction factor of transition blocks
	- dropout_rate: dropout rate
	- weight_decay: weight decay factor
	- classes: optional number of classes to classify images
	- weights_path: path to pre-trained weights
	
	
```python
def densenet169_model(img_rows, img_cols,color_type=1, nb_dense_block=4, growth_rate=32, nb_filter=64, reduction=0.5, dropout_rate=0, weight_decay=1e-4, num_classes=None, weights_path=None, info_dict={})：
```


2. 定义conv_block所需的参数:
	- x: input tensor输入数量
	- stage: index for dense block——dense block的数
	- branch: layer index within each dense block--每个dense block的层数
	- nb_filter: number of filters--卷积核数
	- dropout_rate: dropout rate--dropout率
	- weight_decay: weight decay factor--权重衰减
	
	
```python
def conv_block(x, stage, branch, nb_filter, dropout_rate=None, weight_decay=1e-4):
```


3. 定义transition_block所需的参数：
	- x: input tensor
	- stage: index for dense block
	- nb_filter: number of filters
	- compression: calculated as 1 - reduction. Reduces the number of feature maps in the transition block.
	- dropout_rate: dropout rate
	- weight_decay: weight decay factor
	
	
```python
def transition_block(x, stage, nb_filter, compression=1.0, dropout_rate=None, weight_decay=1E-4):
```


4. 定义dense_block所需的参数:
	- x: input tensor
	- stage: index for dense block
	- nb_layers: the number of layers of conv_block to append to the model.
	- nb_filter: number of filters
	- growth_rate: growth rate
	- dropout_rate: dropout rate
	- weight_decay: weight decay factor
	- grow_nb_filters: flag to decide to allow number of filters to grow
	
	
```python
def dense_block(x, stage, nb_layers, nb_filter, growth_rate, dropout_rate=None, weight_decay=1e-4, grow_nb_filters=True):
```


### 生成记录文件

1.  生成.txt日志文件，用来记录训练参数：mode、filepath、savepath、weightpath、batch_size、extension、training_ratio、validation_ratio、size、channels、input_mode、epoches、classname、training samples、validate samples、test samples、Totally time cost、Test score和Test accuracy。


```python
recorder_name = info_dict["savepath"] + "/training_log_" + part_string + ".txt"
#savepath是变量名，在修改部分统一设置。
```


2. 生成记录loss和acc的表格.csv路径。


```python
csv_savename = info_dict["savepath"] + "/" + "Densenet_169_" + part_string + "_loss.csv"
```


3. 生成模型的.h5文件


```python
model_savename = info_dict["savepath"] + "/" + "Densenet_169_" + part_string + "_model.h5"
```

4. 生成记录loss和acc的.png文件


```python
fig_savename = info_dict["savepath"] + "/" + "Densenet_169_" + part_string + ".png"
```



### 使用程序修改的语句

1. 测试结果保存路径的修改

```python
if Y_predict[i]==0:
#测试时测试结果为0，自动分类到设置的文件夹
1pred = X_valid[i]
pred_img = array_to_img(pred)
pred_img.save('E:/new15/test/f0/' + 'new' + str(i) + '_T.' + str(Y_actual[i]) + '_P.' + str(Y_predict[i]) + '.jpg')
#此部分可以更改测试结果为'0'分类的路径
i += 1
```


2. 测试的输入输出设置

为了程序使用的方便，程序前部分都使用了变量名的方式，此处修改可以更改整个程序对应的变量。

```python
input_dict= {}
input_dict["mode"] = "train" 
#训练模式设置为'train'，测试模式设置为'test'
input_dict["filepath"] = 'E:/jam/data7/resized'
#读入的文件路径设置
input_dict["savepath"] = 'E:/jam/data7/results'
#保存结果的路径设置
input_dict["weightpath"] = 'E:/program/CNN/keras_weights'
#权重路径，训练模式为下载好的训练权重；测试模式时为训练的模型
input_dict["batch_size"] = 8 
#同时处理的图片个数
input_dict["extension"] = [".jpg", ".png"]
#读取的文件的类型格式
input_dict["training_ratio"] = 0.8  
#训练比例一般0.8：0.1：0.1
input_dict["validation_ratio"] = 0.1
#验证比例为0.1
input_dict["size"] = 224  
#图片的尺寸
 input_dict["channels"] = 3  
#图片的通道
 input_dict["epoches"] = 20  
#迭代次数
input_dict["classname"] = ["0", "1"]
#设置分类类名，尽量不用英文
```

---
!!! info "注释"
	运行程序时候会自动读入文件路径的Json文件，为了不影响程序的运行，需要提前删除设置的Json文件。





