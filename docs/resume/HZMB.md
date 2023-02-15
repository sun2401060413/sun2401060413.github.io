# 三维数字孪生应急演练系统
??? info "版本信息"
	Created by 孙铸 at 2022-03-06 23:59, Version 1.0.0  

## 简介
此页主要展示“浙江省黄衢南高速达坞隧道应急演练系统”效果。此应急演练系统基于Unity3D开发,采用实际路段的地势地貌、道路线型以及设备设施数据构成了三位数字孪生系统。并可以与实际道路监控系统进行联动，从而模拟各种事故事件下，通过监控系统对道路沿线设备设施的控制，演练应急事件处置中的调度指挥与交通组织等决策的实施效果。
### 观察模式
观察模式即观看演练场景和事件的摄像机视角以及移动模式，包括三种：漫游模式、驾驶模式和巡线模式。  

![观察模式效果](https://sunzhu-aliyun-oss-bucket.oss-cn-beijing.aliyuncs.com/img/00 摄像机模式.png)
<center>观察模式效果</center>

### 光照变化
演练系统可根据时间变化光照状态。  

![光照变化效果](https://sunzhu-aliyun-oss-bucket.oss-cn-beijing.aliyuncs.com/img/01 光照模式.png)
<center>光照变化效果</center>

### 天气模式
演练系统可设置晴、多云、雨、雪等多种天气类型。  

![天气状态效果](https://sunzhu-aliyun-oss-bucket.oss-cn-beijing.aliyuncs.com/img/02 天气模式.png)
<center>天气状态效果</center>

### 事故模式
演练系统可模拟不同类型事故事件，如车辆追尾、起火、危化品事故。  

![事故模式](https://sunzhu-aliyun-oss-bucket.oss-cn-beijing.aliyuncs.com/img/04 事故模式.png)
<center>事故模式</center>

### 设备设施
仿真系统可部署高速公路常见设备设施，如情报板、可变限速标志、信号灯、车道指示灯、风机、栏杆机等。  

![设备设施](https://sunzhu-aliyun-oss-bucket.oss-cn-beijing.aliyuncs.com/img/03 机电设备.png) 
<center>设备设施</center> 

各个设备设施的工作状态，可基于TCP的通信协议，通过另外一台计算机或另外的控制系统进行控制。   

![设备联机](https://sunzhu-aliyun-oss-bucket.oss-cn-beijing.aliyuncs.com/img/FreewayEquipments.png)
<center>设备联机</center> 

### 模拟驾驶
演练系统可基于Logitech模拟驾驶设备进行模拟驾驶仿真。  

![模拟驾驶](https://sunzhu-aliyun-oss-bucket.oss-cn-beijing.aliyuncs.com/img/CarDriveSimulation_2.png)
<center>模拟驾驶</center> 

### 多客户端联机
演练系统支持基于Didicated模式（专用服务器模式，服务端与客户端不在一个系统实例上）和Host模式（主机服务器模式，服务器与客户端可以在一个系统实例上）的多客户端联机。  

![多客户端](https://sunzhu-aliyun-oss-bucket.oss-cn-beijing.aliyuncs.com/img/06 多客户端联机.png)
<center>多客户端联机</center>

### 视频推流
演练系统支持不同摄像机视角推送RTMP/RTSP流媒体协议视频流的功能、以及视频的录制/播放等多媒体功能。  

![视频推流](https://sunzhu-aliyun-oss-bucket.oss-cn-beijing.aliyuncs.com/img/05 推流效果.png)  
<center>视频推流</center>
