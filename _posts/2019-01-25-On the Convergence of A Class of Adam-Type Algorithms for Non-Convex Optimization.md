---
layout: post
title:  "On the Convergence of A Class of Adam-Type Algorithms for Non-Convex Optimization"
date:   2019-01-25
catalog: true
author:  Icy
header-img:
tags: [Algorithm]
---

本篇论文[(1)]是基于`momentum algorithm`来研究一类`adaptive gradient`的。这类算法， 在本篇论文里面成为`Adam-Type`，主要包括`Adam`, `AMSGrad` and `AdaGrad`等算法。这些算法在求解nonconvex problem 上还是一个存在争议的问题。所以这篇文章就讨论了一下使得算法收敛的mild sufficient condition. This paper prove that under some conditions, the Adam-type methods can achieve the convergence rate of order $$O(logT/\sqrt(T))$$  

---
<center>Part 1: 算法介绍</center>
---

> 写在前面： 为了保证理解的一致性， 本部分会依次介绍`momentum algorithm`, `Adam`, `AMSGrad`, `AdaGrad`, 然后再正式开始阅读论文。 必要情况下， 会贴出相关的算法的实现代码（`python` or `Matlab`）.

#### Stochastic Gradient Descent
```
# Vanilla update:
    x += -learning_rate * dx
```
#### momentum algorithm[(2)]
From a physical perspective of the optimization problem, the loss can be interpreted as the height of a hilly terrain. Initializing the parameters with random numbers is equivalent to setting a particle with zero initial velocity at some location. The optimization process can then be seen as equivalent to the process of simulating the parameter vector as rolling on the landscape. The physics view suggests an update in which the gradient only directly influences the velocity, which in turen has an effect on the position.[(3)]也就是说， 从物理角度来看能量是个高度成正比的， 进$$U = mgh$$, 而我们施加在particle上的作用力使其向负梯度方向走， 即$$F = -\delta U = ma$$.之前的SGD都是gradient直接决定更新后的位置， 但是momentum是直接决定速度， 然后影响到最终更新到的位置。  

```
# Momentum update
    v = mu * v - learning_rate * dx
    x += v
```
这里面的$$v$$是速度， $$mu$$可以解释为摩擦系数之类的，可以降低系统能量，使particle最终能够无能量而停止。所以与标准的SGD相比， momentum是在迭代中， 增加了前一步的信息，使得两步方向相同的部分得到增强，两步方向不同的部分削弱。因此可以在很大程度上解决zig-zag问题， 使学习速率可以大一点。  
#### Nesterov Momentum
该版本的Momentum，和标准的相比，多了一个lookahead position `x_ahead = x + mu * v`可以在理论上保证凸函数的收敛，实际中的表现也比标准的momentum好。
```
x_ahead = x + mu * v
v = mu * v - learning_rate * dx_ahead
x += v
```

#### AdaGrad[(4)]
Adagrad 是一种自适应学习速率的方法。
```
cache += dx**2
x += -learning_rate * dx / (np.sqrt(cache) + eps)
```
相比于SGD， AdaGrad增加了cache, cache是一个和x同样维度的变量，用以对`dx` 进行element-wise的normalize。因此， 若某个方向之前走了比较大的长度， 会被逐渐累计到cache， 然后相应的在这个方向上的步子就会迈的谨慎很多；而其他方向，走的比较少，之后就会扩大步伐。

#### RMSprop[(3)]
```
cache = decay_rate * cache + (1-decay_rate) * dx**2
x += -learning_rate * dx / (np.sqrt(cache) + eps)
```
The RMSprop update adjusts the Adagrad method in a very simple way in an attempt to resuce its aggressive, monotonically decreasing learning rate. 不会再发生更新停止的情况的。

#### Adam
是Adagrad 和 momentum的结合。
```
m = beta_1 * m + (1 - beta_1) * dx
v = beta_2 * v + (1 - beta_2) * (dx ** 2)
x += -learning_rate * m / (np.sqrt(v) + eps)
```

Bias correction mechanism：
```
m = beta_1 * m + (1 - beta_1) * dx
mt = m / (1 - beta_1 ** t)
v = beta_2 * v + (1 - beta_2) * (dx ** 2)
vt = v / (1 - beta_2 ** t)
x += - learning_rate * mt / (np.sqrt(vt) + eps)
```
相当于在时间上做了discount。
#### AMSGrad
Input: $$x_1 \in F$$, step size $$\{\alpha_t\}_{t=1}^{T}$$, $$\{\beta_{1, t}\}_{t=1}^T$$, $$\beta_2$$  
Set $$m_0 = 0, v_0 = 0$$ and $$\hat{v}_0 = 0$$  
for t = 1 to T do  
&emsp;&emsp;$$g_t = \nabla f_t(x_t)$$  
&emsp;&emsp;$$m_t = \beta_{1, t}m_{t-1} + (1 - \beta_{1, t})g_t$$  
&emsp;&emsp;$$v_t = \beta_2v_{t - 1} + (1 - \beta_2)g_t$$  
&emsp;&emsp;$$\hat{v}_t = max(\hat{v}_{t-1}, v_t)$$ and $$\hat{V}_t = diag(\hat{v}_t)$$  
&emsp;&emsp;$$x_{t+1} = \prod_{F, \sqrt{\hat{V}_t}} (x_t - \alpha_tm_t / \sqrt{\hat{v}_t})$$  
end for;[(5)]  

---
<center>Part 2: 论文正文</center>
---
## 本篇论文的贡献：  
- 首次证明了， 在合适的条件下（步长和算法参数）， Adam-Type算法 all converge to first-order stationary solutions of original nonconvex problem[(1)].  
- 证明了论文提出的条件是非常重要的， 违反他们会使得整个算法不收敛。

## Challenges：
- The update directions are no longer ubiased estimates of the true gradients for nonconvex version of Adam-Type adaptive gradient methods.  
- The involved form of the learning rate
- The existing convex analysis does not apply to the nonconvex scenario since nonconvex optimization requires a different convergence criterion

## some Notations:
- $$x / y, x \odot y, x^2, \sqrt{x}$$: element-wise division, element-wise product, element-wise square, element-wise square root
- $$[N]$$: $$\{1, 2, ..., N\}$$

## some assumptions:
- function $$f(x)$$ is continuously differentiable and has Lipshcitz continuous gradient

## Algorithm
Consider the following generic problem:  
<center>$$min_{x \in R^d} f(x) = E_{\xi}[f(x; \xi)]$$</center>
where $$\xi$$ is a certain random variable representing randomly selected data sample or random noise.  

Generic form of exponentially weighted stochastic gradient descent algorithm:  
**Algorithm 1: Gradient Adam**  
(S0). Initialize $$m_0 = 0$$ and $$x_1$$  
For t = 1, ..., T, do  
&emsp;&emsp;(S1). $$m_t = \beta_{1, t} m_{t-1} + (1 - \beta_{1, t}) g_t$$  
&emsp;&emsp;(S2). $$\hat{v_t} = f_t(g_1, g_2, ..., g_t)$$  
&emsp;&emsp;(S3). $$x_{t+1} = x_t - \alpha_t m_t / \sqrt{\hat{v_t}}$$  
end;  
Where $$\alpha_t$$ is the step size at time $$t$$, $$\beta_{1, t} > 0$$ is a sequence of problem parameters; $$m_t \in R^d$$ denotes some (exponentially weighted) gradient estimate; $$\hat{v_t} = f_t(g_1, g_2, ..., g_t) \in R^d$$ takes all the past gradients as input and returns a vector of dimension $$d$$.  
**Special case of the generic algorithm:**
- If $$\beta_{1, t} = 0, (\hat{v_t})_j = 1, \forall t > 0, j \in [d]$$,   
  (S1). $$m_t = 0 * m_{t-1} + 1 * g_t = g_t$$  
  (S2). $$\hat{v_t} = f_t(g_1, ..., g_t) = 1$$  
  (S3). $$x_{t+1} = x_t - \alpha_t g_t / \sqrt{\hat{v_t}} = x_t - \alpha_t g_t$$
  The algorithm becomes the classic SGD.
- If $$\beta_{1, t}=\beta_1 \in (0, 1), \hat{v_t}_j = 1, \forall t > 0, j \in [d]$$,   
  (S1). $$m_t = \beta_1 m_{t-1} + (1 - \beta_1)g_t$$  
  (S2). $$\hat{v_t} = 1$$  
  (S3). $$x_{t+1} = x_t - \alpha_tm_t$$  
  The algorithm becomes the SGD with Momentum
- If $$\beta_{1, t} = \beta_1 \in (0, 1), \hat{v}_t = \beta_2 \hat{v}_{t-1} + (1 - \beta_2)g_t^2$$ for some $$\beta_2 \in (0, 1)$$,   
  (S1). $$m_t = \beta_1 m_{t - 1} + (1 - \beta_1)g_t$$    
  (S2). $$\hat{v_t} = \beta_2 \hat{v}_{t-1} + (1 - \beta_2)g_t^2$$  
  (S3). $$x_{t+1} = x_t - \alpha_t m_t / \sqrt{\hat{v}_t} $$  
  the algorithm becomes the Adad algorithm.
- If $$\hat{v}_t = max(\hat{v}_{t-1}, v_t)$$ and $$v_t = \beta_2 v_{t-1} + (1 - \beta_2)g_t^2$$,  
  (S1). $$m_t = \beta_{1, t}m_{t-1} + (1 - \beta_{1, t})g_t$$  
  (S2). $$\hat{v}_t = f_t(g_1, g_2, ..., g_t) = max(\hat{v}_{t-1}, \beta_2 v_{t-1} + (1 - \beta_2)g_t^2)$$  
  (S3). $$x_{t+1} = x_t - \alpha_t m_t / \sqrt{\hat{v}_t} $$  
  this algorithm becomes AMSGrad algorithm.
- If $$\beta_{1, t} = 0$$ and $$\hat{v}_t = \sum_{i =1}^t{g_i^2 / t}$$, i.e., $$\hat{v}_t = (1 - 1/t)\hat{v}_{t-1} + (1/t)g_t^2$$,  
  (S1). $$m_t = h_t$$  
  (S2). $$\hat{v}_t = f_t(g_1, g_2, ..., g_t) = (1 - 1/t)\hat{v}_{t-1} + (1/t)g_t^2$$  
  (S3). $$x_{t+1} = x_t - \alpha_tm_t/\sqrt{\hat{v}_t}$$  

Taking into account the possibility that the data point will be picked deterministically and incrementally, we can get the **Algorithm 2**:  
(S0). Initialize $$x_1$$ and $$m_0^n = 0$$  
For $$t = 1, ..., T$$, do  
(S1). Let $$m_t^0 = m_{t-1}^n, x_t^1 = x_t$$  
&emsp;&emsp;For $$i=1, ..., n$$, do  
&emsp;&emsp;(S1-1). $$m_t^i = \beta_{1, t}m_t^{i-1} + (1 - \beta_{1, t})\nabla f_i(x_t^i)$$  
&emsp;&emsp;(S1-2). $$\hat{v}_t^i = f_{t, i}(\nabla f_i(x_1^1), \nabla f_i(x_1^2),..., \nabla f_i(x_t^i))$$  
&emsp;&emsp;(S1-3). $$x_t^{i+1} = x_t^i - \alpha_t^i m_t^i / \sqrt{\hat{v}_t^i}$$  
&emsp;&emsp;end;  
(S2). $$x_{t+1} = x_t^{n+1}$$  
其中T代表更新的次数，n代表batch的size。  
## Convergence Analysis for Generalized Adam  
### Assumptions  
1 $$f$$ is differentiable and has L-Lipschitz gradient, i.e. $$\forall x, y$$ <center>$$||\nabla f(x) - \nabla f(y)|| \leq L||x-y||$$</center>
  It is also lower bounded, i.e. $$f(x^*) > - \infty$$
2 At time $$t$$, the algorithm can access a bounded noisy gradient and the true gradient is bounded, i.e. <center>$$||\nabla f(x_t)|| \leq H, ||g_t|| \leq H. \forall t > 1.$$</center>  
3 The noisy gradient is unbiased and the noise is independent, i.e. $$g_t = \nabla f(x_t) + \xi_t$$, $$E[\xi_t] = 0$$  and $$\xi_i$$ is independent of $$\xi_j$$ if $$i \neq j$$.  

The result of this paper shows that if the coordinate-wise weighting term $$\sqrt{v}_t$$ in Algorithm 1 is properly chosen, we can ensure the global convergence as well as the sublinear convergence rate of the algorithm.  
**Theorem 3.1** suppose that Assumptions 1 is satisfied, $$\beta_1 \geq \beta_{1, t}, \beta_{1, t} \in [0, 1)$$ is non-increasing, and $$||\alpha_t m_t / \sqrt{v}_t|| \leq G$$ for any t, then Algorithm 1 yields   
$$E[\sum_{t=1}^T \alpha_t \langle \nabla f(x_t), \frac{\nabla f(x_t)}{\sqrt{\hat{v}_t}}\rangle]$$  
$$ \leq E[C_1 \sum_{t=1}^T ||\alpha_t\frac{ g_t}{\sqrt{\hat{v}_t}}||^2 + C_2 \sum_{t=2}^T ||\frac{\alpha_t}{\sqrt{\hat{v}_t}} - \frac{\alpha_{t-1}}{\sqrt{\hat{v}_{t-1}}}||_1 + C_3 \sum_{t=2}^T || \frac{\alpha_t}{ \sqrt{\hat{v}_t} } - \frac{\alpha_{t-1} } {\sqrt{\hat{v}_{t-1}} } ||^2 ] + C_4 $$  
where $$C_1, C_2, C_3$$ are constants $$d$$ and $$T$$, $$C_4$$ is a constant independent of $$T$$, the expectation is taken with repect to all the randomness corresponding to $$\{g_t\}$$.  






[(1)]: https://arxiv.org/pdf/1808.02941.pdf
[(2)]: https://blog.paperspace.com/intro-to-optimization-momentum-rmsprop-adam/
[(3)]: http://cs231n.github.io/neural-networks-3/#sgd
[(4)]: https://blog.csdn.net/huplion/article/details/79184338
[(5)]: https://openreview.net/pdf?id=ryQu7f-RZ
