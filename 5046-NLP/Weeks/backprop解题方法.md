## 训练优化
1. Forward pass  → 算中间变量 + 输出
2. Compute loss  → 写出L
3. Backward pass → 从后往前算梯度
4. Update (可选) → 用GD更新参数

梯度 = 上游梯度 × 局部梯度
