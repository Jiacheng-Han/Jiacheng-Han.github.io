## Meta Learning

工业界很多GPU资源训练未知参数，而学术上GPU资源较少，如何更好确定未知参数。

而元学习Meta Learning（learn to learn），就是带着这种对人类这种“学习能力”的期望诞生的。Meta Learning希望使得模型获取一种“学会学习”的能力，使其可以在获取已有“知识”的基础上快速学习新的任务。

![](C:\Users\lenovo\AppData\Roaming\marktext\images\2025-06-15-16-32-36-image.png)

### 机器学习和元学习的区别

（1）ML的目标是找到一个function F，而Meta Learning的目标是找到一个可以训练function f的Function F使得f可以完成ML中F的任务（学会学习）

（2）ML是训练出一个数据集任务的函数（Within-task Training）。而Meta Learning是有很多训练的任务（Across-task Training），它的训练是包括一大堆任务的。

<img title="" src="file:///C:/Users/lenovo/AppData/Roaming/marktext/images/2025-06-15-18-14-16-image.png" alt="" width="520" data-align="center">

### 元学习的训练步骤

* step1：learning algorithm中的那些需要学习

定义：$\phi$为可学习的参数，learning algorithm标志为$F_\phi$

* step2：定义$F_\phi$的损失函数$L(\phi)$

计算方法和ML计算类似

<img title="" src="file:///C:/Users/lenovo/AppData/Roaming/marktext/images/2025-06-15-16-41-16-image.png" alt="" data-align="center" width="473">

但Meta Learning不只是在一个Task上要表现最好，而是在多个任务上整体表现较好。

$L(\phi)=\sum_{n=1}^N l^n$

和ML不同的是，Meta Learning不是在训练资料上得到Loss，而是<u>在测试资料上得到Loss</u>。

* step3：找到能让$L(\phi)$最小化的$\phi$
  
  $\phi^*=agr min_\phi L(\phi)$


