# Unreal-MAP：基于虚幻引擎的多智能体强化学习通用平台

[English](README.md) | [中文](README_CN.md)

---

## 🏛️ 关于本仓库

本仓库由 **[CASIA-Collect-AI](https://github.com/CASIA-Collect-AI)** 维护，作为高质量 MARL 仿真环境的精选集合。

📌 **原始仓库（推荐访问）：** [binary-husky/unreal-map](https://github.com/binary-husky/unreal-map)
⭐ **如果本工作对你有帮助，请前往原始仓库点 Star 支持作者！**

🔗 **配套算法框架：** [HMAP/HMP2G](https://github.com/binary-husky/hmp2g) — 专为与 Unreal-MAP 协同使用而设计的强大 MARL 实验框架。

> **团队：** 中国科学院自动化研究所 飞行器智能技术团队（群体智能团队-蒲志强）
> CASIA-Collect-AI 是由中国科学院自动化研究所 AI 团队维护的开源代码收录平台。

---

[![Version](https://img.shields.io/badge/version-3.14-blue)](https://github.com/binary-husky/unreal-map)
[![License](https://img.shields.io/badge/license-MIT-green)](LICENSE)
[![Python](https://img.shields.io/badge/python-3.7+-blue)](https://www.python.org/)
[![Unreal Engine](https://img.shields.io/badge/Unreal%20Engine-4.27-blue)](https://www.unrealengine.com/)
[![stars](https://img.shields.io/github/stars/binary-husky/unreal-map)](https://github.com/binary-husky/unreal-map)

这是 **Unreal 多智能体竞技场**（Unreal-MAP），一个基于[虚幻引擎](https://www.unrealengine.com/)的多智能体通用平台。在这里，你可以使用虚幻引擎的全部能力（蓝图、行为树、物理引擎、AI 导航、3D 模型/动画及插件资源等）构建优雅（同时计算高效）且宏大（同时实验可复现）的多智能体环境。

Unreal-MAP 不仅可用于开发常规多智能体仿真环境，还专门针对多智能体强化学习（MARL）仿真进行了优化。你可以用它开发各种真实且复杂的 MARL 场景，也可以将 Unreal-MAP 与我们开发的 [HMAP](https://github.com/binary-husky/hmp2g)（强大的 MARL 专用实验框架）结合使用，轻松开发 MARL 场景并快速部署前沿算法。

> 本研究团队正在寻找潜在合作伙伴。如有意向，欢迎联系我们在中科院的办公室：tenghai.qiu@ia.ac.cn，hutianyi2021@ia.ac.cn。

**请 ```star``` 本 Github 项目。你的鼓励对我们研究人员极为重要：```https://github.com/binary-husky/unreal-map```** !

<div align="center">
<img src="Docs/Imgs/Overall.png"/ width="550"> 
</div>

# 1. 简介
### 1.1 基本介绍
基于虚幻引擎的多智能体竞技场（Unreal-MAP）是新一代基于虚幻引擎的多智能体通用平台。本平台支持集群与算法间的对抗训练，是第一个（也是目前唯一一个）基于虚幻引擎的可扩展 RL/MARL 环境，支持多团队训练。

### 1.2 架构
<div align="center">
<img src="Docs/Imgs/Architecture.png"/ width="800"> 
</div>

Unreal-MAP 采用分层五层架构，每一层建立在前一层基础之上。从下到上，五层分别为：*原生层*、*规范层*、*基础类层*、***高级模块层*** 和 ***接口层***。**你只需关注*高级模块层*（蓝图）和*接口层*（Python）。** 从创建标准 MARL 场景的角度来看，使用这两层就足以修改任务中的所有元素（如 POMDP）——包括状态、动作、观测、转移等。

### 1.3 特性

Unreal-MAP 可用于开发各种多智能体仿真场景。我们的案例研究已涵盖大规模、异构和多团队特性的场景。
**与其他 RL 通用平台相比**（如 [Unity ML-Agents](https://unity-technologies.github.io/ml-agents/)），Unreal-MAP 在科学研究和实验方面具有以下优势：

**(1) 完全开源且易于修改**：Unreal-MAP 采用分层设计，从底层引擎到顶层接口的所有组件均已开源。

**(2) 专门针对 MARL 优化**：Unreal-MAP 的底层引擎经过优化，提升了大规模智能体仿真和数据传输的效率。

**(3) 并行多进程执行与可控单进程时间流**：Unreal-MAP 支持多个仿真进程的并行执行，以及单进程仿真时间流速的调整。你可以加速仿真以加快训练，或减速仿真进行详细的慢动作分析。

**与所有现有 MARL 仿真环境相比**，Unreal-MAP 在科学研究和实验方面的优势：

- 利用[虚幻引擎市场](https://www.fab.com/)中丰富的资源**自由构建真实任务**。
- 同时支持**大规模、异构、多团队**仿真。
- **高效训练**：TPS（每秒时间步）高达 10k+，FPS（每秒帧数）高达 10M+。
- **可控仿真时间**：可加速仿真以加快训练（直到 CPU 充分利用，加速不消耗额外内存或显存），或减速进行慢动作分析。
- **强可复现性**：消除了虚幻引擎中可能导致实验不可复现的各种蝴蝶效应因素。
- **多平台支持**：在 Windows、Linux 和 MacOS 上编译无头模式和渲染模式客户端。
- **丰富的渲染机制**：支持 a) 在 UE 编辑器中渲染，b) 在编译好的纯渲染客户端上渲染，c) 跨平台实时渲染。你可以在 Linux 服务器上训练，同时在 Windows 主机上渲染！

<div align="center">
<img src="Docs/unreal-island.jpg" height="200" width="320"/> <img src="https://github.com/binary-husky/unreal-map/assets/96192199/985c2c27-bc0a-4c90-a036-ec676d7aec1d" height="200" width="320"/> 
</div>

<div align="center">
<img src="Docs/Demo/uhmap-bbad.jpg" height="200" width="320"/> <img src="Docs/Demo/uhmap-hete.jpg" height="200" width="320"/> 
</div>
<div align="center">
<img src="Docs/Demo/2023-02-12 155956.jpg" height="200" width="320"/> <img src="Docs/Demo/2023-02-12 151938.jpg" height="200" width="320"/> 
</div>

### 1.4 未来工作

Unreal-MAP 将现代游戏引擎引入 MARL 领域，具有巨大潜力。这种潜力主要体现在两个维度：**可扩展性**和**真实性**。在可扩展性方面，用户不仅可以利用虚幻引擎社区极其丰富的资源***自由***构建环境，还可以利用虚幻引擎未来的生成式 AI 插件（如 [ACE](https://developer.nvidia.com/ace)）***快速***按照自己的想法构建环境。

在真实性方面，用户可以利用 Unreal-MAP 构建***高度真实***的 MARL 环境，甚至开发现实场景的***数字孪生***。我们使用 Unreal-MAP 尝试了一个 sim2real 演示。在这个演示中，我们首先在实验场中部署了多无人机-无人地面车辆博弈场景，然后使用 Unreal-MAP 重建了该场景（包括模型比例、智能体运动学和动力学等）。我们在仿真环境中进行训练，然后在现实场景中进行验证，取得了初步的积极结果。

<div align="center">
<img src="Docs/Imgs/Sim2RealEXP.png" width="800"/> 
</div>

<div align="center">
<img src="Docs/Imgs/Sim2RealFra.png" width="700"/> 
</div>

# 2. 如何安装

## 2.1 专业版

- 步骤 1：必须从源代码安装虚幻引擎。详细信息参见虚幻引擎官方文档：`https://docs.unrealengine.com/4.27/zh-CN/ProductionPipelines/DevelopmentSetup/BuildingUnrealEngine/`
- 步骤 2：克隆 git 仓库 `git clone https://github.com/binary-husky/unreal-hmp.git`
- 步骤 3：下载 github 无法管理的大文件。运行 `python Please_Run_This_First_To_Fetch_Big_Files.py`
- 步骤 4：右键点击步骤 3 中下载的 `UHMP.upproject`，选择 `switch unreal engine version`，然后选择 `source build at xxxxx` 确认。然后打开生成的 `UHMP.sln` 并编译
- 最后，双击 `UHMP.upproject` 进入虚幻引擎编辑器。

注意步骤 1 和 4 较为困难。建议参考以下视频（视频中 0:00->1:46 为步骤 1，1:46->结束为步骤 4）：`https://ageasga-my.sharepoint.com/:v:/g/personal/fuqingxu_yiteam_tech/EawfqsV2jF5Nsv3KF7X1-woBH-VTvELL6FSRX4cIgUboLg?e=Vmp67E`

## 2.2 仅编译二进制版本

`https://github.com/binary-husky/hmp2g/blob/master/ZDOCS/use_unreal_hmap.md`

# 3. 教程
文档持续完善中。简单演示的视频教程请参见 `EnvDesignTutorial.pptx`（需完成安装步骤 3 后下载此 pptx 文件）

目录：
- 第一章 虚幻引擎
  - 构建地图（关卡）
  - 建立智能体 Actor
  - 设计智能体蓝图程序逻辑
  - 回合关键事件通知机制
  - 定义自定义动作（虚幻引擎侧）
  - Python 侧控制智能体自定义参数
- 第二章 Python 接口
  - 创建任务文件（SubTask）
  - 修改智能体初始化代码
  - 修改智能体奖励代码
  - 选择每个团队的控制算法
  - 完整闭环调试方法
- 第三章 附录
  - 无头加速与跨编译 Linux 包
  - 定义自定义动作（需先熟悉完整闭环调试方法）
  - 安装跨编译工具链指南

# 4. 如何构建二进制客户端
运行以下脚本：
```
python BuildlinuxRender.py
python BuildLinuxServer.py
python BuildWinRender.py
python BuildWinServer.py
```
- 其中，`Render/Server` 表示 `包含图形渲染 / 仅计算`，后者通常用于 RL 训练。
- 其中，`Windows/linux` 表示目标操作系统。注意需要安装 `虚幻引擎跨编译工具` 才能在 Windows 上编译 Linux 程序。

# 引用本项目

```
@article{unrealmap,
  title={Unreal-MAP: Unreal-Engine-Based General Platform for Multi-Agent Reinforcement Learning},
  author={Hu, Tianyi and Fu, Qingxu and Pu, Zhiqiang and Wang, Yuan and Qiu, Tenghai},
  journal={arXiv preprint arXiv:2503.15947},
  year={2025}
}
```
