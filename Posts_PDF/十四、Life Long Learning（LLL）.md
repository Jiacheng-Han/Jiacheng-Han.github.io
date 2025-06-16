## Life Long Learning（LLL）

### 引入

先学任务一再学任务二不如同时学任务一二的性能表现。

<img title="" src="file:///C:/Users/lenovo/AppData/Roaming/marktext/images/2025-06-13-15-43-13-image.png" alt="" width="446" data-align="center">

模型有学习的能力但核心问题是机器会遗忘依序学的任务（Catastrophic Forgetting）。

<img title="" src="file:///C:/Users/lenovo/AppData/Roaming/marktext/images/2025-06-13-15-46-19-image.png" alt="" width="467" data-align="center">

目前一个模型的多任务学习存在以下问题：
（1）不可能同时保存多个任务的训练资料（Storage issue，当任务多了以后根本存不下）

（2）多任务数据集训练训练的时间长（Computation issue）

* 对比Life-Long Learning和Transfer Learning？

两者都是利用了多个任务，但是Transfer Learning只在意当前任务，而Life-Long Learning在意过去的任务。

### 评估

<img title="" src="file:///C:/Users/lenovo/AppData/Roaming/marktext/images/2025-06-13-16-08-00-image.png" alt="" width="316" data-align="center">

$R_{i,j}$代表在训练了任务i后在任务j上的表现，指标：

$Accuracy = \frac{1}{T} \sum_{i=1}^T R_{T,i}$

$Backward\ Transfer =\frac{1}{T-1}\sum_{i=1}^{T-1} R_{T,i} - R_{i,i}$判断新学任务之后精度的变化（结果通常是负数）

$Forward\ Transfer = \frac{1}{T-1} \sum_{i=2}^T R_{i-1,i}-R_{0,i}$

### 解决方法

#### Selective Synaptic Plasticity

Catastrophic Forgetting的原因：多个任务具有不同的error suface，从而导致灾难性遗忘。

<img title="" src="file:///C:/Users/lenovo/AppData/Roaming/marktext/images/2025-06-14-15-02-43-image.png" alt="" width="541" data-align="center">

模型中的一些参数对之前的任务很重要，因此之后训练任务中只更改不重要的参数。当$L(\theta)$是当前任务的损失时，就会发生遗忘。因此我们要minimize的损失需要得到改变。$\theta^b$是从之前任务中学习的参数，$\theta_i^b$指模型的每个参数，而$b_i$是对应的重要性程度。

$L'(\boldsymbol{\theta}) = L(\boldsymbol{\theta}) + \lambda \sum_i b_i \left( \theta_i - \theta_i^b \right)^2$

我们期待的是$\theta_i - \theta_i^b$尽量越接近越好，同时我们要利用$b_i$确定接近的程度（$b_i$越大说明两者要越接近）。我们不要求每个参数都一摸一样，而是某些参数接近即可：当$b_i=0$说明肯定会遗忘；当$b_i=\infty$表示不妥协，一定会相同。

<img title="" src="file:///C:/Users/lenovo/AppData/Roaming/marktext/images/2025-06-14-15-22-45-image.png" alt="" width="213" data-align="center">

#### Gradient Episodic Memory(GEM)

刻意改变梯度下降的方向

<img title="" src="file:///C:/Users/lenovo/AppData/Roaming/marktext/images/2025-06-14-15-25-57-image.png" alt="" width="488" data-align="center">

但记住$g^b$要求需要存储之前训练的资料。

#### Progressive Neural Networks

<img title="" src="file:///C:/Users/lenovo/AppData/Roaming/marktext/images/2025-06-14-15-55-15-image.png" alt="" width="443" data-align="center">

#### Generating Data

<img title="" src="file:///C:/Users/lenovo/AppData/Roaming/marktext/images/2025-06-14-15-59-20-image.png" alt="" width="450" data-align="center">
