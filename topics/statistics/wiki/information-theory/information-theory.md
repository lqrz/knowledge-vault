# Information Theory

Information theory studies uncertainty, communication, compression, and relationships between probability distributions.

The central idea is that probability and code length are linked. Common events can be assigned short codes; rare events require longer codes. The best possible average code length for a distribution is its [[entropy|entropy]].

## Core Questions

Information theory gives precise answers to questions such as:

- How uncertain is a distribution?
- How many bits are needed to communicate an outcome?
- How inefficient is it to use a code optimized for the wrong distribution?
- How different are two distributions?
- How much does knowing one variable tell us about another?

## Main Quantities

- [[entropy]] measures the average information needed to communicate an outcome from one distribution.
- [[cross-entropy]] measures the average code length when outcomes from one distribution are encoded using a code optimized for another.
- [[kl-divergence]] measures the extra code length caused by that mismatch.
- [[joint-and-conditional-entropy|Joint entropy]] measures uncertainty over pairs or tuples of variables.
- [[joint-and-conditional-entropy|Conditional entropy]] measures uncertainty that remains after another variable is known.
- [[mutual-information]] measures the information shared by two variables.

## Machine Learning Connection

Machine learning often compares predicted distributions with target distributions. This is why [[cross-entropy]] and [[kl-divergence]] appear in classification, language modeling, variational inference, distillation, and reinforcement-style fine-tuning.

See [[topics/deep-learning/wiki/neural-networks/losses/cross-entropy-loss|cross-entropy-loss]] for the deep-learning training objective and [[topics/llm-post-training/wiki/kl-divergence-penalty|kl-divergence-penalty]] for an LLM post-training use case.

## Related

- [[prefix-codes-and-entropy]]
- [[entropy]]
- [[cross-entropy]]
- [[kl-divergence]]
- [[joint-and-conditional-entropy]]
- [[mutual-information]]
- [[../probability/probability-distributions|probability-distributions]]

## Sources

- [[../../raw/articles/colah/visual-information-theory|Visual Information Theory]] by Christopher Olah, 2015.
