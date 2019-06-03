## GOTURN: Generic Object Tracking Using Regression Networks

### 亮点

GOTURN这个方法有如下的特点和优势：

- Offline Training：利用了大量数据离线训练的优势

- Generic Object Tracking：跟踪未见过的类别样例，对特定类别样例的跟踪效果更好

- Avoid Online Fine-turning：以获取100FPS的速度

- Regression-based Approach：不对patch进行classification，而是对object的bounding-box进行回归，以获得更高的FPS

### 算法框架

- 整个算法的框架其实非常简单：输入当前帧和前一帧进入网络，输出当前帧bounding-box的位置。

### 如何理解Regression

整个文章的关键点就是这，回归的是什么？当然是bounding-box的坐标，那么回归的输入变量就是current frame，输出为bounding-box的坐标。当然前提是知道previous frame中object的坐标在中心位置。那么这个Regression Network学习到的就是：object在视频中前后帧的motion到object坐标的变化！知道了object在前一帧的中心，找到object在当前帧的位置。

