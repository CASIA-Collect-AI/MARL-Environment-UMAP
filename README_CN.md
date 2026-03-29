# Unreal-MAP：基于虚幻引擎的多智能体通用仿真平台

[English](README.md) | **中文**

[![Version](https://img.shields.io/badge/version-3.14-blue)](https://github.com/binary-husky/unreal-map)
[![License](https://img.shields.io/badge/license-MIT-green)](LICENSE)
[![Python](https://img.shields.io/badge/python-3.7+-blue)](https://www.python.org/)
[![Unreal Engine](https://img.shields.io/badge/Unreal%20Engine-4.27-blue)](https://www.unrealengine.com/)
[![stars](https://img.shields.io/github/stars/binary-husky/unreal-map)](https://github.com/binary-husky/unreal-map)

---

## 🏛️ 关于本仓库 | About This Repository

本仓库由 **[CASIA-Collect-AI](https://github.com/CASIA-Collect-AI)** 收录维护，作为多智能体仿真环境领域的优质开源平台集合。

📌 **原始仓库（推荐访问）：** [binary-husky/unreal-map](https://github.com/binary-husky/unreal-map)
⭐ **如果本工作对你有帮助，请前往原始仓库点 Star 支持作者！**

🔗 **配套算法框架：** [HMAP/HMP2G](https://github.com/binary-husky/hmp2g) — 与 Unreal-MAP 配合使用的强化学习实验框架

> CASIA-Collect-AI 是中国科学院自动化研究所 AI 团队维护的开源代码收录平台，专注于收录和整理 MARL、LLM、机器人等领域的高质量研究代码。

---

## 简介

**Unreal-MAP**（Unreal Multi-Agent Playground）是基于**虚幻引擎（Unreal Engine）**的新一代多智能体通用仿真平台。它支持大规模、异构、多队对抗训练，是目前**唯一**基于虚幻引擎、支持多队训练的可扩展 MARL 仿真环境。

<div align="center">
<img src="Docs/Imgs/Overall.png" width="550"/>
</div>

---

## 📖 平台深度解读

### 为什么选择虚幻引擎？与主流平台的对比

| 平台 | 引擎 | 场景多样性 | 大规模异构 | 渲染质量 | Sim2Real |
|------|------|----------|----------|---------|---------|
| **Unreal-MAP** | Unreal Engine 4/5 | ⭐⭐⭐⭐⭐ | ✅ | ⭐⭐⭐⭐⭐ | ✅ |
| Unity ML-Agents | Unity | ⭐⭐⭐⭐ | 部分支持 | ⭐⭐⭐⭐ | 有限 |
| SMAC | 星际争霸 II | ⭐⭐ | ✅ | ⭐⭐ | ❌ |
| MuJoCo | 物理引擎 | ⭐⭐⭐ | 有限 | ⭐⭐ | 有限 |
| Isaac Gym | NVIDIA | ⭐⭐⭐ | ✅ | ⭐⭐⭐ | 有限 |

**Unreal-MAP 的核心优势：**
- **完全开源可修改**：从底层引擎到顶层接口全部开源，分层设计便于扩展
- **为 MARL 专项优化**：底层针对大规模智能体仿真和数据传输做了专项优化
- **TPS 10k+, FPS 10M+**：支持高速并行多进程执行，实现超高效训练
- **可控仿真时间**：可加速训练（直到 CPU 满负荷，不消耗额外内存/显存），也可减速做慢动作分析
- **强复现性**：系统性消除了虚幻引擎中可能导致实验不可复现的蝴蝶效应因素

---

### 五层架构详解

<div align="center">
<img src="Docs/Imgs/Architecture.png" width="800"/>
</div>

Unreal-MAP 采用**五层分层架构**，从下到上依次为：

| 层级 | 名称 | 职责 | 用户是否需要关注 |
|------|------|------|--------------|
| 第1层 | **Native Layer（原生层）** | 虚幻引擎底层 C++ 代码，物理、渲染、通信 | ❌ 无需修改 |
| 第2层 | **Specification Layer（规范层）** | MARL 相关协议定义（POMDP、动作空间等） | ❌ 无需修改 |
| 第3层 | **Base Class Layer（基类层）** | 智能体、环境的抽象基类 | ❌ 继承即可 |
| 第4层 | **Advanced Module Layer（高级模块层）** | 蓝图（Blueprint）：状态/动作/观测/转移定义 | ✅ **主要工作层** |
| 第5层 | **Interface Layer（接口层）** | Python 接口：与 RL 算法连接 | ✅ **主要工作层** |

**核心结论：构建标准 MARL 场景只需关注第 4、5 层。** 通过蓝图可视化编程修改任务要素（POMDP），通过 Python 接口连接算法侧。

---

### 核心特性深度分析

#### 高性能仿真
- **TPS（时间步/秒）10k+**：通过专项优化的数据传输机制实现，支持并行多进程
- **FPS（帧/秒）10M+**：无头模式下纯计算速度，适合大规模训练
- 加速时不消耗额外内存/显存——本质是缩短仿真中每帧的等待时间

#### 大规模异构多队仿真
已验证场景包括：
- **无人机-无人车协同**（UAV-UGV，多队对抗）
- **大规模集群对抗**（百级智能体）
- **异构角色协同**（不同能力/角色的混合编队）

#### 强复现性设计
虚幻引擎存在多种可能导致不同运行结果的"蝴蝶效应"来源（如物理模拟的浮点误差积累、异步事件顺序不确定性）。Unreal-MAP 系统性地识别并消除了这些因素，确保**相同随机种子 → 完全相同实验结果**。

#### 多平台交叉渲染
- 在 Linux 服务器上训练，同时在 Windows/Mac 主机上实时渲染——无需迁移模型
- 支持：a) UE 编辑器内渲染；b) 编译后纯渲染客户端；c) 跨平台实时渲染

---

### Sim2Real 案例：无人机-无人车协同实验

<div align="center">
<img src="Docs/Imgs/Sim2RealEXP.png" width="800"/>
</div>

<div align="center">
<img src="Docs/Imgs/Sim2RealFra.png" width="700"/>
</div>

**实验路径：**
1. 在真实实验场地部署多 UAV-UGV 对抗场景
2. 在 Unreal-MAP 中**精确还原**场景（包括模型比例、运动学/动力学参数）
3. 在仿真环境中训练策略
4. 迁移到真实场景验证，取得**初步正向结果**

**Unreal-MAP 在此框架中的双重角色：**
- 仿真环境构建器
- 数据传输中间件（连接真实场景与算法侧）

---

### 与 HMAP 框架的配合使用

| 组件 | 角色 | 负责内容 |
|------|------|---------|
| **Unreal-MAP** | 仿真环境 | 场景构建、物理仿真、观测/动作接口 |
| **[HMAP/HMP2G](https://github.com/binary-husky/hmp2g)** | 算法框架 | MARL 算法实现、训练管理、实验追踪 |

两者通过标准化 Python 接口连接，可独立使用也可组合使用。HMAP 内置多种主流 MARL 算法，与 Unreal-MAP 配合可快速完成"环境搭建 → 算法部署 → 实验分析"的全流程。

---

### 适用场景建议

Unreal-MAP 特别适合以下研究方向：
- **大规模集群 MARL**（需要高 TPS 和大量并行智能体）
- **异构多队对抗**（多种角色、多个队伍）
- **Sim2Real 迁移研究**（需要高保真物理仿真和数字孪生）
- **视觉 MARL**（需要高质量渲染和丰富视觉场景）
- **自定义复杂任务**（利用虚幻引擎 Marketplace 的海量资源快速构建）

---

## 安装

### 专业版（含 Unreal Engine 编辑器）

参考 [英文 README](README.md) 的完整安装说明。

### 精简版（仅运行时）

```bash
# 下载预编译的 Unreal-MAP 客户端
# 详见原始仓库：https://github.com/binary-husky/unreal-map
```

---

## 快速开始

请参考 [英文 README](README.md) 的完整教程，或联系作者获取技术支持。

---

## 联系方式

如有意向合作，欢迎联系中科院自动化所团队：
- tenghai.qiu@ia.ac.cn（仇腾海）
- hutianyi2021@ia.ac.cn（胡天翼）
