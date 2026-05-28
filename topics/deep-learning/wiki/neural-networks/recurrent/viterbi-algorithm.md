# Viterbi Algorithm

The Viterbi algorithm is an exact dynamic-programming algorithm for finding the highest-probability path through a structured sequence model, classically a hidden Markov model.

It is related to [[beam-search]] because both are decoding algorithms: both try to find a high-scoring sequence under a model. They differ in what assumptions they exploit and whether they guarantee the optimal sequence.

## Core Idea

In a first-order hidden Markov model, the best hidden-state sequence can be found by dynamic programming because the score of a partial path ending in state $s_t$ depends only on:

- the best previous path ending in each previous state $s_{t-1}$
- the transition probability into $s_t$
- the emission likelihood of the current observation

The algorithm stores, for each time step and state, the best score of any path that reaches that state. It also stores backpointers so the best full path can be reconstructed at the end.

This works because the model has optimal substructure: once two partial paths end in the same state at the same time, only the better-scoring one can matter for any future continuation under the Markov assumptions.

## Relationship to Beam Search

Beam search and Viterbi both search over possible sequences, but beam search is a pruning heuristic while Viterbi is exact dynamic programming for models with the right structure.

Viterbi can safely merge competing histories that arrive at the same state because future scores depend on the state, not on the full history.

Neural autoregressive decoders usually condition on the whole generated prefix:

$$
p(y_t \mid x, y_1, \ldots, y_{t-1})
$$

Two different prefixes are not interchangeable just because they have the same length or last token. The decoder hidden state or attention context can encode the whole prefix, so there is usually no small finite state that lets the algorithm merge histories exactly. Beam search keeps only the top $B$ prefixes and discards the rest, which makes it tractable but not guaranteed to find the global optimum.

## Comparison

| Property | Viterbi algorithm | Beam search |
| --- | --- | --- |
| Main use | HMMs, trellises, finite-state models, structured decoders | Neural sequence generation, machine translation, speech recognition, captioning |
| Search type | Exact dynamic programming under model assumptions | Approximate heuristic search |
| Keeps | Best path per time/state cell | Top $B$ partial hypotheses per decoding step |
| Pruning | Safe merging by dynamic-programming optimal substructure | Unsafe pruning by beam width |
| Guarantee | Finds the best path in the represented state graph | Not guaranteed to find the global best sequence |
| Special parameter | Number of states comes from the model | Beam width $B$ is chosen by the user |

## Practical Rule

Use the Viterbi lens when the model can be represented as a trellis or finite-state dynamic program where histories that reach the same state can be merged exactly.

Use the beam-search lens when the model is an autoregressive generator whose distinct prefixes must remain distinct hypotheses, and exact search would be too expensive.

## Related

- [[beam-search]]
- [[sequence-decoding-search]]
- [[conditional-language-models]]
- [[rnn-architecture-types]]

## Sources

- [[../../../raw/courses/coursera/sequence-models/beam-search-video-transcript|Coursera Sequence Models: Beam Search Video Transcript]]
- [Viterbi Algorithm for Hidden Markov Model, R HiddenMarkov documentation](https://search.r-project.org/CRAN/refmans/HiddenMarkov/html/Viterbi.html)
- [Best-First Beam Search, TACL / MIT Press](https://direct.mit.edu/tacl/article/doi/10.1162/tacl_a_00346/96473/Best-First-Beam-Search)
