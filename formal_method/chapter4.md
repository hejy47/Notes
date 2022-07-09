# chapter4 程序验证

## 谓词

### 程序正确性

### 程序逻辑

* 语法
  $$
  \begin{aligned}
  \phi::=& \text { true }\left|\phi_{1} \wedge \phi_{2}\right| \phi_{1} \vee \phi_{2}\left|\neg \phi_{0}\right| \phi_{1} \Rightarrow \phi_{2} \mid \\
  & \exists \underline{x}: \phi_{0}\left|\forall \underline{x}: \phi_{0}\right| e_{1}=e_{2}|\cdots| p\left(e_{1}, \cdots, e_{n}\right)
  \end{aligned}
  $$
  $$
  e \quad::=x|\underline{x}| e_{1}+e_{2} \mid f\left(e_{1}, \cdots, e_{n}\right)
  $$
* 语义
  $$
  (\sigma, \underline{\sigma}) \models \phi
  $$
* 提示
  * 在程序中一般有逻辑符号$(x,y,z)$，非逻辑符号（不同程序里面有不同的符号，如$sorted()$等）
  * 在整个程序里面，$\underline{\sigma}$是一样的，非逻辑变量$\sigma$在每个状态可能是不一样的。

## 谓词赋值

$$
\mathrm{P}: \mathrm{Q} \rightarrow \text { Pred }
$$

### 谓词赋值的正确性

谓词赋值P在语义S下是正确的，如果以下条件满足：每当
$$
\left(q_{\circ}, \alpha, q_{\bullet}\right) \in E
$$
且每当有$(\sigma, \underline{\sigma})$，我们有
$$
\left((\sigma, \underline{\sigma}) \models \mathbf{P}\left(q_{\circ}\right) \wedge \sigma^{\prime}=\mathcal{S} \llbracket \alpha \rrbracket(\sigma)\right) \Rightarrow\left(\sigma^{\prime}, \underline{\sigma}\right) \models \mathbf{P}\left(q_{\bullet}\right)
$$
其中$\sigma^{\prime}=\mathcal{S} \llbracket \alpha \rrbracket(\sigma)$指$\mathcal{S} \llbracket \alpha \rrbracket$在$\sigma$上有定义并且得到$\sigma^\prime$。

#### 性质

每当
$$
(\sigma, \underline{\sigma}) \models \mathbf{P}\left(q_{\circ}\right) \text { 且 }\left\langle q_{\circ} ; \sigma\right\rangle \stackrel{\omega}{\Longrightarrow}^{*}\left\langle q_{\bullet} ; \sigma^{\prime}\right\rangle
$$
我们有
$$
\left(\left(\sigma^{\prime}, \underline{\sigma}\right) \models \mathbf{P}\left(q_{\bullet}\right)\right.
$$
$\textbf{证明}$：对$w$的长度做归纳。

## 部分谓词赋值

### 部分谓词赋值覆盖程序图

部分谓词赋值$\mathrm{P}: \mathrm{Q} \hookrightarrow \mathrm{Pred}$覆盖了带初始结点$q_{\triangleright}$和最终结点$q_{\blacktriangleleft}$的程序图PG是指
* $q_{\triangleright}, q_{\blacktriangleleft} \in \operatorname{dom}(\mathbf{P})$
* PG的每个循环都有一个结点属于$dom(P)$
此时，也称$dom(P)$覆盖了程序图PG。

### 部分谓词赋值的短路径片段

给定部分谓词赋值P和边集合E的程序图，P的短路径片段
$$
\begin{array}{c}
\operatorname{spf}(\mathbf{P})=\left\{q_{0} \alpha_{1} \cdots \alpha_{n} q_{n} \mid \forall i \in\{0, \cdots, n-1\}:\left(q_{i}, \alpha_{i+1}, q_{i+1}\right) \in \mathbf{E}\right. \\
n>0, q_{0} \in \operatorname{dom}(\mathbf{P}), q_{n} \in \operatorname{dom}(\mathbf{P}) \\
\left.\forall i \in\{1, \cdots, n-1\}: q_{i} \notin \operatorname{dom}(\mathbf{P})\right\}
\end{array}
$$

若部分谓词赋值P覆盖程序图，则spf(P)是有穷集合。

#### 短路径片段的计算

#### 短路径片段的物理含义

##### 证明义务/Proof Obligation

对$spf(P)$中的路径$q_0\alpha_1\cdot\alpha_n q_{\bullet}$，该路径的证明义务定义如下：
$$
\left[\mathbf{P}\left(q_{\circ}\right)\right] \alpha_{1} \cdots \alpha_{n}\left[\mathbf{P}\left(q_{\bullet}\right)\right]
$$

##### 提升语义函数

$$
\sigma_{n}=\mathcal{S} \llbracket \alpha_{1} \cdots \alpha_{n} \rrbracket\left(\sigma_{0}\right)
$$
iff存在$\sigma_{1} \cdots \sigma_{n-1}$满足
$$
\sigma_{i}=\mathcal{S} \llbracket \alpha_{i} \rrbracket\left(\sigma_{i-1}\right) \text { for all } i \in\{1, \cdots, n\}
$$

##### 部分谓词赋值的正确性

* 正确性定义
  如果对任意的$\left(q_{\circ} \alpha_{1} \cdots \alpha_{n} q_{\bullet}\right) \in \operatorname{spf}(\mathbf{P})$和所有的合适的内存对$(\sigma, \underline{\sigma})$
    $$
    \left((\sigma, \underline{\sigma}) \models \mathbf{P}\left(q_{\circ}\right) \wedge \sigma^{\prime}=\mathcal{S} \llbracket \alpha_{1} \cdots \alpha_{n} \rrbracket(\sigma)\right) \Rightarrow\left(\sigma^{\prime}, \underline{\sigma}\right) \models \mathbf{P}\left(q_{\bullet}\right)
    $$
  成立，则称P对于$\mathcal{S} \llbracket \cdot \rrbracket$是正确的。
* 性质
  若$q_{\circ} \in \operatorname{dom}(\mathbf{P}),(\sigma, \underline{\sigma}) \models \mathbf{P}\left(q_{\circ}\right), \left\langle q_{\circ} ; \sigma\right\rangle \stackrel{\omega}{\Longrightarrow}^* \left\langle q_{\bullet} ; \sigma^{\prime}\right\rangle$, 且 $q_{\bullet} \in \operatorname{dom}(\mathbf{P})$, 则 $\left(\sigma^{\prime}, \underline{\sigma}\right) \models \mathbf{P}\left(q_{\bullet}\right)$。

## 带谓词的卫兵命令

1. 语法
2. 生成程序图
   从AP到PG
   1. 程序图
      $$
      \mathrm{PG}=<\mathrm{Q}, q_{\triangleright}, q_{\boldsymbol{\triangleleft}}, \mathbf{A C T}, \mathbf{E}>
      $$
      * $\mathbf{Q}, \quad q_{\triangleright}$ 和 $q_{\blacktriangleleft}$从E提取
      * ACT形如$x:=a, A\left[a_{1}\right]:=a_{2}$, skip, $b$
      * E从$(\mathbf{E}, \mathbf{P})=\operatorname{edges}_{v}\left(q_{\triangleright} \rightsquigarrow q_{\mathbf{-}}\right) \llbracket A P \rrbracket\left(\right.$ 且 $\left.q_{\triangleright} \neq q_{\mathbf{\triangleleft}}\right)$提取
   2. P从$(\mathbf{E}, \mathbf{P})=\operatorname{edges}_{v}\left(q_{\triangleright} \rightsquigarrow q_{\mathbf{-}}\right) \llbracket A P \rrbracket\left(\right.$提取
3. 给定带标注的程序AP，以及$(\mathbf{E}, \mathbf{P})=\operatorname{edges}_{v}\left(q_{\triangleright} \rightsquigarrow q_{\mathbf{-}}\right) \llbracket A P \rrbracket\left(\right.$则有
   dom(P)覆盖了由E提取的程序图PG

## 反向后序（Reverse Postorder）

### 深度优先展开树和反向后序

$\left\{q_{\triangleright}, q_{\triangleleft}\right\} \cup\left\{q^{\prime} \mid \exists q, \alpha:\left(q, \alpha, q^{\prime}\right) \in \mathbf{E} \wedge \mathbf{r P}[q] \geq \mathbf{r} \mathbf{P}\left[q^{\prime}\right]\right\}$覆盖程序图。
