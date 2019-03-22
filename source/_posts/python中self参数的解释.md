---
title: python中self参数的解释
date: 2019-03-22 15:26:09
categories: python
tags:
	- python
comments:
---

这篇文章中我们将会讨论self 变量在python 中的应用。很多初学者都对python 中self的使用方法表示困惑。如果你也是这样，那么你应该读一下这篇文章。

一个简单的类：

```python
class Restaturant(object):
	bankrupt = False
    def open_branch(self):
        if not self.bankrupt:
            print("branch opened")
```

首先我们从非专业的角度解释这段代码。首先我们创建一个类 Restaurant。然后我们生命它的一个属性 bankrupt 为 false。然后我们定义一个函数open_branch ，仅当 bankrupt 为 false 的时候触发打印功能，这意味着Restautant 还有钱。

创建一个 resturant：

现在，我们来创建一个Restaurant类的对象，让我们创建一个resturant：

```python
x = Restaurant()
```

现在 x 是一个Restaurant，并且具有bankrupt 属性以及一个open_branch 函数。现在我们可以通过以下代码访问这个bankrupt属性：

```python
x.bankrupt
```

以下命令具有相同的效果：

```python
Restaurant().bankrupt
```

现在你可以看到self 指的是绑定的变量或者对象。在第一个例子中，由于我们已经将Restaurant 类赋值到x，所以self 就是x，然而第二个例子中就是`Restaurant()` 。现在我们有另一个Restaurant y，self 将会自动访问y的bankrupt而不是x的。例如：

```python
>> x = Restaurant()
>> x.bankrupt
>> x.open_branch()
False
branch opened

>> y = Restaurant()
>> y.bankrupt = True
>> y.bankrupt
>> y.open_branch()
True


>> x.bankrupt
>> x.open_branch()
False
branch opened
```

**在上面的例子中，当修改了x.bankrupt 之后， x.open_branch()也无法正常打印。见到那的说，self就是类自身，通过self.xxx可以引用当前方法所在类的属性和方法。而在使用某个方法的时候并不需要为self参数传递值。**

每一个类的第一个方法（包括init），总是一个引用当前类的实例。更通俗的说，这个参数总是被成为self。在init 方法中，self 指的是新创建的对象；在其他类的方法中，它代表实例的方法被调用。下面的例子有相同的效果：

```python
class Restaurant(object):
    bankrupt = False
    def open_branch(this):
        if not this.bankrupt:
            print("branch opened")
```

然而 self 在python 中并不是一个保留关键字，仅仅是一个普遍共识。许多人说为什么我们必须要写self？为什么不能像java一样自动设置？一些人也提交了PEP（改进建议）并建议移除这个self的声明。然而 GuidoVan Rossum 写了一个[bolg](http://neopythonic.blogspot.com/2008/10/why-explicit-self-has-to-stay.html)解释为什么不得不声明self。





引用：

[1] <https://pythontips.com/2013/08/07/the-self-variable-in-python-explained/>
[2] <http://neopythonic.blogspot.com/2008/10/why-explicit-self-has-to-stay.html>

