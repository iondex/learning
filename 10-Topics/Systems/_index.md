---
title: Systems
tags: [moc, topic/systems]
---

# ⭐ Systems for LLMs（含推理优化）

**重点主题。** LLM 的系统与效率，涵盖**训练侧**和**推理侧**：

## 训练 / 底层
- GPU 体系结构、显存层级
- Triton kernel、算子优化
- 并行策略：data / tensor / pipeline parallelism、optimizer sharding

## 推理优化（Inference Optimization）
- **KV cache** 与显存管理
- **量化**（quantization）
- **投机解码**（speculative decoding）
- 批处理（continuous batching）与**服务化**（serving / 吞吐与延迟）

## 笔记
_（暂无，开始记录后在此列出）_

## 相关
- [[10-Topics/Transformer/_index|Transformer]] · [[10-Topics/Pretraining/_index|Pretraining]] · [[10-Topics/Scaling-Laws/_index|Scaling Laws]]
