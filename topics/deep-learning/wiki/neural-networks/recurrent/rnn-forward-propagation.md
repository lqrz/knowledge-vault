# RNN Forward Propagation

Forward propagation in a basic [[recurrent-neural-networks|recurrent neural network]] scans a sequence one step at a time. Each step receives the current input and the previous activation.

For an input sequence $x_1, x_2, \ldots, x_{T_x}$, the initial activation is commonly set to the zero vector:

$$
a_0 = \vec{0}
$$

At time step $t$, the RNN computes:

$$
a_t = g_a(W_{aa}a_{t-1} + W_{ax}x_t + b_a)
$$

and then predicts:

$$
\hat{y}_t = g_y(W_{ya}a_t + b_y)
$$

The hidden activation $a_t$ is the recurrent state passed to the next time step. The output activation $g_y$ depends on the task: binary sequence labeling can use sigmoid, while multi-class prediction can use softmax.

## Shared Parameters

The same parameter matrices are reused at every time step:

- $W_{ax}$ maps the current input $x_t$ into the activation computation.
- $W_{aa}$ maps the previous activation $a_{t-1}$ into the next activation.
- $W_{ya}$ maps the current activation $a_t$ into the output prediction.

This parameter sharing is one reason RNNs can handle variable-length sequences more naturally than a fixed-size feedforward network. It also lets a feature learned at one position be reused at other positions.

For example, if a name-recognition model learns that "Harry" is evidence for a person name at position $1$, the same input-to-hidden parameters can help recognize "Harry" at later positions.

## Compact Matrix Form

The recurrent update is often written by concatenating the previous activation and current input:

$$
a_t = g_a(W_a[a_{t-1}, x_t] + b_a)
$$

where:

$$
W_a = [W_{aa} \; W_{ax}]
$$

and:

$$
[a_{t-1}, x_t]
$$

means stacking the two vectors into one longer vector.

This compact form is equivalent because:

$$
W_a[a_{t-1}, x_t] = W_{aa}a_{t-1} + W_{ax}x_t
$$

The output equation can similarly be written:

$$
\hat{y}_t = g_y(W_y a_t + b_y)
$$

## Unrolled View

RNNs are often drawn in two equivalent ways:

- an unrolled chain, with one copy of the recurrent cell per time step
- a compact loop, where the recurrent connection feeds the activation back into the same cell after one time delay

The unrolled view makes the computation easier to read: the model processes $x_1$, then $x_2$, then $x_3$, carrying activations forward through the sequence.

## Related

- [[recurrent-neural-networks]]
- [[backpropagation-through-time]]
- [[rnn-architecture-types]]
- [[lstm-networks]]
- [[lstm-gates-and-cell-state]]
- [[../activations/tanh-activation|tanh-activation]]
- [[../activations/sigmoid-activation|sigmoid-activation]]

## Sources

- [[../../../raw/courses/coursera/sequence-models/recurrent-neural-networks-video-transcript|Coursera Sequence Models: Recurrent Neural Networks Video Transcript]]
