<!-- @import "[TOC]" {cmd="toc" depthFrom=1 depthTo=6 orderedList=false} -->

<!-- code_chunk_output -->

- [尝试使用 MPE 书写论文数学公式](#尝试使用-mpe-书写论文数学公式)
  - [特征提取相关公式](#特征提取相关公式)
    - [AAC 和 PseAAC 公式](#aac-和-pseaac-公式)
    - [CKSAAP 计算公式](#cksaap-计算公式)
    - [PSSM 相关公式](#pssm-相关公式)
  - [特征排序相关公式](#特征排序相关公式)
  - [梯度提升模型](#梯度提升模型)
    - [逻辑斯蒂](#逻辑斯蒂)
    - [KL散度](#kl散度)
    - [二分类交叉熵（损失函数）](#二分类交叉熵损失函数)
    - [梯度下降](#梯度下降)
    - [XGBoost目标函数](#xgboost目标函数)
  - [其它 MPE 元素](#其它-mpe-元素)

<!-- /code_chunk_output -->

# 尝试使用 MPE 书写论文数学公式

## 特征提取相关公式

### AAC 和 PseAAC 公式

$$
    x_{i}=\begin{cases}
        \dfrac{f_{i}}{\sum_{j=1}^{20}f_{j}+w\sum_{j=1}^{\lambda}\theta_{j}},\left ( 1 \leq i \leq 20 \right ) \\
        \dfrac{w\theta_{i-20}}{\sum_{j=1}^{20}f_{j}+w\sum_{j=1}^{\lambda}\theta_{j}},\left ( 21\leq i \leq 20+\lambda \right )
    \end{cases}
$$

$$ \theta_{j}=\frac{1}{L-j}\sum_{i=1}^{L-j}\Theta_{R_{i},R{i+j}} $$

$$ \Theta_{R_{i},R_{i+j}}=\frac{1}{3} \{ \left[ H_{1}(R_{j}) - H_{1}(R_{i}) \right]^2 + \left[ H_{2}(R_{j}) - H_{2}(R_{i}) \right]^2 \\
    + \left[ M(R_{j}) - M(R_{i}) \right]^2 \} $$

$$
    \begin{cases}
        H(R_{i})=\frac{H(R_{i})-\sum_{i=1}^{20}\frac{H(R_{i})}{20}}{\sqrt{\frac{\sum_{i=1}^{20}\left[ H(R_{i})- \sum_{i=1}^{20}\frac{H(R_{i})}{20}\right]^2}{20}}} \\
        M(R_{i})=\frac{M(R_{i})-\sum_{i=1}^{20}\frac{M(R_{i})}{20}}{\sqrt{\frac{\sum_{i=1}^{20}\left[ M(R_{i})- \sum_{i=1}^{20}\frac{M(R_{i})}{20}\right]^2}{20}}}
    \end{cases}
$$

$$
    \begin{bmatrix}
        x_{1} & x_{2} & \ldots x_{20} & x_{20+1} & \ldots & x_{20+20}
    \end{bmatrix}
$$

### CKSAAP 计算公式
$$
    \underbrace{( \frac{N_{AA}}{N_{Total}},\frac{N_{AC}}{N_{Total}},...,\frac{N_{i,j}}{N_{Total}},...,\frac{N_{YY}}{N_{Total}} )}_{400}
$$

### PSSM 相关公式
$$ 
    \mathbf{X}_{PSSM}=
        \begin{bmatrix}
            x_{1,1} & x_{1,2} & \ldots & x_{1,20}\\
            x_{2,1} & x_{2,2} & \ldots & x_{2,20}\\
            \vdots & \vdots & \ddots & \vdots\\
            x_{21,1} & x_{21,2} & \ldots & x_{21,20}\\
        \end{bmatrix}
$$
## 特征排序相关公式

## 梯度提升模型
### 逻辑斯蒂
$$
    P(y=1|x)=\frac{1}{1+e^{-\sum_{m=1}^{M}h_m(x)}}
$$
$$
    P(y=0|x)=1-P(y=1|x)=\frac{e^{-\sum_{m=1}^{M}h_m(x)}}{1+e^{-\sum_{m=1}^{M}h_m(x)}}
$$
### KL散度
$$
    D_{KL}(p||q)=\sum_{i=1}^{n}p(x_i)log(p(x_i))-\sum_{i=1}^{n}p(x_i)log(q(x_i))
$$
### 二分类交叉熵（损失函数）
$$
    L(x, y)=-P(x)log\hat{P(x)}-(1-P(x))log(1-\hat{P(x)})
$$
$$
     L(x, y)=ylog(1+e^{-\sum_{m=1}^{M}h_m(x)})+(1-y)[\sum_{m=1}^{M}h_m(x)+log(1+e^{-\sum_{m=1}^{M}h_m(x)})]
$$
### 梯度下降
$$
    \nabla=-\frac{\partial L}{\partial f_m(x)}|_{x, y}=y-\frac{1}{1+e^{-f_m(x)}} \\
    \{ x, y-f_{m-1}(x) \}
$$
### XGBoost目标函数
$$
    Obj^{(m)}=\sum_{i=1}^{n}L(y_i, \hat{y}_{i}^{(m-1)}+f_{m}(x_i))+\sum_{i=1}^{m}\Omega(f_i)
$$
$$
    Obj^{(m)}=\sum_{i=1}^{n}[L(y_i, \hat{y}_{i}^{(m-1)}+g_if_{m}(x_i)+\frac{1}{2}h_if_{m}^2(x_i))]+\sum_{i=1}^{m}\Omega(f_i)
$$
$$
    Obj^{(m)}=\sum_{i=1}^{n}[g_if_{m}(x_i)+\frac{1}{2}h_if_{m}^2(x_i))]+\sum_{i=1}^{m}\Omega(f_i)
$$
$$
    \Omega(f_i)=\gamma T+\frac{1}{2}\lambda\sum_{j=1}{T}w_{j}^{2}
$$
$$
    Obj^{(m)}=\sum_{j=1}^{T}[(\sum_{i \in I_j}g_i)w_j+\frac{1}{2}(\sum_{i \in I_j}h_i+\lambda)w_j^2]+\gamma T
$$
## 其它 MPE 元素
```dot
digraph G {
    A -> B
    B -> C
    B -> D
}
```
| a | b |
|---|---|
| c | d |
