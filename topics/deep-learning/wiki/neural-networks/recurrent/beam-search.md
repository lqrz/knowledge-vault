# Beam Search

Beam search is an approximate decoding algorithm for sequence models. It tries to find a high-probability output sequence without enumerating the exponentially large set of all possible sequences.

It is commonly used for [[conditional-language-models|conditional language models]] in tasks such as machine translation and speech recognition.

## Beam Width

Beam search has a beam width $B$.

At each decoding step, the algorithm keeps the top $B$ partial hypotheses rather than only the single best prefix. If $B = 1$, beam search reduces to [[sequence-decoding-search#Greedy Search|greedy search]].

Larger $B$ explores more alternatives but costs more computation and memory.

## Algorithm

At the first output step, the decoder scores every vocabulary token:

$$
p(y_1 \mid x)
$$

Beam search keeps the top $B$ first-token candidates.

At the next step, each retained prefix is expanded by every vocabulary token. The score for a two-token prefix is:

$$
p(y_1, y_2 \mid x)
= p(y_1 \mid x)p(y_2 \mid x, y_1)
$$

More generally, a prefix score is:

$$
p(y_{1:t} \mid x)
= \prod_{i=1}^{t} p(y_i \mid x, y_{1:i-1})
$$

After expanding all retained prefixes, beam search keeps the top $B$ new prefixes and discards the rest. This repeats until hypotheses emit an end-of-sequence token or another stopping rule is met.

In practice, implementations usually accumulate log probabilities rather than multiplying raw probabilities:

$$
\log p(y_{1:t} \mid x)
= \sum_{i=1}^{t} \log p(y_i \mid x, y_{1:i-1})
$$

The provided course transcript explains the probability-product version; the log-probability version is the numerically stable implementation form.

## Example

With beam width $B = 3$ and vocabulary size $10{,}000$, the first step keeps the top three first words, such as:

- "in"
- "Jane"
- "September"

The second step expands each prefix by all $10{,}000$ possible next words, producing $3 \times 10{,}000 = 30{,}000$ two-token candidates. It then keeps only the top three, such as:

- "in September"
- "Jane is"
- "Jane visits"

The next step repeats the same process, expanding only these retained prefixes.

## Why It Helps

Greedy search can fail because it commits to one local choice before seeing its longer-term consequences. Beam search keeps multiple plausible prefixes alive, giving the decoder a chance to recover when the locally best prefix is not part of the best full sequence.

Beam search is not guaranteed to find the exact global maximum of $p(y \mid x)$, because it discards most prefixes at every step. It is a tractable approximation that often works well enough.

## Limitations and Mitigations

Beam search is useful, but it is not a neutral way to "get the best sequence." The score, stopping rule, and beam width all shape the output.

### Length Bias

Raw sequence probabilities shrink as sequences get longer because they multiply many probabilities less than one:

$$
p(y_{1:t} \mid x)
= \prod_{i=1}^{t} p(y_i \mid x, y_{1:i-1})
$$

Equivalently, log probabilities become more negative as more tokens are added:

$$
\log p(y_{1:t} \mid x)
= \sum_{i=1}^{t} \log p(y_i \mid x, y_{1:i-1})
$$

This can bias decoding toward shorter outputs or early end-of-sequence tokens.

Common mitigations include:

- length normalization, such as dividing the log score by a length-dependent penalty
- minimum output length constraints, which suppress end-of-sequence before a threshold
- explicit length penalties, tuned on validation data

A simple normalized score is:

$$
\frac{\log p(y \mid x)}{|y|^\alpha}
$$

where $\alpha$ controls how strongly the decoder compensates for length.

### Search Errors

Beam search can discard a prefix that later would have become part of the best full sequence. This is the central approximation: once a prefix falls outside the beam, it is gone.

Common mitigations include:

- increasing beam width
- using better-calibrated model scores
- reranking a larger n-best list with additional features or a stronger model

Increasing beam width is not always enough, because larger beams can sometimes make length bias or model miscalibration more visible.

### Low Diversity

The top beam hypotheses often differ only slightly from each other. Many candidates may share the same early prefix, so the beam spends capacity on near-duplicates instead of exploring meaningfully different continuations.

Common mitigations include:

- diverse beam search, which penalizes beams that are too similar
- grouping beams and encouraging different groups to explore different prefixes
- sampling-based decoding when diversity is more important than one best sequence

### Repetition and Degeneration

Autoregressive models can assign high probability to repetitive continuations. Beam search may amplify this because it keeps high-probability prefixes even when they become semantically poor.

Common mitigations include:

- n-gram blocking, which prevents repeated n-grams
- repetition penalties
- coverage penalties in attention-based translation models, discouraging outputs that ignore or repeatedly attend to the same source content
- task-specific constraints or reranking

### Model-Score Mismatch

The highest-probability sequence under the model is not always the best output by human or task metrics. Beam search optimizes the model score, not meaning, usefulness, factuality, or stylistic quality directly.

Common mitigations include:

- validation-set tuning of beam width, length penalty, and constraints
- reranking candidates with task metrics, reward models, or discriminative scorers
- using decoding methods matched to the task, such as sampling for open-ended generation and beam search for constrained transcription or translation

### Compute Cost

At each step, beam search expands $B$ partial hypotheses. Larger beams increase compute and memory cost roughly in proportion to $B$.

Common mitigations include:

- smaller beam widths chosen by validation performance rather than by default
- batched beam expansion
- early stopping when completed hypotheses are already better than unfinished candidates under the chosen score
- vocabulary pruning or top-$k$ candidate expansion

## Relation to Viterbi

Beam search is related to the [[viterbi-algorithm|Viterbi algorithm]] because both are sequence decoding methods. The important difference is that Viterbi is exact dynamic programming for models with a finite trellis structure, such as hidden Markov models, while beam search is an approximate pruning heuristic for large search spaces.

Viterbi can safely keep only the best partial path for each state at each time because future probabilities depend on that state rather than the full path history. Neural autoregressive decoders usually condition on the whole prefix, so different prefixes cannot generally be merged safely. Beam search keeps the top $B$ prefixes instead, which is useful but not globally optimal.

## Related

- [[sequence-decoding-search]]
- [[viterbi-algorithm]]
- [[conditional-language-models]]
- [[rnn-architecture-types]]
- [[recurrent-neural-networks]]

## Sources

- [[../../../raw/courses/coursera/sequence-models/beam-search-video-transcript|Coursera Sequence Models: Beam Search Video Transcript]]
- [[../../../raw/courses/coursera/sequence-models/conditional-language-modeling-and-search-video-transcript|Coursera Sequence Models: Conditional Language Modeling and Search Video Transcript]]
- [Best-First Beam Search, TACL / MIT Press](https://direct.mit.edu/tacl/article/doi/10.1162/tacl_a_00346/96473/Best-First-Beam-Search)
