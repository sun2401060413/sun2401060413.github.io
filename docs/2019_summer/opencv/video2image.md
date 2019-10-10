# opencv 常用程序（一）

??? info "版本信息"
	Created by 张志刚 2019-09-25 20:14 Version 1.0.1    
	Last Update：2019-09-27 10:01

---

本页介绍了利用Opencv进行：  
 
* 视频分解为图片;
* 给图片打马赛克;
* 图片缩放;
* 绘制直方图;
* 绘制掩模直方图;

---

### 视频分解为图片

**代码示例**
```python
'''
Example: 视频分解为图片
'''
import cv2

# 视频读入
# 读入本地视频文件
cap=cv2.VideoCapture("path")

# 判断能否读取视频
isOpened=cap.isOpened
print("Video is opened:", isOpened)

#获取总帧数
total=cap.get(cv2.CAP_PROP_FRAME_COUNT)
Print(total)

#获取每秒帧数
fps=cap.get(cv2.CAP_PROP_FPS)
print(fps)

# 获取每一帧图片的宽高
width=int(cap.get(cv2.CAP_PROP_FRAME_WIDTH))
height=int(cap.get(cv2.CAP_PROP_FRAME_HEIGHT))
print(width,height)
i=0
# 设置间隔帧数
apart=xx
while (isOpened):
    # 设置终止帧数
    if i==total:
        break
    else:
        i=i+1
    # 从视频流中读取一帧图像
    (frag,frame)=cap.read()
	#给读取出的图像命名
    filename="image"+str(i)
    if frag == True and i%apart==0:
         print(filename)
         # 图片保存
         cv2.imwrite('savepath',frame)
print("end")
```

**关键语句**


1- 视频读入

```python
# 视频读入，导入本地视频文件，path为本地视频文件地址
cap=cv2.VideoCapture("path")
```
  
  
2- 获取视频信息

```python
# 判断能否读取视频
isOpened=cap.isOpened
print(isOpened)#若不能读取，则打印出的是Flase

#获取总帧数
total=cap.get(cv2.CAP_PROP_FRAME_COUNT)
Print(total)

#获取每秒帧数
fps=cap.get(cv2.CAP_PROP_FPS)
print(fps)

# 获取每一帧图片的宽高
width=int(cap.get(cv2.CAP_PROP_FRAME_WIDTH))
height=int(cap.get(cv2.CAP_PROP_FRAME_HEIGHT))
print(width,height)

``` 

3- 设置间隔帧数apart，和终止帧数i  

```python
# 设置间隔帧数
apart=xx
```
  
  
```python
# 设置终止帧数，若大于总帧数，则提前停止
if i==xx:
	break
```

4- 图片保存，savepath为保存地址

```python
# 图片保存
cv2.imwrite('savepath',frame)
```

!!! 注意
	1. 文件读入与保存路径不要包含中文字符;

---

### 给图片打马赛克

**代码示例**

```python
'''
Example:给图片打马赛克
'''
import cv2
img=cv2.imread('imagepath')		#读取图片，imagepath为图片地址
cv2.imshow('src',img)			#显示原图片，src可自己命名
imginfo=img.shape				#获取图片信息
height=imginfo[0]
width=imginfo[1]
mode=imginfo[2]
print(height,width,mode)
(x1,y1)=(100,100)				#设置马赛克起始位置
(x2,y2)=(300,300)				#设置马赛克结束为止
degree=10						#设置马赛克程度
for m in range(x1,x2):
    for n in range(y1,y2):
        if m%degree==0 and n%degree==0:
            for i in range(0,degree):
                for j in range(0,degree):
                    (b,g,r)=img[m,n]
                    img[i+m,j+n]=(b,g,r)
cv2.imshow('2',img)
cv2.waitKey(0)
```

**关键语句**

1- 设置马赛克位置，根据实际需求自己设定

 ```python
(x1,y1)=(100,100)#设置马赛克起始位置
(x2,y2)=(300,300)#设置马赛克结束位置
 ```

2- 设置马赛克程度，10表示10*10的像素进行像素同一化

 ```python
degree=10#设置马赛克程度
 ```
 
---


### 图片缩放

**代码示例**

```python
'''
Example:图片缩放
'''
import cv2
img=cv2.imread('imagepath')				#读取图片，imagepath为图片地址
cv2.imshow('src',img)					#显示原图片，src可自己命名
imginfo=img.shape						#获取图片信息
width=imginfo[0]
height=imginfo[1]
mode=imginfo[2]
print(width,height,mode)				#打印图片信息
dstwidth=500							#设置期望的图片宽度
dstheight=500							#设置期望的图片高度
dst=cv2.resize(img,(dstwidth,dstheight))#将图片缩放到期望尺寸
cv2.imshow('dst',dst)					#显示处理后的照片，dst可自己命名
cv2.waitKey(0)
```

**关键语句**

1- 设置期望的图片尺寸，根据实际需求自己设定

 ```python
dstwidth=500							#设置期望的图片宽度
dstheight=500							#设置期望的图片高度
 ```
 

2- 对图片进行缩放

 ```python
dst=cv2.resize(img,(dstwidth,dstheight))#将图片缩放到期望尺寸
 ```
 
---


### 绘制直方图

**代码示例**

```python
'''
Example:绘制直方图
'''
import cv2
import matplotlib.pyplot as plt
img=cv2.imread('imagepath')				#导入图片
histb=cv2.calcHist([img],[0],None,[256],[0,255])
histg=cv2.calcHist([img],[1],None,[256],[0,255])
histr=cv2.calcHist([img],[2],None,[256],[0,255])
plt.plot(histb,color='b')
plt.plot(histg,color='g')
plt.plot(histr,color='r')
plt.show()
```

**关键语句**

1- 设置直方图
 
```python
histb=cv2.calcHist([img],[0],None,[256],[0,255])
histg=cv2.calcHist([img],[1],None,[256],[0,255])
histr=cv2.calcHist([img],[2],None,[256],[0,255])
#第一个参数“[img]”表示要绘制直方图的原始图像，是使用“[]”括起来的。
#第二个参数表示要统计哪个通道的直方图。
#第三个是掩模图像，在本例中为“None”，表示计算整幅图像的直方图。
#第四个参数“[256]”表示BINS的值是256。BINS的具体指，表示灰度级的分组情况。
#第五个参数“[0，255]”表示灰度级的范围是[0,255]。
```
 
2- 绘制直方图

```python
plt.plot(histb,color='b')		#“color=’b’”表示绘图颜色是蓝色
plt.plot(histg,color='g')		#“color=’g’”表示绘图颜色是绿色
plt.plot(histr,color='r')		#“color=’r’”表示绘图颜色是红色
```
 
 ---

### 绘制掩模直方图

**代码示例**

```python
'''
Example:绘制掩模直方图
'''
import cv2
import numpy as np
import matplotlib.pyplot as plt
img=cv2.imread('imagepath')
gray=cv2.cvtColor(img,cv2.COLOR_BGR2GRAY)
print(img.shape)
mask=np.zeros(gray.shape,np.uint8)					#配置一个掩模矩阵
mask[10:100,20:100]=255
histb=cv2.calcHist([gray],[0],None,[256],[0,255])	#绘制原始直方图
histb1=cv2.calcHist([gray],[0],mask,[256],[0,255])	#绘制掩模直方图
plt.plot(histb,color='b')
plt.plot(histb1,color='g')
plt.show()
```

**关键语句**

1- 设置掩模矩阵  


```python
mask=np.zeros(gray.shape,np.uint8)	#配置一个掩模矩阵
mask[10:100,20:100]=255				#此例子中为（10，20）至（100，100），可根据实际情况自定义
```
 
2- 绘制直方图  


```python
histb=cv2.calcHist([gray],[0],None,[256],[0,255])	#绘制原始直方图
histb1=cv2.calcHist([gray],[0],mask,[256],[0,255])	#绘制掩模直方图
```