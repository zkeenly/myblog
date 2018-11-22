---
title: CNN中感受野的计算方法
date: 2018-11-18 14:01:07
categories: 深度学习
tags: 
	- 深度学习
comments: true
---
简单的说，感受野就是当前卷积层每个像素点是从多少个input层的像素点通过不断卷积得到的。
一般卷积网络越深，每个像素的感受野就越大（反卷积则相反）， 这在目标识别/检测网络设计中起到一个很重要的参考因素。

计算公式：
<img width="759" alt="image" src="https://user-images.githubusercontent.com/6647857/48189318-ddb5e200-e37a-11e8-8b1c-fd98ffdb6c89.png">

<img width="761" alt="image" src="https://user-images.githubusercontent.com/6647857/48189462-3e451f00-e37b-11e8-882c-52e7e5030f5d.png">
假设有一个5*5的input层
通过kernel=3*3，padding=1，stride=2 的卷积方式
将会得到3*3的layer1.（下图为layer1元素对应在input layer中的位置）
<img width="167" alt="image" src="https://user-images.githubusercontent.com/6647857/48199662-c802e580-e397-11e8-95e5-7acf7519d567.png">
其中，每一个元素都是input由3*3的kernel卷积生成，所以每一个像素的感受野为3

<img width="712" alt="image" src="https://user-images.githubusercontent.com/6647857/48189781-e8bd4200-e37b-11e8-8864-9ba4a98eaac7.png">
将此层再通过kernel=3 * 3，padding=1，stride=2的卷积方式卷积。
将会得到2 * 2的layer2。
<img width="156" alt="image" src="https://user-images.githubusercontent.com/6647857/48199704-ed8fef00-e397-11e8-9ef0-e47e1a5e3234.png">

此时，每一个元素都是由layer1的3 * 3个元素通过3 * 3的卷积核生成。
所以layer2的每个元素感受野为3 * 3个layer1元素的感受野。
然而layer1元素在原始数据上对应位置的排列如图：
<img width="167" alt="image" src="https://user-images.githubusercontent.com/6647857/48199662-c802e580-e397-11e8-95e5-7acf7519d567.png">
相当于layer2每个元素都感知了3 * 3个layer1元素，然而layer1每个3*3在input上的实际感受野为7 * 7（每个元素都可以感知周边的九个元素。）
<img width="172" alt="image" src="https://user-images.githubusercontent.com/6647857/48201743-026f8100-e39e-11e8-937c-39d101f1b506.png">


那么怎么计算呢，如果规定stride=1的，padding=1，kernel=3的话：每次卷积结果都会是同样尺寸的。
每次卷积都是扩大了一个边界的感受野，就是：(kernel-1)大小的区域。
那么加上约束stride=s之后，每次stride跳动并不会影响到下一层layer在本层的感受野，但是会影响到下下次layer在本层的感受野。
因为当前input-layer1的stride 无论多大，layer1在input上的感受野总是只与卷积核有关。
但是当进行到layer1-layer2,需要求知layer2在input上的感受野，就会受到input-layer1的stride影响，stride越大，所影响input的范围就越大（一般stride不会超过(k-1)/2的，这会造成部分元素无法感知）。
于是
input : r=1
input->layer1 : s=2,k=3,p=1 
得到layer1感受野为r=r[input] + (k-1)

layer1->layer2 : s=2,k=3,p=1
得到layer2在input的感受野为r=r[layer1] + (k[1->2]-1) * s[input->layer1]

layer2->layer3 : s=2,k=3,p=1
得到layer3在layer1的感受野为r = r[layer2] + (k[2->3]-1) * s[layer1->layer2]
随着stride的累加，造成的感受野增加量也是成倍的扩大。
得到layer3的input感受野为r = r[layer2] + (k[2->3]-1) * s[layer1->layer2] * s[input->layer1]

因此得到一般化的公式：
r[current] = r[pre] + (k[pre->current]-1) * s[pre->current] * s[prepre->pre] * ... * s[input->layer1]
此计算公式与padding是无关的。
其中： (k[pre->current]-1) * s[pre->current] * s[prepre->pre] * ... * s[input->layer1] 是在上一层感受野基础上多出来的(k-1)边界造成在input上的感受野。




引用：
https://medium.com/mlreview/a-guide-to-receptive-field-arithmetic-for-convolutional-neural-networks-e0f514068807
https://mathematica.stackexchange.com/questions/133927/how-to-compute-the-receptive-field-of-a-neuron
https://stackoverflow.com/questions/35582521/how-to-calculate-receptive-field-size
https://www.reddit.com/r/MachineLearning/comments/6o6cr8/d_how_does_one_calculate_the_receptive_field_of_a/
