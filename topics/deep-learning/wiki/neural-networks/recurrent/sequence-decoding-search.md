# Sequence Decoding Search

Sequence decoding is the inference-time problem of choosing an output sequence from a model's next-token probability distributions.

For an autoregressive decoder, the probability of a sequence factors into conditional next-token probabilities:

$$
p(y_1, \ldots, y_{T_y} \mid x)
= \prod_{t=1}^{T_y} p(y_t \mid x, y_1, \ldots, y_{t-1})
$$

The decoding algorithm decides how to use those probabilities to produce an output.

## Greedy Search

Greedy search chooses the locally most likely next token at each step:

$$
\hat{y}_t = \arg\max_{y_t} p(y_t \mid x, \hat{y}_1, \ldots, \hat{y}_{t-1})
$$

This is cheap, but it can make irreversible early choices. **A token that looks best locally can force a lower-probability or lower-quality continuation later.**

In the course example, after the prefix "Jane is", a locally common continuation such as "going" may beat "visiting" for the next token, even if the full sentence "Jane is visiting Africa in September" is better than a more verbose alternative.

## Search Space

The output space grows exponentially with length. If the vocabulary size is $V$ and the output length is $T_y$, there are $V^{T_y}$ possible token sequences of exactly that length.

This makes exact search infeasible for realistic vocabularies and sequence lengths. Decoding therefore uses approximations.

## Beam Search

[[beam-search]] keeps several partial hypotheses at each step rather than only one. It is still approximate, but it reduces the risk that one early local choice eliminates a better full sequence.

Beam search is common in machine translation and speech recognition because those tasks often require a single high-probability output rather than diverse random samples.

## Viterbi Decoding

[[viterbi-algorithm|Viterbi decoding]] is another sequence-decoding method. Unlike beam search, it is exact when the model has a finite-state dynamic-programming structure, such as a hidden Markov model trellis.

The contrast is useful: Viterbi merges histories that reach the same state because the model assumptions make that merge safe. Beam search keeps separate prefixes and prunes by beam width because neural autoregressive decoders usually do not provide a small state space where all equivalent histories can be merged exactly.

## Related

- [[conditional-language-models]]
- [[beam-search]]
- [[viterbi-algorithm]]
- [[rnn-architecture-types]]
- [[recurrent-neural-networks]]

## Sources

- [[../../../raw/courses/coursera/sequence-models/conditional-language-modeling-and-search-video-transcript|Coursera Sequence Models: Conditional Language Modeling and Search Video Transcript]]
- [[../../../raw/courses/coursera/sequence-models/beam-search-video-transcript|Coursera Sequence Models: Beam Search Video Transcript]]
- [Viterbi Algorithm for Hidden Markov Model, R HiddenMarkov documentation](https://search.r-project.org/CRAN/refmans/HiddenMarkov/html/Viterbi.html)
