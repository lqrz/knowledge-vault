# Deep Learning

This topic covers the mathematical and conceptual foundations of deep learning, beginning with linear algebra, single neurons, and the transformations that neural networks compose.

## Map

### Linear Algebra

- [[linear-algebra/vector-spaces|vector-spaces]]
- [[linear-algebra/vector-space-basis|vector-space-basis]]
- [[linear-algebra/polynomial-vector-spaces|polynomial-vector-spaces]]
- [[linear-algebra/piecewise-linear-functions|piecewise-linear-functions]]
- [[linear-algebra/linear-transformations|linear-transformations]]
- [[linear-algebra/affine-transformations|affine-transformations]]
- [[linear-algebra/hyperplanes|hyperplanes]]

### Neural Networks

- [[neural-networks/single-neurons-and-layers|single-neurons-and-layers]]

### Recurrent Neural Networks

- [[neural-networks/recurrent/recurrent-neural-networks|recurrent-neural-networks]]
- [[neural-networks/recurrent/rnn-forward-propagation|rnn-forward-propagation]]
- [[neural-networks/recurrent/backpropagation-through-time|backpropagation-through-time]]
- [[neural-networks/recurrent/rnn-architecture-types|rnn-architecture-types]]
- [[neural-networks/recurrent/conditional-language-models|conditional-language-models]]
- [[neural-networks/recurrent/sequence-decoding-search|sequence-decoding-search]]
- [[neural-networks/recurrent/beam-search|beam-search]]
- [[neural-networks/recurrent/viterbi-algorithm|viterbi-algorithm]]
- [[neural-networks/recurrent/recursive-neural-networks|recursive-neural-networks]]
- [[neural-networks/recurrent/gated-recurrent-units|gated-recurrent-units]]
- [[neural-networks/recurrent/lstm-networks|lstm-networks]]
- [[neural-networks/recurrent/lstm-gates-and-cell-state|lstm-gates-and-cell-state]]
- [[neural-networks/recurrent/lstm-variants|lstm-variants]]

### Loss Functions

- [[neural-networks/losses/loss-functions|loss-functions]]
- [[neural-networks/losses/cross-entropy-loss|cross-entropy-loss]]
- [[neural-networks/losses/binary-cross-entropy-loss|binary-cross-entropy-loss]]

### Optimization

- [[neural-networks/optimization/optimization-algorithms|optimization-algorithms]]
- [[neural-networks/optimization/gradient-descent|gradient-descent]]
- [[neural-networks/optimization/stochastic-gradient-descent|stochastic-gradient-descent]]
- [[neural-networks/optimization/gradient-descent-with-momentum|gradient-descent-with-momentum]]
- [[neural-networks/optimization/rmsprop|rmsprop]]
- [[neural-networks/optimization/exponential-moving-averages|exponential-moving-averages]]
- [[neural-networks/optimization/bias-correction-for-exponential-moving-averages|bias-correction-for-exponential-moving-averages]]

### Activation Functions

- [[neural-networks/activations/activation-functions|activation-functions]]
- [[neural-networks/activations/activation-saturation-and-gradients|activation-saturation-and-gradients]]
- [[neural-networks/activations/relu-activation|relu-activation]]
- [[neural-networks/activations/sigmoid-activation|sigmoid-activation]]
- [[neural-networks/activations/tanh-activation|tanh-activation]]
- [[neural-networks/activations/gelu-activation|gelu-activation]]
- [[neural-networks/activations/silu-swish-activation|silu-swish-activation]]

### Maintenance

- [[log]]

## Core Concepts

- Vectors
- Vector spaces
- Linear transformations
- Affine transformations
- Matrices
- Basis vectors
- Polynomial vector spaces
- Dot products
- Hyperplanes
- Piecewise linear functions
- Single neurons
- Layers
- Activation functions
- Activation saturation
- Gradients
- Loss functions
- Cross-entropy loss
- Binary cross-entropy loss
- Optimization algorithms
- Gradient descent
- Stochastic gradient descent
- Gradient descent with momentum
- RMSProp
- Exponential moving averages
- Exponentially weighted averages
- Smoothing hyperparameters
- Bias correction
- Recurrent neural networks
- RNN forward propagation
- Backpropagation through time
- RNN architecture types
- Recursive neural networks
- Encoder-decoder RNNs
- Conditional language models
- Sequence decoding search
- Greedy search
- Beam search
- Viterbi algorithm
- Parameter sharing
- Long-term dependencies
- Gated recurrent units
- Update gate
- Reset gate
- LSTM networks
- Cell state
- Forget gate
- Input gate
- Output gate
- Gated recurrent units
- [[neural-networks/activations/relu-activation|ReLU]]
- [[neural-networks/activations/sigmoid-activation|Sigmoid]]
- [[neural-networks/activations/tanh-activation|Tanh]]
- [[neural-networks/activations/gelu-activation|GELU]]
- [[neural-networks/activations/silu-swish-activation|SiLU / Swish]]

## Learning Path

- Start with [[linear-algebra/vector-spaces|vector-spaces]] to understand vectors, closure, and why dot products are extra structure.
- Use [[linear-algebra/polynomial-vector-spaces|polynomial-vector-spaces]] to separate input dimension from vector space dimension.
- Then read [[linear-algebra/vector-space-basis|vector-space-basis]] to understand coordinates, basis vectors, spanning, and independence.
- Then read [[linear-algebra/linear-transformations|linear-transformations]] to understand how matrices act on vectors and why this matters for neural network layers.
- Read [[linear-algebra/affine-transformations|affine-transformations]] to understand how bias terms shift linear maps in neural network layers.
- Read [[linear-algebra/hyperplanes|hyperplanes]] to understand the geometry of neuron thresholds.
- Read [[linear-algebra/piecewise-linear-functions|piecewise-linear-functions]] to understand how nonlinear functions can be built from linear regions.
- Read [[neural-networks/single-neurons-and-layers|single-neurons-and-layers]] to understand how one neuron maps a vector to a scalar and how multiple neurons form a vector-valued layer.
- Then read [[neural-networks/activations/activation-functions|activation-functions]] to understand why nonlinearities prevent deep networks from collapsing into one affine transformation.
- Read [[neural-networks/activations/activation-saturation-and-gradients|activation-saturation-and-gradients]] to understand why saturated activations can weaken gradient learning.
- Read [[neural-networks/losses/loss-functions|loss-functions]] to understand how model outputs become scalar training objectives for backpropagation.
- Read [[neural-networks/losses/cross-entropy-loss|cross-entropy-loss]] and [[neural-networks/losses/binary-cross-entropy-loss|binary-cross-entropy-loss]] for common classification losses and their PyTorch implementations.
- Read [[neural-networks/optimization/optimization-algorithms|optimization-algorithms]] to understand how optimizers turn gradients into parameter updates.
- Read [[neural-networks/optimization/gradient-descent|gradient-descent]] as the baseline first-order optimizer.
- Read [[neural-networks/optimization/stochastic-gradient-descent|stochastic-gradient-descent]] to understand mini-batch gradient estimates and per-dimension updates.
- Read [[neural-networks/optimization/gradient-descent-with-momentum|gradient-descent-with-momentum]] to understand how a running velocity can accelerate and smooth updates.
- Read [[neural-networks/optimization/rmsprop|rmsprop]] to understand adaptive per-parameter scaling from squared-gradient moving averages.
- Read [[neural-networks/optimization/exponential-moving-averages|exponential-moving-averages]] to understand the smoothing primitive used by more advanced optimization algorithms.
- Read [[neural-networks/optimization/bias-correction-for-exponential-moving-averages|bias-correction-for-exponential-moving-averages]] after the next bias-correction source is ingested.
- Read [[neural-networks/recurrent/recurrent-neural-networks|recurrent-neural-networks]] to understand sequence models that pass hidden state across time.
- Read [[neural-networks/recurrent/rnn-forward-propagation|rnn-forward-propagation]] for the basic activation and output equations, parameter sharing, and unrolled notation.
- Read [[neural-networks/recurrent/backpropagation-through-time|backpropagation-through-time]] to understand sequence losses, reverse-time gradient flow, and shared-parameter gradient accumulation.
- Read [[neural-networks/recurrent/rnn-architecture-types|rnn-architecture-types]] to distinguish one-to-many, many-to-one, equal-length many-to-many, and encoder-decoder sequence layouts.
- Read [[neural-networks/recurrent/conditional-language-models|conditional-language-models]] to understand machine translation as modeling $p(y \mid x)$.
- Read [[neural-networks/recurrent/sequence-decoding-search|sequence-decoding-search]] and [[neural-networks/recurrent/beam-search|beam-search]] to understand why inference uses approximate sequence search instead of random sampling or greedy next-token choices.
- Read [[neural-networks/recurrent/viterbi-algorithm|viterbi-algorithm]] to compare exact dynamic-programming decoding against beam search.
- Read [[neural-networks/recurrent/recursive-neural-networks|recursive-neural-networks]] to distinguish tree-structured recursive models from sequence-structured recurrent models.
- Read [[neural-networks/recurrent/gated-recurrent-units|gated-recurrent-units]] to understand update gates, reset gates, and how GRUs preserve memory across long gaps.
- Then read [[neural-networks/recurrent/lstm-networks|lstm-networks]] and [[neural-networks/recurrent/lstm-gates-and-cell-state|lstm-gates-and-cell-state]] for the LSTM memory path, gate equations, and update mechanism.
- Read [[neural-networks/recurrent/lstm-variants|lstm-variants]] to compare peephole LSTMs, coupled gates, and GRUs.
- For specific nonlinearities, read [[neural-networks/activations/relu-activation|relu-activation]], [[neural-networks/activations/sigmoid-activation|sigmoid-activation]], [[neural-networks/activations/tanh-activation|tanh-activation]], [[neural-networks/activations/gelu-activation|gelu-activation]], and [[neural-networks/activations/silu-swish-activation|silu-swish-activation]].

## Source Queue

Add papers, books, course notes, articles, transcripts, repos, and personal notes to `../raw/`, then ingest them into this wiki.
