---
title: 现代控制理论复习思维导图
date: 2026-06-01
updated: 2026-06-01
categories: 笔记
tags:
  - 笔记
  - 现代控制理论
description: 分享我复习时做的现代控制理论系统思维导图。
cover: /img/modern_control_theory/现代控制理论.png
mathjax: true
---

# 现代控制理论复习思维导图

## 复习主线

- 现控的复习主要分为两部分，一是以控制系统能控能观为考察点的状态空间和具体能控能观分析
- 二是由李雅普诺夫稳定性和状态反馈、状态观测器以及最优控制、估计为主的系统整体分析

# 状态空间模型

状态空间模型的基本形式为：

$$
\begin{cases}
\dot{x}=Ax+Bu \\
y=Cx+Du
\end{cases}
$$

其中，$x$ 为状态向量，$u$ 为输入向量，$y$ 为输出向量；$A$ 为系统矩阵，$B$ 为输入矩阵，$C$ 为输出矩阵，$D$ 为直接传递矩阵。

### 由物理机理建立动态方程

状态变量个数等于系统中独立储能元件的个数。建模时先根据物理定律列动态方程，再选取能完整描述系统能量状态的变量作为状态变量。

### 由微分方程建立动态方程

#### 不含输入的导数项

设系统微分方程为：

$$
y^{(n)}+a_{n-1}y^{(n-1)}+\cdots+a_1\dot{y}+a_0y=bu
$$

取状态变量：

$$
x_1=y,\quad x_2=\dot{y},\quad \cdots,\quad x_n=y^{(n-1)}
$$

则有：

$$
\begin{bmatrix}
\dot{x}_1\\
\dot{x}_2\\
\vdots\\
\dot{x}_{n-1}\\
\dot{x}_n
\end{bmatrix}
=
\begin{bmatrix}
0&1&0&\cdots&0\\
0&0&1&\cdots&0\\
\vdots&\vdots&\vdots&\ddots&\vdots\\
0&0&0&\cdots&1\\
-a_0&-a_1&-a_2&\cdots&-a_{n-1}
\end{bmatrix}
\begin{bmatrix}
x_1\\x_2\\\vdots\\x_{n-1}\\x_n
\end{bmatrix}
+
\begin{bmatrix}
0\\0\\\vdots\\0\\b
\end{bmatrix}u
$$

$$
y=\begin{bmatrix}1&0&\cdots&0\end{bmatrix}x
$$

注：第一能观标准型（对偶于第一能控标准型）。

#### 包含输入的导数项

设系统微分方程含有输入及其导数项：

$$
y^{(n)}+a_{n-1}y^{(n-1)}+\cdots+a_1\dot{y}+a_0y
=b_nu^{(n)}+b_{n-1}u^{(n-1)}+\cdots+b_1\dot{u}+b_0u
$$

可写成矩阵形式：

$$
\begin{bmatrix}
\dot{x}_1\\
\dot{x}_2\\
\vdots\\
\dot{x}_{n-1}\\
\dot{x}_n
\end{bmatrix}
=
\begin{bmatrix}
0&0&\cdots&0&-a_0\\
1&0&\cdots&0&-a_1\\
0&1&\cdots&0&-a_2\\
\vdots&\vdots&&\cdots&\vdots\\
0&0&\cdots&1&-a_{n-1}\\
\end{bmatrix}
\begin{bmatrix}
x_1\\x_2\\\vdots\\x_{n-1}\\x_n
\end{bmatrix}
+
\begin{bmatrix}
b_0-a_0b_n\\b_1-a_1b_n\\b_2-a_2b_n\\\vdots\\b_{n-1}-a_{n-1}b_n
\end{bmatrix}u
$$

$$
y=\begin{bmatrix}0&0&\cdots&1\end{bmatrix}\begin{bmatrix}
x_1\\x_2\\\vdots\\x_{n-1}\\x_n
\end{bmatrix}
+b_nu
$$



若 $b_n=0$，则 $B=\begin{bmatrix}b_0&b_1&\cdots&b_{n-1}\end{bmatrix}^T$，$D=0$，输入导数项引入的影响只体现在 $B$ 中。

注：第二能观标准型（对偶于第二能控标准型）。

### 由传递函数建立动态方程

状态空间模型与传递函数之间满足：

$$
G(s)=C(sI-A)^{-1}B+D
$$

#### 直接分解

设传递函数为：

$$
G(s)=\frac{Y(s)}{U(s)}
=\frac{b_ns^{n}+b_{n-1}s^{n-1}+\cdots+b_1s+b_0}
{s^n+a_{n-1}s^{n-1}+a_{n-2}s^{n-2}+\cdots+a_{1}s+a_0}
$$

写成矩阵形式有：

$$
A=
\begin{bmatrix}
0&1&0&\cdots&0\\
0&0&1&\cdots&0\\
\vdots&\vdots&\vdots&\ddots&\vdots\\
0&0&0&\cdots&1\\
-a_0&-a_1&-a_{2}&\cdots&-a_{n-1}
\end{bmatrix},\qquad
B=\begin{bmatrix}0\\0\\\vdots\\0\\1\end{bmatrix}
$$

$$
C=\begin{bmatrix}b_0-a_0b_n&b_1-a_1b_n&\cdots&b_{n-1}-a_{n-1}b_n\end{bmatrix},\qquad D=b_n
$$

注：第二能控标准型（对偶于第一能观标准型）。

#### 并联分解（互异一阶极点）

当传递函数可分解为互异一阶极点的部分分式：

$$
G(s)=\frac{Y(s)}{U(s)}=\frac{b_m}{a_n}\frac{s+z_1}{s+p_1}\cdots\frac{s+z_m}{s+p_m}\frac{1}{s+p_{m+1}}\cdots\frac{1}{s+p_n}
$$

则对于存在零点的形式：

$$
G (s) = \frac{Y (s)}{U (s)} = \frac{s + z}{s + p}
$$

可画出结构图如下图所示：

<img src="img/modern_control_theory/jiegou1.png" alt="jiegou1" style="zoom: 35%;" />

对于不存在零点的形式：
$$
G (s) = \frac{Y (s)}{U (s)} = \frac{k}{s + p}
$$
可画出结构图如下图所示：

<img src="img/modern_control_theory/jiegou2.png" alt="jiegou2" style="zoom:35%;" />

#### 并联分解（重根或高阶极点）

这里只讨论一个重极点的情况：设$-p_1$为$k$重极点，其余为互异极点，则传递函数可写为：

$$
G (s) = \frac{Y (s)}{U (s)} = \left[ \frac{c_{1}}{\left(s + p_{1}\right)^{k}} + \frac{c_{2}}{\left(s + p_{1}\right)^{k-1}} + \dots + \frac{c_{k}}{\left(s + p_{1}\right)} \right] +\left[ \frac{c_{k+1}}{s + p_{k+1}} + \frac{c_{k+2}}{s + p_{k+2}} + \dots + \frac{c_{n}}{s + p_{n}} \right] + b_{n}
$$
写成矩阵形式为：
$$
\left[
\begin{array}{c}
\dot{x}_1\\
\dot{x}_2\\
\vdots\\
\dot{x}_k\\
\hdashline
\dot{x}_{k+1}\\
\dot{x}_{k+2}\\
\vdots\\
\dot{x}_n
\end{array}
\right]
=
\left[
\begin{array}{cccc:cccc}
-p_1 & 1 &        & 0      & 0          &        &        & 0\\
     & -p_1 & \ddots &        &            & \ddots &        &  \\
     &        & \ddots & 1      &            &        & \ddots &  \\
0    &        &        & -p_1   & 0          &        &        & 0\\
\hdashline
0    &        &        & 0      & -p_{k+1}   &        &        & 0\\
     & \ddots &        &        &            & -p_{k+2} &      &  \\
     &        & \ddots &        &            &        & \ddots &  \\
0    &        &        & 0      & 0          &        &        & -p_n
\end{array}
\right]
\left[
\begin{array}{c}
x_1\\
x_2\\
\vdots\\
x_k\\
\hdashline
x_{k+1}\\
x_{k+2}\\
\vdots\\
x_n
\end{array}
\right]
+
\left[
\begin{array}{c}
0\\
0\\
\vdots\\
1\\
\hdashline
1\\
1\\
\vdots\\
1
\end{array}
\right]u
$$

$$
y = \left[ c_{1} c_{2} \dots c_{n} \right] \left[ \begin{array}{l} x_{1} \\x_{2} \\ \vdots \\x_{n} \end{array} \right] + b_{n} u
$$

## 状态空间表达式的线性变换

### 特征值和特征向量

若：

$$
\lvert \lambda I-A\rvert=0
$$

则 $\lambda$ 为矩阵 $A$ 的特征值；对应的独立特征向量个数为 $n-\operatorname{rank}(\lambda I-A)$。

几何重数始终小于等于代数重数，当特征值互异时，由特征向量$v_i$组成的矩阵$P$非奇异。

若系统矩阵为友矩阵：

$$
A=
\begin{bmatrix}
0&1&0&\cdots&0\\
0&0&1&\cdots&0\\
\vdots&\vdots&\vdots&\ddots&\vdots\\
0&0&0&\cdots&1\\
-a_0&-a_1&-a_2&\cdots&-a_{n-1}
\end{bmatrix}
$$

则其特征多项式为：

$$
\lvert\lambda I-A\rvert=\lambda^n+a_{n-1}\lambda^{n-1}+\cdots+a_1\lambda+a_0
$$

特征方程为：

$$
\lambda^n+a_{n-1}\lambda^{n-1}+\cdots+a_1\lambda+a_0=0
$$

### 线性非奇异变换

设：

$$
x=P\bar{x},\qquad \bar{x}=P^{-1}x
$$

原系统：

$$
\begin{cases}
\dot{x}=Ax+Bu\\
y=Cx+Du
\end{cases}
$$

经非奇异线性变换后：

$$
\begin{cases}
\dot{\bar{x}}=\bar{A}\bar{x}+\bar{B}u\\
y=\bar{C}\bar{x}+\bar{D}u
\end{cases}
$$

其中：

$$
\bar{A}=P^{-1}AP,\qquad \bar{B}=P^{-1}B,\qquad \bar{C}=CP,\qquad \bar{D}=D
$$

### 化为对角线标准型

步骤如下：

- 先求出系统矩阵 $A$ 的全部特征值。
- 对每个特征值求对应特征向量，并由特征向量组成非奇异变换矩阵 $P$。
- 由 $\bar{A}=P^{-1}AP$ 得到对角矩阵，$\bar{B}=P^{-1}B$，$\bar{C}=CP$。

若系统特征值 $\lambda_1,\lambda_2,\ldots,\lambda_n$ 两两互异，且系统矩阵 $A$ 是友矩阵，则对应的变换矩阵可取范德蒙德型：

$$
P=
\begin{bmatrix}
1&1&\cdots&1\\
\lambda_1&\lambda_2&\cdots&\lambda_n\\
\vdots&\vdots&\ddots&\vdots\\
\lambda_1^{n-1}&\lambda_2^{n-1}&\cdots&\lambda_n^{n-1}
\end{bmatrix}
$$

### 化为约当标准型

若系统有 1个 $k$ 重特征值 $\lambda_1$  (只讨论几何重数=1)，$n-k$ 个互异特征值 $\lambda_{k+1},\ldots,\lambda_n$，则变换矩阵 $P$ 由两部分组成：

$$
P=\begin{bmatrix}v_1^{(1)}&v_1^{(2)}&\cdots&v_1^{(k)}&v_{k+1}&\cdots&v_n\end{bmatrix}
$$

其中：

- $v_1^{(1)}$ 是重根 $\lambda_1$ 对应的特征向量。
- $v_1^{(2)},\ldots,v_1^{(k)}$ 是重根 $\lambda_1$ 对应的广义特征向量。

- $v_{k+1},\ldots,v_n$ 是互异根对应的特征向量。

这些特征向量由下式计算得到：
$$
\left\{ \begin{array}{l} \left(\lambda_{1} I - A\right) v_{1}^{(1)} = 0 \\ \left(\lambda_{1} I - A\right) v_{1}^{(2)} = - v_{1}^{(1)} \\ \vdots \\ \left(\lambda_{1} I - A\right) v_{1}^{(k)} = - v_{1}^{(k-1)} \\ \left(\lambda_{k+1} I - A\right) v_{k+1} = 0 \\ \vdots \\ \left(\lambda_{n} I - A\right) v_{n} = 0 \end{array} \right.
$$


约当标准型满足：

$$
\bar{A}
=
P^{-1}AP
=
\left[
\begin{array}{cccc:cccc}
\lambda_1 & 1         &        & 0          & 0             &        &        & 0\\
          & \lambda_1 & \ddots &            &               &        &        &  \\
          &           & \ddots & 1          &               &        &        &  \\
0         &           &        & \lambda_1  & 0             &        &        & 0\\
\hdashline
0         &           &        & 0          & \lambda_{k+1} &        &        & 0\\
          &           &        &            &               & \ddots &        &  \\
          &           &        &            &               &        & \ddots &  \\
0         &           &        & 0          & 0             &        &        & \lambda_n
\end{array}
\right]
$$

## 线性控制系统的运动分析

### 矩阵指数函数

矩阵指数函数定义为：

$$
e^{At}=I+At+\frac{A^2t^2}{2!}+\cdots+\frac{A^kt^k}{k!}+\cdots
$$

$e^{At}$ 又称为状态转移矩阵；它与标量函数 $e^{\lambda t}$ 类似，是 $n\times n$ 阶方阵。

#### 性质

$$
e^{A(t_1+t_2)}=e^{At_1}e^{At_2}
$$

$$
e^{A(t-t)}=e^{A0}=I
$$

$$
(e^{At})^{-1}=e^{-At}
$$

$$
e^{P^{-1}APt}=P^{-1}e^{At}P,
\qquad
 e^{At}=Pe^{\bar{A}t}P^{-1}
$$

$$
\frac{d}{dt}e^{At}=Ae^{At}=e^{At}A
$$

若 $AB=BA$，则：

$$
e^{(A+B)t}=e^{At}e^{Bt}
$$

若 $AB\ne BA$，则一般有：

$$
e^{(A+B)t}\ne e^{At}e^{Bt}
$$

#### 计算

##### 定义求解

按定义展开：

$$
e^{At}=I+At+\frac{A^2t^2}{2!}+\cdots
$$

该方法适合 $A^k$ 有明显规律的矩阵。

##### 拉氏变换

$$
e^{At}=\mathcal{L}^{-1}\left[(sI-A)^{-1}\right]
$$

对于二阶矩阵求逆可记住：主对换，副变号

$$
\begin{bmatrix}a&b\\c&d\end{bmatrix}^{-1}=\dfrac{1}{ad-bc}\begin{bmatrix}d&-b\\-c&a\end{bmatrix}
$$

##### 标准型法

当 $A$ 的特征值 $\lambda_1,\lambda_2,\ldots,\lambda_n$ 两两相异时，可化为对角线标准型：

$$
e^{At}=Pe^{\bar{A}t}P^{-1}
=P
\begin{bmatrix}
e^{\lambda_1t}&0&\cdots&0\\
0&e^{\lambda_2t}&\cdots&0\\
\vdots&\vdots&\ddots&\vdots\\
0&0&\cdots&e^{\lambda_nt}
\end{bmatrix}
P^{-1}
$$

当 $A$ 具有$n$重特征根$\lambda_1$时，可化为约当标准型:

$$
e^{At}=Pe^{\bar{A}t}P^{-1}=e^{\lambda_1 t}\times P
\begin{bmatrix}
1&t&\frac{t^2}{2!}&\cdots&\frac{t^{n-1}}{(n-1)!}\\
0&1&t&\cdots&\frac{t^{n-2}}{(n-2)!}\\
\vdots&\vdots&\ddots&\ddots&\vdots\\
0&0&\cdots&1&t\\
0&0&\cdots&0&1
\end{bmatrix}P^{-1}
$$

##### 待定系数法

凯莱-哈密顿定理：设 $n$ 阶矩阵 $A$ 的特征方程为：

$$
f(\lambda)=\lambda^n+a_{n-1}\lambda^{n-1}+\cdots+a_1\lambda+a_0=0
$$

则矩阵 $A$ 满足自身特征方程：

$$
f(A)=A^n+a_{n-1}A^{n-1}+\cdots+a_1A+a_0I=0
$$

因此可写：

$$
e^{At}=\sum_{i=0}^{n-1}\alpha_i(t)A^i
$$

其中，$\alpha_0(t),\alpha_1(t),\ldots,\alpha_{n-1}(t)$ 为待定标量函数，可由 $A$ 的特征值代入确定：

$$
\alpha_0(t)+\alpha_1(t)\lambda+\cdots+\alpha_{n-1}(t)\lambda^{n-1}=e^{\lambda t}
$$

说明：不管特征值互异还是具有重根，只需要代入 $n$ 个独立条件。互异根直接代入；重根除代入根外，还要对 $\lambda$ 求导补足方程。然后联立求出系数即可。

### 线性连续定常非齐次状态方程

设：

$$
\dot{x}=Ax+Bu
$$

初始状态为 $x(t_0)$ 时，其解为：

$$
x(t)=e^{A(t-t_0)}x(t_0)+\int_{t_0}^{t}e^{A(t-\tau)}Bu(\tau)d\tau
$$

也可写为：

$$
x(t)=\Phi(t-t_0)x(t_0)+\int_{t_0}^{t}\Phi(t-\tau)Bu(\tau)d\tau
$$

其中第一项为由初始状态引起的响应，即零输入响应，第二项为输入引起的响应，即零状态响应。

#### 直接求解

直接利用矩阵指数函数和卷积积分求解：

$$
x(t)=e^{A(t-t_0)}x(t_0)+\int_{t_0}^{t}e^{A(t-\tau)}Bu(\tau)d\tau
$$

#### 拉氏变换

当 $t_0=0$ 时：

$$
sX(s)-x(0)=AX(s)+BU(s)
$$

$$
X(s)=(sI-A)^{-1}x(0)+(sI-A)^{-1}BU(s)
$$

$$
x(t)=\mathcal{L}^{-1}\left[(sI-A)^{-1}x(0)+(sI-A)^{-1}BU(s)\right]
$$

## 线性控制系统的能控性与能观性

### 能控性

能控性描述输入 $u$ 是否能够在有限时间内把系统状态从任意初始状态转移到任意目标状态。

#### 能控性判别矩阵

对于线性连续定常系统：

$$
\dot{x}=Ax+Bu
$$

状态完全能控的充要条件是能控性判别矩阵：

$$
Q_c=\begin{bmatrix}B&AB&A^2B&\cdots&A^{n-1}B\end{bmatrix}
$$

满秩，即：

$$
\operatorname{rank}(Q_c)=n
$$

#### 对角型与约当型下的能控性判据

若线性系统：

$$
\dot{x}=Ax+Bu
$$

具有两两相异的特征值 $\lambda_1,\lambda_2,\ldots,\lambda_n$，则状态完全能控的充要条件是：系统经线性非奇异变换后的对角线标准型

$$
\dot{\bar{x}}=
\begin{bmatrix}
\lambda_1&0&\cdots&0\\
0&\lambda_2&\cdots&0\\
\vdots&\vdots&\ddots&\vdots\\
0&0&\cdots&\lambda_n
\end{bmatrix}\bar{x}+\bar{B}u
$$

中，$\bar{B}$ 不包含元素全为 $0$ 的行。

若系统具有重特征值，且每个重特征值只对应一个独立的特征向量，则状态完全能控的充要条件是：系统经线性非奇异变换后的约当标准型

$$
\dot{\bar{x}}=
\begin{bmatrix}
J_1&0&\cdots&0\\
0&J_2&\cdots&0\\
\vdots&\vdots&\ddots&\vdots\\
0&0&\cdots&J_k
\end{bmatrix}\bar{x}+\bar{B}u
$$

中，$\bar{B}$ 阵中与每个约当小块 $J_i\,(i=1,2,\ldots,k)$ 最后一行所对应的元素不全为零。

#### 标准型

##### 第一能控标准型

若 SI线性定常系统状态完全能控，则存在非奇异变换：

$$
x=P_{c1}\bar{x},
\qquad
P_{c1}=\begin{bmatrix}b&Ab&\cdots&A^{n-1}b\end{bmatrix}
$$

使系统化为第一能控标准型：

$$
\begin{cases}
\dot{\bar{x}}=\bar{A}\bar{x}+\bar{B}u\\
y=\bar{C}\bar{x}
\end{cases}
$$

其中：

$$
\bar{A}=
\begin{bmatrix}
0&0&\cdots&0&-a_0\\
1&0&\cdots&0&-a_{1}\\
0&1&\cdots&0&-a_{2}\\
\vdots&\vdots&\ddots&\vdots&\vdots\\
0&0&\cdots&1&-a_{n-1}
\end{bmatrix},
\qquad
\bar{B}=\begin{bmatrix}1\\0\\\vdots\\0\end{bmatrix}
$$

$$
\bar{C}=CP_{c1}=\begin{bmatrix}\beta_0&\beta_1&\cdots&\beta_{n-1}\end{bmatrix}
$$

##### 第二能控标准型★

第二能控标准型常用的非奇异变换矩阵可写为：

$$
P_{c2}=\begin{bmatrix}A^{n-1}b&A^{n-2}b&\cdots&b\end{bmatrix}
\begin{bmatrix}
1&0&\cdots&0\\
a_{n-1}&1&\cdots&0\\
a_{n-2}&a_{n-1}&\ddots&\vdots\\
\vdots&\vdots&\ddots&1\\
a_1&a_2&\cdots&a_{n-1}
\end{bmatrix}
$$

若 SI 线性定常系统状态完全能控，则也可化为第二能控标准型：

$$
\begin{cases}
\dot{\bar{x}}=\bar{A}\bar{x}+\bar{B}u\\
y=\bar{C}\bar{x}
\end{cases}
$$

其中：

$$
\bar{A}=
\begin{bmatrix}
0&1&0&\cdots&0\\
0&0&1&\cdots&0\\
\vdots&\vdots&\vdots&\ddots&\vdots\\
0&0&0&\cdots&1\\
-a_0&-a_{1}&-a_{2}&\cdots&-a_{n-1}
\end{bmatrix},
\qquad
\bar{B}=\begin{bmatrix}0\\0\\\vdots\\0\\1\end{bmatrix}
$$

$$
\bar{C}=CP_{c2}=\begin{bmatrix}\beta_0&\beta_1&\cdots&\beta_{n-1}\end{bmatrix}
$$

对于 SI 系统，用第二能控标准型可方便地求系统传递函数：

$$
G(s)=\bar{C}(sI-\bar{A})^{-1}\bar{B}
=\frac{\beta_{n-1}s^{n-1}+\beta_{n-2}s^{n-2}+\cdots+\beta_1s+\beta_{0}}
{s^n+a_{n-1}s^{n-1}+\cdots+a_0}
$$

分母和分子多项式系数分别就是 $\bar{A}$ 和 $\bar{C}$ 中相应的系数；当给定传递函数时，可直接获得第二能控标准型。

### 能观性

能观性描述系统输出 $y$ 是否能够在有限时间内确定系统内部状态 $x$。

#### 能观性判别矩阵

对于线性连续定常系统：

$$
\dot{x}=Ax,\qquad y=Cx
$$

状态完全能观测的充要条件是能观性判别矩阵：

$$
Q_o=\begin{bmatrix}
C\\
CA\\
\vdots\\
CA^{n-1}
\end{bmatrix}
=
\begin{bmatrix}C^T&A^TC^T&\cdots&(A^T)^{n-1}C^T\end{bmatrix}^T
$$

满秩，即：

$$
\operatorname{rank}(Q_o)=n
$$

#### 对角型与约当型下的能观性判据

若线性系统：

$$
\dot{x}=Ax,\qquad y=Cx
$$

具有两两相异的特征值 $\lambda_1,\lambda_2,\ldots,\lambda_n$，则状态完全能观测的充要条件是：系统经线性非奇异变换后的对角线标准型中，$\bar{C}$ 不包含元素全为零的列。

若系统具有重特征值，且每个重特征值只对应一个独立的特征向量，则状态完全能观测的充要条件是：系统经线性非奇异变换后的约当标准型

$$
\dot{\bar{x}}=
\begin{bmatrix}
J_1&0&\cdots&0\\
0&J_2&\cdots&0\\
\vdots&\vdots&\ddots&\vdots\\
0&0&\cdots&J_k
\end{bmatrix}\bar{x},\qquad y=\bar{C}\bar{x}
$$

中，$\bar{C}$ 阵中与每个约当小块 $J_i\,(i=1,2,\ldots,k)$ 首列所对应的列，其元素不全为零。

#### 标准型

##### 第一能观标准型

若 SO 线性定常系统状态完全能观测，则存在非奇异变换：

$$
x=P_{o1}\bar{x},
\qquad
P_{o1}^{-1}=Q_o=\begin{bmatrix}c\\cA\\\vdots\\cA^{n-1}\end{bmatrix}
$$

将状态方程化为第一能观标准型：

$$
\begin{cases}
\dot{\bar{x}}=\bar{A}\bar{x}+\bar{B}u\\
y=\bar{C}\bar{x}
\end{cases}
$$

其中：

$$
\bar{A}=
 \begin{bmatrix} 0 & 1 & 0 & \dots & 0 \\ 0 & 0 & 1 & \ddots & \vdots \\ \vdots & \vdots & \ddots & \ddots & 0 \\ 0 & 0 & \dots & 0 & 1 \\ - a_{0} & - a_{1} & - a_{2} & \dots & - a_{n-1} \end{bmatrix} 
$$

$$
\bar{B}=P_{o1}^{-1}B=
\begin{bmatrix}\beta_0\\\beta_1\\\vdots\\\beta_{n-1}\end{bmatrix},
\qquad
\bar{C}=CP_{o1}=\begin{bmatrix}1&0&\cdots&0\end{bmatrix}
$$

##### 第二能观标准型★

第二能观标准型常用的非奇异变换矩阵可写为：

$$
P_{o2}^{-1}=
\begin{bmatrix}
1&a_{n-1}&\cdots&a_2&a_1\\
0&1&\cdots&a_3&a_2\\
\vdots&\vdots&\ddots&\vdots&\vdots\\
0&0&\cdots&1&a_{n-1}\\
0&0&\cdots&0&1
\end{bmatrix}
\begin{bmatrix}
cA^{n-1}\\
cA^{n-2}\\
\vdots\\
cA\\
c
\end{bmatrix}
$$

若 SO 线性定常系统状态完全能观测，则可化为第二能观标准型：

$$
\begin{cases}
\dot{\bar{x}}=\bar{A}\bar{x}+\bar{B}u\\
y=\bar{C}\bar{x}
\end{cases}
$$

其中：

$$
\bar{A}=
\begin{bmatrix}
 0 & 0 & \dots & 0 & - a_{0} \\ 1 & 0 & \dots & 0 & - a_{1} \\ 0 & 1 & \ddots & \vdots & - a_{2} \\ \vdots & \ddots & \ddots & 0 & \vdots \\ 0 & \dots & 0 & 1 & - a_{n-1} 
\end{bmatrix}
$$

$$
\bar{B}=P_{o2}^{-1}B=
\begin{bmatrix}\beta_0\\\beta_1\\\vdots\\\beta_{n-1}\end{bmatrix},
\qquad
\bar{C}=CP_{o2}=\begin{bmatrix}0&0&\cdots&1\end{bmatrix}
$$

对于 SO 系统，采用第二能观标准型可方便地求出系统的传递函数矩阵:
$$
G(s)=\bar{C}(sI-\bar{A})^{-1}\bar{B}
=\frac{\beta_{n-1}s^{n-1}+\beta_{n-2}s^{n-2}+\cdots+\beta_1s+\beta_{0}}
{s^n+a_{n-1}s^{n-1}+\cdots+a_0}
$$
给定传递函数阵时，可直接获得第二能观标准型。

### 能控性与能观性的传递函数判据

系统传递函数的最简形式只反映系统中既能控又能观测的那部分动态行为。

结论：SISO 线性系统状态完全能控且完全能观测的充要条件是传递函数中分子、分母没有零极点相消：

$$
G(s)=c(sI-A)^{-1}b
$$

若分子、分母没有零、极点对消，则系统完全能控且完全能观测。

对于串联系统：

- 若串联排列次序中，被消去的零点在前一传递函数中，则系统状态不完全能控，但完全能观测。
- 若串联排列次序中，被消去的零点在后一传递函数中，则系统状态完全能控，但不完全能观测。

### 对偶原理

对偶系统写为：

$$
\Sigma_1:
\begin{cases}
\dot{x}=Ax+Bu\\
y=Cx
\end{cases}
\qquad
\Sigma_2:
\begin{cases}
\dot{x}^*=A^*x^*+B^*u^*\\
y^*=C^*x^*
\end{cases}
$$

若满足：

$$
A^*=A^T,
\qquad
B^*=C^T,
\qquad
C^*=B^T
$$

则称两个系统互为对偶系统。

互为对偶的系统具有以下性质：

1. 互为对偶的系统，其传递函数阵是互为转置的

$$
G_2(s)=C^*(sI-A^*)^{-1}B^*
=B^T(sI-A^T)^{-1}C^T
=\left[C(sI-A)^{-1}B\right]^T
=G_1^T(s)
$$

​	SISO 系统时，传递函数相等。

2. 互为对偶的系统具有相同的特征方程：

$$
\lvert sI-A^*\rvert=\lvert sI-A^T\rvert=\lvert sI-A\rvert=0
$$

3. 若 $\Sigma_1$ 与 $\Sigma_2$ 互为对偶，则 $\Sigma_1$ 的能控性等价于 $\Sigma_2$ 的能观测性，$\Sigma_1$ 的能观测性等价于 $\Sigma_2$ 的能控性。
4. 互为对偶的系统，化为能控标准型和能观标准型的非奇异矩阵互为转置逆。

### 线性系统的结构分解

#### 能控性分解

若线性定常系统：

$$
\begin{cases}
\dot{x}=Ax+Bu\\
y=Cx
\end{cases}
$$

状态不完全能控，且：

$$
\operatorname{rank}(Q_c)=n_1<n
$$

则存在非奇异变换：

$$
x=R_c\hat{x}
$$

$$
R_c=\begin{bmatrix}R_1&\cdots&R_{n1}&\cdots&R_n\end{bmatrix}
$$

前$n-1$列为$Q_c$中 $n_1$个线性无关的列，其余列保证$R_c$非奇异任选。

将状态空间描述变换为：
$$
\begin{cases}
\dot{\hat{x}}=\hat{A}\hat{x}+\hat{B}u\\
y=\hat{C}\hat{x}
\end{cases}
$$

其中：

$$
\hat{x}=\begin{bmatrix}\hat{x}_1\\\hat{x}_2\end{bmatrix},\qquad
\hat{A}=R_c^{-1}AR_c=
\begin{bmatrix}
\hat{A}_{11}&\hat{A}_{12}\\
0&\hat{A}_{22}
\end{bmatrix}
$$

$$
\hat{B}=R_c^{-1}B=\begin{bmatrix}\hat{B}_1\\0\end{bmatrix},
\qquad
\hat{C}=CR_c=\begin{bmatrix}\hat{C}_1&\hat{C}_2\end{bmatrix}
$$

其中 $\hat{x}_1$ 为能控子空间，$\hat{x}_2$ 为不能控子空间。

#### 能观性分解

若线性定常系统状态不完全能观测，且：

$$
\operatorname{rank}(Q_o)=n_1<n
$$

则存在非奇异变换：

$$
x=R_o\hat{x}
$$

$$
R_o^{-1}=\begin{bmatrix}R_1\\\vdots\\R_{n1}\\\vdots\\R_n\end{bmatrix}
$$

前$n_1$行为$Q_o$中$n_1$个线性无关的行，其余行保证$R_o$的逆阵非奇异任选。

将状态空间描述变换为：
$$
\begin{cases}
\dot{\hat{x}}=\hat{A}\hat{x}+\hat{B}u\\
y=\hat{C}\hat{x}
\end{cases}
$$

其中：

$$
\hat{x}=\begin{bmatrix}\hat{x}_1\\\hat{x}_2\end{bmatrix},\qquad
\hat{A}=R_o^{-1}AR_o=
\begin{bmatrix}
\hat{A}_{11}&0\\
\hat{A}_{21}&\hat{A}_{22}
\end{bmatrix}
$$

$$
\hat{B}=R_o^{-1}B=\begin{bmatrix}\hat{B}_1\\\hat{B}_2\end{bmatrix},
\qquad
\hat{C}=CR_o=\begin{bmatrix}\hat{C}_1&0\end{bmatrix}
$$

其中 $\hat{x}_1$ 为能观子空间，$\hat{x}_2$ 为不能观子空间。

## 李雅普诺夫稳定性分析

### 稳定类型

#### 稳定与一致稳定

设 $x_e$ 为系统的一个孤立平衡状态。如果对球域 $S(\varepsilon)$ 或任意正实数 $\varepsilon>0$，都可以找到另一个正实数 $\delta(\varepsilon,t_0)$，使得当初始状态 $x_0$ 满足：

$$
\lVert x_0-x_e\rVert\le \delta(\varepsilon,t_0)
$$

时，对由此出发的状态轨迹均满足：

$$
\lVert x(t)-x_e\rVert\le \varepsilon
$$

则平衡状态 $x_e$ 在李雅普诺夫意义下稳定。如果 $\delta$ 与初始时刻 $t_0$ 无关，则称平衡状态一致稳定。

#### 渐近稳定与一致渐近稳定

设 $x_e$ 为系统的孤立平衡状态。如果它在李雅普诺夫意义下稳定，并且存在实数 $r>0$，对状态初值满足：

$$
\lVert x_0-x_e\rVert\le r
$$

的任意状态 $x(t)$，均有：

$$
\lim_{t\to\infty}x(t)=x_e
$$

则称平衡状态 $x_e$ 是渐近稳定的。

如果与初始时刻 $t_0$ 无关，则称为一致渐近稳定。

#### 全局渐近稳定

如果渐近稳定的吸引域为全状态空间中所有初始状态出发的轨线，即所有轨迹最终趋于平衡点，则称平衡状态为大范围渐近稳定或全局渐近稳定。

必要性：整个状态空间中只有一个平衡状态。

### 第一法（特征值法）

外部稳定（有界输入—有界输出）：线性连续定常系统的传递函数 $G(s)=C(sI-A)^{-1}B$，当且仅当其==极点==均具有负实部时，零初始状态下系统对任何有界输入均给出有界输出。

内部稳定（渐近稳定）：线性连续定常系统内部稳定的充要条件是系统矩阵 $A$ 的所有==特征值==均具有负实部，等价于所有特征方程的根全部位于 $s$ 平面的左半平面。

两者关系：内部稳定蕴含外部稳定；外部稳定不一定蕴含内部稳定（除非系统能控能观）。

判别规则：

$$
\begin{array}{l} A \mathrm{为 非 奇 异 阵 时 , 用 第一 法 计算} A \mathrm{的 特 征 根 (一定没有 0)} \\ \mathrm{全 部 特 征 值 实 部} < 0 \Rightarrow \mathrm{全 局 渐 近 稳 定} \\ \mathrm{有 特 征 值 实 部} > 0 \Rightarrow \mathrm{不 稳 定} \\A \mathrm{为 奇 异 阵 时 , 用 第一 法 计算} A \mathrm{的 特 征 根 (一定有 0)} \\ \mathrm{其 他 特 征 值 实 部} < 0 \Rightarrow \mathrm{稳 定 不 渐 进 稳 定} \\ \mathrm{其 他 特 征 值 实 部} > 0 \Rightarrow \mathrm{不 稳 定} \\ \end{array}
$$

### 第二法（李雅普诺夫函数）

#### 非线性系统的稳定性分析

设系统状态方程为：

$$
\dot{x}=f(x)
$$

若 $x_e=0$ 为其平衡状态，且存在一个标量函数 $V(x)$，在具有连续一阶偏导数的区域内满足：

$$
V(0)=0
$$

若在原点的某一邻域内 $V(x)$ 是正定的，并且 $\dot{V}(x)$ 是负半定的，则系统在原点处的平衡状态是稳定的。

若 $V(x)$ 是正定的，$\dot{V}(x)$ 是负定的，则系统在原点处的平衡状态是渐近稳定的。

若 $V(x)$ 是正定的，$\dot{V}(x)$ 是负定的，且当 $\lVert x\rVert\to\infty$ 时：

$$
V(x)\to\infty
$$

则平衡状态是大范围渐近稳定的。

注：若只能满足稳定条件而不能满足渐近稳定条件，则结论只能是李雅普诺夫意义下稳定，而不是渐近稳定。

#### 连续线性定常系统的李雅普诺夫稳定性分析

设线性连续定常系统为：

$$
\dot{x}=Ax
$$

在平衡状态 $x_e=0$ 处渐近稳定的充要条件是：给定任意正定对称矩阵 $Q$，都存在唯一正定对称矩阵 $P$，使得：

$$
A^TP+PA=-Q
$$

并且李雅普诺夫函数可取为：

$$
V(x)=x^TPx
$$

其导数为：

$$
\dot{V}(x)=\dot{x}^TPx+x^TP\dot{x}=x^T(A^TP+PA)x=-x^TQx<0
$$

常用判断：

- 若 $V(x)=x^TQx$，且 $Q$ 的顺序主子式均大于 $0$，则 $V(x)$ 正定。
- 若 $V(x)$ 正定且 $\dot{V}(x)$ 负定，则原点渐近稳定。
- 对线性系统，若存在 $P=P^T>0$ 满足 $A^TP+PA=-Q$ 且 $Q=Q^T>0$，则系统渐近稳定。
- 一般取$Q=I$

## 状态反馈与观测器

### 状态反馈

状态反馈用状态变量 $x$ 形成反馈信号，使闭环极点满足期望配置。

设状态反馈为：

$$
u=v-Kx
$$

原系统：

$$
\begin{cases}
\dot{x}=Ax+Bu\\
y=Cx+Du
\end{cases}
$$

代入后得到闭环系统：

$$
\begin{cases}
\dot{x}=(A-BK)x+Bv\\
y=(C-DK)x+Dv
\end{cases}
$$

<img src="img/modern_control_theory/state_feedback_block_diagram.png" alt="状态反馈结构图" width="35%" />

#### 直接求解法

若系统状态完全能控，则可用状态反馈配置闭环极点。步骤为：

- 写出状态反馈闭环系统的特征多项式：

$$
f(\lambda)=\lvert \lambda I-(A-BK)\rvert
$$

- 根据给定或期望的闭环特征根写出期望特征多项式：

$$
f^*(\lambda)=\prod_{i=1}^{n}(\lambda-\lambda_i^*)
$$

- 令：

$$
f(\lambda)=f^*(\lambda)
$$

比较同次幂系数，求出状态反馈矩阵 $K$。

#### 能控标准型法

先判断系统 $\Sigma=(A,b,c)$ 的能控性。如果状态完全能控，则按下列步骤继续：

- 求原系统特征多项式，确定系统系数 $a_i$，并求出将系统化为能控标准型的变换矩阵 $P_{c2}$。若系统已为能控标准型，则：

$$
P_{c2}=I
$$

原系统特征多项式为：
$$
f(\lambda)=|\lambda I-A|
=\lambda^n+a_{n-1}\lambda^{n-1}+\cdots+a_1\lambda+a_0
$$

- 确定期望闭环极点，并由期望极点写出期望特征多项式，得到期望系数 $a_i^*$。

- 求能控标准型下的反馈阵：

$$
\bar{k}=\begin{bmatrix}a_0^*-a_0
\quad
a_1^*-a_1
\quad
\cdots
\quad
a_{n-1}^*-a_{n-1}
\end{bmatrix}
$$

- 转换回原系统坐标系，得到原系统 $\Sigma_0=(A,b,c)$ 的状态反馈阵：

$$
k=\bar{k}P_{c2}^{-1}
$$

### 状态观测器

状态观测器利用输入 $u$ 和输出 $y$ 估计系统状态 $x$。

全维状态观测器结构为：

<img src="img/modern_control_theory/state_observer_block_diagram.png" alt="全维状态观测器结构图" width="50%" />

其状态方程为：

$$
\dot{\hat{x}}=A\hat{x}+Bu+K_e(y-\hat{y})
$$

由于：

$$
\hat{y}=C\hat{x}
$$

故：

$$
\dot{\hat{x}}=A\hat{x}+Bu+K_e(y-C\hat{x})=(A-K_eC)\hat{x}+Bu+K_ey
$$

#### 直接求解法

若系统状态完全能观测，则可配置观测器极点。步骤为：

- 判断系统能观测性；若状态完全能观测，则继续。
- 求观测器的特征多项式：

$$
f(\lambda)=\lvert \lambda I-(A-K_eC)\rvert
$$

- 写出状态观测器期望特征多项式：

$$
f^*(\lambda)=(\lambda-\lambda_1)(\lambda-\lambda_2)\cdots(\lambda-\lambda_n)
=\lambda^n+a_1^*\lambda^{n-1}+\cdots+a_n^*
$$

- 由 $f(\lambda)=f^*(\lambda)$ 确定状态观测器增益矩阵：

$$
K_e=\begin{bmatrix}k_{e1}&k_{e2}&\cdots&k_{en}\end{bmatrix}^T
$$

#### 能观标准型法

步骤如下：

- 判断系统能观测性；若状态完全能观测，则继续。

- 确定将原系统化为第二能观标准型的变换矩阵
  $$
  \boldsymbol{P}_{o2}^{-1} = \left[ \begin{array}{c c c c c} 1 & a_{n-1} & \dots & a_{2} & a_{1} \\ 0 & \ddots & \ddots & & a_{2} \\ \vdots & \ddots & \ddots & \ddots & \vdots \\ & & \ddots & \ddots & a_{n-1} \\ 0 & & \dots & 0 & 1 \end{array} \right] \left[ \begin{array}{c} \boldsymbol{c A}^{n-1} \\ \boldsymbol{c A}^{n-2} \\ \vdots \\ \boldsymbol{c A} \\ \boldsymbol{c} \end{array} \right]
  $$

- 设定期望观测器特征多项式：

$$
f^*(\lambda)=\lambda^n+a_1^*\lambda^{n-1}+\cdots+a_n^*
$$

- 在第二能观标准型下直接写出观测器增益：

$$
\bar{K}_e=
\begin{bmatrix}
\bar{k}_{e1}\\\bar{k}_{e2}\\\vdots\\\bar{k}_{en}
\end{bmatrix}
=
\begin{bmatrix}
a_0^*-a_0\\
a_{1}^*-a_{1}\\
\vdots\\
a_{n-1}^*-a_{n-1}
\end{bmatrix}
$$

- 原系统状态观测器的增益矩阵为：

$$
K_e=P_o\bar{K}_e
$$

### 状态反馈与观测器的组合

带状态反馈的全维状态观测器结构如下：

<img src="img/modern_control_theory/full_state_feedback_observer_diagram.png" alt="加入状态反馈的全维状态观测器结构图" width="50%" />

结论：传递函数阵和状态反馈部分相同，与观测器无关；用观测器的估计状态进行反馈，不影响系统的输入输出特性。

结论：特征值由状态反馈闭环系统的特征值与观测器误差系统的特征值共同组成，所以状态反馈矩阵 $K$ 和观测器增益矩阵 $K_e$ 可单独设计。这一性质称为分离特性。

## 最优控制

### 离散系统动态规划

离散系统动态规划从连续定常系统离散化开始，然后依据贝尔曼最优性原理倒推最优控制序列。

#### 连续定常系统的离散化

线性定常系统为：

$$
\begin{cases}
\dot{x}=Ax+Bu\\
y=Cx+Du
\end{cases}
$$

离散化精确方程为：

$$
\begin{cases}
x[(k+1)T]=G(T)x(kT)+H(T)u(kT)\\
y(kT)=Cx(kT)+Du(kT)
\end{cases}
$$

其中：

$$
G(T)=\Phi(T)=e^{AT}
$$

$$
H(T)=\left(\int_0^T \Phi(\tau)d\tau\right)B
=\left(\int_0^T e^{A\tau}d\tau\right)B
$$

#### 贝尔曼最优性原理

##### 系统动态

$$
X(k+1)=f(X(k),u(k),k)
$$

##### 性能指标

$$
J=\Phi[X(N),N]+\sum_{i=0}^{N-1}L[X(i),u(i),i]
$$

##### 步骤

- 令 $K=N$，求解终端代价。
- 令 $K=N-1$，求解使当前阶段代价与未来最优代价之和最小的 $u^*(N-1)$。
- 对 $K=N-2,\ldots,1,0$ 逐步倒推：

$$
J^*[x(k)]=\min_{u(k)}\{L[x(k),u(k),k]+J^*[x(k+1)]\}
$$

- 逐步逆向递推，求出可以获得最优性能的控制序列：

$$
u^*(N-2),\ldots,u^*(1),u^*(0)
$$

- 将最优控制序列代入状态方程，得到最优状态序列：

$$
x(0),x(1),x(2),\ldots,x(N)
$$

## 最优估计

### 最小二乘法

设测量模型为：

$$
z=Hx+v
$$

其中 $H\in\mathbb{R}^{m\times n}$，$x\in\mathbb{R}^n$ 是未知参数向量，$v$ 为白噪声误差，且：

$$
E[v]=0,\qquad E[vv^T]=R>0
$$

若 $W\in\mathbb{R}^{m\times m}$ 为对称非负定阵，则加权最小二乘估计为：

$$
\hat{x}_{WLS}=\arg\min_x (z-Hx)^TW(z-Hx)
$$

标准解为：

$$
\hat{x}_{WLS}=(H^TWH)^{-1}H^TWz
$$

估计误差协方差为：

$$
E[\tilde{x}\tilde{x}^T]=(H^TWH)^{-1}H^TWRWH(H^TWH)^{-1}
$$

其中：

$$
\tilde{x}=x-\hat{x}_{WLS}
$$

#### 一般最小二乘估计

当 $W=I$ 时，加权最小二乘退化为一般最小二乘估计：

$$
\hat{x}_{LS}=(H^TH)^{-1}H^Tz
$$

估计误差协方差阵为：

$$
E[\tilde{x}\tilde{x}^T]=(H^TH)^{-1}H^TRH(H^TH)^{-1}
$$

#### 马尔科夫估计

当 $W=R^{-1}$ 时，加权最小二乘估计称为马尔科夫估计：

$$
\hat{x}_{M}=(H^TR^{-1}H)^{-1}H^TR^{-1}z
$$

估计误差协方差阵为：

$$
E[\tilde{x}\tilde{x}^T]=(H^TR^{-1}H)^{-1}
$$

马尔科夫估计的误差协方差阵最小，是加权最小二乘估计中的最优者。

---

# 下载

<a href="/files/现代控制理论.pdf" download>点击下载原文件</a>
