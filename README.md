# 直方图图像增强实验报告
#### 自动化66 杨德宇 2161500050
#### 摘要
&emsp;&emsp;本文利用MATLAB软件的ind2gray函数将附件所给的索引图像转化为灰度图像，接着使用imhist函数绘制出了附件所给14幅图像的灰度分布直方图，
并用histeq函数对其进行了均衡化处理，分析并对比了与原图像的差异；进一步把图像按照对源图像直方图的观察，各自自行指定不同源图像的直方图，
进行直方图匹配增强并对比了效果；接着对elain和lena图像进行了7*7的局部直方图增强；最后利用直方图对其进行了分割。
#### 关键词  MATLAB 灰度直方图 直方图增强 图像分割
#### 一.直方图绘制
&emsp;&emsp;灰度直方图是关于灰度级分布的函数，是对图像中灰度级分布的统计。横坐标为灰度级，纵坐标为该灰度级像素点的个数。
由于附件所给图像格式均为索引图像格式，首先利用MATLAB中的`ind2gray()`函数将索引图像转化为灰度图像，代码如下：
``` 
[f,map]=imread(filepath);
image=ind2gray(f,map);
```
&emsp;&emsp;然后利用`imhist`函数,调用格式为`imhist(image,n)`,n为灰度级，默认为256级;

&emsp;&emsp;处理得到附件中14幅图像的灰度直方图，如下图所示。

<img src="https://github.com/poisonwine/hw3/blob/master/%E5%9B%BE%E7%89%87/citywall.bmp" width="400"/> <img src="https://github.com/poisonwine/hw3/blob/master/%E5%9B%BE%E7%89%87/citywall1.bmp" width="400"/>
<img src="https://github.com/poisonwine/hw3/blob/master/%E5%9B%BE%E7%89%87/citywall2.bmp" width="400"/> <img src="https://github.com/poisonwine/hw3/blob/master/%E5%9B%BE%E7%89%87/elain.bmp" width="400"/>
<img src="https://github.com/poisonwine/hw3/blob/master/%E5%9B%BE%E7%89%87/elain1.bmp" width="400"/> <img src="https://github.com/poisonwine/hw3/blob/master/%E5%9B%BE%E7%89%87/elain2.bmp" width="400"/>
<img src="https://github.com/poisonwine/hw3/blob/master/%E5%9B%BE%E7%89%87/elain3.bmp" width="400"/> <img src="https://github.com/poisonwine/hw3/blob/master/%E5%9B%BE%E7%89%87/lena.bmp" width="400"/>
<img src="https://github.com/poisonwine/hw3/blob/master/%E5%9B%BE%E7%89%87/lena1.bmp" width="400"/> <img src="https://github.com/poisonwine/hw3/blob/master/%E5%9B%BE%E7%89%87/lena2.bmp" width="400"/>
<img src="https://github.com/poisonwine/hw3/blob/master/%E5%9B%BE%E7%89%87/lena4.bmp" width="400"/> <img src="https://github.com/poisonwine/hw3/blob/master/%E5%9B%BE%E7%89%87/woman.bmp" width="400"/>
<img src="https://github.com/poisonwine/hw3/blob/master/%E5%9B%BE%E7%89%87/woman1.bmp" width="400"/> <img src="https://github.com/poisonwine/hw3/blob/master/%E5%9B%BE%E7%89%87/woman2.bmp" width="400"/>
#### 二.直方图均衡
&emsp;&emsp;直方图均衡化是一种利用灰度变换自动调节图像对比度质量的方法，基本思想是通过灰度级的概率密度函数求出灰度变换函数，它是一种以累计分布函数变换法为基础的直方图修正法。
#### 2.1
&emsp;&emsp;当输入直方图H(r)(此处指每个灰度级占有的像素数);灰度级范围[r0,rk]；目的是找到一个s=T(r)使得输出图像的直方图G(s)在整个灰度级范围内均匀分布。且需满足:
* 0——L(灰度范围)单调递增，避免黑白颠倒
* 0<r<L,时0<s<L，保持动态范围一致

&emsp;&emsp;累积分布函数需要满足以下要求
<img src="https://github.com/poisonwine/hw3/blob/master/%E5%9B%BE%E7%89%87/%E5%85%AC%E5%BC%8F%E4%B8%80.png" width="150"/>;
&emsp;&emsp;转化为离散形式为
<img src="https://github.com/poisonwine/hw3/blob/master/%E5%9B%BE%E7%89%87/%E5%85%AC%E5%BC%8F2.png" width="250"/>;

&emsp;&emsp;一般来说
<img src="https://github.com/poisonwine/hw3/blob/master/%E5%9B%BE%E7%89%87/%E5%85%AC%E5%BC%8F3.png" width="180"/>;

#### 2.2

&emsp;&emsp;直方图均衡化处理的步骤如下：
* 求出给定待处理图像的直方图;
* 利用累计分布函数对原图像的统计直方图做变换，得到新的图像灰度;
* 进行近似处理，将新灰度代替旧灰度，同时将灰度值相等或相近的每个灰度直方图合并在一起

&emsp;&emsp;根据算法原理编写的函数见`源代码.txt`文件；MATLAB中自带函数`histeq`是专门用于直方图均衡的函数，调用格式为`f=histeq(image,n)`，n
为输入的灰度级数，默认为64。

#### 2.3 MATLAB自带函数与自编函数效果对比

<img src="https://github.com/poisonwine/hw3/blob/master/%E5%9B%BE%E7%89%87/lena%E5%AF%B9%E6%AF%94.bmp" width="435"/><img src="https://github.com/poisonwine/hw3/blob/master/%E5%9B%BE%E7%89%87/elain%E5%AF%B9%E6%AF%94.bmp" width="435"/>

&emsp;&emsp;可以观察到，自编函数的效果与MATLAB自带函数运行效果相同，将两个处理后的图像做差，所得图像矩阵基本为全0，故验证了直方图均衡化算法的正确性。

#### 2.4 均衡化结果
&emsp;&emsp;使用MATLAB自带函数对附件图像直方图进行均衡化前，仍需要先将索引图像转化为灰度图像，然后再使用`histeq`函数进行直方图均衡化。均衡化的图像结果如下列图所示：


