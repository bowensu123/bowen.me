---
title: Hello World
date: 2025-02-22T16:00:00.000+00:00
lang: zh
duration: 10min
---
# KV Cache (Key-Value Cache)

## 概述

KV Cache（键值缓存）是一种高效的数据存储方法，可以用于加速推理任务，如自然语言处理（NLP）中的自回归生成。

在 Transformer 结构中，KV Cache 主要用于存储过去的 Key 和 Value，以避免重复计算，提高生成速度。

## 实现原理

### 1. KV Cache 结构
KV Cache 主要存储:
- `keys`: 过往的键向量
- `values`: 过往的值向量

在推理过程中，每次新生成的 token 仅计算一次 attention 并追加到缓存中。

### 2. KV Cache 更新策略
- **首次推理**: 计算并存储 `keys` 和 `values`
- **后续推理**: 追加新的 `keys` 和 `values`
- **缓存裁剪**: 根据最大缓存大小裁剪较早的 KV 数据

## 代码示例

### 1. PyTorch 代码示例
```python
import torch

class KVCache:
    def __init__(self, max_length: int, d_model: int):
        self.max_length = max_length  # 最大缓存长度
        self.d_model = d_model  # 模型隐藏维度
        self.keys = None
        self.values = None
    
    def update(self, new_keys: torch.Tensor, new_values: torch.Tensor):
        """ 更新 KV Cache """
        if self.keys is None:
            self.keys = new_keys
            self.values = new_values
        else:
            self.keys = torch.cat([self.keys, new_keys], dim=1)
            self.values = torch.cat([self.values, new_values], dim=1)
        
        # 如果超出最大缓存大小，则裁剪
        if self.keys.shape[1] > self.max_length:
            self.keys = self.keys[:, -self.max_length:, :]
            self.values = self.values[:, -self.max_length:, :]

    def get_cache(self):
        return self.keys, self.values
```

### 2. 使用示例
```python
# 假设 batch_size=1, d_model=512, max_length=100
kv_cache = KVCache(max_length=100, d_model=512)

# 模拟新的 keys 和 values (batch_size=1, seq_len=10, d_model=512)
new_keys = torch.randn(1, 10, 512)
new_values = torch.randn(1, 10, 512)

# 更新 KV Cache
kv_cache.update(new_keys, new_values)

# 获取缓存
cached_keys, cached_values = kv_cache.get_cache()
print(f"Cached Keys Shape: {cached_keys.shape}, Cached Values Shape: {cached_values.shape}")
```

## 适用场景
- 生成式 AI 模型，如 ChatGPT、Transformer 语言模型
- 提高推理效率，减少重复计算
- 长文本处理和自回归生成任务

## 总结
KV Cache 通过存储和复用过往计算的键值对，加速推理过程，是现代 Transformer 结构优化的重要技术之一。
