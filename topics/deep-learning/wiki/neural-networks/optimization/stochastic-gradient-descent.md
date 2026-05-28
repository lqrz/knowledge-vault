# Stochastic Gradient Descent

Stochastic gradient descent, or SGD, is a variant of [[gradient-descent|gradient descent]] that estimates the gradient **from one training example or a mini-batch instead of the full training set**.

For parameters $\theta$, mini-batch $B_t$, loss $J_B(\theta)$ on that batch, and learning rate $\alpha$:

$$
\theta_t = \theta_{t-1} - \alpha \nabla_\theta J_{B_t}(\theta_{t-1})
$$

In deep learning, "SGD" usually means mini-batch SGD rather than a single-example update.

## Per-Dimension Updates

The gradient is a vector with one component per parameter:

$$
g_t = \nabla_\theta J_{B_t}(\theta_{t-1})
$$

For each parameter dimension $i$:

$$
\theta_{t,i} = \theta_{t-1,i} - \alpha g_{t,i}
$$

Plain SGD uses the **same global learning rate $\alpha$ for every dimension**. The dimensions differ because their gradient components $g_{t,i}$ differ, not because SGD assigns each dimension its own adaptive step size.

Each gradient component can vary by:

- sign: whether the parameter is pushed up or down
- magnitude: how large the update is
- noise: how much the mini-batch estimate differs from the full-data gradient
- frequency: how often that parameter receives a nonzero or meaningful gradient

Although the arithmetic update is coordinatewise, the dimensions are not conceptually independent. The loss couples parameters through the model, so changing one parameter can change future gradients for many other parameters.

## Contribution

SGD makes large-scale neural-network training practical:

- It avoids computing a full-dataset gradient for every update.
- It provides frequent, cheap updates.
- Its gradient noise can sometimes help training move through flat or sharp local structure.
- It is the baseline optimizer that many later methods modify with [[gradient-descent-with-momentum|momentum]] or adaptive per-parameter scaling.

## Limitations

Plain SGD has no memory and no per-parameter adaptation.

Important limitations include:

- All **dimensions share one learning rate**, even if their **gradient scales differ**.
- Noisy mini-batch gradients can cause jittery updates.
- It can oscillate in steep directions and move slowly in shallow directions.
- Rarely updated parameters can learn slowly.
- Learning-rate schedules are often needed for strong performance.

These limitations motivate [[gradient-descent-with-momentum|momentum]] and adaptive optimizers such as [[rmsprop|RMSProp]] that maintain moving statistics of gradients or squared gradients.

## Related

- [[gradient-descent]]
- [[gradient-descent-with-momentum]]
- [[rmsprop]]
- [[exponential-moving-averages]]
- [[optimization-algorithms]]

## Sources

- [Deep Learning, Chapter 8: Optimization for Training Deep Models](https://www.deeplearningbook.org/contents/optimization.html) by Ian Goodfellow, Yoshua Bengio, and Aaron Courville, 2016.
- [An overview of gradient descent optimization algorithms](https://arxiv.org/abs/1609.04747) by Sebastian Ruder, 2016.
