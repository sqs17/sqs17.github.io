+++
title = "A-MEM: Agentic Memory for LLM Agents"
date = 2026-04-14T14:40:00+08:00
draft = false
slug = "a-mem-agentic-memory-for-llm-agents"
summary = "A-MEM 将长期记忆建成可自组织、可链接、可演化的 note network，用代理式记忆替代静态写入与检索流程。"
tags = ["论文笔记", "LLM Agent", "Memory", "Agentic AI"]
categories = ["论文笔记"]
+++

<div class="paper-hero">
  <img src="images/architecture-overview.png" alt="A-MEM 整体框架图">
  <div class="paper-highlight">
    <strong>一句话总结</strong>
    <p>A-MEM 把 LLM Agent 的长期记忆从静态存取模块改造成可自动建 note、自动连边、自动演化的记忆网络，因此在长程对话和多跳推理上明显强于传统 memory baselines。</p>
  </div>
</div>

## 一、基本信息

- 题目：A-MEM: Agentic Memory for LLM Agents
- 年份：2025
- 会议 / 期刊：arXiv preprint
- 机构（第一署名单位）：Rutgers University
- 论文链接：https://arxiv.org/abs/2502.12110
- 代码链接：https://github.com/WujiangXu/AgenticMemory
- 系统代码：https://github.com/WujiangXu/A-mem-sys

## 二、Motivation

### 2.1 任务定义

- 任务目标：为 LLM Agent 设计一个支持长期交互的通用记忆系统。
- 任务输入：Agent 与环境的历史交互内容，以及当前查询或当前轮交互。
- 任务输出：能够被后续推理调用的相关记忆，以及随新经验持续更新的记忆结构。

### 2.2 研究问题

- 现有记忆系统通常只支持预定义的写入、检索和存储结构，灵活性差。
- 即便引入图数据库，很多方法依然依赖静态 schema 和固定关系，难以适应开放环境中的新知识。
- 作者要回答的问题是：能否让记忆系统像 agent 一样主动组织记忆，而不是只做被动存储与召回。

## 三、方法

### 3.1 整体流程

1. Agent 与环境交互后，系统将一段新经验写成一个结构化 note。
2. 对这个新 note 生成上下文描述、关键词、标签和 embedding。
3. 用 embedding 从历史记忆中检索 top-k 相关 note。
4. 让 LLM 判断新旧记忆之间是否应该建立链接，形成“盒子 / box”式关联结构。
5. 再根据新记忆反向更新旧记忆的上下文、关键词与标签，完成 memory evolution。
6. 检索阶段先找与当前 query 最相关的 note，再联动取出同 box 中的 relative memory 供 agent 使用。

### 3.2 关键模块

- Note Construction：把原始交互转成结构化 note，字段包括内容、时间戳、关键词、标签、上下文描述、embedding 和 links。
- Link Generation：先做相似度召回，再让 LLM 判断哪些历史记忆应该与当前记忆建立连接。
- Memory Evolution：新记忆写入后，不只新增节点，还会触发旧记忆的描述与属性更新。
- Memory Retrieval：检索当前 query 对应的核心记忆，并把相关 linked memory 一起带出来。

### 3.3 创新技术

- 借鉴 Zettelkasten 方法，把记忆组织成原子化 note 与动态链接网络，而不是固定表结构。
- 把“连边”和“演化”都交给 LLM 决策，使记忆结构能随着新经验持续重组。
- 检索的不只是语义最相近的一条记忆，而是围绕相关 note 进一步展开相对记忆，支持多跳推理。

## 四、实验分析

### 4.1 数据集

- LoCoMo：长程对话问答数据集，平均对话约 9K token，最多跨 35 个 session，包含 single-hop、multi-hop、temporal、open-domain、adversarial 等问题类型。
- DialSim：来源于长程多方对话场景，覆盖 Friends、The Big Bang Theory、The Office 等剧集，适合测试长程对话记忆能力。

### 4.2 对比基线

- LoCoMo
- ReadAgent
- MemoryBank
- MemGPT

### 4.3 评价指标

- 主指标：F1、BLEU-1
- 补充指标：ROUGE-L、ROUGE-2、METEOR、SBERT Similarity

### 4.4 实验问题与结果

- 主要结果：A-MEM 在非 GPT 基础模型上持续优于所有基线；在 GPT 系模型上，虽然 Open Domain 和 Adversarial 某些项基线也很强，但 A-MEM 在 Multi-Hop 复杂推理上优势最明显。
- 典型结果 1：在 DialSim 上，A-MEM 的 F1 为 3.45，高于 LoCoMo 的 2.55，也显著高于 MemGPT 的 1.18。
- 典型结果 2：论文补充实验中，GPT-4o-mini + A-MEM 在 Multi-Hop 上的 ROUGE-L 达到 44.27，而 LoCoMo 为 18.09，基本是翻倍优势。
- 消融结果：完整模型在所有类别都优于去掉 Link Generation 和 Memory Evolution 的版本；以 Multi-Hop F1 为例，完整模型 27.02，去掉 ME 后 21.35，同时去掉 LG 和 ME 后只有 9.65。
- 效率结果：A-MEM 单次 memory operation 约使用 1,200 到 2,500 token，相比 LoCoMo 和 MemGPT 约 16,900 token，减少约 85% 到 93%；使用 GPT-4o-mini 时平均处理约 5.4 秒，本地 Llama 3.2 1B 约 1.1 秒。
