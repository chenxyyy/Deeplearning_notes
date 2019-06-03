## SGBM算法

整个算法实现分为

1. 预处理

2. 代价计算

3. 动态规划（默认4条路径）

4. 后处理


### 预处理

Step1：SGBM采用水平Sobel算子，把图像做处理，公式为：

Sobel(x,y)=2[P(x+1,y)-P(x-1,y)]+ P(x+1,y-1)-P(x-1,y-1)+ P(x+1,y+1)-P(x-1,y+1)

Step2：用一个函数将经过水平Sobel算子处理后的图像上每个像素点（P表示其像素值）映射成一个新的图像：PNEW表示新图像上的像素值。

### 代价计算

代价有两部分组成：

1. 经过预处理得到的图像的梯度信息经过基于采样的方法得到的梯度代价

2. 原图像经过基于采样的方法得到的SAD代价

### 动态规划

规划公式：

https://img-blog.csdn.net/20160709143639689?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center

默认4条路径，其中动态规划很重要两个参数P1，P2是这样设定的：

> P1 =8*cn*sgbm.SADWindowSize*sgbm.SADWindowSize;

> P2 = 32*cn*sgbm.SADWindowSize*sgbm.SADWindowSize;

cn是图像的通道数, SADWindowSize是SAD窗口大小，数值为奇数。

可以看出，当图像通道和SAD窗口确定下来，SGBM的规划参数P1和P2是常数。

Step1：唯一性检测：视差窗口范围内最低代价是次低代价的(1 + uniquenessRatio/100)倍时，最低代价对应的视差值才是该像素点的视差，否则该像素点的视差为0。其中uniquenessRatio是一个常数参数。
Step2：亚像素插值：
Step3：左右一致性检测：误差阈值disp12MaxDiff默认为1，可以自己设置。
Step4：连通区域的检测：简述：对左右一致性检测后的视差图再一次检测误匹配点，根据与当前处理的视差点满足连通条件的像素点个数来判断当前处理的视差点是否是误匹配点，个数小于一个阈值就认为是误匹配点。

方法：循环遍历每一个像素点，对每一个视差像素点d而言，检测其周围（上下左右）的视差是否满足这样的条件（称为视差连通条件）：

1. 首先是LRcheck后，视差有效的点

2. 和中心视差值的（变化）绝对值不超过speckleRange。注：speckleRange是一个常数参数，可以自己设定。Opencv例程中speckleRange=10.
