# Dissection of a Bug Dataset: Anatomy of 395 Patches from Defects4J

## Abstract

1. 设计良好的bug数据集对于推进故障定位和程序修复的作用。（重要性）
2. 这些数据集需要被研究者深入了解。（需求）
3. 目前没有广泛的方法对其进行分析。（现状）
4. 基于此现状，作者提出的方法。（工作）
5. 实验结果的分析。（结果）
6. 实验结果的影响。（贡献）

## Introduction

1. 公开可用的bug数据集的作用。
2. 研究者们需要数据集的详细信息
   1. 根据所考虑的技术（采样和包含标准），仅选择具有所需属性的bug
   2. 根据缺陷或补丁的某些属性，对新提出的技术的性能进行高级分析（相关性分析）
3. 作者聚焦于Defect4J数据集，做了深入的分析。
4. 作者回答了以下四个问题。
5. 这篇论文研究的意义：利用只包含添加代码的补丁的技术；特定于修复模式的修复算法。

## Research Questions

1. What is the size distribution of Defects4J patches?
2. To what extent are Defects4J patches spread in source code?
3. What is the composition of Defects4J patches in terms of repair actions (i.e. addition, removal and modification) over code elements (e.g. conditional and method call)?
4. What repair patterns can be found in Defects4J using a manual thematic analysis?