# chapter6 基于语言的安全/Language-Based Security

## 信息流

* 显式流
* 隐式流

### 安全性

* 机密性
* 完整性
* 可用性

### 流关系

$$
\rightarrow \subseteq(\text { Var } \cup \text { Arr }) \times(\text { Var } \cup \text { Arr })
$$

* $x \rightarrow y$：x流向y是允许的；不在流关系的流是不允许的
* 机密性：若x是private，y是public，则$y \rightarrow x$ 且 $x \not \rightarrow y$
* 完整性：若x是trusted，y是dubious，则$x \rightarrow y$ 且 $y \rightarrow x$

$$
\rightrightarrows \subseteq 2^{\text {Var} \cup \text{Arr }} \times 2^{\text {Var} \cup \text{Arr }}
$$

* $X \rightrightarrows Y$ 指 $\forall x \in X: \forall y \in Y: x \rightarrow y$
* $\rightrightarrows$不必自反、对称或传递

## 引用监控语义/Reference-Monitor Semantics

$$
\mathcal{S} \llbracket x:=a \rrbracket \sigma=\left\{\begin{array}{ll}
\sigma[x \mapsto \mathcal{A}[a] \sigma] & \text { if } \mathcal{A} \llbracket a \rrbracket \sigma \text { is defined } \\
\text { undefined } & \text { otherwise }
\end{array}\right.
$$

* 显式流
    $$
    \mathcal{S} \llbracket x:=a \rrbracket \sigma=\left\{\begin{array}{cc}
    \sigma[x \mapsto \mathcal{A} \llbracket a \rrbracket \sigma] & \text { if } \quad \mathcal{A} \llbracket a \rrbracket \sigma \text { is defined } \\
    & \text { and } \mathrm{fv}(a) \rightrightarrows\{x\} \\
    \text { undefined } & \text { otherwise }
    \end{array}\right.
    $$
* 隐式流
    $$
    \mathcal{S} \llbracket x:=a\{X\} \rrbracket \sigma=\left\{\begin{array}{ll}
    \sigma[x \mapsto \mathcal{A} \llbracket a \rrbracket \sigma] & \text { if } \quad \mathcal{A} \llbracket a \rrbracket \sigma \text { is defined } \\
    & \quad \text { and } X \cup \mathrm{fv}(a) \rightrightarrows\{x\} \\
    \text { undefined } & \text { otherwise }
    \end{array}\right.
    $$

### 插桩程序图

* 边构造函数$edge_s$
* 卫兵命令的边构造函数$edge_{s2}$

### 定义语义

* 语义论域
    $$
    \operatorname{Mem}=(\operatorname{Var} \cup\{A[i] \mid A \in \operatorname{Arr}, 0 \leq i<\operatorname{length}(A)\}) \rightarrow \operatorname{lnt}
    $$
* 语义函数
    $$
    \mathcal{S}_{i}[\cdot]: \mathrm{ACT} \rightarrow(\mathrm{Mem} \hookrightarrow \mathrm{Mem})
    $$
    * $S_0$：普通语义
    * $S_1$：引用监控语义

#### 语义

* 执行步
    $$
    \left\langle q_{\circ} ; \sigma\right\rangle \stackrel{\alpha}{\Longrightarrow}_{i}\left\langle q_{\bullet} ; \sigma^{\prime}\right\rangle \text { if } \mathcal{S}_{i} \llbracket \alpha \rrbracket \sigma=\sigma^{\prime}
    $$
* 执行多步
    $$
    \left\langle q_{0} ; \sigma\right\rangle \stackrel{\omega}{\Longrightarrow}_{i}^{*}\left\langle q_{\bullet} ; \sigma^{\prime}\right\rangle
    $$
* 语义的关系
    若 $\left\langle q_{\circ} ; \sigma\right\rangle \Longrightarrow_{1}^{*}\left\langle q_{\bullet} ; \sigma^{\prime}\right\rangle$, 则 $\left\langle q_{\circ} ; \sigma\right\rangle \Longrightarrow_{0}^{*}\left\langle q_{\bullet} ; \sigma^{\prime}\right\rangle$，反之不成立。

## 安全性分析/Security Analysis

### sec分析函数

$$
\sec \llbracket C \rrbracket(X)=\text { true }
$$
则C关于X（隐含依赖关系）是安全的。

* 命令的安全分析
* 卫兵命令的安全分析

### 安全性分析的性质

若$\sec \llbracket C\rrbracket(X), q_{\circ} \neq q_{\bullet}$，且
$$
\left(q, x:=a\left\{X^{\prime}\right\}, q^{\prime}\right) \in \operatorname{edges}_{s}\left(q_{\circ} \rightsquigarrow q_{\bullet}\right) \llbracket C \rrbracket(X)
$$
则$X^{\prime} \cup \mathrm{fv}(a) \rightrightarrows\{x\}$ 且 $X \subseteq X^{\prime}$

若$\sec \llbracket C\rrbracket(X), q_{\circ} \neq q_{\bullet}$，且
$$
\left(q, A[a_1]:=a_2\left\{X^{\prime}\right\}, q^{\prime}\right) \in \operatorname{edges}_{s}\left(q_{\circ} \rightsquigarrow q_{\bullet}\right) \llbracket C \rrbracket(X)
$$
则$X^{\prime} \cup \mathrm{fv}(a_1) \cup \mathrm{fv}(a_2) \rightrightarrows\{x\}$ 且 $X \subseteq X^{\prime}$

#### 可靠性

假设$\sec \llbracket C\rrbracket (X), \Longrightarrow_{0}$ 和 $\Longrightarrow_{1}$分别是由插桩程序图
$$
\text { edges }_{s}\left(q_{\circ} \rightsquigarrow q_{\bullet}\right) \llbracket C \rrbracket(X)\left(q_{\circ} \neq q_{\bullet}\right)
$$
而得到的普通语义和引用监控语义。有：
$$
\text { 若 }\langle q ; \sigma\rangle \Longrightarrow_{0}^{*}\left\langle q^{\prime} ; \sigma^{\prime}\right\rangle, \text { 则 }\langle q ; \sigma\rangle \Longrightarrow_{1}^{*}\left\langle q^{\prime} ; \sigma^{\prime}\right\rangle
$$

## 多层次安全

### 安全格

偏序集：
* 自反
* 传递
* 反对称

若偏序集L既是$\sqcup$-半格又是$\sqcap$-半格，则L是安全格

### 图形表示

Hasse图
* 假设$(L, \rightarrow)$是一个有向无环u
* 令$\sqsubseteq=\rightarrow^{*}$
* 则$(L, \sqsubseteq)$是一个偏序集

### 从安全格生成安全策略

## 无干扰性/Non-Interference

### 插桩的程序图之若干性质

* 若$\left(q^{\prime}, \alpha, q^{\prime \prime}\right) \in \operatorname{edges}_{s}\left(q_{\circ} \rightsquigarrow q_{\bullet}\right) \llbracket C \rrbracket(X)$（其中$q_{\circ} \neq q$）则$q^{\prime} \neq q$。
* 若 $\left(q^{\prime}, \alpha_{1}, q_{1}\right),\left(q^{\prime}, \alpha_{2}, q_{2}\right) \in \operatorname{edges}_{s}\left(q_{\circ} \rightsquigarrow q_{\bullet}\right)[C \rrbracket(X)($ 其中$\left.q_{\circ} \neq q_{\bullet}\right)$, 且 $\left(q^{\prime}, \alpha_{1}, q_{1}\right) \neq\left(q^{\prime}, \alpha_{2}, q_{2}\right)$则对某$b_1$和$b_2$有$\alpha_1 = b_1$且$\alpha_2=b_2$
* 若 $\left(q^{\prime}, \alpha_{1}, q_{1}\right),\left(q^{\prime}, \alpha_{2}, q_{2}\right) \in \operatorname{edges}_{s}\left(q_{\circ} \rightsquigarrow q_{\bullet}\right) \llbracket C \rrbracket(X)\left(\right.$ 其中 $\left.q_{\circ} \neq q_{\bullet}\right)$, 且$\left(q^{\prime}, \alpha_{1}, q_{1}\right) \neq\left(q^{\prime}, \alpha_{2}, q_{2}\right)$，则，对合适的$q^{\prime\prime}$和$X^\prime$（满足$X \subseteq X^{\prime}$）,以下两种情况之一成立：
  * C包含子程序if GC fi满足
    $$
    \begin{aligned}
    \left(q^{\prime}, \alpha_{1}, q_{1}\right),\left(q^{\prime}, \alpha_{2}, q_{2}\right) & \in \text { edges }_{s}\left(q^{\prime} \rightsquigarrow q^{\prime \prime}\right) \llbracket \text { if } G C \mathrm{f} 1 \rrbracket\left(X^{\prime}\right) \\
    & \subseteq \text { edges }_{s}\left(q_{\circ} \rightsquigarrow q_{\bullet}\right) \llbracket C \rrbracket(X)
    \end{aligned}
    $$
  * C包含子程序do GC od满足
    $$
    \begin{aligned}
    \left(q^{\prime}, \alpha_{1}, q_{1}\right),\left(q^{\prime}, \alpha_{2}, q_{2}\right) & \in \text { edges }_{s}\left(q^{\prime} \rightsquigarrow q^{\prime \prime}\right) \llbracket \text { do } G C \text { od } \rrbracket\left(X^{\prime}\right) \\
    & \subseteq \text { edges }_{s}\left(q_{\circ} \rightsquigarrow q_{\bullet}\right) \llbracket C \rrbracket(X)
    \end{aligned}
    $$

### 影响集

定义
$$
\uparrow x=\{y \in \operatorname{Var} \cup \operatorname{Arr} \mid y \rightarrow x\}
$$
性质
$$
x \in \uparrow x
$$

### 无干扰性

假设$\sec \llbracket C \rrbracket(X)$, 由插装程序图 $edges \left._{s}(q_{\circ} \rightsquigarrow q_{\bullet}\right) \llbracket C\rrbracket (X)\left(q_{\circ} \neq q_{\bullet}\right)$ 获得的普通语义和引用监控语义分别是 $\Longrightarrow_{0}$ 和 $\Longrightarrow_{1}$ 。若
$$
\begin{array}{l}
\left\langle q ; \sigma_{1}\right\rangle \Longrightarrow_{0}^{*}\left\langle q_{\bullet} ; \sigma_{1}^{\prime}\right\rangle \text { 且 }\left\langle q ; \sigma_{2}\right\rangle \Longrightarrow_{0}^{*}\left\langle q_{\bullet} ; \sigma_{2}^{\prime}\right\rangle \\
\text { 且 } \forall y \in \uparrow x: \sigma_{1}(y)=\sigma_{2}(y)
\end{array}
$$
则
$$
\sigma_{1}^{\prime}(x)=\sigma_{2}^{\prime}(x)
$$

证明：使用归纳法证明