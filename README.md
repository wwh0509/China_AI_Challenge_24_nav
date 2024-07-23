## 中国人工智能大赛2024



## 任务说明

定点导航：机器人需要在RGB+Depth图像的指引下，到达由三维坐标指定的目标点。机器人允许和环境发生碰撞，机器人会被限制总共的执行步数。我们使用Success weighted by Path Length（SPL）指标评估模型的好坏



## 评估指标

成功率：当机器人在目标点为圆心，半径为0.5m的圆内时，便认为机器人成功完成了这一轮的任务。如果机器人在规定的执行步数内未到达目标点，则失败。

SPL：衡量机器人走过的路径和最短路径之间的差异。SPL的定义如下：
$$
SPL=\frac{1}{N}\sum_{i=1}^{N}S_i\frac{l_i}{max(p_i,l_i)}\\
其中，l_i表示最短路径的长度\\
\qquad\qquad\quad p_i表示机器人走过路径的长度\\
\qquad\qquad\quad S_i表示这一轮机器人是否成功
$$
SPL用来衡量机器人最短寻路的能力。当然，如果SPL的差异并不显著，我们可能会考虑成功率对于排名的影响。



## 数据集

我们采用iGIbson的公开场景数据集，不提供训练使用的episode数据集。用于评测的episode数据集对选手保密。具体下载方式请参考：

https://stanfordvl.github.io/iGibson/dataset.html

选手需要下载dataset和assets。



## 比赛设置

我们采用以下的比赛设置：

* **观察信息**：（1）目标点在极坐标下相对机器人的位置。（2）当前机器人的线速度和角速度（3）RGB和Depth图像
* **动作**：（1）当前帧期望机器人采用的线速度和角速度
* **奖励**：根据选手设置的奖励函数而得到的真实奖励值
* **结束条件**：机器人执行了512个步数，或者机器人到达了目标点



## 参赛指南

### 项目配置

* **步骤1**：安装anaconda并且创建一个环境：

  ```
  conda create -n gibson_2024 python=3.6
  conda activate gibson_2024
  ```

* **步骤2**：安装EGL依赖：

  ```
  sudo apt-get install libegl1-mesa-dev
  ```

* **步骤3**：安装iGibson

  ```bash
  git clone https://github.com/StanfordVL/iGibson.git --recursive
  cd iGibson
  pip install -e .
  ```

* **步骤4**：安装baseline：

  ```
  git clone git@github.com:wwh0509/rl.git --recursive
  pip install -r requirements.txt
  pip install -e .
  ```

  





### 训练

#### 使用docker



#### 不使用docker

* **步骤1**：进入training文件夹：

  ```
  cd rl/agent/training
  ```

* **步骤2**：启动训练：

  ```
  bash point_nav_ppo_train.sh
  ```

  这个启动了两个环境进行训练，在单卡3090上约5天完成训练

### 本地评测

* **步骤1**：进入training文件夹：

  ```
  cd rl/agent/training
  ```

* **步骤2**：随机生成一些场景的episode data：

  ```
  bash generate_data.sh
  ```

* **步骤3**：启动检查点的评估：

  ```
  bash point_nav_ppo_eval.sh
  ```



### 在线评测





## 参考资料
