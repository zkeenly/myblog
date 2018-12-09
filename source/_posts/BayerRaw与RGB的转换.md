---
title: BayerRaw与RGB的转换
date: 2018-11-18 14:20:24
categories: 图像处理
tags: 
	- 图像处理
	- python
comments: true
---
代码：![code](https://github.com/zkeenly/articles/blob/master/RAW2RGB.py)
本文采取sony拍摄的arw格式图像测试，测试图像在![论文](https://github.com/cchen156/Learning-to-See-in-the-Dark)中下载：

bayer图像格式
![image](https://user-images.githubusercontent.com/6647857/47952226-e8512f80-dfa6-11e8-8a7a-facbd94c30e6.png)
由于人眼对GREEN颜色的感知更为强烈，所以一般raw 格式的图像每个2*2矩阵中，都有两个G分量，一个R以及一个B分量，G分量的排列方式可以有所不同，如下图：
<img width="302" alt="image" src="https://user-images.githubusercontent.com/6647857/47952815-36b6fc00-dfb0-11e8-999f-519c9f004b59.png">

对于转换为RGB格式，一般采取插值的方法，将原始图像中的R/G/B三个分量，插值到附近，分别产生新的三个矩阵，分别为R,G,B。
<img width="117" alt="image" src="https://user-images.githubusercontent.com/6647857/47952791-db850980-dfaf-11e8-847b-0751b4fb98b8.png">
以下为代码，对于两个G分量采用均值方法优化插值：

<img width="423" alt="image" src="https://user-images.githubusercontent.com/6647857/47952218-c0fa6280-dfa6-11e8-9ce1-1feb84a3008f.png">


1-自己写的函数最终得到图像结果：
放大：
<img width="600" alt="image" src="https://user-images.githubusercontent.com/6647857/47952892-f0ae6800-dfb0-11e8-9bbb-51b472db5740.png">
原图：
<img width="600" alt="image" src="https://user-images.githubusercontent.com/6647857/47952822-4a626280-dfb0-11e8-8d16-fcea9ab15813.png">

2-使用sony插件直接查看arw图像的结果：
放大：
<img width="600" alt="image" src="https://user-images.githubusercontent.com/6647857/47952873-d07ea900-dfb0-11e8-9cd6-406e80d583b0.png">
原图：
<img width="600" alt="image" src="https://user-images.githubusercontent.com/6647857/47952832-6f56d580-dfb0-11e8-9277-4f973f0e8fb8.png">

3-使用python rawpy库函数的postprocess 方法默认参数处理：
放大：
<img width="600" alt="image" src="https://user-images.githubusercontent.com/6647857/47952898-00c64780-dfb1-11e8-974c-28adb36d771e.png">
原图：
<img width="600" alt="image" src="https://user-images.githubusercontent.com/6647857/47952854-990ffc80-dfb0-11e8-9c54-83e15026a3e5.png">

4-使用learning to see in dark 中的postprocess 方法所设置的参数处理：
rgb = raw.postprocess(use_camera_wb=True, half_size=False, no_auto_bright=True, output_bps=16)
放大：
<img width="600" alt="image" src="https://user-images.githubusercontent.com/6647857/47952903-0f146380-dfb1-11e8-82b4-4ef52cd2c49e.png">
原图：
<img width="600" alt="image" src="https://user-images.githubusercontent.com/6647857/47952865-c0ff6000-dfb0-11e8-98dd-013d85bb501a.png">


最终分析结果：
再亮度上：
2>3>4>1

在视觉效果上：
4>2>3>1
在清晰度上：
4=3>2=1
在还原RGB方面，使用参数调节后的rawpy.postprocess方法（use_camera_wb）更好。
造成以上差异的主要因素还是插值方法的不同：
在文章HIGH-QUALITY LINEAR INTERPOLATION FOR DEMOSAICING OF BAYER-PATTERNED COLOR IMAGES 中介绍了更好的插值方法：
<img width="357" alt="image" src="https://user-images.githubusercontent.com/6647857/47953171-c9599a00-dfb4-11e8-8c12-5343fff0d97f.png">

引用：
https://letmaik.github.io/rawpy/api/rawpy.Params.html
https://www.microsoft.com/en-us/research/wp-content/uploads/2016/02/Demosaicing_ICASSP04.pdf
http://www.imatest.com/docs/raw/
http://blog.sina.com.cn/s/blog_ebbe6d790101e56e.html
https://blog.csdn.net/peng864534630/article/details/78177211
https://www.cnblogs.com/zhongguo135/p/7755287.html
Learning-to-see-in-draking

