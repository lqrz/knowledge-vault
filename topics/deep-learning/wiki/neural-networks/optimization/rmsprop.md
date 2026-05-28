# RMSProp

RMSProp is an adaptive first-order optimizer that rescales each parameter's gradient by a running estimate of that parameter's recent squared-gradient magnitude.

It keeps an [[exponential-moving-averages|exponential moving average]] of squared gradients:

$$
s_t = \beta s_{t-1} + (1 - \beta)g_t^2
$$

Then it updates parameters with:

$$
\theta_t = \theta_{t-1} - \alpha \frac{g_t}{\sqrt{s_t} + \epsilon}
$$

where $g_t = \nabla_\theta J_{B_t}(\theta_{t-1})$, $s_t$ has one component per parameter, $\alpha$ is the base learning rate, $\beta$ controls the squared-gradient moving average, and $\epsilon$ is a small constant for numerical stability.

The square, square root, and division are elementwise.

## Contribution

RMSProp contributes **per-parameter adaptive scaling**.

Plain [[stochastic-gradient-descent|SGD]] uses the same learning rate for every parameter dimension:

$$
\theta_{t,i} = \theta_{t-1,i} - \alpha g_{t,i}
$$

RMSProp instead divides each coordinate's gradient by the root mean square of its recent gradient magnitudes:

$$
\theta_{t,i} = \theta_{t-1,i} - \alpha \frac{g_{t,i}}{\sqrt{s_{t,i}} + \epsilon}
$$

This means:

- dimensions with consistently large gradients get smaller effective steps
- dimensions with consistently small gradients get relatively larger effective steps
- oscillations in steep directions are damped because large squared gradients increase the denominator
- the optimizer can move more evenly across directions with different curvature or gradient scale

RMSProp can be viewed as a response to the problem that a single global learning rate is often poorly matched to every dimension of a neural network's parameter space.

## Why Shrink Large Gradients?

A large gradient can mean two different things:

- the parameter has a useful strong signal and should move substantially
- the coordinate is steep or poorly scaled, so a large raw step may overshoot or oscillate

RMSProp treats persistently large gradient magnitudes as evidence for the second case. If one coordinate repeatedly has large gradients, then $s_{t,i}$ grows and the denominator $\sqrt{s_{t,i}} + \epsilon$ shrinks that coordinate's effective step.

For example, if $g_i = 100$ and $s_{t,i} \approx 10000$, then:

$$
\frac{g_i}{\sqrt{s_{t,i}}} \approx \frac{100}{100} = 1
$$

If another coordinate has $g_j = 1$ and $s_{t,j} \approx 1$, then:

$$
\frac{g_j}{\sqrt{s_{t,j}}} \approx \frac{1}{1} = 1
$$

So RMSProp normalizes coordinates by their typical gradient scale. It still moves in the gradient direction, but it trusts direction more than raw magnitude.

## Relation to Momentum

[[gradient-descent-with-momentum|Momentum]] averages gradients and uses that average as a velocity-like update direction.

RMSProp averages **squared gradients** and uses that average to normalize the current gradient. It changes the effective step size per parameter, but the basic RMSProp update does not by itself maintain a momentum velocity.

The short distinction is:

- Momentum remembers the **direction** of recent gradients.
- RMSProp remembers the **size** of recent gradients.

Momentum answers: which direction have gradients consistently pointed? RMSProp answers: how large are gradients usually in this parameter dimension?

Adam combines both ideas: momentum-like moving averages of gradients and RMSProp-like moving averages of squared gradients.

## Hyperparameters

Common hyperparameters are:

- $\alpha$: base learning rate
- $\beta$: decay rate for the squared-gradient average
- $\epsilon$: small stabilizer, often around $10^{-8}$ in modern implementations

RMSProp examples often use $\beta$ values such as $0.9$. The best value depends on the implementation and task.

## Limitations

RMSProp is useful but not automatic.

Important limitations include:

- It still requires learning-rate tuning.
- It can take very large steps when $s_{t,i}$ is very small, so $\epsilon$ and $\alpha$ matter.
- Its per-parameter scaling is based on recent gradient magnitudes, not a full curvature model.
- It does not directly use momentum unless combined with a momentum term.
- Like other adaptive optimizers, it may generalize differently from SGD with momentum, so validation performance matters.

## Related

- [[stochastic-gradient-descent]]
- [[gradient-descent]]
- [[gradient-descent-with-momentum]]
- [[exponential-moving-averages]]
- [[optimization-algorithms]]

## Sources

- [Lecture 6.5-rmsprop: Divide the Gradient by a Running Average of Its Recent Magnitude](https://www.scirp.org/reference/referencespapers?referenceid=1911091) by T. Tieleman and G. Hinton, 2012.
- [Deep Learning, Chapter 8: Optimization for Training Deep Models](https://www.deeplearningbook.org/contents/optimization.html) by Ian Goodfellow, Yoshua Bengio, and Aaron Courville, 2016.
- [An overview of gradient descent optimization algorithms](https://arxiv.org/abs/1609.04747) by Sebastian Ruder, 2016.
