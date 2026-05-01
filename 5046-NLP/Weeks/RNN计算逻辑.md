## RNN计算
RNN = 当前输入 + 历史信息（memory） → 当前状态 → 输出
### RNN单个cell:
h_t = tanh(W x_t + V h_{t-1} + b_h)
y_t = tanh(U h_t + b_y)
- x_t：当前输入（word embedding）
- h_{t-1}：上一时刻状态（记忆）
- h_t：当前状态（融合历史+当前）
- y_t：当前输出
- W：输入 → hidden
- V：hidden → hidden（记忆传递）
- U：hidden → output

Input : I really like chocolate  
Output: PRON ADV VERB NOUN 

### Forward Pass（前向传播）

for t = 1 → T:
    h_t = tanh(W x_t + V h_{t-1})
    y_t = softmax(U h_t)

---

### Loss Function（损失函数）

L = Σ_t -log(y_t[true_label])

> 每个时间步都有loss

---

### Backpropagation Through Time（BPTT）

#### 核心
从最后时间步开始向前传播梯度

#### 梯度来源
∂L/∂h_t = 当前输出贡献 + 下一时间步贡献

#### 本质
将RNN展开为一个深层网络，然后应用普通反向传播

---

### 参数更新（Gradient Descent）

θ = θ - η ∂L/∂θ

---

### 训练流程

1. Forward pass（计算预测）
2. Compute loss（计算误差）
3. Backward pass（BPTT计算梯度）
4. Gradient descent（更新参数）
