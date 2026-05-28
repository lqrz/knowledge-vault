# Optimization Algorithms

Optimization algorithms update neural-network parameters to reduce a training objective. Basic [[../losses/loss-functions|loss functions]] produce scalar objectives, [[../recurrent/backpropagation-through-time|backpropagation]] computes gradients, and the optimizer decides how to turn those gradients into parameter updates.

[[gradient-descent|Gradient descent]] is the baseline first-order optimizer. [[stochastic-gradient-descent|Stochastic gradient descent]] estimates the gradient from mini-batches. [[gradient-descent-with-momentum|Gradient descent with momentum]] extends it by using a decayed running average of recent gradients as the update direction. [[rmsprop|RMSProp]] instead keeps a decayed running average of squared gradients to adapt each parameter's effective step size.

In deep learning, many optimizers are built from moving summaries of noisy training signals rather than using only the current mini-batch gradient. [[exponential-moving-averages|Exponential moving averages]] are one such building block.

## Why Moving Averages Matter

Mini-batch gradients are noisy estimates of the full training-set gradient. A moving average can smooth that noise by combining the current observation with a persistent running state. The tradeoff is latency: stronger smoothing reacts more slowly when the underlying signal changes.

This smoothing-versus-responsiveness tradeoff appears in optimization methods that maintain running statistics of gradients, squared gradients, or parameter values.

## Adaptive Scaling

Some optimizers use moving statistics to rescale the update per parameter dimension. [[rmsprop|RMSProp]] tracks an exponential moving average of squared gradients, then divides each current gradient component by the root of that running average.

This differs from plain [[stochastic-gradient-descent|SGD]], where every dimension shares the same global learning rate and only the raw gradient component changes the coordinate update.

## Efficiency

Exponential moving averages are common in optimization partly because they are cheap. They require one running value per tracked quantity and one recurrence update per step. That matters when an optimizer tracks statistics for many model parameters.

A fixed-window moving average can be more exact for a chosen window size, but it requires storing the window and repeatedly aggregating it. EMA trades that exact window interpretation for constant-memory streaming updates.

## Related

- [[gradient-descent]]
- [[stochastic-gradient-descent]]
- [[gradient-descent-with-momentum]]
- [[rmsprop]]
- [[exponential-moving-averages]]
- [[bias-correction-for-exponential-moving-averages]]
- [[../losses/loss-functions|loss-functions]]
- [[../recurrent/backpropagation-through-time|backpropagation-through-time]]

## Sources

- [[../../../raw/courses/coursera/improving-deep-neural-networks/exponentially-weighted-averages-video-transcript|Coursera Improving Deep Neural Networks: Exponentially Weighted Averages Video Transcript]]
- [[../../../raw/courses/coursera/improving-deep-neural-networks/understanding-exponentially-weighted-averages-video-transcript|Coursera Improving Deep Neural Networks: Understanding Exponentially Weighted Averages Video Transcript]]
- [Deep Learning, Chapter 8: Optimization for Training Deep Models](https://www.deeplearningbook.org/contents/optimization.html) by Ian Goodfellow, Yoshua Bengio, and Aaron Courville, 2016.
- [Lecture 6.5-rmsprop: Divide the Gradient by a Running Average of Its Recent Magnitude](https://www.scirp.org/reference/referencespapers?referenceid=1911091) by T. Tieleman and G. Hinton, 2012.
