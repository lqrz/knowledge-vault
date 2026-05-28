# Log

## 2026-05-13

- Created the initial `deep-learning` topic wiki index and log.
- Added raw seed note `raw/personal-notes/linear-transformations-seed.md` from the initial research direction.
- Added `linear-transformations.md`, connecting linear transformations, vector spaces, matrices, basis vectors, affine transformations, and the single-neuron pre-activation computation.
- Added `vector-spaces-and-basis-vectors.md`, defining vector spaces, coordinates, basis vectors, spanning, and linear independence.
- Clarified that addition and scalar multiplication are the minimal operations required for a vector space, while dot products are additional inner-product structure.
- Split `vector-spaces-and-basis-vectors.md` into `vector-spaces.md` and `vector-space-basis.md`, then updated wiki links.
- Clarified in `vector-spaces.md` that dot products are not automatic for every vector space and must be specified as additional structure.
- Added a polynomial vector space example to explain what it means for a vector space not to have a specified dot product.
- Added `polynomial-vector-spaces.md` to explain why polynomials can be vectors, and linked it from `vector-spaces.md` and the topic index.
- Clarified in `linear-transformations.md` that linear transformations can map between different vector spaces, while same-space linear maps are a special case.
- Expanded the `Linear vs Affine` section in `linear-transformations.md` to make explicit that affine transformations are only linear when the bias is zero.
- Split affine-transformation material out of `linear-transformations.md` into `affine-transformations.md` and updated topic links.
- Added `activation-functions.md`, covering linear versus nonlinear activations and how nonlinearities change representation geometry.
- Added `single-neurons-and-layers.md`, explaining one linear neuron as a scalar projection and multiple neurons as a vector-valued layer.
- Expanded `single-neurons-and-layers.md` with the geometry of multiple affine neurons as multiple hyperplanes partitioning input space.
- Added `hyperplanes.md`, defining hyperplanes, their formula, and why the weight vector is perpendicular to the hyperplane.
- Expanded `hyperplanes.md` with a signed-score explanation and a 2D line example.
- Updated neuron score references to link directly to `hyperplanes.md` section `Signed Score`.
- Added SVG diagrams to `hyperplanes.md` for signed-score sides and the perpendicular weight vector.
- Moved the hyperplane diagrams into their matching sections in `hyperplanes.md`.
- Added a bias-shift SVG diagram under the `Role of the Bias` section in `hyperplanes.md`.
- Added separate activation pages with SVG curve diagrams for ReLU, sigmoid, tanh, GELU, and SiLU / Swish, and linked them from `activation-functions.md` and the topic index.
- Restructured the wiki into `linear-algebra/`, `neural-networks/`, and `neural-networks/activations/` subfolders, then updated diagram paths, raw-source citations, and the topic index.
- Added `linear-algebra/piecewise-linear-functions.md` and linked ReLU and activation-function references to it.
- Added `neural-networks/activation-saturation-and-gradients.md` explaining why saturation weakens backpropagated gradients and linked sigmoid, tanh, ReLU, and the activation overview to it.
- Added `neural-networks/loss-functions.md`, explaining the required characteristics of losses for gradient-based neural network training and linked it from the topic index.
- Added `neural-networks/cross-entropy-loss.md` and `neural-networks/binary-cross-entropy-loss.md`, including intuition, caveats, and PyTorch implementations.
- Moved loss-related pages into `neural-networks/losses/` and updated the topic index links.
- Moved activation overview pages into `neural-networks/activations/` and updated related wikilinks.
- Downloaded Christopher Olah's 2015 article `Understanding LSTM Networks` into `raw/articles/colah/understanding-lstm-networks.html`.
- Mirrored the article's RNN/LSTM diagram images into `raw/articles/colah/img/` for offline reference.
- Added recurrent-network wiki pages covering RNNs, LSTM networks, LSTM gates and cell state, and LSTM variants, with citations back to the raw article.
- Updated the deep-learning index with recurrent neural network links, LSTM concepts, and learning-path entries.
- Expanded `neural-networks/recurrent/recurrent-neural-networks.md` with missing RNN limitations: vanishing/exploding gradients, hidden-state bottleneck, sequential computation, backpropagation-through-time cost, truncated credit assignment, and directional context.
- Added Coursera Sequence Models `Recurrent Neural Networks` transcript to `raw/courses/coursera/sequence-models/recurrent-neural-networks-video-transcript.md`.
- Enhanced `neural-networks/recurrent/recurrent-neural-networks.md` with feedforward-network limitations for sequences and basic RNN forward-propagation equations.
- Added `neural-networks/recurrent/rnn-forward-propagation.md` covering initialization, shared parameters, compact matrix notation, and unrolled RNN diagrams.
- Added Coursera Sequence Models `Backpropagation Through Time` transcript to `raw/courses/coursera/sequence-models/backpropagation-through-time-video-transcript.md`.
- Added `neural-networks/recurrent/backpropagation-through-time.md` covering reverse-time gradient flow, per-step sequence losses, shared-parameter gradient accumulation, and BPTT limitations.
- Linked BPTT from the recurrent-network overview, RNN forward propagation page, and deep-learning index.
- Expanded `neural-networks/recurrent/backpropagation-through-time.md` to clarify that repeated uses of a shared recurrent matrix contribute additive terms to one gradient tensor, followed by one optimizer update.
- Added SVG diagrams for BPTT unrolled forward/backward flow, sequence-loss summation, and shared-parameter gradient accumulation, then embedded them in `neural-networks/recurrent/backpropagation-through-time.md`.
- Revised BPTT SVG diagrams to increase spacing, wrap long labels, and reduce text/connector overlap risk.
- Added Coursera Sequence Models `Types of RNN` transcript to `raw/courses/coursera/sequence-models/types-of-rnn-video-transcript.md`.
- Added `neural-networks/recurrent/rnn-architecture-types.md` covering one-to-one, one-to-many, many-to-one, equal-length many-to-many, and encoder-decoder RNN layouts.
- Enhanced `neural-networks/recurrent/recurrent-neural-networks.md` with an input/output length section and linked the architecture-types page from recurrent RNN pages and the topic index.
- Added Coursera Sequence Models `Gated Recurrent Units` transcript to `raw/courses/coursera/sequence-models/gated-recurrent-units-video-transcript.md`.
- Added `neural-networks/recurrent/gated-recurrent-units.md` covering simplified and full GRU equations, update/reset gate intuition, vector gates, and comparison with LSTMs.
- Added GRU SVG diagrams for update-gate interpolation and full update/reset-gate cell structure, then linked GRUs from the recurrent overview, LSTM pages, variants page, and topic index.
- Revised GRU SVG labels to avoid XML parsing ambiguity and reduce text overlap in equation boxes.
- Embedded mirrored Colah LSTM diagrams from `raw/articles/colah/img/` into `lstm-networks.md`, `lstm-gates-and-cell-state.md`, and `lstm-variants.md` to improve visual explanations.
- Created white-background JPEG derivatives of the embedded Colah LSTM diagrams under `wiki/assets/` and updated LSTM pages to use those derived assets while preserving raw images unchanged.
- Strengthened reciprocal references between `lstm-networks.md` and `lstm-gates-and-cell-state.md`, linking overview concepts to the detailed gate equations and back.
- Added `neural-networks/recurrent/recursive-neural-networks.md` explaining tree-structured recursive neural networks and cross-linked it from the recurrent overview and topic index.
- Clarified in `neural-networks/recurrent/rnn-architecture-types.md` that many-to-one RNNs usually apply the output head and loss only at the final real input state, though implementations may compute ignored intermediate outputs.

## 2026-05-14

- Added Coursera Sequence Models transcript `conditional-language-modeling-and-search-video-transcript.md` to the existing sequence-models raw course folder.
- Added Coursera Sequence Models transcript `beam-search-video-transcript.md` to the existing sequence-models raw course folder.
- Added `neural-networks/recurrent/conditional-language-models.md`, framing machine translation as estimating $p(y \mid x)$ with an encoder-decoder model.
- Added `neural-networks/recurrent/sequence-decoding-search.md`, covering random sampling, greedy search, exponential output spaces, and approximate decoding.
- Added `neural-networks/recurrent/beam-search.md`, covering beam width, prefix expansion, sequence probability scoring, and the greedy-search special case.
- Linked the new decoding pages from the recurrent overview, RNN architecture types page, and deep-learning index.
- Added `neural-networks/recurrent/viterbi-algorithm.md` to compare exact Viterbi dynamic-programming decoding with approximate beam search, then linked it from the decoding pages and index.
- Expanded `neural-networks/recurrent/beam-search.md` with limitations and mitigations, including length normalization, search errors, low diversity, repetition, score mismatch, and compute cost.

## 2026-05-15

- Added Coursera Improving Deep Neural Networks transcript `exponentially-weighted-averages-video-transcript.md` to `raw/courses/coursera/improving-deep-neural-networks/`.
- Created `neural-networks/optimization/optimization-algorithms.md` as the starting map for deep learning optimizer notes.
- Created `neural-networks/optimization/exponential-moving-averages.md`, covering the EMA recurrence, effective-window-size heuristic, and smoothing-versus-latency tradeoff.
- Updated the deep-learning index with the new optimization section, concepts, and learning-path entries.
- Added Coursera Improving Deep Neural Networks transcript `understanding-exponentially-weighted-averages-video-transcript.md` to the same raw course folder.
- Expanded `neural-networks/optimization/exponential-moving-averages.md` with the expanded weighted-sum form, exponential-decay intuition, constant-memory implementation, and moving-window comparison.
- Added placeholder page `neural-networks/optimization/bias-correction-for-exponential-moving-averages.md` to mark the transcript's deferred technical detail.
- Updated `neural-networks/optimization/optimization-algorithms.md` and the deep-learning index with the new EMA implementation and bias-correction links.
- Added `neural-networks/optimization/gradient-descent.md`, explaining the update rule, contribution as the baseline first-order optimizer, and limitations around learning rate, curvature, and stochastic noise.
- Added `neural-networks/optimization/gradient-descent-with-momentum.md`, explaining velocity-based updates, the connection to exponential moving averages, Polyak's heavy-ball method, and limitations around overshoot, stale velocity, and hyperparameter coupling.
- Updated the optimization map, EMA page, and deep-learning index with gradient descent and momentum links.
- Added `neural-networks/optimization/stochastic-gradient-descent.md`, explaining mini-batch SGD and how each parameter dimension receives its own gradient component while sharing one global learning rate.
- Updated the gradient descent page, optimization map, and deep-learning index with SGD links.
- Added Coursera Improving Deep Neural Networks transcript `gradient-descent-with-momentum-video-transcript.md` to the raw course folder.
- Expanded `neural-networks/optimization/gradient-descent-with-momentum.md` with Coursera notation for $v_{dW}$ and $v_{db}$, oscillation damping, common $\beta = 0.9$, bias-correction practice, initialization shapes, and velocity scaling conventions.
- Added the momentum transcript as a source for the EMA page's optimizer connection.
- Added `neural-networks/optimization/rmsprop.md`, explaining squared-gradient exponential moving averages, adaptive per-parameter scaling, relation to momentum, hyperparameters, and limitations.
- Updated the optimization map, SGD page, EMA page, momentum page, and deep-learning index with RMSProp links.
- Clarified the `rmsprop.md` relation-to-momentum section: momentum remembers recent gradient direction, while RMSProp remembers recent gradient size for per-parameter scaling.
- Added a `Why Shrink Large Gradients?` section to `rmsprop.md`, explaining that RMSProp normalizes persistent gradient scale while preserving gradient direction.
