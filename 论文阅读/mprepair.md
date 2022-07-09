# Multi-point repair

## Papers

- ARJA: Automated Repair of Java Programs via Multi-Objective Genetic Programming (ARJA)
- A hybrid evolutionary system for automatic software repair (ARJA-e)
- Making Better Use of Repair Templates in Automated Program Repair: A Multi-Objective Approach (ARJA-p)

## Approaches

- Program synthesis
  - Angelix (Angelix: Scalable multiline program patch synthesis via symbolic analys is.)
- Pattern-based repair
  - HERCULES (Harnessing evolution for multi-hunk program repair.)
- Iterative repair
  - DeepFix (Deepfix: Fixing common c language errors by deep learning.)
- Search-based repair

## Multi-objective evolutionary algorithm, MOEA

- Multi-objective optimization problem (MOP): 
$$
\begin{array}{r}
\min \mathbf{f}(\mathbf{x})=\left(f_{1}(\mathbf{x}), f_{2}(\mathbf{x}), \ldots, f_{m}(\mathbf{x})\right)^{\mathrm{T}} \\
\text { subject to } \mathbf{x} \in \Omega \subseteq \mathbb{R}^{n}
\end{array}
$$
- Pareto dominance-based MOEAs (e.g. NSGA-II, $\theta$-DEA, MOEA/D-DU)
- Multi-Objective Genetic Programming
- **Multi-Objective Different Evolution (MODE)**
  - faster convergence rate
  - efficient global search capability
- Self-organizing multi-objective evolutionary algorithm (SMEA)