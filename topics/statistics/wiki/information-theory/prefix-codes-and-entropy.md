# Prefix Codes and Entropy

A code maps symbols to bit strings. A prefix code is a variable-length code where no codeword is the prefix of another codeword.

The prefix property makes decoding unambiguous. If $0$ and $01$ were both codewords, a received string beginning with $01\ldots$ would be ambiguous because the first codeword might be $0$ or $01$.

## Codeword Length Tradeoff

Short codewords are valuable because they reduce average message length. But a short codeword consumes a large part of the possible codeword space.

For binary codes:

$$
\begin{aligned}
\text{length 1 codeword cost} &= \frac{1}{2} \\
\text{length 2 codeword cost} &= \frac{1}{4} \\
\text{length 3 codeword cost} &= \frac{1}{8}
\end{aligned}
$$

In general:

$$
\text{cost} = \frac{1}{2^L}
$$

where $L$ is the codeword length.

## Optimal Intuition

The optimal coding pattern spends more of the codeword budget on common events and less on rare events.

For an event with probability $p(x)$, the ideal code length is:

$$
L(x) = \log_2 \frac{1}{p(x)}
$$

Common events get short codewords; rare events get long codewords.

## Link to Entropy

The expected length under the optimal code is [[entropy|entropy]]:

$$
H(p) = \sum_x p(x)\log_2 \frac{1}{p(x)}
$$

This makes entropy a lower bound on average code length for communicating outcomes from a distribution.

## Related

- [[entropy]]
- [[fractional-bits]]
- [[cross-entropy]]
- [[../probability/probability-distributions|probability-distributions]]

## Sources

- [[../../raw/articles/colah/visual-information-theory|Visual Information Theory]] by Christopher Olah, 2015.
