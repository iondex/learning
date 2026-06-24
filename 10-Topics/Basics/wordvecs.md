---
title: Word Vectors & word2vec
tags: [topic/basics, course/cs224n, status/growing]
created: 2026-06-24
source: CS224N L02 slides + Note 1 (Intro & Word2Vec)
status: growing
---

# Word Vectors & word2vec

## 核心思想：分布式语义
**Distributional hypothesis**（分布假设，Firth 1957）："You shall know a word by the company it keeps."
—— 一个词的含义由它**经常一起出现的上下文词**决定。word2vec 据此从语料**自监督**学出稠密向量。

## 词表示的演进
1. **One-hot**：每个词是标准基向量。词与词正交，$v_{tea}^\top v_{coffee}=0$ → **无法表达相似度**。
2. **离散标注**（WordNet 的 synonym/hypernym 等）：靠人工，维度高、不全、更新贵。
3. **共现矩阵**（co-occurrence matrix）：扫语料统计共现计数，构成 $|V|\times|V|$ 矩阵。
   - 窗口小 → 偏**句法**；窗口大/文档级 → 偏**语义/主题**。
   - 原始计数被高频词（the）主导 → 取 **log frequency**。代表方法：**GloVe**。
4. **word2vec**：直接学**低维稠密**向量（维度 ≪ $|V|$），本笔记重点。

> 术语：word **type**（词表中的抽象词）vs word **token**（语境中的一次具体出现）。

## word2vec 模型
每个词有**两套向量**：作中心词时用 $v_w$，作上下文词时用 $u_w$（即矩阵 $V,U\in\mathbb{R}^{|V|\times d}$）。

给定中心词 $c$，预测上下文词 $o$ 的概率用 **softmax**：
$$P(o\mid c)=\frac{\exp(u_o^\top v_c)}{\sum_{w\in V}\exp(u_w^\top v_c)}$$
- 分子 $u_o^\top v_c$ = 相似度打分（点积越大越相似）；
- 分母 = **partition function（归一化）**。

### softmax
把任意实数打分向量 $z$ 变成概率分布：$\text{softmax}(z)_i=\dfrac{\exp(z_i)}{\sum_j\exp(z_j)}$。
- **必是概率分布**：exp 让每项 $>0$（非负），除以总和让和为 1。
- 名字 = **soft + max**：保序放大最大项，但不把其余项压成 0（可导，区别于硬 argmax）。
- 性质：平移不变 $\text{softmax}(z+c)=\text{softmax}(z)$（实现时减 $\max$ 防溢出）；温度 $T$ 越小分布越尖（$\text{softmax}(z/T)$）。

## 目标函数
对语料每个位置的中心词、窗口（半径 $m$）内每个上下文词，**最大化 likelihood** ⟺ **最小化负对数似然**（即 cross-entropy loss）：
$$J(U,V)=-\frac{1}{T}\sum_{t=1}^{T}\sum_{\substack{-m\le j\le m\\ j\ne 0}}\log P(w_{t+j}\mid w_t)$$

## 优化：GD vs SGD
- 优化对象是向量参数 $\theta=\{U,V\}$（每个词的 $u_w,v_w$）。
- **GD**：扫完整语料所有窗口算一次梯度才更新一步。**SGD**：随机抽一个/一批窗口估梯度即更新——语料大时唯一可行，且单窗口梯度稀疏、便宜。
- 初始化为小随机向量，迭代 $\theta\leftarrow\theta-\alpha\nabla_\theta J$。

**关键梯度（直觉）**：对中心词向量求导得
$$\frac{\partial}{\partial v_c}\big(-\log P(o\mid c)\big)=u_o-\sum_{w\in V}P(w\mid c)\,u_w=\underbrace{u_o}_{\text{observed}}-\underbrace{\mathbb{E}[u_w]}_{\text{expected}}$$
即"**真实上下文词 − 模型期望的上下文词**"：把 $v_c$ 往真实出现的词拉近、往模型高估的词推远。

## 两种结构
- **Skip-gram**：用中心词预测上下文词（上式即此）。
- **CBOW**（Continuous Bag-of-Words）：反过来，用上下文词预测中心词。

## Negative Sampling（SGNS）
softmax 分母要对**全词表**做点积+exp+求和，太贵。**Skip-gram with Negative Sampling** 改为：把问题变成区分"真实词对"与"中心词+随机负样本"的逻辑回归（$\sigma$ 为 sigmoid）：
$$\log\sigma(u_o^\top v_c)+\sum_{\ell=1}^{k}\log\sigma(-u_\ell^\top v_c),\quad u_\ell\sim P_{neg}$$
- 第一项：让真实对 $(c,o)$ 相似度变大；第二项：让 $c$ 与 $k$ 个随机负样本变小。
- 直觉：每步只"压低"少数随机负词，平均效果近似压低全词表，省去 partition function。

## 关联
- [[_index|Basics]] · 后续：上下文相关表示 → [[10-Topics/Transformer/_index|Transformer]]

> TODO：Word Vectors 2（CBOW / SVD 细节）看完后补充。
