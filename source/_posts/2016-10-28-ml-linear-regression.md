---
layout:     post
title:      "Machine Learning: Linear Regression"
date:       2016-10-28 21:05:31
categories: [Machine Learning, Math, Matlab]
tags:       [machine learning, math, matlab]
published:  true
---

Let $y$ be a dependent variable of a feature vector $x = (x_1, ..., x_n)$. We know $m$ values of $y^{(i)}$ for $m$ vectors $(x^{(i)}_1, ..., x^{(i)}_n)$, $1 \le i \le m$.

Problem: Given a vector $x^{(0)} = (x^{(0)}_1, ..., x^{(0)}_n)$ find the best value of $y^{(0)}$.

<!-- more -->

## Gradient Descent

Step 0. Normalize each feature $(x^{(0)}_j, ..., x^{(m)}_j)^T$ using mean $\mu$ and standard deviation $\sigma$.

Step 1. Prepend $1$ as a $0^{th}$ "feature" to each vector $(x^{(i)}_1, ..., x^{(i)}_n)$ and denote the result as $X$

$$
X =
\begin{pmatrix}
1 & x^{(1)}_1 & x^{(1)}_2 & \cdots & x^{(1)}_n \\
1 & x^{(2)}_1 & x^{(2)}_2 & \cdots & x^{(2)}_n \\
1 & x^{(3)}_1 & x^{(3)}_2 & \cdots & x^{(3)}_n \\
\vdots & \vdots & \vdots & \ddots & \vdots \\
1 & x^{(m)}_1 & x^{(m)}_2 & \cdots & x^{(m)}_n \\
\end{pmatrix}
\quad
y =
\begin{pmatrix}
y^{(1)} \\
y^{(2)} \\
y^{(3)} \\
\vdots \\
y^{(m)} \\
\end{pmatrix}
$$

Step 2. Find vector $\theta^{(0)} = (\theta^{(0)}_0, ..., \theta^{(0)}_n)^T$ that minimizes cost function

$$
J(\theta) = \frac{1}{2m} \|X\theta - y\|^2 = \frac{1}{2m}(X\theta - y)^T (X\theta - y)
$$

Step 3. The best value for $y^{(0)}$ will be $(1, x^{(0)})\theta^{(0)}$, where each feature in $x^{(0)}$ is normalized using the same values of $\mu$ and $\sigma$ as in Step 0.

_Gradient Descent_ method for finding (global) minimum of cost funtion $J(\theta)$: choose small learning rate $\alpha > 0$ and find the fixed point of the function

$$
f(\theta) = \theta - \frac{\alpha}{m}X^T (X\theta - y)
$$

### Matlab implementation

{% codeblock featureNormalize.m lang:matlab %}
function [X_norm, mu, sigma] = featureNormalize(X)
    mu = mean(X, 1);
    sigma = std(X, 0, 1);
    X_norm = (X - mu) ./ sigma;
end
{% endcodeblock %}

{% codeblock gradientDescent.m lang:matlab %}
function [theta] = gradientDescent(X, y, theta, alpha, num_iters)
    m = length(y);
    for iter = 1:num_iters
        h = X * theta;
        delta = h - y;
        theta = theta - X' * delta * alpha / m;
    end
end
{% endcodeblock %}

## Normal Equation

Unlike Gradient Descent this method does not require feature normalization (Step 0) and conversion loop. _Normal Equation_ gives the closed-form solution to linear regression

$$
\theta = (X^T X)^{-1} X^T y
$$

### Matlab implementation

{% codeblock normalEquation.m lang:matlab %}
function [theta] = normalEquation(X, y)
    theta = pinv(X' * X) * X' * y
end
{% endcodeblock %}
