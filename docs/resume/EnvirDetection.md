# 高速公路环境状态辨识
??? info "版本信息"
	Created by 孙铸 at 2022-03-07 23:59, Version 1.0.0  


<link rel="stylesheet" href="https://g.alicdn.com/de/prismplayer/2.9.19/skins/default/aliplayer-min.css" />
<script type="text/javascript" charset="utf-8" src="https://g.alicdn.com/de/prismplayer/2.9.19/aliplayer-min.js"></script>

## 拥堵辨识数据增广
为解决基于数据驱动的高速公路异常交通状态辨识任务中异常场景（拥堵场景）数据量较少的问题，项目研究开发了一种拥堵场景数据增广程序，程序可通过实际道路的未拥堵状态场景下的图像序列生成异常交通状态场景数据，交通拥堵的程度以及拥堵发生的方向位置可由程序设定生成。

<div class="prism-player" id="player-con_1"></div>
<script>
var player = new Aliplayer({
  "id": "player-con_1",
  "source": "https://outin-9eecc16a9e9311ec83d500163e1c35d5.oss-cn-shanghai.aliyuncs.com/db292002d62344a592038ff856ec5137/64ab8383517a44349c464b18cc8bb740-358d440f94b4738b046f4481c320fcfd-ld.mp4",
  "width": "100%",
  "height": "500px",
  "autoplay": false,
  "isLive": false,
  "rePlay": false,
  "playsinline": true,
  "preload": true,
  "controlBarVisibility": "hover",
  "useH5Prism": true
}, function (player) {
    console.log("The player is created");
  }
);
</script>
<center>拥堵场景数据增广</center>

<div class="prism-player" id="player-con_2"></div>
<script>
var player = new Aliplayer({
  "id": "player-con_2",
  "source": "https://outin-9eecc16a9e9311ec83d500163e1c35d5.oss-cn-shanghai.aliyuncs.com/f1400e24567e46b79cb7e5bf215e219f/f77f8222e8a845e28695fcd63e01dda7-ld.mp4",
  "width": "100%",
  "height": "500px",
  "autoplay": false,
  "isLive": false,
  "rePlay": false,
  "playsinline": true,
  "preload": true,
  "controlBarVisibility": "hover",
  "useH5Prism": true
}, function (player) {
    console.log("The player is created");
  }
);
</script>
<center>大流量场景数据增广</center>

## 环境状态辨识
包括交通环境状态的辨识和气象环境状态的辨识。
![环境状态辨识](https://sunzhu-aliyun-oss-bucket.oss-cn-beijing.aliyuncs.com/img/00 天气辨识.png)
<center>环境状态辨识（气象环境状态和交通环境状态）</center>

![气象环境辨识](https://sunzhu-aliyun-oss-bucket.oss-cn-beijing.aliyuncs.com/img/00_天气检测.png)
<center>气象环境辨识可视化</center>


## 跨摄像机多目标连续跟踪
为辨识特大桥（项目中为杭州湾大桥）实时的、准确的交通荷载谱，需要获取特大桥上车辆的车型、车辆ID、位置、速度等参量。项目基于全程监控的摄像机阵列，对大桥上的车辆模板进行了跨监控区域的多个目标的检测与连续跟踪。

<div class="prism-player" id="player-con_3"></div>
<script>
var player = new Aliplayer({
  "id": "player-con_3",
  "source": "https://outin-9eecc16a9e9311ec83d500163e1c35d5.oss-cn-shanghai.aliyuncs.com/577bffeba6d14c6ebd331a957cd12858/ff49092ed59045bebb01258224d241d5-2d55d684dc95ee35c7475998905a1a70-ld.mp4",
  "width": "100%",
  "height": "500px",
  "autoplay": false,
  "isLive": false,
  "rePlay": false,
  "playsinline": true,
  "preload": true,
  "controlBarVisibility": "hover",
  "useH5Prism": true
}, function (player) {
    console.log("The player is created");
  }
);
</script>
<center>跨摄像机多目标连续跟踪</center>
