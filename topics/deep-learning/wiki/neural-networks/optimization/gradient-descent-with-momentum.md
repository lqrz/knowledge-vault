# Gradient Descent With Momentum

Gradient descent with momentum modifies [[gradient-descent|gradient descent]] by accumulating a **running velocity from past gradients**. Instead of updating parameters from only the current gradient, the optimizer keeps moving in directions that have been consistently useful.

One common form is:

$$
v_t = \beta v_{t-1} + (1 - \beta)\nabla_\theta J(\theta_{t-1})
$$

$$
\theta_t = \theta_{t-1} - \alpha v_t
$$

where $v_t$ is an [[exponential-moving-averages|exponential moving average]] of recent gradients, $\beta$ controls the momentum memory, and $\alpha$ is the learning rate.

Some presentations omit the $(1 - \beta)$ factor and absorb that scaling into the learning rate. The core idea is the same: the update direction is a decayed accumulation of past gradients.

For neural-network weights and biases, the Coursera notation writes this as:

$$
v_{dW,t} = \beta v_{dW,t-1} + (1 - \beta)dW_t
$$

$$
v_{db,t} = \beta v_{db,t-1} + (1 - \beta)db_t
$$

$$
W_t = W_{t-1} - \alpha v_{dW,t}
$$

$$
b_t = b_{t-1} - \alpha v_{db,t}
$$

The velocity tensors are initialized to zero with the same shapes as their matching gradients: $v_{dW}$ has the same shape as $dW$ and $W$, while $v_{db}$ has the same shape as $db$ and $b$.

## Contribution

Momentum **contributes memory to gradient descent**.

This helps because noisy or curved loss landscapes often produce gradients that point partly in useful directions and partly in directions that cancel over time. **Momentum reinforces directions that remain consistent across steps and damps directions that alternate sign.**

Practical benefits include:

- Faster progress along long, shallow directions when gradients repeatedly point the same way.
- Reduced oscillation across steep valley walls because opposing gradient components cancel in the velocity.
- Smoother updates under mini-batch gradient noise.
- A bridge from plain gradient descent to later optimizers that maintain moving statistics, such as Adam.

Historically, this idea is closely related to Polyak's heavy-ball method, which interprets optimization as a particle moving through a landscape with inertia.

## Oscillation Damping

Momentum is especially useful when the loss surface has narrow, elongated contours.

Plain [[gradient-descent|gradient descent]] can bounce across the steep direction of the valley while making slow progress along the shallow direction. The oscillating gradient components tend to alternate sign, so their moving average shrinks toward zero. Gradient components that consistently point toward the minimum reinforce each other, so their moving average remains large.

This means momentum can slow movement in the oscillating direction while preserving faster movement along the useful direction.

## Hyperparameters and Bias Correction

Momentum adds $\beta$ in addition to the learning rate $\alpha$. A common default is:

$$
\beta = 0.9
$$

Using the [[exponential-moving-averages#Effective Window Size|effective-window-size heuristic]], this averages roughly the last $10$ gradient observations.

The EMA starts biased toward zero because the velocity is initialized at zero. In principle, the velocity could be bias-corrected by dividing by $1 - \beta^t$. In the Coursera transcript, this correction is described as uncommon for momentum because the moving average warms up after the first few iterations.

## Scaling Convention

Some sources use:

$$
v_t = \beta v_{t-1} + g_t
$$

instead of:

$$
v_t = \beta v_{t-1} + (1 - \beta)g_t
$$

Both can work, but they scale $v_t$ differently. Omitting $(1 - \beta)$ makes the velocity roughly larger by a factor of $\frac{1}{1 - \beta}$, so the best learning rate $\alpha$ changes. Keeping $(1 - \beta)$ makes the velocity easier to interpret as an exponential moving average of gradients.

## Limitations

Momentum **adds another hyperparameter**, $\beta$, and makes the learning-rate choice more coupled with the optimizer dynamics.

Important limitations include:

- Too much momentum can overshoot minima or create unstable oscillations.
- Momentum can continue moving in a stale direction after the local gradient has changed.
- It **does not adapt learning rates per parameter**; all parameters still share the same base step-size scheme unless combined with another method.
- It does not remove the need for careful learning-rate tuning.
- On highly nonstationary objectives or very noisy gradients, the running velocity can lag behind the current useful direction.
- Different scaling conventions for the velocity can require retuning $\alpha$ when $\beta$ changes.

Momentum is therefore a strong default improvement over plain gradient descent in many neural-network settings, but it is still a first-order method with tuning and curvature limitations.

## Related

- [[gradient-descent]]
- [[rmsprop]]
- [[exponential-moving-averages]]
- [[bias-correction-for-exponential-moving-averages]]
- [[optimization-algorithms]]

## Sources

- [Some methods of speeding up the convergence of iteration methods](https://www.mathnet.ru/eng/zvmmf7713) by B. T. Polyak, 1964.
- [Deep Learning, Chapter 8: Optimization for Training Deep Models](https://www.deeplearningbook.org/contents/optimization.html) by Ian Goodfellow, Yoshua Bengio, and Aaron Courville, 2016.
- [[../../../raw/courses/coursera/improving-deep-neural-networks/exponentially-weighted-averages-video-transcript|Coursera Improving Deep Neural Networks: Exponentially Weighted Averages Video Transcript]]
- [[../../../raw/courses/coursera/improving-deep-neural-networks/understanding-exponentially-weighted-averages-video-transcript|Coursera Improving Deep Neural Networks: Understanding Exponentially Weighted Averages Video Transcript]]
- [[../../../raw/courses/coursera/improving-deep-neural-networks/gradient-descent-with-momentum-video-transcript|Coursera Improving Deep Neural Networks: Gradient Descent With Momentum Video Transcript]]
