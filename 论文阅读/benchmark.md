# Benchmark

## 文章

- BEARS: An Extensible Java Bug Benchmark for Automatic Program Repair Studies
- Exploring True Test Overfitting in Dynamic Automated Program Repair using Formal Methods

## 准则

### Bears

1. 故障修复的两种情况
   1. 无测试代码更改的failing-passing的建立
   2. 有测试代码更改的passing-passing的建立
2. 项目引入准则
   1. 项目在GitHub上公开可用
   2. 必须使用Travis持续集成服务（CI）
   3. 必须是Maven项目
3. 缺陷引入准则
   1. 缺陷一定可复现
   2. 缺陷一定已经被人类修复

### Java+JML、Buggy Java Programs

1. Java+JML：带有完整形式化行为规约的Java程序数据集
2. 测试套件：使用fuzzer（如Kelinci fuzzing tool）生成测试，避免偏差
3. 数据集构建：使用PITest进行故障注入

## Verilog数据集构建准则

1. 使用educoder中的学生作业项目进行构建：
   1. 考虑无测试代码更改的failing-passing的构建
2. 缺陷引入准则
   1. 缺陷一定可复现
   2. 缺陷已经被学生修复
3. Testbench构建准则
   1. 在已有的testbench上记录更小密度的时间戳值
   2. 使用fuzzing进行测试用例的扩充（如果需要，避免偏差）

## 一些想法

1. 故障版本和修复版本不一定时连续的前后提交。一般而言，学生的最后一次提交是正确版本，之前的版本都是故障版本，可以构建多个<故障，修复>对。
   1. 问题：更早的故障版本可能错误不止一处。
2. 提取学生的修复动作，将其反转复现其学生的故障版本。
   1. 每次的故障版本包括一个缺陷；
   2. 不通过checkout进行构建，是否违背了真实性？