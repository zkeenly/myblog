---
title: tensorflow部分层初始化参数以及finetune
date: 2019-01-10 17:59:00
categories: 深度学习
tags: 
	 - tensorflow
	 - slim
comments: true
---





本文在 tensorflow+slim 中实现基于原始卷积网络扩展增加新的层，然后finetune新的网络层参数。

### 1，锁定部分变量（网络层），只finetune另一部分变量(网络层)

首先重置默认图,防止出现意外错误

```python
tf.reset_default_graph()  # 重置默认图。
```
定义网络操作pipeline,其中network为使用slim-api定义的一些列卷积操作

```python
in_image = tf.placeholder(tf.float32, [None, None, None, 4])
gt_image = tf.placeholder(tf.float32, [None, None, None, 3])
out_image = network(in_image)  # 定义pipeline
G_loss = tf.reduce_mean(tf.abs(out_image - gt_image))  # 定义损失函数
lr = tf.placeholder(tf.float32)  # 定义学习率
```
获取所有的网络参数(即定义网络层的scope="")

```python
t_vars = tf.trainable_variables()  # 获取所有的变量
```

分别获取需要保持不变的变量和需要finetune的变量,scope中包含`g_add`的为附加finetune层.

```python
g_vars = [var for var in t_vars if 'g_add' in var.name]  # 附加的finetune网络层
var_list = [var for var in t_vars if 'g_conv' in var.name]  # 不需要改变的网络层
```

生成train_op,使用`var_list=g_vars` 设置仅需要finetune的部分

```python
G_opt = tf.train.AdamOptimizer(learning_rate=lr).minimize(G_loss, var_list=g_vars)  # 只训练g_vars部分
```

以上,其他部分按照正常操作即可以只训练g_vars部分,而不改变vart_list部分

### 2，加载部分默认网络层参数,而不改变其他网络层的参数

首先定义variables_to_restore,其中`exclude=["g_add"]` 为不需要恢复的变量,也可以使用`include=["g_conv"]` 来定义仅需要恢复的变量.[12] [14]

```python
variables_to_restore = slim.get_variables_to_restore(exclude=["g_add"])  # 一定要双引号,否则仅exclude 'g_add'一个层.
```

建立一个re_saver来从已有的模型中恢复g_conv系列参数到网络中.
```python
re_saver = tf.train.Saver(variables_to_restore)  # 建立一个saver 用来保存需要恢复的模型变量
```

再建立一个saver,用来训练的时候保存整个模型的ckpt

```python
saver = tf.train.Saver()  # 建立一个模型，用来保存所有的模型变量
```


将外置模型参数加载到将要训练的模型sess中,并且初始化其余未初始化的参数,训练以及保存模型.
```python
with tf.Session() as sess:
    model_path = './model.ckpt'  # 后缀名称仅需要写ckpt即可,后面的00001-00000不必添加
    re_saver.restore(sess=sess, save_path=model_path)  # 恢复模型的参数到新的模型
    un_init = tf.variables_initializer(get_uninitialized_variables(sess))  # 获取没有初始化(通过已有model加载)的变量 
    sess.run(un_init)  # 对没有初始化的变量进行初始化并训练.
    for epoch in range(lastepoch, 4001):
    	_, G_current, output = sess.run([G_opt, G_loss, out_image],
                                        feed_dict={in_image: input_patch, gt_image: gt_patch, lr: learning_rate})  # 执行训练
    saver.save(sess, checkpoint_dir + 'model.ckpt')  # 使用saver.save保存训练模型 
```



### 总结

​	网上查的资料大多大同小异,但是有一点都未曾提及,就是训练中保存变量的时候不应该用恢复网络参数所使用的saver来保存全部的参数.否则将只保存原始加载的那部分参数.而新finetune 的layer将无法保存.导致再测试的时候出现找不到某些层的bug

`[TensorFlow: NotFoundError: Key not found in checkpoint]`



引用：

[1] http://www.tensorfly.cn/tfdoc/how_tos/variables.html

[2] https://blog.csdn.net/wjc1182511338/article/details/82112181  获得未初始化的变量

[3] https://blog.csdn.net/mr_muli/article/details/80868826

[4] https://blog.csdn.net/ArtistA/article/details/52860050

[5] https://zhuanlan.zhihu.com/p/42183653

[6] https://www.quora.com/Is-it-possible-to-only-train-the-final-layer-of-a-Neural-Net-in-TensorFlow-that-was-already-trained

[7] https://stackoverflow.com/questions/37326002/is-it-possible-to-make-a-trainable-variable-not-trainable

[8] https://stackoverflow.com/questions/45093499/how-to-fine-tune-weights-in-specific-layers-in-tensorflow

[9] https://stackoverflow.com/questions/34001922/failedpreconditionerror-attempting-to-use-uninitialized-in-tensorflow

[10] https://stackoverflow.com/questions/47765595/tensorflow-attempting-to-use-uninitialized-value-beta1-power/47780342

[11] https://blog.csdn.net/u011961856/article/details/76850335

[12] https://blog.csdn.net/abc8350712/article/details/78437250

[13] http://mashangxue123.com/TensorFlow/687648778.html

[14] https://github.com/tensorflow/tensorflow/tree/master/tensorflow/contrib/slim

[15] https://cv-tricks.com/tensorflow-tutorial/save-restore-tensorflow-models-quick-complete-tutorial/?tdsourcetag=s_pctim_aiomsg

[16] https://github.com/cchen156/Learning-to-See-in-the-Dark