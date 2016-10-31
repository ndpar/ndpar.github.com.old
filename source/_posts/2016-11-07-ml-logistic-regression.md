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
$x \in \mathbb{R}^{n+1}$, $x_0 = 0$, $y \in $ {0,1},
find _classification function_

$$
h_\theta(x) = P(y = 1 | x; \theta)
$$

<!-- more -->

## Gradient Descent

Let's build function $h_\theta(x)$ as a sigmoid function of $\theta\cdot x$

$$
h_\theta(x) = g(\theta\cdot x) = \frac{1}{1 + e^{-\theta\cdot x}}
$$

To find optimal parameter $\theta \in \mathbb{R}^{n+1}$ we are going to use
built-in MATLAB function `fminunc` that implements optimized gradient descent
method with automatic selection of learning rate $\alpha$.
This function takes as parameters cost function $J(\theta)$ and its gradient.
For logistic regression they are

$$
J(\theta) = -\frac{1}{m} \left( y^T \ln h_\theta(X) + (1-y)^T \ln (1-h_\theta(X)) \right) \\
\nabla J(\theta) = \frac{1}{m} X^T (h_\theta(X) - y)
$$

where $X = (x^{(i)}_j) _{m \times n+1}$ is a matrix of training examples from
the [previous lecture][1].

### MATLAB implementation

{% codeblock sigmoid.m lang:matlab %}
function g = sigmoid(z)
    g = 1 ./ (1 + exp(-z));
end
{% endcodeblock %}

{% codeblock costFunction.m lang:matlab %}
function [J, grad] = costFunction(theta, X, y)
    m = length(y); % number of training examples
    h = sigmoid(X * theta);
    J = (y' * log(h) + (1 - y)' * log(1 - h)) / -m;
    grad = X' * (h - y) / m;
end
{% endcodeblock %}

{% codeblock regression.m lang:matlab %}
options = optimset('GradObj', 'on', 'MaxIter', 400);
[theta, cost] = fminunc(@(t)(costFunction(t, X, y)), initial_theta, options);
{% endcodeblock %}

## Regularization

Analogous to linear regression, logistic regression can be regularized too

$$
J(\theta) = -\frac{1}{m} \left( y^T \ln h_\theta(X) + (1-y)^T \ln (1-h_\theta(X)) \right) + \frac{\lambda}{2m} \| E\theta \|^2 \\
\nabla J(\theta) = \frac{1}{m} \left( X^T (h_\theta(X) - y) + \lambda E\theta \right)
$$

[1]: /2016/10/28/ml-linear-regression
