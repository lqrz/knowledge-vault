# Cross-Entropy

Cross-entropy measures the average code length when outcomes from one distribution are encoded using a code optimized for another distribution.

Using standard notation, the cross-entropy of true distribution $p$ under model distribution $q$ is:

$$
H(p, q) = \sum_x p(x)\log_2 \frac{1}{q(x)}
$$

This is larger than or equal to the [[entropy]] of $p$, with equality when $q = p$.

## Coding Interpretation

If $p$ is the distribution that generates outcomes and $q$ is the distribution used to design the code, then cross-entropy is the expected message length.

The more $q$ mismatches $p$, the more inefficient the code becomes.

## Machine Learning Interpretation

In classification, the target distribution is usually the true or empirical label distribution, and the model outputs a predicted distribution.

Minimizing cross-entropy pushes the model to assign high probability to the observed target outcomes. In one-hot classification, the loss for one example reduces to:

$$
-\log q(\text{true class})
$$

This connects directly to [[topics/deep-learning/wiki/neural-networks/losses/cross-entropy-loss|cross-entropy-loss]] and [[topics/deep-learning/wiki/neural-networks/losses/binary-cross-entropy-loss|binary-cross-entropy-loss]].

## Not Symmetric

Cross-entropy is not symmetric:

$$
H(p, q) \neq H(q, p)
$$

The penalty depends on which distribution generates outcomes and which distribution supplies the code or predictions.

## Relationship to KL Divergence

The gap between cross-entropy and entropy is [[kl-divergence]]:

$$
H(p, q) = H(p) + D_{\mathrm{KL}}(p \parallel q)
$$

This decomposes cross-entropy into unavoidable uncertainty plus avoidable mismatch.

## Related

- [[entropy]]
- [[kl-divergence]]
- [[information-theory]]
- [[topics/deep-learning/wiki/neural-networks/losses/cross-entropy-loss|cross-entropy-loss]]
- [[topics/deep-learning/wiki/neural-networks/losses/binary-cross-entropy-loss|binary-cross-entropy-loss]]
- [[topics/deep-learning/wiki/neural-networks/losses/loss-functions|loss-functions]]

## Sources

- [[../../raw/articles/colah/visual-information-theory|Visual Information Theory]] by Christopher Olah, 2015.
