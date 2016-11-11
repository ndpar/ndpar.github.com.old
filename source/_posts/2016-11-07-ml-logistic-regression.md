---
layout:     post
title:      "Machine Learning: Logistic Regression"
date:       2016-11-07 21:05:31
categories: [Machine Learning, Math, MATLAB]
tags:       [machine learning, math, matlab]
published:  true
---

_Logistic regression_ is a classification case of [linear regression][1] whith
dependent variable $y$ taking binary values.

Problem: Given a training set $\langle x^{(i)}, y^{(i)} \rangle$, $1 \le i \le m$,
$x \in \mathbb{R}^{n+1}$, $x^{(i)} _ 0 = 0$, $y^{(i)} \in $ {0,1},
find _classification function_

$$
h_\theta(x) = P(y = 1 | x; \theta)
$$

<!-- more -->

## Gradient Descent

Let's build function $h_\theta(x)$ as a sigmoid function of $\theta\cdot x$

$$
h_\theta(x) = g(\theta\cdot x), \quad
g(z) = \mathbb{sigmoid}(z) = \frac{1}{1 + e^{-z}}
$$

Sigmoid function has rank infinity, i.e. it operates on scalars, vectors and matrices.

{% codeblock sigmoid.m lang:matlab %}
function g = sigmoid(z)
    g = 1 ./ (1 + exp(-z));
end
{% endcodeblock %}

To find optimal parameter $\theta \in \mathbb{R}^{n+1}$ we are going to use
optimized gradient descent method which takes as arguments
cost function $J(\theta)$ and its gradient.
For logistic regression they are

$$
J(\theta) = -\frac{1}{m} \left( y^T \ln h_\theta(X) + (1-y)^T \ln (1-h_\theta(X)) \right) \\
\nabla J(\theta) = \frac{1}{m} X^T (h_\theta(X) - y)
$$

where $X = (x^{(i)}_j) _{m \times n+1}$ is a matrix of the training examples from
the [previous lecture][1].

Analogous to linear regression, logistic regression can be regularized too

$$
J(\theta) = -\frac{1}{m} \left( y^T \ln h_\theta(X) + (1-y)^T \ln (1-h_\theta(X)) \right) + \frac{\lambda}{2m} \| E\theta \|^2 \\
\nabla J(\theta) = \frac{1}{m} \left( X^T (h_\theta(X) - y) + \lambda E\theta \right)
$$

{% codeblock costFunction.m lang:matlab %}
function [J, grad] = costFunction(theta, X, y, lambda)
    m = length(y); % number of training examples
    h = sigmoid(X * theta);
    J = (y' * log(h) + (1 - y)' * log(1 - h)) / -m;
    grad = X' * (h - y) / m;

    % Regularization
    th = theta; th(1) = 0;

    J = J + th' * th * lambda / m / 2;
    grad = grad + th * lambda / m;
end
{% endcodeblock %}

Having computed $\theta$ we can now implement the prediction function

{% codeblock predict.m lang:matlab %}
function p = predict(theta, X)
    p = sigmoid(X * theta) >= 0.5;
end
{% endcodeblock %}

which can be used to classify new examples and check the prediction accuracy
on the training set

{% codeblock accuracy.m lang:matlab %}
function a = accuracy(theta, X, y)
    p = predict(theta, X);
    a = mean(double(p == y)) * 100;
end
{% endcodeblock %}

## Multi-class Classification

Logistic regression works for binary $y$.
Suppose now that $y^{(i)} \in ${$1,...,K$}, where $K > 2$.
In this case we can use _One-vs-All_ variation of this algorithm.

Step 1. Convert vector $y$ into a binary matrix $Y$

$$
y =
\begin{pmatrix}
y^{(1)} \\
y^{(2)} \\
y^{(3)} \\
\vdots \\
y^{(m)} \\
\end{pmatrix}
\quad
\rightarrow
\quad
Y =
\begin{pmatrix}
y^{(1)}_1 & y^{(1)}_2 & \cdots & y^{(1)}_K \\
y^{(2)}_1 & y^{(2)}_2 & \cdots & y^{(2)}_K \\
y^{(3)}_1 & y^{(3)}_2 & \cdots & y^{(3)}_K \\
\vdots & \vdots & \ddots & \vdots \\
y^{(m)}_1 & y^{(m)}_2 & \cdots & y^{(m)}_K \\
\end{pmatrix}
$$

where $y^{(i)}_k = \delta _{k y^{(i)}}$, i.e. $y^{(i)}_k = 1$ when $y^{(i)} = k$, otherwise $y^{(i)}_k = 0$.

Step 2. Train logistic classifier on every column of matrix $Y$.
The result will be a matrix $\Theta = (\theta_{jk})_{n+1 \times K}$

$$
\Theta =
\begin{pmatrix}
\theta_{01} & \theta_{02} & \cdots & \theta_{0K} \\
\theta_{11} & \theta_{12} & \cdots & \theta_{1K} \\
\theta_{21} & \theta_{22} & \cdots & \theta_{2K} \\
\vdots & \vdots & \ddots & \vdots \\
\theta_{n1} & \theta_{n2} & \cdots & \theta_{nK} \\
\end{pmatrix}
$$

Step 3. For any given vector $x$ compute vector $h = x^T \Theta$. Then
the predicted value $y$ will be

$$
y = \{ p \: | \: h_p = \mathbb{max} (h_k), 1 \le k \le K \}
$$

To compute accuracy of the one-vs-all classifier on the training set
use `accuracy.m` script from above with modified `predict.m`

{% codeblock predict.m lang:matlab %}
function p = predict(Theta, X)
    a = sigmoid(X * Theta);
    [v, p] = max(a, [], 2);
end
{% endcodeblock %}

[1]: /2016/10/28/ml-linear-regression
