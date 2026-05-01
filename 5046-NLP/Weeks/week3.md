# Week 3 - Models: Non-linear


# 🧠 一、核心主线

线性模型不够 → 加非线性 → 神经网络  
→ 用梯度下降训练 → Backprop  
→ 处理序列 → RNN  
→ RNN问题 → LSTM  
→ 应用于 NLP（POS / NER / 分类）

---

# 📌 二、Non-linear Model 非线性模型

## 1. 为什么需要非线性？

线性模型：score(x) = x · W

问题：
- 只能学 **线性关系**
- 无法处理复杂组合（如 negation）

例：
- "very good" → positive
- "not good" → negative ❌（线性模型难处理）

---

## 2. 非线性模型结构
score(x) = g(xW1 + b1) W2 + b2

核心：
- g() = activation function
- 引入 non-linearity

---

## 3. 关键结论（考试重点）

❗ 多层线性 = 单层线性
W2(W1x) = W'x

👉 没有 activation → 深度没有意义

---

# ⚡ 三、Activation Function 激活函数

## 作用

👉 引入非线性，使模型更强

---

## 1. Sigmoid
σ(x) = 1 / (1 + e^-x)

特点：
- 输出：(0,1)
- 用于概率
- ❌ 容易 vanishing gradient

---

## 2. Tanh
tanh(x)

特点：
- 输出：(-1,1)
- 比 sigmoid 更对称

---

## 3. ReLU ⭐（最重要）
ReLU(x) = max(0, x)

特点：
- 简单高效
- 不容易梯度消失
- NLP中最常用

---

# 🧠 四、Neural Network 神经网络

## 结构
Input → Linear → Activation → Linear → Output

组成：

| 组件 | 含义 |
|------|------|
| Weight | 权重 |
| Bias | 偏置 |
| Hidden Layer | 中间层 |
| Activation | 非线性 |

---

## 本质理解

👉 神经网络 = 自动学习 feature representation

---

# 🎯 五、XOR问题（理解非线性本质）

问题：
0 XOR 0 = 0
0 XOR 1 = 1
1 XOR 0 = 1
1 XOR 1 = 0

❌ 线性不可分  
✅ 加 hidden layer 可解决

👉 本质：
非线性 = 改变表示空间

---

# 📉 六、Loss & Learning 学习机制

## 1. Loss Function
loss = -log P(y|x)

含义：
- 正确答案概率越高 → loss 越小
- 模型目标：最小化 loss

---

## 2. Gradient 梯度

表示：
参数改变 → loss变化方向

更新：
W = W - η * gradient


👉 η = learning rate

---

# 🔁 七、Backpropagation 反向传播

## 核心

👉 用 chain rule 计算梯度

---

## 两个步骤

### 1. Forward Pass
输入 → 输出 → loss

### 2. Backward Pass
loss → 反向计算梯度 → 更新参数

---

## 本质

👉 Efficient gradient computation

---

# ⚠️ 八、Overfitting 过拟合

## 定义
训练好，测试差

原因：
- 模型太复杂
- 记住数据

---

## 解决方法（Regularization）

### 1. L2 Regularization
loss + λ||W||²

### 2. L1 Regularization
loss + λ|W|

### 3. Dropout
随机丢弃神经元

---

# 🔁 九、RNN（Recurrent Neural Network）

## 1. 为什么需要RNN？

语言是序列：
I really like chocolate

问题：
- 长度不固定
- 依赖上下文

---

## 2. RNN公式
ht = tanh(Wxt + Vht-1 + b)

含义：

| 符号 | 含义 |
|------|------|
| xt | 当前输入 |
| ht-1 | 上一状态 |
| ht | 当前状态 |

---

## 本质理解

👉 hidden state = memory（上下文记忆）

---

# 🧩 十、RNN任务类型

## 1. Sequence Labeling

输入：I love NLP

输出：PRON VERB NOUN

应用：
- POS tagging
- NER

---

## 2. Sequence Classification

输入：This movie is great

输出：Positive

方法：
👉 用最后一个 hidden state

---

# ⚠️ 十一、RNN问题

## 1. Vanishing Gradient

👉 梯度越来越小  
👉 学不到 long-term dependency

---

## 2. Exploding Gradient

👉 梯度太大

解决：Gradient clipping

---

# 🧠 十二、LSTM（重点理解）

## 作用

👉 解决 long-term dependency

---

## 核心思想
记住重要信息
忘记无用信息

---

## 关键结构

- Cell state（长期记忆）
- Gates（控制信息）

---

# 🏷️ 十三、POS Tagging

## 定义
每个词 → 一个词性标签

例：
Janet will back the bill
NNP MD VB DT NN

---

## 本质

👉 Sequence Labeling Task

---

# 🏷️ 十四、NER（Named Entity Recognition）

## 定义

识别实体：
PER / LOC / ORG / GPE

---

## 例子
[PER John] lives in [LOC Sydney]

---

## 难点

- 边界识别（span）
- 类型歧义

---

# 🔑 十五、BIO标注（考试重点）
B = Begin
I = Inside
O = Outside

例：
John lives in New York
B-PER O O B-LOC I-LOC

---

# 🧠 十六、最终总结（必背）

## 模型发展
Linear → Non-linear → Neural Network → RNN → LSTM

---

## 核心能力

| 模型 | 能力 |
|------|------|
| Linear | 简单关系 |
| NN | 非线性表达 |
| RNN | 序列建模 |
| LSTM | 长期依赖 |

---

## NLP应用
POS tagging
NER
Sentiment Analysis
Sequence Modeling
