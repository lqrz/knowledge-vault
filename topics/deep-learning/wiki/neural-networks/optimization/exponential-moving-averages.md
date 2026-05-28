# Exponential Moving Averages

An exponential moving average, also called an exponentially weighted average, is a recursive running average that gives the newest observation some weight while preserving a decayed memory of previous observations.

For a sequence of observations $\theta_t$, initialize:

$$
v_0 = 0
$$

Then update:

$$
v_t = \beta v_{t-1} + (1 - \beta)\theta_t
$$

where $\beta$ is a smoothing hyperparameter between $0$ and $1$.

## Intuition

The update has two terms:

- $\beta v_{t-1}$ keeps a fraction of the previous average.
- $(1 - \beta)\theta_t$ adds the current observation.

When $\beta$ is larger, the average keeps more history and changes slowly. When $\beta$ is smaller, the average follows the current observation more closely but becomes noisier.

The Coursera transcript illustrates this with daily temperatures. A noisy daily temperature curve can be smoothed by replacing each raw observation with a running average that blends yesterday's average with today's temperature.

## Effective Window Size

A useful approximation is that $v_t$ averages over roughly:

$$
\frac{1}{1 - \beta}
$$

recent observations.

Examples from the transcript:

- $\beta = 0.9$ behaves roughly like an average over the last $10$ observations.
- $\beta = 0.98$ behaves roughly like an average over the last $50$ observations.
- $\beta = 0.5$ behaves roughly like an average over the last $2$ observations.

This is a heuristic, not a hard cutoff. Older observations still contribute, but their weights shrink exponentially.

## Expanded Form

The recurrence can be expanded to show the weights on past observations. For example, with $\beta = 0.9$:

$$
v_{100} = 0.1\theta_{100} + 0.1(0.9)\theta_{99} + 0.1(0.9)^2\theta_{98} + 0.1(0.9)^3\theta_{97} + \cdots
$$

More generally, the observation from $k$ steps ago receives weight:

$$
(1 - \beta)\beta^k
$$

So the average is computed by multiplying recent observations by an exponentially decaying weight curve, then summing the products.

Up to the startup effect handled by [[bias-correction-for-exponential-moving-averages|bias correction]], these weights sum to approximately $1$, which is why the result behaves like an average.

## Exponential Decay Intuition

The effective-window-size rule comes from the fact that:

$$
(1 - \epsilon)^{1 / \epsilon} \approx \frac{1}{e}
$$

If $\beta = 0.9$, then $\epsilon = 0.1$, and:

$$
0.9^{10} \approx 0.35 \approx \frac{1}{e}
$$

So after about $10$ steps, an observation's weight has decayed to roughly one third of the current observation's weight. If $\beta = 0.98$, then $\epsilon = 0.02$, and:

$$
0.98^{50} \approx \frac{1}{e}
$$

This is why $\beta = 0.98$ is described as focusing on roughly the last $50$ observations. The threshold is only an intuition aid; the exponential weights never abruptly stop.

## Smoothing Versus Latency

High $\beta$ values create smoother curves because each new observation receives a smaller weight. For example, with $\beta = 0.98$, the current observation only receives weight $0.02$.

The cost is lag. If the underlying signal changes, a high-$\beta$ average adapts slowly because most of the update comes from the previous running value.

Low $\beta$ values adapt quickly because the current observation receives more weight. With $\beta = 0.5$, the current observation receives weight $0.5$, so the average reacts quickly but is more sensitive to noise and outliers.

## Implementation

The implementation does not need to store separate values $v_1, v_2, \ldots, v_t$. It can keep one running variable and overwrite it:

```text
v = 0
for theta_t in observations:
    v = beta * v + (1 - beta) * theta_t
```

This is less exact than explicitly storing a fixed-size moving window, summing the last $n$ observations, and dividing by $n$. The advantage is efficiency: an exponential moving average uses constant memory and one simple update per observation, even when tracking many variables.

## Why This Matters for Optimization

[[optimization-algorithms|Deep learning optimization algorithms]] often need stable estimates from noisy training signals. Exponential moving averages provide a compact way to track these estimates without storing a full window of past values.

This makes them a building block for faster optimization algorithms that smooth gradient-related quantities before choosing parameter updates.

For example, [[gradient-descent-with-momentum|gradient descent with momentum]] can be viewed as using an exponential moving average of recent gradients to form a velocity-like update direction. [[rmsprop|RMSProp]] uses an exponential moving average of squared gradients to rescale each parameter's current gradient.

## Related

- [[optimization-algorithms]]
- [[gradient-descent]]
- [[gradient-descent-with-momentum]]
- [[rmsprop]]
- [[bias-correction-for-exponential-moving-averages]]
- [[../losses/loss-functions|loss-functions]]
- [[../recurrent/backpropagation-through-time|backpropagation-through-time]]

## Sources

- [[../../../raw/courses/coursera/improving-deep-neural-networks/exponentially-weighted-averages-video-transcript|Coursera Improving Deep Neural Networks: Exponentially Weighted Averages Video Transcript]]
- [[../../../raw/courses/coursera/improving-deep-neural-networks/understanding-exponentially-weighted-averages-video-transcript|Coursera Improving Deep Neural Networks: Understanding Exponentially Weighted Averages Video Transcript]]
- [[../../../raw/courses/coursera/improving-deep-neural-networks/gradient-descent-with-momentum-video-transcript|Coursera Improving Deep Neural Networks: Gradient Descent With Momentum Video Transcript]]
