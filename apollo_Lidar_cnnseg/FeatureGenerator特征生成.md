# 特征生成器feature_generator.cc
```
该函数的作用：
将raw cloudpoint 生成为caffe model模型所需要的特征数据，因为点云的格式为``[x,y,z，intensity]``无序的，而cnnseg是将点云数据投影处理，生成bird-view输入到网络进行预测，接下来看具体过程。粗略看了一下Apollo3.5的模块，lib里还有cnnseg，应该没有多大改动
```
## 1.先来看看为什么要bird-view
大家熟知的cnn一般输入都是有序的张量（可以理解为多维度的矩阵）作2D卷积，但是作为无序的点云数据结构[x,y,z,intensity]是否可以直接作为输入呢？答案虽然是可以的，但是往往部分研究者会将其转化为体素网格作3d卷积，虽然有效的保留各个点云在空间的相互关系，但是数据冗余，浪费计算资源。还有学者就是将点云转为多视图投影方法，将投影图作为输入训练网络模型，cnnseg就是其一。但是否有更好的方法，能否将点云数据作为直接输入呢？答案再次是肯定的，请参考Stanford学者提出的pointnet模型
### 1.1.1 投影原理 
明显，将（x,y）作为网格坐标，将z作为数据即为投影。想象，常用的图片数据格式为m行n列多通道，每个单元格存的强度值就可以当做是我们现在的高度值z或者反射强度值intensity。但是z这么多(激光雷达的扫描分辨率假设为0.1m一个，如果10m就是100个，往往激光雷达的范围为几十米，我们要投射的网格岂不是要几千×几千)分辨率要这么高吗？很明显不是的，为了提高计算效率，我们设定大小为有限的单元格，Apollo默认的为640*640，Lidar ROI[-60 60m]，因此没个格子就代表60/640米，这样很多点就会投影到同一个单元格中，如何合理的提取出这些特征，因此feature_generator要做的就是此
### 1.1.2 坐标转化
[-60 60] ---> [0 640]
[-a a] ---> [0 b]
因此可得：
$$ idx=floor[\frac{0.5b}{a}*(b-x)]$$   
``floor()``表示向下取整    
如当x=60时，idx=0,即第1行   
当x=-60时，idx=639 第640行
```
0.5 * static_cast<float>(width_) / static_cast<float>(range_); //宽度分辨率
int pos_y = F2I(points[i].x, range_, inv_res_y);  // row  
int F2I(float val, float ori, float scale) {
  return static_cast<int>(std::floor((ori - val) * scale));
}
```
注意Apollo中的坐标转换刚好将x投射到row，y投射到列，caffe中的输入格式为n*c*h*w(批量数×通道数×高度×宽度)，相当于将x,y transpose了一下

 > below : cite from caffe 
``` 
The conventional blob dimensions for batches 
of image data are number N x channel 
K x height H x width W. Blob memory is row-major in layout, 
so the last / rightmost dimension changes fastest. 
For example, in a 4D blob, the value at index (n, k, h, w) is physically located at 
index ((n * K + k) * H + h) * W + w.
```
``转化过程看图吧(暂不ppt painting了)``
![坐标转换](../pictures/坐标转换.jpg)
 <center>图 1 坐标转换</center>

### 1.1.3 各个通道数据
```

```