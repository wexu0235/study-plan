# Week 4 — Inference & Search

---

# 🔥 1. 核心主线（整周最重要）

## NLP系统本质：
Model + Inference

- Model：给 (input, output) 打分  
- Inference：找到最高分输出  

👉 Lecture定义：Inference = 找最高分输出  

---

## 🚨 核心困难：搜索空间爆炸

| 任务 | 搜索空间 |
|------|----------|
| Tagging | |tags|^|words| |
| Generation | vocab^length |
| Parsing | 所有树 |
| Coreference | 所有cluster |

👉 必须使用近似搜索（approximate inference）

---

# 🧩 2. Exhaustive Search（不可行但重要）

## 方法：
枚举所有输出 → 选最大分  

## 复杂度：
O(|tags|^|words|)

## 特点：
- ✅ 全局最优  
- ❌ 不可扩展  

---

# 🧩 3. Greedy Search（第一层近似）

## 核心思想：
每一步选当前最优  

👉 argmax(P)

---

## 复杂度：
O(|tags| × |words|)

---

## 问题：
局部最优 ≠ 全局最优  

👉 一旦选错无法回头  

---

## 结论：
👍 快，但可能错误  

---

# 🧩 4. Sampling（Greedy扩展 | J+M Ch7.4）

👉 用于文本生成（LLM decoding）

---

## 4.1 Greedy（Top-1）
- 选最大概率  
👉 输出稳定但单一  

---

## 4.2 Random Sampling
- 按概率随机  

👉 提升多样性  

---

## 4.3 Top-K Sampling
- 只在前K个中选  

👉 控制质量 vs 多样性  

---

## 4.4 Top-P（Nucleus）
- 选累计概率 ≥ P  

👉 更自适应  

---

## 4.5 Contrastive Sampling
- 惩罚重复  

👉 减少重复输出  

---

## 总结：
Sampling = 在“合理候选空间中随机”

---

# 🧩 5. Beam Search（第二层近似 | J+M Ch12.4）

## 核心思想：
保留 K 条路径，而不是 1 条  

---

## 算法流程：
1. 初始化  
2. 扩展所有候选  
3. 保留 top K  
4. 选最优  

---

## 复杂度：
O(|steps| × K × |options|)

---

## 对比：

| 方法 | 路径数 |
|------|--------|
| Greedy | 1 |
| Beam | K |
| Exhaustive | 全部 |

---

## 特点：
- ✅ 比 greedy 更好  
- ❌ 仍非最优  

👉 本质：延迟决策  

---

# 🧩 6. Sequence Tagging（结构化预测）

## 任务：
输入：words  
输出：labels  

例：
Joe likes trucks → B-PER O O  

---

## 🚨 问题：
标签之间存在依赖  

👉 Independent prediction 会错误  

---

# 🧩 7. 模型分解：Emission + Transition

## Emission：
P(word | label)

## Transition：
P(label_t | label_{t-1})

---

## 总目标：
argmax_y ∏ emission × transition  

---

# 🧩 8. Viterbi Algorithm（J+M Ch17.4.5）

## 核心：
动态规划  

---

## 状态：
dp[t][label]

---

## 转移：
dp[t][y] = emission × max(previous × transition)

---

## 复杂度：
O(|words| × |labels|²)

---

## 优势：
- ✅ 避免指数爆炸  
- ✅ 找到全局最优  

👉 本质：在标签网格中找最优路径  

---

# 🧩 9. Graph Search（统一视角）

## 思想：
把问题表示为图

- 节点 = 状态  
- 边 = 转移  
- 路径 = 输出  

---

## 优化：
score = 当前分数 + 未来估计  

👉 类似 A*  

---

# 🧩 10. Dependency Parsing（J+M Ch19）

## 本质：
在所有可能的树中找最高分  

---

## 表示：
- 节点 = 单词  
- 边 = dependency（Head → Dependent）  

---

## 示例：
I saw a dog  

结构：
saw → I  
saw → dog  
dog → a  

---

## 评分：
Score(tree) = 所有边分数之和  

---

## 约束：
1. 单一 root  
2. 无环  
3. 无交叉  

---

## 总结：
句子 → 树  
边打分 → 求和  
加约束 → 合法树  
搜索 → 最优树  

---

# 🧩 11. Coreference（J+M Ch23）

## 问题：
哪些 mention 指同一个 entity  

---

## 难点：
搜索空间极大  

---

## 简化：

### 1. Mention Detection  
找候选  

### 2. Pair Linking  
找最可能前一个  

### 3. Transitive Closure  
形成 cluster  

---

## 本质：
从全局组合 → 局部决策  

---

# 🔥 12. 统一框架（最重要）

argmax_y Score(x, y)

---

## 对应关系：

| 任务 | 输出 | 方法 |
|------|------|------|
| Generation | token序列 | sampling |
| MT | 序列 | beam |
| Tagging | 标签序列 | Viterbi |
| Parsing | 树 | graph search |
| Coreference | cluster | heuristic |

---

# 🧠 13. 核心对比（考试重点）

## Greedy vs Beam vs Exhaustive

| 方法 | 最优 | 速度 |
|------|------|------|
| Exhaustive | ✅ | ❌ |
| Greedy | ❌ | ✅ |
| Beam | ⚠️ | ⚖️ |

---

## Top-K vs Top-P

| 方法 | 特点 |
|------|------|
| Top-K | 固定数量 |
| Top-P | 动态概率 |

---

## Independent vs Sequence

| 方法 | 问题 |
|------|------|
| Independent | 不一致 |
| Sequence | 建模依赖 |

---

## Emission vs Transition

| 类型 | 作用 |
|------|------|
| Emission | word → label |
| Transition | label → label |

---

# 🎯 14. 一句话总结

Model 负责打分  
Inference 负责在巨大搜索空间中找到最优结构
