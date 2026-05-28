# Conditional Language Models

A conditional language model assigns probabilities to output sequences after observing some conditioning input.

For sequence-to-sequence translation, the model estimates:

$$
p(y \mid x)
$$

where $x$ is the source sentence and $y$ is a possible target sentence. This differs from an unconditional language model, which models $p(y)$ without a source input.

## Encoder-Decoder View

In an [[rnn-architecture-types|encoder-decoder RNN]], the encoder reads the input sequence and produces a representation of it. The decoder then behaves like an autoregressive language model whose initial state, context, or conditioning information comes from the encoder rather than from only a zero vector or learned start state.

For translation, this means the decoder is not simply asking "what English sentence is likely?" It is asking "what English sentence is likely given this source sentence?"

This is the sense in which machine translation is a conditional language-modeling problem.

## Decoding Objective

At inference time, translation usually does not want a random sample from $p(y \mid x)$. It wants a high-probability output sequence:

$$
\hat{y} = \arg\max_y p(y \mid x)
$$

This is a sequence-level objective. The model must choose the complete output sequence, not merely a locally likely next token at each step.

The number of possible output sequences is exponential in sequence length. With a vocabulary of $10{,}000$ tokens, there are $10{,}000^{10}$ possible length-10 token sequences. Exhaustive search is therefore infeasible.

Practical systems use approximate decoding algorithms such as [[beam-search]].

## Random Sampling Versus Search

Random sampling is useful when diversity is desired, such as open-ended text generation. Translation and speech recognition often need a single best hypothesis, so decoding is usually framed as search for a high-probability sequence.

This distinction is task-dependent. The same conditional model can support sampling, greedy decoding, or beam search; the decoding rule changes what kind of output is returned.

## Related

- [[rnn-architecture-types]]
- [[sequence-decoding-search]]
- [[beam-search]]
- [[recurrent-neural-networks]]

## Sources

- [[../../../raw/courses/coursera/sequence-models/conditional-language-modeling-and-search-video-transcript|Coursera Sequence Models: Conditional Language Modeling and Search Video Transcript]]
- [[../../../raw/courses/coursera/sequence-models/types-of-rnn-video-transcript|Coursera Sequence Models: Types of RNN Video Transcript]]
