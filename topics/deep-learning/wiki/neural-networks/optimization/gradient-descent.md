# Gradient Descent

Gradient descent is a first-order optimization algorithm that updates parameters in the direction that most rapidly decreases the objective according to the local gradient.

For model parameters $\theta$, objective function $J(\theta)$, and learning rate $\alpha$:

$$
\theta_t = \theta_{t-1} - \alpha \nabla_\theta J(\theta_{t-1})
$$

The gradient $\nabla_\theta J(\theta)$ points in the **direction of steepest local increase**, so subtracting it moves parameters in a local descent direction.

## Contribution

Gradient descent provides the basic training rule behind neural-network optimization:

- It turns differentiable [[../losses/loss-functions|loss functions]] into parameter updates.
- It works with [[../recurrent/backpropagation-through-time|backpropagation]], which computes the gradients needed for each parameter.
- It scales to high-dimensional parameter vectors because it **only needs first-order derivatives**, not a full Hessian matrix.
- Its stochastic and mini-batch variants make deep learning practical by estimating gradients from subsets of training data rather than the whole dataset every step.

In the optimization family, gradient descent is the baseline method that later algorithms modify. [[gradient-descent-with-momentum|Gradient descent with momentum]], RMSProp, Adam, and other optimizers can be understood as changing how the update direction or step size is computed from recent gradients.

[[stochastic-gradient-descent|Stochastic gradient descent]] keeps the same per-parameter update form but estimates the gradient from one example or a mini-batch.

## Limitations

Gradient descent is local. The update uses the gradient at the current point, so it does not directly know about the global shape of the loss surface.

Important limitations include:

- The learning rate $\alpha$ is sensitive. Too large can overshoot or diverge; too small can make training very slow.
- It can move inefficiently through narrow valleys or ill-conditioned curvature, oscillating across steep directions while making slow progress along shallow directions.
- It can stall or slow down in flat regions, saddle regions, or areas with very small gradients.
- Full-batch gradient descent can be expensive because each update requires a gradient over the whole training set.
- Mini-batch or stochastic gradient descent is cheaper per step but introduces gradient noise.

These limitations motivate momentum and adaptive optimizers, which try to use information from previous gradients to smooth, accelerate, or rescale updates.

## Related

- [[optimization-algorithms]]
- [[stochastic-gradient-descent]]
- [[gradient-descent-with-momentum]]
- [[exponential-moving-averages]]
- [[../losses/loss-functions|loss-functions]]
- [[../recurrent/backpropagation-through-time|backpropagation-through-time]]

## Sources

- [Deep Learning, Chapter 8: Optimization for Training Deep Models](https://www.deeplearningbook.org/contents/optimization.html) by Ian Goodfellow, Yoshua Bengio, and Aaron Courville, 2016.
- [An overview of gradient descent optimization algorithms](https://arxiv.org/abs/1609.04747) by Sebastian Ruder, 2016.
