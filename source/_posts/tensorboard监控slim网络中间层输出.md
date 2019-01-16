---
title: tensorboard监控slim网络中间层输出
date: 2019-01-08 18:19:03
categories: 深度学习
tags: 
	- 深度学习
	- tensorflow
	- python
comments: true
---

使用tensorboard查看中间层输出结果一般需要在源程序中添加以下几条命令

1:

```python
summary_op = tf.summary.merge_all()
summary_writer = tf.summary.FileWriter('./mid_result', tf.get_default_graph())
```

需要添加在sess.run()开始循环迭代之前，不要添加在循环内部，可能会造成多次创建总结数据。

2:

```python
summary_str = sess.run(summary_op, feed_dict={in_image: input_full})
summary_writer.add_summary(summary_str, 1)
```

需要添加在sess.run()之后。

3：

`show_feature_map_direct(layer=conv10, layer_name="conv10", num_or_size_splits=12, axis=3, max_outputs=3)`

在每一个想要查看的中间层代码后，添加如上代码。

layer: 想要输出的层结果，

layer_name:输出层的scope/name,

num_or_size_splits:输出层的层数。

可以放在卷积层后，也可以放在残差层后：

`conv10 = slim.conv2d(conv9, 12, [1, 1], rate=1, activation_fn=None, scope='g_conv10')`

`conv10 = tf.add(conv10, add_result, name='g_add_conv10')`

4:

添加显示图函数到代码中供调用：

```
def show_feature_map_direct(layer, layer_name, num_or_size_splits, axis, max_outputs):
    split = tf.split(layer, num_or_size_splits=num_or_size_splits, axis=axis)

    for i in range(num_or_size_splits):
        tf.summary.image(layer_name + "-" + str(i), split[i], max_outputs)
```

5：

运行时，在控制台命令行执行tensorboard命令，将会同时记录日志文件到mid_result文件夹中。

`tensorboard --logdir='./mid_result'`

浏览器访问http://127.0.0.1:6006  即可看到tensorboard的控制台。

6：

如果为远程服务器，可以从本地命令行新建立ssh链接

`ssh -L 16006:127.0.0.1:6006 account@server.address`

然后在本地浏览器访问http://127.0.0.1:16006 即可查看tensorboard控制台。

引用：

https://www.tensorflow.org/guide/summaries_and_tensorboard

https://zhuanlan.zhihu.com/p/31865749?from_voters_page=true

https://www.jianshu.com/p/4e8e5f516d84