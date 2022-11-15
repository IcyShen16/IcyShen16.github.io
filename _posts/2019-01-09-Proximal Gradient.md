---
layout: post
title: "Introduction to Block Chain"
date: 2019-01-09
catalog: true
author:  Icy
header-img:
tags: [Algorithm, Matlab, Matrix Completion]
---


## Introduction
**Proximal Gradient**等价于implicit gradient. 他的梯度更新不是用$$x^k$$处的gradient estimate，而是用在$$x^{k+1}$$的gradient estimate  
implicit gradient: $$x^{k+1} = x^{k} - \lambda \delta{f(x^{k+1})}$$  
explicit gradient descent: $$x^{k+1} = x^{k} - \lambda \delta{f(x^{k})}$$  
**proximal update**:    
$$prox_f(x^k) = argmin f(x) + \frac{1}{2\lambda} \lVert x - x^k\rVert^2$$  
the optimal condition is: $$0 = \lambda \delta f(x^*) + (x^* - x^k)$$  
which is just the final update: $$x^* = x^k - \lambda \delta f(x^*)$$.  

Therefore, Proximal Gradient Algorithm is just a series of iteritive updating step to find the fixed point: $$x^* = prox_f(x^*)$$  
**application**：  


## Algorithm  
$$x^k = prox_{t_k, h} (y - t_k \delta g(y))$$  
$$y = x^k + \frac{t_k - 1}{t_{k + 1}} (x^k - x^{k-1})$$  

## Theory
### Proximal Operator
if f is a convex function taking on finite values, then the proximal operator on f is :  
$$prox_f(v) = argmin_x(f(x) + \frac{1}{2} \lVert x-v \rVert^2_2)$$  
* the proximal operator of the scaled function $$\lambda f$$, where $$\lambda > 0$$, which can be expressed as  
$$prox_{\lambda f}(\nu) = argmin_x (f(x) + 1/({2\lambda}) \lVert x - \nu \rVert_2^2$$  
the parameter $$\lambda $$ control the extent to which the proximal operator maps points towards the minimum of $$f$$, with larger values of $$\lambda $$ associated with mapped points near the minimum, and smaller values giving a smaller movement towards the minimum.
<figure>
    <a href="https://thumbnail10.baidupcs.com/thumbnail/e154a9fcb76501d38c2c883fa7bb96a4?fid=1483288374-250528-257889517280387&rt=pr&sign=FDTAER-DCb740ccc5511e5e8fedcff06b081203-tDcsLsN5N14Rm2Ed0o93ymYgSlA%3d&expires=8h&chkbd=0&chkv=0&dp-logid=577735282787323296&dp-callid=0&time=1548421200&size=c10000_u10000&quality=90&vuk=1483288374&ft=image"><img src="https://thumbnail10.baidupcs.com/thumbnail/e154a9fcb76501d38c2c883fa7bb96a4?fid=1483288374-250528-257889517280387&rt=pr&sign=FDTAER-DCb740ccc5511e5e8fedcff06b081203-tDcsLsN5N14Rm2Ed0o93ymYgSlA%3d&expires=8h&chkbd=0&chkv=0&dp-logid=577735282787323296&dp-callid=0&time=1548421200&size=c10000_u10000&quality=90&vuk=1483288374&ft=image"></a>
    <figcaption><a href="https://thumbnail10.baidupcs.com/thumbnail/e154a9fcb76501d38c2c883fa7bb96a4?fid=1483288374-250528-257889517280387&rt=pr&sign=FDTAER-DCb740ccc5511e5e8fedcff06b081203-tDcsLsN5N14Rm2Ed0o93ymYgSlA%3d&expires=8h&chkbd=0&chkv=0&dp-logid=577735282787323296&dp-callid=0&time=1548421200&size=c10000_u10000&quality=90&vuk=1483288374&ft=image" title="Proximal Gradient Method">Explain The Algorithm</a>.</figcaption>
</figure>

也就是说， 如果是在定义域里面的点， proximal operator是使该点向最小化方向走，对于不在定义域的点， 会使该点向最小化且是在定义域内的方向走。  

### 算法的收敛性分析：
Proximal Operator具有Nonexpansive Operator的性质，即：  
$$\lVert prox_f(x) - prox_f(y) \rVert_2^2 \le (x - y)^T (prox_f(x) - prox_f(y))$$; where $$x, y \in R^n$$.  
可以证明的是如果N是nonexpansive， 那么operator：$$T = (1-\alpha)I + \alpha N$$, where $$\alpha \in (0, 1 )$$， has fixed point. 因为N和T的迭代会收敛到fixed point T. i.e. $$x^{k+1} = (1-\alpha) x^{k} + \alpha N(x^k)$$ will converge to a fixed point of N.
