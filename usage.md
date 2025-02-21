# 配置文件使用说明

配置文件用于定义内存访问模拟器的行为，包括内存区域的定义和访问模式的配置。以下是配置文件的详细说明。

## 配置文件格式

配置文件分为两个主要部分：regions（内存区域定义）和phases（访问阶段配置）。

### Regions 部分

```
#regions
name, length
```

- `name`: 内存区域的名称，用字母表示
- `length`: 内存区域的大小（以字节为单位）

示例：
```
a, 100000000
b, 100000000
```

### Phases 部分

```
#phases
phase_name
duration
region_name, randomness, stride, probability
```

每个phase包含以下参数：

- `phase_name`: 阶段名称
- `duration`: 阶段持续时间（毫秒）
- 访问模式参数：
  - `region_name`: 要访问的内存区域名称
  - `randomness`: 访问随机性（0表示顺序访问，1表示随机访问）
    - randomness = 0：表示顺序访问（sequential access），即按照固定步长（stride）依次访问内存区域
    - randomness = 1：表示随机访问（random access），即在内存区域内随机选择位置进行访问
  - `stride`: 访问步长（字节）
  - `probability`: 访问概率（1-100之间的整数）

## 配置示例说明

以zigzag.cfg为例，该配置文件定义了一个交替访问模式：

1. 定义了两个100MB的内存区域（a和b）
2. 包含多个阶段：
   - 初始阶段（10000ms）：同时顺序访问a和b区域
   - 交替阶段：在a和b区域之间交替进行密集访问
     - 每个low phase访问a区域
     - 每个high phase访问b区域
   - 每个阶段持续5000ms
   - 使用4096字节的步长
   - 50%的访问概率

这种配置可以用来模拟内存访问模式在不同区域之间来回切换的场景，有助于研究内存系统的性能特征。