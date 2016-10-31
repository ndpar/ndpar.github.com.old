---
layout:     post
title:      "Machine Learning: Linear Regression"
date:       2016-10-28 21:05:31
categories: [Machine Learning, Math, MATLAB]
tags:       [machine learning, math, matlab]
published:  true
---

Let $y$ be a dependent variable of a feature vector $x$

$$
x =
\begin{pmatrix}
x_1 \\
x_2 \\
\vdots \\
x_n \\
\end{pmatrix}
$$

Problem: Given a training set $\langle x^{(i)}, y^{(i)} \rangle$, $1 \le i \le m$,
find the value of $y$ on any input vector $x$.

We solve this problem by finding a _hypothesis funciton_ $h_\theta(x)$
using one of the methods below.

<!-- more -->

## Notation

To use matrix/vector equations, let's extend vector $x$ with
a constant "feature" $x_0 = 1$ and introduce matrix $X$ and vector $y$

$$
x =
\begin{pmatrix}
x_0 \\
x_1 \\
x_2 \\
\vdots \\
x_n \\
\end{pmatrix}
=
\begin{pmatrix}
1 \\
x_1 \\
x_2 \\
\vdots \\
x_n \\
\end{pmatrix}
\quad
X =
\begin{pmatrix}
1 & x^{(1)}_1 & x^{(1)}_2 & \cdots & x^{(1)}_n \\
1 & x^{(2)}_1 & x^{(2)}_2 & \cdots & x^{(2)}_n \\
1 & x^{(3)}_1 & x^{(3)}_2 & \cdots & x^{(3)}_n \\
\vdots & \vdots & \vdots & \ddots & \vdots \\
1 & x^{(m)}_1 & x^{(m)}_2 & \cdots & x^{(m)}_n \\
\end{pmatrix}
\quad
\mathbf{y} =
\begin{pmatrix}
y^{(1)} \\
y^{(2)} \\
y^{(3)} \\
\vdots \\
y^{(m)} \\
\end{pmatrix}
$$

## Gradient Descent

Step 1. Normalize each feature $(x^{(0)}_j, ..., x^{(m)}_j)$, $1 \le j \le n$ by mean $\mu$ and standard deviation $\sigma$

$$
x^{(i)}_j \leftarrow \frac{x^{(i)}_j - \mu_j}{\sigma_j}
$$

Step 2. Find vector $\theta = (\theta_0, ..., \theta_n)^T$ that minimizes cost function

$$
J(\theta) = \frac{1}{2m} \|X\theta - y\|^2 = \frac{1}{2m}(X\theta - y)^T (X\theta - y)
$$

Step 3. Compute hypothesis function as

$$
h_\theta(x) = \theta \cdot x = x^T \theta = \theta^T x
$$

where vector $x$ is normalized using the same values of $\mu$ and $\sigma$ as in Step 1.

_Gradient Descent_ method for finding (global) minimum of cost funtion $J(\theta)$: choose small learning rate $\alpha > 0$ and find the fixed point of the function

$$
f(\theta) = \theta - \frac{\alpha}{m}X^T (X\theta - y)
$$

### MATLAB implementation

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

Unlike Gradient Descent this method does not require feature normalization (Step 1) and convergence loop. _Normal Equation_ gives the closed-form solution to linear regression

$$
\theta = (X^T X)^{-1} X^T y
$$

### MATLAB implementation

{% codeblock normalEquation.m lang:matlab %}
function [theta] = normalEquation(X, y)
    theta = pinv(X' * X) * X' * y
end
{% endcodeblock %}

## Regularization

In case of overfitting both methods can be tweaked by introducing polynomial
features and adjusting equations as follows. Let $\lambda > 0$ and

$$
E =
\begin{pmatrix}
0 & & & & \\
& 1 & & & \\
& & 1 & & \\
& & & \ddots & \\
& & & & 1 \\
\end{pmatrix}_{n+1 \times n+1}
$$

Then the cost function for gradient descent becomes

$$
J(\theta) = \frac{1}{2m} \left( \|h_\theta(X) - y\|^2 + \lambda \| E\theta \|^2 \right) \\
f(\theta) = \theta - \frac{\alpha}{m} \left( X^T (h_\theta(X) - y) + \lambda E\theta \right)
$$

and normal equation

$$
\theta = \left( X^T X +\lambda E \right)^{-1} X^T y
$$
