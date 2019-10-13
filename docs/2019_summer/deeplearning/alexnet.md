### AlexNet网络
1. 在`AlexNet`网络程序运行之前要接入`loss`、`test_performance`、`flags`、`input`、`optimizers_settings`这五个模块。
```python
import loss
import test_performance as tp
import flags
import input
import optimizers_setting
```

2. AlexNet网络要求输入的图片大小为`224*224`。
3. 生成`.txt`日志文件，用来记录训练参数：`mode`、`filepath`、`savepath`、`weightpath`、`batch_size`、`extension`、`training_ratio`、`validation_ratio`、`size、channels`、`input_mode`、`epoches`、`classname`、`training samples`、`validate samples`、`test samples`、`Totally time cost`、`Test score`和`Test accuracy`。
```python
# 训练参数配置记录
recorder_name = info_dict["savepath"] + "/training_log_" + part_string + ".txt"
``` 

4. 生成一个表格，用来记录loss和acc值。
 ```python
 # 训练过程数据记录
 csv_savename = info_dict["savepath"] + "/" + "AlexNet_" + part_string + "_loss.csv"
 ```

5. 生成一张图片，用来训练集和验证集的loss和acc值。
 ```python
 # 训练过程数据绘制图形
 fig_savename = info_dict["savepath"] + "/" + "AlexNet_" + part_string + ".png"
 ```

6. 生成一个.h5文件，用来保存训练模型。
 ```python
 # 训练保存模型
 model_savename = info_dict["savepath"] + "/" + "AlexNet_" + part_string + "_model.h5"
 ```

7. 当前模式为训练模式。
 ```python
 # 运行模式配置
 input_dict["mode"] = "train"
 ```

8. `‘r’`用来切换正斜杠和反斜杠，`‘I:\lvtong1\resized’`是你自己数据集（图片大小为`224*224`）。`‘I:\lvtong1\results’`用来保存你训练的结果，在此程序中会生成`.png`、`.csv`、`.h5`、`.txt`文件。
```python
# 数据集读取地址配置
input_dict["filepath"] = r'I:\lvtong1\resized'
# 模型保存地址配置
input_dict["savepath"] = r'I:\lvtong1\results'
```

9. 迁移权重的读取地址。
```python
# 迁移权重读取根目录
input_dict["weightpath"] = 'F:/sz/keras_weights'
```

10. `‘batch_size’`：单次训练用的样本数；`‘extention’`:扩展名；`‘training_ratio’`：训练集占样本总量的比率；`‘validation_ratio’`:验证集占样本总量的比率；训练集、验证集和测试集的比率一般为：8：1：1。
`‘size’`:图片大小；`‘channels’`: 通道数；`‘input_mode’`:   输入模式；`‘epoches’`:训练轮；`‘classname’`:分类的类名。
 ```python
 input_dict["batch_size] = 16
 input_dict["extension"] = [".jpg",".png"]
 ipnut_dict["size"] = 224
 input_dict["channels"] = 3
 input_dict["input_mode"] = "resized"
 input_dict["epoches"] = 10
 input_dict["classname"] = ["name_1","name_2","name_3",...,"name_N"] # 根据实际数据集配置,注:不要使用中文

 ```

---







