---
title: 运动控制系统复习思维导图
date: 2026-05-18
updated: 2026-05-18
categories: 笔记
tags:
  - 笔记
  - 运控
description: 分享我复习时做的运动控制系统思维导图。
cover: /img/Motion-control-system/运控.png
mathjax: true
photos:
  -
---

# 运动控制系统思维导图

![原始思维导图总览](/img/Motion-control-system/运控.png)

---

##  总体复习脉络

这张思维导图可以按两条主线复习。

第一条是**直流调速系统**：

$$
\text{V-M 系统基础}\rightarrow
\text{转速开环}\rightarrow
\text{转速闭环}\rightarrow
\text{电流截止负反馈}\rightarrow
\text{转速、电流双闭环}\rightarrow
\text{系统校正}\rightarrow
\text{双闭环调节器工程设计}
$$

第二条是**交流调速系统**：

$$
\text{异步电动机变压调速}\rightarrow
\text{变压变频调速}\rightarrow
\text{PWM/SVPWM}\rightarrow
\text{转差频率控制}\rightarrow
\text{电力电子变频器}
$$

复习时要把握三个层次：

1. **结构层**：系统由哪些环节组成，例如功率变换器、电动机、反馈环节、调节器。  
2. **公式层**：机械特性、静差率、闭环速降、典型系统、调节器参数。  
3. **设计层**：先确定指标，再选典型系统，再计算调节器参数，最后校验近似条件和超调量。

---

# 直流部分

## V-M 系统

V-M 系统指**晶闸管可控整流器—直流电动机调速系统**。它的核心思路是：通过改变晶闸管触发角 $\alpha$，改变整流输出电压 $U_d$，再改变直流电动机电枢电压，从而调节电机转速。

<img src="/img/Motion-control-system/extracted/img_01.png" alt="V-M 系统主电路" style="width:67%;">

### 可控整流输出电压

 V-M 系统主电路公式为：

$$
U_d=2.34U_2\cos\alpha
$$

其中：

- $U_d$：整流装置输出的平均直流电压；
- $U_2$：整流变压器二次侧相电压或等效交流输入电压；
- $\alpha$：晶闸管触发角。

复习时只要记住：**增大触发角 $\alpha$，$\cos\alpha$ 减小，所以输出电压 $U_d$ 减小，电机转速降低。**

### V-M 系统机械特性

<img src="/img/Motion-control-system/extracted/img_02.png" alt="V-M 系统机械特性" style="width:40%;">

### V-M 系统动态结构

晶闸管触发和整流装置存在滞后，准确表达可写成带纯滞后的比例环节：

$$
W_s(s)=K_se^{-T_ss}
$$

工程上常用一阶惯性环节近似：

$$
W_s(s)\approx \frac{K_s}{T_ss+1}
$$

<img src="/img/Motion-control-system/extracted/img_03.png" alt="晶闸管触发与整流装置动态结构" style="width:67%;">

常见整流电路的平均失控时间如下图所示，复习时主要用它来确定 $T_s$。

<img src="/img/Motion-control-system/extracted/img_04.png" alt="各种整流电路失控时间" style="width:67%;">

---

## 转速开环控制的直流调速系统

转速开环控制没有测速反馈，系统只根据给定信号改变电枢电压，不能自动修正负载扰动、电网波动和参数变化带来的速度误差。

### 开环调速方式

转速开环控制的调速方式为：

1. **调节电枢电压 $U_d$**：基速以下常用，机械特性平行移动；
2. **弱磁调速**：减小磁通 $\Phi$，常用于基速以上；
3. **电枢回路串电阻调速**：增大 $R$，机械特性变软，效率低。

### 机械特性

直流电动机电枢电压平衡方程为：

$$
U_d=E+I_dR
$$
则机械特性可写成：

$$
n=\dfrac{U_d-I_dR}{C_e}=n_0-\dfrac{R}{C_e}I_d=n_0-\dfrac{R}{C_eC_m}T_e
$$
其中：
$$
C_e=\dfrac{U_N-R_aI_N}{n_N}=K_e\Phi
$$


额定负载时：
$$
\Delta n_{N}=\frac{R_\Sigma I_N}{C_e}
$$

这里 $R_\Sigma$ 通常包括电枢电阻和整流装置等效内阻。

---

## 转速闭环控制的直流调速系统

转速闭环系统加入测速反馈，把实际转速 $n$ 转换成反馈电压 $U_n$，与给定电压 $U_n^*$ 比较后形成偏差信号，再通过放大器和整流装置控制电枢电压。

### 调速指标

直流调速系统的调速指标主要包括**调速范围**和**静差率**。

调速范围为：

$$
D=\frac{n_{\max}}{n_{\min}}
$$

静差率为：

$$
s=\frac{\Delta n}{n_0}
$$

一般给出的$s$为静差率上限，即$n_0=n_{min}+\Delta n_N$，则可推导出如下三个常用公式：
$$
s=\dfrac{D\Delta n_N}{n_N+D\Delta n_N}\qquad
n_{min}=\dfrac{(1-s)\Delta n_N}{s}\qquad
D=\dfrac{n_N s}{\Delta n_N(1-s)}
$$
其中最常用的是第三个公式，用于计算闭环系统的额定速降。

**复习理解**：开环系统的 $\Delta n$ 往往较大，所以在给定 $D$ 和 $s$ 时，单靠开环通常难以满足调速精度，需要引入闭环反馈。

### 稳态结构

<img src="/img/Motion-control-system/extracted/img_05.png" alt="转速负反馈闭环直流调速系统稳态结构" style="width:67%;">

闭环静特性整理为：

$$
n=\frac{K_pK_sU_n^*}{C_e(1+K)}-\frac{RI_d}{C_e(1+K)}
$$

即：

$$
n=n_{0cl}-\Delta n_{cl}
$$

其中：

$$
K=\frac{K_pK_s\alpha}{C_e}
$$


$$
\alpha=\dfrac{KU_n^*}{(n+\Delta n_{cl})(1+K)}\text{(精确计算)} \quad \alpha=\dfrac{U_n^*}{n}\text{(近似计算)}
$$

开环速降为：

$$
\Delta n_{op}=\frac{RI_d}{C_e}
$$

因此闭环速降与开环速降之间有：

$$
\Delta n_{cl}=\frac{\Delta n_{op}}{1+K}
$$

**结论：**转速负反馈的主要作用是把静态速降压缩为开环的 $1/(1+K)$，从而提高静态调速精度。

### 动态结构与稳定条件

<img src="/img/Motion-control-system/extracted/img_06.png" alt="转速负反馈闭环直流调速系统动态结构" style="width:67%;">

动态结构中需要考虑：

- 晶闸管整流装置滞后 $T_s$；
- 电枢回路电磁时间常数 $T_l$；
- 机电时间常数 $T_m$；
- 转速反馈系数 $\alpha$；
- 放大器放大系数 $K_p$。

稳定条件为：

$$
K<K_{cr}
$$

其中临界放大系数写成：

$$
K_{cr}=\frac{T_m(T_l+T_s)+T_s^2}{T_lT_s}
$$

复习时要注意闭环系统的矛盾：

- 增大 $K$ 可以减小静差；
- 但 $K$ 过大可能破坏动态稳定性。

因此闭环调速不是无限增大放大倍数，而是要在**稳态精度**和**动态稳定性**之间折中。

### 电流截止负反馈

电流截止负反馈用于限制过大的电枢电流。正常负载范围内它不动作；当电流超过截止电流时，它投入工作，使系统静特性变陡，从而限制电流。

<img src="/img/Motion-control-system/extracted/img_07.png" alt="带电流截止负反馈的闭环直流调速系统" style="width:40%;">

<img src="/img/Motion-control-system/extracted/img_08.png" alt="电流截止负反馈静特性" style="width:40%;">

截止电流为：

$$
I_{dcr}=\frac{U_{com}}{R_s}
$$

堵转电流为：

$$
I_{dbl}=\frac{U_n^*+U_{com}}{R_s}
$$

常用经验范围为：

$$
I_{dcr}\approx (1.1\sim1.2)I_N
$$

$$
I_{dbl}\approx (1.5\sim2)I_N
$$

分段静特性可写成：

当 $I_d\le I_{dcr}$ 时，电流截止负反馈未投入：

$$
n=\frac{K_pK_sU_n^*}{C_e(1+K)}-\frac{RI_d}{C_e(1+K)}
$$

当 $I_d>I_{dcr}$ 时，电流截止负反馈投入：

$$
n=\frac{K_pK_s(U_n^*+U_{com})}{C_e(1+K)}-
\frac{(K_pK_sR_s+R)I_d}{C_e(1+K)}
$$

其中第二段斜率更大，所以电流受到明显限制。

---

## 转速、电流双闭环直流调速系统

双闭环系统由**转速外环**和**电流内环**组成。复习时要抓住一句话：

$$
\text{转速调节器 ASR 的输出就是电流调节器 ACR 的给定。}
$$

也就是说，转速环根据速度误差决定“需要多大电流”，电流环再快速控制电枢电流达到这个给定值。

### 稳态结构

<img src="/img/Motion-control-system/extracted/img_09.png" alt="双闭环直流调速系统稳态结构" style="width:67%;">

主要环节包括：

- ASR：转速调节器；
- ACR：电流调节器；
- UPE：电力电子变换装置；
- $\alpha$：转速反馈系数；
- $\beta$：电流反馈系数；
- $R$：电枢回路总电阻；
- $C_e$：电动势系数。

### 动态结构

<img src="/img/Motion-control-system/extracted/img_10.png" alt="双闭环直流调速系统动态结构" style="width:67%;">

ASR 与 ACR 常采用 PI 调节器。调节器形式为：

$$
W_{ASR}(s)=K_n\frac{\tau_ns+1}{\tau_ns}
$$

$$
W_{ACR}(s)=K_i\frac{\tau_is+1}{\tau_is}
$$

### 双闭环系统的启动过程

双闭环直流调速系统启动时一般经历三个阶段：

1. **电流上升阶段**：转速很低，ASR 输出迅速达到限幅，ACR 控制电流快速上升；
2. **恒流升速阶段**：电流保持在允许最大值附近，电机以近似恒加速度升速；
3. **转速调节阶段**：转速接近给定值，ASR 退出饱和，电流下降到负载电流，系统进入稳态。

复习重点是：**电流内环提高电流响应速度并限制最大电流，转速外环保证最终转速精度。**

---

## 系统校正

系统校正的目的，是通过选择调节器结构和参数，把控制系统校正成典型系统，使它满足超调量、调节时间、抗扰性能等指标。

## 典型 I 型系统

典型 I 型系统结构为：

<img src="/img/Motion-control-system/extracted/img_12.png" alt="典型 I 型系统" style="width:40%;">

其闭环特征参数为：

$$
\omega_n=\sqrt{\frac{K}{T}}
$$

$$
\xi=\frac{1}{2\sqrt{KT}}
$$

所以 $KT$ 越大，系统速度越快，但阻尼比下降，超调量增大。

### 典型 I 型系统性能表

<img src="/img/Motion-control-system/extracted/img_13.png" alt="典型 I 型系统动态性能表" style="width:67%;">

常用选择：如果希望超调量较小，常取：

$$
KT=0.5
$$

此时：

$$
\xi=0.707,\qquad \sigma\approx4.3\%
$$

### 校正为典型 I 型系统的调节器选择

<img src="/img/Motion-control-system/extracted/img_14.png" alt="校正为典型 I 型系统的调节器选择" style="width:67%;">

复习时只要抓住：如果控制对象本身缺少积分环节，就要通过调节器引入积分；如果对象中有大惯性环节，常用 PI 调节器的零点去抵消对象中的一个惯性环节。

## 典型 II 型系统

典型 II 型系统结构为：

<img src="/img/Motion-control-system/extracted/img_15.png" alt="典型 II 型系统" style="width:40%;">

引入中频宽参数：

$$
h=\frac{\tau}{T}=\frac{w_2}{w_1}
$$

若题目中没有要求，$h$一般取5。

常用参数关系为：
$$
K=\frac{h+1}{2h^2T^2}
$$

典型 II 型系统相比典型 I 型系统多一个积分环节，因此稳态精度更高，但设计时要特别注意超调量。

### 典型 II 型系统阶跃跟随性能

<img src="/img/Motion-control-system/extracted/img_16.png" alt="典型 II 型系统阶跃输入跟随性能表" style="width:67%;">

### 典型 II 型系统抗扰性能

<img src="/img/Motion-control-system/extracted/img_17.png" alt="典型 II 型系统抗扰性能表" style="width:67%;">

### 校正为典型 II 型系统的调节器选择

<img src="/img/Motion-control-system/extracted/img_18.png" alt="校正为典型 II 型系统的调节器选择" style="width:67%;">

复习时常见套路：

$$
\text{对象中已有一个积分环节}\rightarrow
\text{用 PI 调节器再引入一个积分环节}\rightarrow
\text{校正成典型 II 型系统}
$$

## 传递函数近似

### 高频段小惯性近似

两个小惯性环节串联时，可近似合并为一个惯性环节：

$$
\frac{1}{(T_2s+1)(T_3s+1)}\approx
\frac{1}{(T_2+T_3)s+1}
$$

近似条件为：

$$
\omega_c\le \frac{1}{3\sqrt{T_2T_3}}
$$

### 高阶系统降阶近似

高阶系统可在一定条件下降阶：

$$
W(s)=\frac{K}{as^3+bs^2+cs+1}\approx \frac{K}{cs+1}
$$

近似条件为：

$$
\omega_c\le\frac{1}{3}\min\left(\sqrt{\frac{1}{b}},\sqrt[3]{\frac{1}{a}}\right)
$$

### 低频段大惯性近似

大惯性环节在低频段可近似为积分环节：

$$
\frac{1}{Ts+1}\approx\frac{1}{Ts}
$$

近似条件为：

$$
\omega_c\ge\frac{3}{T}
$$

---

## 工程设计双闭环系统调节器

工程设计双闭环系统时，应先设计电流内环，再设计转速外环：

$$
\text{电流环}\rightarrow\text{等效成转速环中的小惯性环节}\rightarrow\text{转速环}\rightarrow\text{超调量校验}
$$

从双闭环动态结构中分离出电流环：

<img src="/img/Motion-control-system/extracted/img_22.png" alt="电流环位置" style="width:67%;">

电流反馈滤波和转速反馈滤波时间常数为：

$$
T_{0i}:\text{电流反馈滤波时间常数}
$$

$$
T_{0n}:\text{转速反馈滤波时间常数}
$$

### 电流调节器设计

电流环等效结构为：

<img src="/img/Motion-control-system/extracted/img_24.png" alt="电流环等效结构" style="width:67%;">

电流调节器采用 PI 调节器：

$$
W_{ACR}(s)=K_i\frac{\tau_is+1}{\tau_is}
$$

设计时通常令 PI 零点抵消电枢电磁时间常数：

$$
\tau_i=T_l
$$

电流环小时间常数为：

$$
T_{\Sigma i}=T_s+T_{0i}
$$

其中电流环开环增益为：

$$
K_I=\frac{K_iK_s\beta}{\tau_iR}
$$

所以电流调节器比例系数为：

$$
K_i=\frac{K_I\tau_iR}{K_s\beta}
$$

电流调节器模拟电路参数为：

<img src="/img/Motion-control-system/extracted/img_25.png" alt="电流调节器电路" style="width:40%;">
$$
K_i=\frac{R_i}{R_0}
$$

$$
\tau_i=R_iC_i
$$

$$
T_{0i}=\frac{1}{4}R_0C_{0i}
$$

### 电流环近似条件

电流环的截止频率$w_c=K_I$，近似条件可整理为如下三条：

1. 忽略晶闸管整流装置滞后的条件：

$$
\omega_{ci}\le \frac{1}{3T_s}
$$

2. 忽略反电动势变化对电流环动态影响的条件：

$$
\omega_{ci}\ge 3\sqrt{\frac{1}{T_mT_l}}
$$

3. 电流环小惯性近似处理的条件：

$$
\omega_{ci}\le \frac{1}{3}\sqrt{\frac{1}{T_sT_{0i}}}
$$

### 转速调节器设计

电流环设计完成后，可将电流环等效为转速环中的一个小惯性环节，再设计转速外环。

<img src="/img/Motion-control-system/extracted/img_27.png" alt="转速环动态结构及其简化" style="width:67%;">

转速调节器采用 PI 调节器：

$$
W_{ASR}(s)=K_n\frac{\tau_ns+1}{\tau_ns}
$$

转速环小时间常数为：

$$
T_{\Sigma n}=\frac{1}{K_I}+T_{0n}
$$

按典型 II 型系统设计，取：

$$
\tau_n=hT_{\Sigma n}
$$

典型 II 型系统要求转速环开环增益为：

$$
K_N=\frac{h+1}{2h^2T_{\Sigma n}^2}
$$

代入 $\tau_n=hT_{\Sigma n}$ 后，得到转速调节器比例系数：

$$
K_n=\frac{(h+1)\beta C_eT_m}{2h\alpha RT_{\Sigma n}}
$$

转速调节器模拟电路参数为：

<img src="/img/Motion-control-system/extracted/img_28.png" alt="转速调节器电路" style="width:40%;">
$$
K_n=\frac{R_n}{R_0}
$$

$$
\tau_n=R_nC_n
$$

$$
T_{0n}=\frac{1}{4}R_0C_{0n}
$$

### 转速环近似条件

思维导图中的转速环近似条件为：

$$
\omega_{cn}\le\frac{1}{3}\sqrt{\frac{K_I}{T_{\Sigma i}}}
$$

$$
\omega_{cn}\le\frac{1}{3}\sqrt{\frac{K_I}{T_{0n}}}
$$

### 退饱和转速超调量

退饱和转速超调量公式为：

$$
\sigma_n=2\left(\frac{\Delta C_{max}}{C_b}\right)(\lambda-z)
\frac{\Delta n_NT_{\Sigma n}}{n^*T_m}
$$

其中额定速降为：

$$
\Delta n_N=\frac{I_{dN}R}{C_e}
$$

空载启动时常取：

$$
z=0
$$

---

# 交流部分

## 闭环控制的异步电动机变压调速系统

闭环控制的异步电动机变压调速系统通过改变定子电压 $U_s$ 来调节电磁转矩和转速。它和直流电动机闭环调压系统不同，异步电动机机械特性存在稳定运行区和不稳定运行区，不能无限延长。

### 稳态模型

异步电动机稳态模型常用 T 型等效电路表示。

<img src="/img/Motion-control-system/extracted/img_31.png" alt="异步电动机稳态等效电路" style="width:67%;">

### 机械特性

改变定子电压时，异步电动机机械特性会发生变化。电压越低，最大电磁转矩越小，稳定运行范围也变窄。

<img src="/img/Motion-control-system/extracted/img_32.png" alt="异步电动机不同电压下机械特性" style="width:67%;">

### 静特性

<img src="/img/Motion-control-system/extracted/img_33.png" alt="闭环控制变压调速系统静特性" style="width:50%;">

变压调速系统的特点：

​	异步电动机闭环变压调速系统不同于直流电动机闭环调压系统，其静特性左右两边都有限，不能无限延长，当负载变化使电压调节到极限值时，闭环系统失去控制能力，此时工作点只能沿着极限开环机械特性变化。

### 动态模型

<img src="/img/Motion-control-system/extracted/img_35.png" alt="异步电动机闭环变压调速系统动态结构" style="width:40%;">

转速调节器：

$$
W_{ASR}(s)=K_n\frac{\tau_ns+1}{\tau_ns}
$$

晶闸管变压装置：

$$
W_{GT-V}(s)=\frac{K_s}{T_ss+1}
$$

测速反馈环节：

$$
W_{FBS}(s)=\frac{\alpha}{T_{0n}s+1}
$$

异步电动机在工作点 $A$ 附近线性化后可近似为：

$$
W_{MA}(s)=\frac{K_{MA}}{T_ms+1}
$$

其中 $MA$ 表示异步电动机，$FBS$ 表示测速反馈环节。

---

## 笼型异步电机变压变频调速系统

变压变频调速的核心是协调改变电压和频率，使气隙磁通维持在合适范围内。

### 定子每相电动势

公式为：

$$
E_g=4.44f_1N_sk_{Ns}\Phi_m
$$

其中：

- $E_g$：气隙磁通在定子每相中感应电动势的有效值；
- $f_1$：定子频率；
- $N_s$：定子每相绕组串联匝数；
- $k_{Ns}$：基波绕组系数；
- $\Phi_m$：每极气隙磁通量。

由此可知，要保持磁通 $\Phi_m$ 不变，需要保持电动势与频率之比恒定。

变压变频调速系统的控制特性如下图所示，基频以下磁通不变，近似为恒转矩调速；基频以上$U_{sN}$不变，近似为恒功率调速。

<img src="/img/Motion-control-system/extracted/img_37.png" alt="变压变频调速控制特性" style="width:50%;">

### 基频以下——恒压频比

基频以下，为保持磁通近似恒定，常采用：

$$
\frac{U_s}{w_1}=\text{Const.}
$$

低频时，定子电阻压降所占比例增大，因此简单保持 $U_s/w_1$ 恒定会导致磁通下降，需要进行低频电压补偿。

<img src="/img/Motion-control-system/extracted/img_38.png" alt="恒压频比控制机械特性" style="width:50%;">

如果采用恒气隙电动势控制，则为：

$$
\frac{E_g}{\omega_1}=\text{Const.}
$$

<img src="/img/Motion-control-system/extracted/img_39.png" alt="恒 $E_g/\omega_1$ 控制机械特性" style="width:50%;">

### 基频以上——恒压变频

基频以上，电压不能继续升高，一般保持电压恒定并继续升高频率，磁通减小，进入弱磁或近似恒功率调速区。

$$
U_s=\text{Const.},\qquad f_1>f_{1N}
$$

<img src="/img/Motion-control-system/extracted/img_40.png" alt="基频以上恒压变频机械特性" style="width:50%;">

---

## 变频变压调速系统中的 PWM 技术与 SVPWM

SVPWM 是**磁链跟踪控制技术**，基本思想是：

> 把逆变器和交流电动机视为一体，按照跟踪圆形旋转磁场来控制逆变器的工作。

### SVPWM 的复习理解

三相逆变器有多个开关状态，不同开关状态会产生不同的空间电压矢量。SVPWM 通过合理安排这些电压矢量的作用时间，使合成矢量尽可能逼近圆形旋转磁场。

### 开关状态顺序原则

在实际系统中，应尽量减少开关状态变化引起的开关损耗,不同开关状态的顺序必须遵守下述原则：

> 每次切换开关状态时，只切换一个功率开关器件,这样可以满足最小开关损耗。

---

## 转速闭环转差频率控制的变压变频调速系统

转差频率控制的关键思想是：

$$
\text{控制转差频率}\ \omega_s\ \text{就代表控制电磁转矩}\ T_e
$$

- 在较小转差频率范围内：$\omega_s\le \omega_{sm}$，电磁转矩与转差频率近似成正比：$T_e\propto \omega_s$.

- 当 $T_e$ 达到最大值 $T_{emax}$ 时，转差频率达到：$\omega_s=\omega_{smax}$.

<img src="/img/Motion-control-system/extracted/img_43.png" alt="恒电流控制时 $T_e=f(\omega_s)$ 特性" style="width:50%;">

转差频率控制的规律是：

1. 在 $\omega_s\le\omega_{sm}$ 的范围内，转矩 $T_e$ 基本上与 $\omega_s$ 成正比，条件是气隙磁通不变；
2. 在不同定子电流下，按下图所示的函数关系 $U_s=f(\omega_1,I_s)$ 控制定子电压和频率，就能保持气隙磁通 $\Phi_m$ 恒定。

不同定子电流下的电压-频率关系为：

$$
U_s=f(\omega_1,I_s)
$$

<img src="/img/Motion-control-system/extracted/img_44.png" alt="不同定子电流时的电压-频率特性" style="width:40%;">

---

## 电力电子变压变频器

电力电子变压变频器是交流调速系统中的功率变换环节，分为两类：

$$
\text{交-直-交变压变频器}\quad\text{和}\quad\text{交-交变压变频器}
$$

### 交-直-交变压变频器

交-直-交变频器先把交流电整流成直流电，再由逆变器输出电压和频率可调的交流电。

<img src="/img/Motion-control-system/extracted/img_46.png" alt="交-直-交 PWM 变压变频器" style="width:50%;">

交-直-交变压变频器的结构简单，稳态、动态性能较好，是现代变频调速系统中非常常见的形式。

### 交-交变压变频器

交-交变频器不经过直流中间环节，直接把一种频率的交流电变换成另一种频率的交流电。

<img src="/img/Motion-control-system/extracted/img_47.png" alt="交-交直接变压变频器" style="width:50%;">

交-交变压变频器的输入功率因数低，最高输出频率一般不超过电网频率的 $1/3\sim1/2$，需要大量晶闸管，适合低速大容量调速场合。

### 电压源型逆变器 VSI 与电流源型逆变器 CSI

按交-直-交变压变频器中直流环节的电源类型可分为VSI与CSI。

<img src="/img/Motion-control-system/extracted/img_48.png" alt="电压源型与电流源型逆变器" style="width:67%;">

电压源型逆变器 VSI 的直流侧近似恒压源，通常用大电容维持直流电压：
$$
U_d\approx\text{Const.}
$$

电流源型逆变器 CSI 的直流侧近似恒流源，通常用大电感维持直流电流：

$$
I_d\approx\text{Const.}
$$

两者对比：

<img src="/img/Motion-control-system/extracted/img_49.png" alt="VSI 与 CSI 对比表" style="width:67%;">

---

# 数字化运动控制系统*

思维导图中把数字化运动控制系统作为最后一个分支。可按如下链路理解：

$$
\text{传感器采样}\rightarrow
\text{A/D 转换}\rightarrow
\text{数字控制算法}\rightarrow
\text{PWM 或 D/A 输出}\rightarrow
\text{功率变换器}\rightarrow
\text{电动机}
$$

数字化运动控制系统的复习要点：

- 控制器以微处理器、DSP、PLC 或运动控制卡为核心；
- 控制算法以离散形式实现；
- 便于参数整定、通信、监控和故障诊断；
- 常与 PWM 变换器、编码器、测速反馈、位置反馈结合使用。

---

# 例题整理

<img src="/img/Motion-control-system/extracted/img_54.png" alt="1-10" style="zoom:67%;" />

<img src="/img/Motion-control-system/extracted/img_53.png" alt="img_53" style="zoom:67%;" />

<img src="/img/Motion-control-system/extracted/img_51.png" alt="img_51" style="zoom:67%;" />

<img src="/img/Motion-control-system/extracted/img_50.png" alt="img_50" style="zoom:67%;" />

# 公式速查

## 直流调速基础

$$
U_d=E+I_dR
$$

$$
E=C_e n
$$

$$
T_e=C_m I_d
$$

$$
n=\frac{U_d-I_dR}{C_e}
$$

$$
C_e=\frac{U_N-I_NR_a}{n_N}
$$

$$
\Delta n=\frac{RI_d}{C_e}
$$

## 调速指标

$$
D=\frac{n_{\max}}{n_{\min}}
$$

$$
s=\frac{\Delta n}{n_0}=\frac{\Delta n}{n+\Delta n}
$$

$$
\Delta n_d=\frac{s}{1-s}\cdot\frac{n_N}{D}
$$

## 转速闭环

$$
K=\frac{K_pK_s\alpha}{C_e}
$$

$$
\Delta n_{cl}=\frac{\Delta n_{op}}{1+K}
$$

$$
K<K_{cr}=\frac{T_m(T_l+T_s)+T_s^2}{T_lT_s}
$$

## 电流截止负反馈

$$
I_{dcr}=\frac{U_{com}}{R_s}
$$

$$
I_{dbl}=\frac{U_n^*+U_{com}}{R_s}
$$

$$
I_{dcr}\approx(1.1\sim1.2)I_N
$$

$$
I_{dbl}\approx(1.5\sim2)I_N
$$

## 典型系统

典型 I 型：

$$
W(s)=\frac{K}{s(Ts+1)}
$$

$$
\omega_n=\sqrt{\frac{K}{T}},\qquad
\xi=\frac{1}{2\sqrt{KT}}
$$

典型 II 型：

$$
W(s)=\frac{K(\tau s+1)}{s^2(Ts+1)}
$$

$$
h=\frac{\tau}{T},\qquad
K=\frac{h+1}{2h^2T^2}
$$

## 双闭环工程设计

电流调节器：

$$
W_{ACR}(s)=K_i\frac{\tau_is+1}{\tau_is}
$$

$$
\tau_i=T_l
$$

$$
T_{\Sigma i}=T_s+T_{0i}
$$

$$
K_i=\frac{K_I\tau_iR}{K_s\beta}
$$

转速调节器：

$$
W_{ASR}(s)=K_n\frac{\tau_ns+1}{\tau_ns}
$$

$$
T_{\Sigma n}=\frac{1}{K_I}+T_{0n}
$$

$$
\tau_n=hT_{\Sigma n}
$$

$$
K_n=\frac{(h+1)\beta C_eT_m}{2h\alpha RT_{\Sigma n}}
$$

退饱和超调量：

$$
\sigma_n=2\left(\frac{\Delta C_{max}}{C_b}\right)(\lambda-z)
\frac{\Delta n_NT_{\Sigma n}}{n^*T_m}
$$

## 交流调速

$$
E_g=4.44f_1N_sk_{Ns}\Phi_m
$$

$$
\frac{U_s}{f_1}=\text{Const.}
$$

$$
\frac{U_s}{\omega_1}=\text{Const.}
$$

$$
\frac{E_g}{\omega_1}=\text{Const.}
$$

$$
T_e\propto\omega_s\quad(\omega_s\le\omega_{sm})
$$

$$
U_s=f(\omega_1,I_s)
$$

---

  # 下载 

  <a href="/files/运控.pdf" download>点击下载运控思维导图pdf版</a>

