# lecture2程序图

## 模型

模型是一个系统的抽象——提炼关键而忽略细节。模型在一定抽象层次上不仅需要描述系统的结构，而且需要表达系统的行为。

程序图作为模型：
* 图形化表示：程序图给出了系统关于节点和节点间迁移的抽象
* 操作化方法：通过定义系统动作的语义来表达程序图的精确含义
* 多层次抽象：在不同的数据抽象上来表达不同层次的语义

## 程序图的定义

### 程序图的定义

程序图PG
$$
\mathrm{PG}=\left(\mathbf{Q}, q_{\triangleright}, q_{\blacktriangleleft}, \mathbf{A} \mathbf{C} \mathbf{T}, \mathbf{E}\right)
$$
其中：
* Q：有穷结点集
* $q_{\triangleright}, q_{\mathbf{\blacktriangleleft}} \in \mathbf{Q}$：分别是初始结点、终止结点
* ACT：动作集合
* $\mathrm{E} \subseteq \mathbf{Q} \times \mathbf{A C T} \times \mathbf{Q}$：有穷边集。$\left(q_{\circ}, \alpha, q_{\bullet}\right)$中，$q_{\circ}$是源结点，$q_{\bullet}$是目标结点，$\alpha$是标注的动作

### 程序图作为自动机

程序图可以看作是一个自动机从而定义程序图的形式语言。

L(PG)

$$
\mathbf{L}(\mathbf{P G})=\mathbf{L}\left(q_{\triangleright} \rightsquigarrow q_{\blacktriangleleft}\right)(\mathbf{E})
$$

其中

$$
\begin{aligned}
\mathrm{L}\left(q_{\circ} \rightsquigarrow q_{\bullet}\right)(\mathbf{E})=&\left\{\alpha_{1} \cdots \alpha_{n} \mid\right.\\
& \exists q_{0}, \cdots, q_{n}: \forall i \in\{0, \cdots, n-1\}:\left(q_{i}, \alpha_{i}, q_{i+1}\right) \in \mathbf{E}, \\
&\left.q_{0}=q_{\circ}, q_{n}=q_{\bullet}\right\}
\end{aligned}
$$

L(PG)是正规语言

## 程序图的语义

* 语义域：程序或系统的内存空间，其元素是程序和系统在给定点的状态
* 动作的语义动作：执行产生的状态变化
* 程序图语义：动作执行序列的状态变化。
  * 内存：非空集合Mem
  * 语义函数$\mathrm{S} \llbracket \cdot \rrbracket$：$\mathrm{ACT} \rightarrow(\mathrm{Mem} \hookrightarrow \mathrm{Mem})$
    $$
    \mathrm{S} \llbracket \alpha \rrbracket \sigma = \sigma^\prime
    $$

### 格局与迁移

* 格局

$$
\langle q ; \sigma\rangle
$$
其中$q \in \mathbf{Q}, \sigma \in$ Mem。

* 迁移（执行步）

$$
\left\langle q_{\circ} ; \sigma\right\rangle \stackrel{\alpha}{\Longrightarrow}\left\langle q_{\bullet} ; \sigma^{\prime}\right\rangle \text { if } S\llbracket\alpha \rrbracket \sigma=\sigma^{\prime}
$$

其中$\left(q_{o}, \alpha, q_{\bullet}\right) \in \mathbf{E}$

* 迁移闭包
  * $\Rightarrow$的自反传递的闭包：
    $$
    \langle q ; \sigma\rangle \stackrel{\omega}{\Longrightarrow}{ }^{*}\left\langle q^{\prime} ; \sigma^{\prime}\right\rangle
    $$
  * 执行序列：
    $$
    \langle q ; \sigma\rangle \stackrel{\omega}{\Longrightarrow}{ }^{*}\left\langle q^{\prime} ; \sigma^{\prime}\right\rangle
    $$
  * 完全执行序列：
    $$
    \left\langle q_{\triangleright} ; \sigma\right\rangle \stackrel{\omega}{\Longrightarrow}^{*}\left\langle q_{\blacktriangleleft} ; \sigma^{\prime}\right\rangle
    $$
    * 系统有非确定行为
        $$
        \exists \sigma_{1}, \sigma_{2} \cdot\left\langle q_{\triangleright} ; \sigma\right\rangle \stackrel{\omega}{\Longrightarrow}{ }^{*}\left\langle q_{\blacktriangleleft} ; \sigma_{2}\right\rangle \wedge\left\langle q_{\triangleright} ; \sigma\right\rangle \Longrightarrow^{\omega}{\Longrightarrow}\left\langle q_{\blacktriangleleft} ; \sigma_{1}\right\rangle \wedge \sigma_{1} \neq \sigma_{2}
        $$
    * 系统有发散行为
        $$
        \neg\left(\forall \sigma \exists \sigma^{\prime} \cdot\left\langle q_{\triangleright} ; \sigma\right\rangle \stackrel{\omega}{\Longrightarrow}{ }^{*}\left\langle q_{\blacktriangleleft} ; \sigma^{\prime}\right\rangle\right)
        $$

## 内存的结构

### 内存的映射模型

* 内存模型

    $$
    \sigma: \text { Var } \rightarrow \text { Int }
    $$

    其中Var表示$\left\{x_{1}, \cdots, x_{n}\right\}$，亦记为$dom(\sigma)$。

    $$
    \sigma[x \mapsto v](x) \triangleq\left\{\begin{array}{ll}
    v & \text { if } x \in \operatorname{dom}(\sigma) \\
    \sigma(x) & \text { otherwise }
    \end{array}\right.
    $$

* 内存的数组扩展

    $$
    \sigma:(\operatorname{Var} \cup\{A[i] \mid A \in \operatorname{Arr}, 0 \leq i<\operatorname{length}(A)\}) \rightarrow \text { Int }
    $$

    $$
    \sigma[A[i] \mapsto v](x) \triangleq\left\{\begin{array}{ll}
    v & \text { if } x=A[i] \text { and } 0 \leq i<\text { length }(A) \\
    \sigma(x) & \text { otherwise }
    \end{array}\right.
    $$

## 程序图的性质

### 确定的系统

程序图PG及其语义S构成一个确定的系统

$$
\begin{array}{l}
\left\langle q_{\triangleright} ; \sigma\right\rangle \stackrel{\omega^{\prime}}{\Longrightarrow}^{*}\left\langle q_{\blacktriangleleft} ; \sigma^{\prime}\right\rangle \text { and }\left\langle q_{\triangleright} ; \sigma\right\rangle \stackrel{\omega^{\prime \prime}}{\Longrightarrow}^{*}\left\langle q_{\blacktriangleleft} ; \sigma^{\prime \prime}\right\rangle\\
\text { imply that } \sigma^{\prime}=\sigma^{\prime \prime} \text { and that } \omega^{\prime}=\omega^{\prime \prime}
\end{array}
$$

$\textbf{充分条件}$ 如果

$$
\forall\left(q, \alpha_{1}, q_{1}\right),\left(q, \alpha_{2}, q_{2}\right) \in \mathbf{E}:\left(\begin{array}{l}
\left(\alpha_{1}, q_{1}\right) \neq\left(\alpha_{2}, q_{2}\right) \Rightarrow \\
\left(\operatorname{dom}\left(S \llbracket \alpha_{1} \rrbracket\right)\right) \cap\left(\operatorname{dom}\left(S\left[\alpha_{2}\right]\right)\right)=\emptyset
\end{array}\right)
$$

则程序图PG及其语义S构成一个确定的系统。

$\textbf{证明}$

$$
\begin{array}{l}
\text { If }\left\langle q_{\triangleright} ; \sigma\right\rangle \stackrel{\omega^{\prime}}{\Longrightarrow}^{*}\left\langle q^{\prime} ; \sigma^{\prime}\right\rangle,\left\langle q_{\triangleright} ; \sigma\right\rangle \stackrel{\omega^{\prime \prime}}{\Rightarrow}^{*}\left\langle q^{\prime \prime} ; \sigma^{\prime \prime}\right\rangle \text { and }\left|\omega^{\prime}\right|=\left|\omega^{\prime \prime}\right| \\
\text { then } q=q^{\prime \prime}, \sigma^{\prime}=\sigma^{\prime \prime} \text { and } \omega^{\prime}=\omega^{\prime \prime}
\end{array}
$$

### 可演进系统

程序图PG及其语义S构成一个可演进系统

$$
\left\langle q_{\triangleright} ; \sigma\right\rangle \stackrel{\omega^{\prime}}{\Longrightarrow}^{*}\left\langle q^{\prime} ; \sigma^{\prime}\right\rangle
$$

且$q^{\prime} \neq q_{\blacktriangleleft}$，则其总可以扩展为

$$
\left\langle q_{\triangleright} ; \sigma\right\rangle \stackrel{\omega^{\prime \prime}}{\Longrightarrow}^{*}\left\langle q^{\prime \prime} ; \sigma^{\prime \prime}\right\rangle
$$

其中$\omega^{\prime \prime}=\omega^{\prime} \alpha^{\prime}$。

$\textbf{充分条件}$ 如果

$$
\forall q \in \mathbf{Q} \backslash\left\{q_{\blacktriangleleft}\right\}: \forall \sigma \in \mathbf{M e m}: \exists\left(q, \alpha, q^{\prime}\right) \in \mathbf{E}: \sigma \in \operatorname{dom}(S \llbracket \alpha \rrbracket)
$$

则程序图PG及其语义S构成一个可演进系统。

## 位语义

* 建模层次
  * 数学整数
  * 无符号字节
  * 含符号字节
