## 强化学习（RL）

### 引入

ML本质是找到一个公式，三步：

- [x] 找到含有未知数的函数

- [x] 定义从训练过程中得到的loss

- [x] 优化

但RL没有标签，而是借由和环境的互动判断输出是好还是坏。目标是找到一个反馈总和最大的策略。

<img title="" src="file:///C:/Users/lenovo/AppData/Roaming/marktext/images/2025-06-04-21-55-58-image.png" alt="" width="483" data-align="center">

### RL的训练步骤（和ML的训练对比）

* Policy Network：类似一种分类模型，常见做法是输出看作是几率，利用随机采样决定下一步的行动。

![](C:\Users\lenovo\AppData\Roaming\marktext\images\2025-06-13-11-39-51-image.png)

* 定义“loss”
  
  Total reward (return): $R = \sum_{t=1}^T r_t$
  
  最大化Total reward，在ML里是要越小越好，所以一般是负数处理。

* 优化：
  
  $\tau = \{s_1,a_1,s_2,a_2,...\}$训练出来一个参数让Actor计算的$R(\tau)$最大

![](C:\Users\lenovo\AppData\Roaming\marktext\images\2025-06-13-12-16-25-image.png)

但是如果把整个结构看作是一个network？

（1）$a_i$的输出却又具有随机性（sample）

（2）Env是黑盒，Reward是分数规则，都不是network（无法调参），且都可能存在一定的随机性。

所以RL真正的**难点就是如何解决optimzation**。

### 如何控制actor的输出

可以看做是分类模型，通过交叉熵引导。（1）希望咋做（2）不希望咋做

![](C:\Users\lenovo\AppData\Roaming\marktext\images\2025-06-13-13-06-18-image.png)

本质上就是一个supervised learning。根据好和坏的程度对Action加权（$A_i$），计算Total loss。

![](C:\Users\lenovo\AppData\Roaming\marktext\images\2025-06-13-13-10-36-image.png)

但是核心是如何找到Training Data

#### 如何定义$A_i$

ver1：每一步的行为都会影响到后续的行为，那么就统一做加法处理。

![](C:\Users\lenovo\AppData\Roaming\marktext\images\2025-06-13-13-19-37-image.png)

ver2：但是第一步的决策对于最后一步的贡献影响并不大（每一步的贡献不是等值的），因此每次都乘上一个小于1的权重。

![](C:\Users\lenovo\AppData\Roaming\marktext\images\2025-06-13-13-23-10-image.png)

ver3：万一只有一部分好一部分不好，采用ver2整体就可能影响这种局部好坏的决策（因为好或者坏都是相对的），因此要标准化（在这里是使用减去标准差）。

![](C:\Users\lenovo\AppData\Roaming\marktext\images\2025-06-13-13-28-37-image.png)

### 策略梯度

核心在于训练集的资料是在**每次执行**中得到的，不只使用一套训练集的原因是$\{s_i,a_i\}$的可能性对于不同模型迭代的决策判断的差异是很大的（可能$a_i$对一个模型来说是好的，对另一个模型来说是不好的）。

![](C:\Users\lenovo\AppData\Roaming\marktext\images\2025-06-13-13-31-05-image.png)

#### Critic

作用：用来评估actor的好坏

Value function $V^\theta(s)$：当使用actor $\theta$时，计算在看到状态s后的表现情况

<img src="file:///C:/Users/lenovo/AppData/Roaming/marktext/images/2025-06-16-14-15-28-image.png" title="" alt="" data-align="center">

上图，当射击目标的外星人很多时，预期得分很高；反之，得分很低。$V^\theta(s)$和观察对象$\theta$有关的，不同的$\theta$计算结果不同。

#### 训练Critic

* Monte-Carlo(MC) based approach

Critic观察actor $\theta$与环境互动，尽量和对应的G相近。训练需要整个过程的互动。

<img title="" src="file:///C:/Users/lenovo/AppData/Roaming/marktext/images/2025-06-16-14-25-30-image.png" alt="" width="325" data-align="center">

* Temporal-difference (TD) approach

不需要整个过程的s，目标是$s_t,a_t,r_t \rightarrow s_{t+1}$

由于$V^\theta(s_t) = \gamma V^\theta(s_{t+1}) + r_t$所以可以计算得到：$r_t=V^\theta(s_t) - \gamma V^\theta(s_{t+1})$

* 这两种方法算出来的value可能不一样

![](C:\Users\lenovo\AppData\Roaming\marktext\images\2025-06-16-16-30-26-image.png)

两种方法都有可能是正确的，因为假设不同，一个是整体，一个是局部。

此时就出现了ver3.5：$A_i=G_i'-V^\theta(s_t)$（把$V^\theta(s_t)$当作b）

为什么用$V^\theta(s_t)$替代可行呢？因为$V^\theta(s_t)$指在$s_t$场景下任意action reward分数的可能性（所有动作reward平均值），所以如果算出来的$A_t$大于0，那么就说明是“好”操作，否则就是“不好”的操作。

但这种方式用于计算的数值并不都是平均值。

ver4：**Advantage Actor-Critic**（主流）$A_t=r_t+V^\theta(s_{t+1})-V^\theta(s_t)$

![](C:\Users\lenovo\AppData\Roaming\marktext\images\2025-06-16-18-39-33-image.png)

* Actor和Critic训练时可以共用参数

![](C:\Users\lenovo\AppData\Roaming\marktext\images\2025-06-16-18-47-03-image.png)

### reward shaping

Sparse Reward指$r_i=0$的情况。e.g.:机器手臂拧螺丝，只有正好拧好采有负反馈，其他情况全是0。那么就要提供额外的reward才能引导agent学习 -> reward shaping

核心：需要人类对环境的理解强加上去。

### No Reward

在很多真实的环境下，没有标准的Reward标准（不知道要如何定义reward的级别）。

* Imitation Learning：不会从环境中获得reward，而是找Expert示范（自动驾驶汽车学习人类的示范 -> 模仿），但这种复制clone的某些行为可能时不必要的。

* Inverse Reinforcement Learning（IRL）：从示范学习反推Reward Function，再利用reward函数来找到最优的actor。

<img title="" src="file:///C:/Users/lenovo/AppData/Roaming/marktext/images/2025-06-16-19-04-21-image.png" alt="" width="461" data-align="center">

核心是老师的行为是最好的，但学生没必要和老师一样。类似于GAN

<img title="" src="file:///C:/Users/lenovo/AppData/Roaming/marktext/images/2025-06-16-19-05-20-image.png" alt="" data-align="center" width="458">


