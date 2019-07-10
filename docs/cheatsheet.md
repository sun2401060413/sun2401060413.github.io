# CheatSheet

## 基本输入输出

### 图像读\写

```python
# 图像读入
image = cv2.imread(filename)
image = cv2.imread(filename,cv2.CV_LOAD_IMAGE_GRAYSCALE)
# 图像保存
cv2.imwrite(filename,image)
```  

### 视频读\写

```python
# 视频读入
cameraCapture = cv2.VideoCapture(0)			# 创建视频流对象
cameraCapture = cv2.VideoCapture(filename)
cameraCapture = cv2.VideoCapture(url)
success, frame = cameraCapture.read()		# 逐帧读入视频帧
# 视频保存
fourcc = cv2.VideoWriter_fourcc(c1,c2,c3,c4)                    	# 
videoWriter = cv2.VideoWriter(filename, fourcc, fps, frameSize)  	# 视频保存配置

```  

### 图像显示

```python
# 创建显示窗口
cv2.namedWindow(winname)
cv2.namedWindow(winname,flags)
# 显示图像
cv.imshow(winname,mat)
# 图像显示时长
cv2.waitKey()              # 表示等待用户操作.
cv2.waitKey(delay)         # delay为停顿毫秒数. 0表示等待用户操作.
```

## 基础图像处理


### 创建图像矩阵

```python
img = numpy.zeros((height,width,channels),numpy.uint8)
img = numpy.zeros_like(src_img)
```

### 图像矩阵数值操作

```python
img[x,y,z] = [v1,v2,v3]
img_cp = img.copy()		# 图像复制
```

### 图像色彩空间变换

```python
dst_img = cv2.cvtColor(src_img,flags)
dst_img = cv2.cvtColor(src_img,cv2.COLOR_BGR2GRAY)	# BGR转为灰度
```

### 几何变换

```python
# 图像缩放
dst_img = cv2.resize(src_img,(height,width))
# 垂直翻转
dst_img = cv2.flip(src_img, 0)
# 水平翻转
dst_img = cv2.filp(src_img, -1)
# 仿射变换(平移,缩放,旋转)
M = np.float32([[M11,M12,M13],[M21,M22,M23],[0,0,1]])
dst_img = cv2.warpAffine(src_img,M,(height,width))

```

### 图像分析

```python
# 图像分割
ret, dst_img = cv2.threshold(src_img, th, val, flags)
ret, dst_img = cv2.threshold(src_img, th, val, cv2.THRESH_BINARY)
# 边缘检测
dst_img = cv2.Canny(src_img,th1,th2)
dst_img = cv2.Canny(src_img,th1,th2,apertureSize,L2gradient)
# Hough变换
lines = cv2.HoughLines(src_img, 1, np.pi / 180, 100)
lines = cv2.HoughLines(src_img, rho, theta, th, min_theta, max_theta)
lines = cv2.HoughLinesP(src_img, 1, np.pi / 180, 100, 100, 20)
lines = cv2.HoughLinesP(src_img, rho, theta, th, minLineLength, maxLineGap)
```

### 图像绘制

```python
# 绘制直线
cv2.line(img,pt1,pt2,color,thickness)						# pt1为起点,pt2为终点
# 绘制矩形
cv2.rectangle(img,pt1,pt2,color,thickness)					# pt1为左上角,pt2为右下角
# 绘制圆形
cv2.circle(img,pt,radius,color,thickness)					# pt为圆点
cv2.circle(img,pt,2,color,1)
# 添加文字
cv2.putText(img,str,pt,cv2.font,fontScale,color,thickness)	# pt为左上角坐标
cv2.putText(img,str,pt,cv2.FONT_HERSHEY_SIMPLEX,2,color,1)
# 绘制多边形
cv2.polylines(img,[pts],isClosed,color,thickness)			# [pts]为点集
cv2.polylines(img,[pts],True,color,1)						#
# 填充多边形
cv2.fillPoly(img,[pts],color)								# [pts]为点集
```




