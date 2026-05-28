# Recurrent Neural Networks

Recurrent neural networks are neural networks for sequence data. Instead of processing each input independently, an RNN carries a hidden state from one time step to the next.

At time $t$, the network receives the current input $x_t$ and the previous hidden state $h_{t-1}$, then produces a new hidden state $h_t$:

$$
h_t = F(x_t, h_{t-1})
$$

The same transition function $F$ is reused at each step. This makes an RNN equivalent to a chain of repeated copies of one neural module, with each copy passing information forward through the sequence.

See [[rnn-forward-propagation]] for the standard forward-propagation equations and shared-parameter notation.

See [[backpropagation-through-time]] for how gradients are computed through the unrolled recurrent graph.

See [[rnn-architecture-types]] for one-to-many, many-to-one, and many-to-many RNN layouts.

See [[conditional-language-models]], [[sequence-decoding-search]], and [[beam-search]] for sequence-to-sequence inference and translation-style decoding.

## Why Recurrence Matters

Many tasks need context from earlier positions:

- language modeling uses earlier words to predict later words
- speech recognition uses nearby and prior acoustic frames
- translation and captioning depend on previous sequence information
- time-series models use earlier observations to interpret later observations

The hidden state is the mechanism that lets information persist across positions.

## Recurrent Versus Recursive

Recurrent neural networks are different from [[recursive-neural-networks|recursive neural networks]].

An RNN reuses a cell along a sequence:

$$
h_t = F(x_t, h_{t-1})
$$

A recursive neural network reuses a cell across a tree or hierarchy:

$$
h_{\text{parent}} = F(h_{\text{left}}, h_{\text{right}})
$$

Both share parameters across repeated structure, but RNNs usually operate on chains while recursive neural networks operate on trees.

## Why Not Use a Standard Feedforward Network?

A standard feedforward network can be forced onto a sequence task by concatenating a fixed number of input positions. For example, a name-recognition model could take a fixed-length sentence as many one-hot word vectors and output one label per position.

This has several problems.

First, sequence examples can have different input and output lengths. Padding every sequence to a maximum length is possible, but it is an awkward representation and can waste computation.

Second, a feedforward network over fixed positions does not automatically share features across positions. If it learns that "Harry" at position $1$ is evidence for a person name, that does not automatically imply the same feature will be reused at position $t$.

Third, concatenating many high-dimensional one-hot vectors creates a very large input layer. If each word is a $10{,}000$-dimensional one-hot vector, the first weight matrix can become enormous when many positions are concatenated.

RNNs address these issues by reusing the same transition parameters at each time step and carrying an activation forward through the sequence.

## Basic Computation

A basic RNN usually starts with an initial activation:

$$
a_0 = \vec{0}
$$

Then each step computes an activation from the previous activation and current input:

$$
a_t = g_a(W_{aa}a_{t-1} + W_{ax}x_t + b_a)
$$

and an output prediction:

$$
\hat{y}_t = g_y(W_{ya}a_t + b_y)
$$

The activation function $g_a$ is often $\tanh$ in a basic RNN. The output activation $g_y$ depends on the task, such as sigmoid for binary labels or softmax for multi-class labels.

## Input and Output Lengths

RNNs are not limited to cases where the input length $T_x$ equals the output length $T_y$.

Common architecture patterns include:

- many-to-many with $T_x = T_y$, such as named entity recognition
- many-to-one, such as sentiment classification
- one-to-many, such as music or text generation
- many-to-many with $T_x \neq T_y$, such as machine translation with an encoder-decoder architecture

See [[rnn-architecture-types]] for the full map.

## Gated RNN Cells

Basic RNN cells can struggle to preserve information across long gaps. Gated cells add explicit mechanisms for deciding what to keep and what to update.

Two common gated RNN cells are:

- [[gated-recurrent-units|gated recurrent units]], which use update and reset gates
- [[lstm-networks|LSTM networks]], which use cell state plus input, forget, and output gates

## Limitations

Simple RNNs can use nearby context, but they have several important limitations.

### Long-Term Dependency Problem

Simple RNNs often struggle when the relevant signal is many steps back. During training, gradients must pass through many repeated transformations, which can make credit assignment difficult across long gaps.

This is the motivation for [[lstm-networks|LSTM networks]]. LSTMs add an explicit cell state and gates so that information can be preserved, overwritten, or exposed more selectively than in a simple RNN.

### Vanishing and Exploding Gradients

Training an RNN through time is like training a very deep network whose layers share parameters. The gradient repeatedly passes through the recurrent transition.

If the repeated transformations shrink gradients, earlier time steps receive a weak learning signal. This is the vanishing-gradient case.

If the repeated transformations amplify gradients, updates can become unstable. This is the exploding-gradient case. Gradient clipping can help with exploding gradients, but it does not solve the deeper problem of learning useful long-range memory.

### Hidden-State Bottleneck

A simple RNN compresses all past information into one hidden state vector. For short sequences this can be useful; for long or information-dense sequences it becomes a bottleneck.

The model must decide what to preserve and what to overwrite at every step. If useful information is overwritten early, later steps cannot recover it from the hidden state alone.

### Sequential Computation

RNN states depend on previous states:

$$
h_t = F(x_t, h_{t-1})
$$

This dependency makes the forward pass inherently sequential across time. Later positions cannot be computed until earlier states are available. That limits parallelism over sequence length compared with architectures that process many positions at once.

### Backpropagation Through Time Cost

To train across a long sequence, backpropagation through time must retain or recompute intermediate states across many steps. This increases memory and compute cost.

In practice, training often uses truncated backpropagation through time, which backpropagates through only a limited window. Truncation makes training cheaper, but it also limits how directly the loss can assign credit to events far outside the window.

### Directional Context

A standard left-to-right RNN only uses past context when computing $h_t$. It does not see future inputs unless the architecture is modified, for example with a bidirectional RNN.

Bidirectional RNNs can use both past and future context for fixed input sequences, but they are not suitable when predictions must be made online before future tokens or frames are available.

## Related

- [[lstm-networks]]
- [[rnn-forward-propagation]]
- [[backpropagation-through-time]]
- [[rnn-architecture-types]]
- [[conditional-language-models]]
- [[sequence-decoding-search]]
- [[beam-search]]
- [[recursive-neural-networks]]
- [[gated-recurrent-units]]
- [[lstm-gates-and-cell-state]]
- [[lstm-variants]]
- [[../activations/sigmoid-activation|sigmoid-activation]]
- [[../activations/tanh-activation|tanh-activation]]

## Sources

- [[../../../raw/articles/colah/understanding-lstm-networks|Understanding LSTM Networks]] by Christopher Olah, 2015.
- [[../../../raw/courses/coursera/sequence-models/recurrent-neural-networks-video-transcript|Coursera Sequence Models: Recurrent Neural Networks Video Transcript]]
- [[../../../raw/courses/coursera/sequence-models/backpropagation-through-time-video-transcript|Coursera Sequence Models: Backpropagation Through Time Video Transcript]]
- [[../../../raw/courses/coursera/sequence-models/types-of-rnn-video-transcript|Coursera Sequence Models: Types of RNN Video Transcript]]
- [[../../../raw/courses/coursera/sequence-models/gated-recurrent-units-video-transcript|Coursera Sequence Models: Gated Recurrent Units Video Transcript]]
- [[../../../raw/courses/coursera/sequence-models/conditional-language-modeling-and-search-video-transcript|Coursera Sequence Models: Conditional Language Modeling and Search Video Transcript]]
- [[../../../raw/courses/coursera/sequence-models/beam-search-video-transcript|Coursera Sequence Models: Beam Search Video Transcript]]
- [Deep Learning Book: Chapter 10, Sequence Modeling: Recurrent and Recursive Nets](https://www.deeplearningbook.org/contents/rnn.html)
- [Dive into Deep Learning: Modern Recurrent Neural Networks](https://www.d2l.ai/chapter_recurrent-modern/index.html)
