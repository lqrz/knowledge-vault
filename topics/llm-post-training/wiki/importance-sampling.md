# Importance Sampling

Importance sampling estimates an expectation under one distribution using samples drawn from another distribution. It reweights each sample by how likely that sample would have been under the target distribution compared with the sampling distribution.

In its simplest form:

```text
E_target[f(x)] = E_behavior[(p_target(x) / p_behavior(x)) * f(x)]
```

The ratio `p_target(x) / p_behavior(x)` is the importance weight. A sample that is common under the target distribution but rare under the behavior distribution receives a large weight. A sample that is common under the behavior distribution but unlikely under the target distribution receives a small weight.[^sutton-barto]

## What It Is Used For

Importance sampling is useful when the distribution we care about is expensive, risky, unavailable, or inconvenient to sample from directly, but we have samples from another related distribution.

Common uses include:

- Off-policy reinforcement learning and off-policy evaluation.
- Reusing logged or replay-buffer data collected by an older policy.
- Correcting estimates when training data comes from a different distribution than the one being evaluated.
- Estimating rare-event quantities by sampling from a distribution that sees those events more often.

The key requirement is support overlap: if the target distribution can choose an event that the behavior distribution never chooses, the ratio is undefined and the behavior data cannot estimate that target behavior reliably.[^sutton-barto]

## Relation To Off-Policy Reinforcement Learning

In off-policy RL, data is collected by a behavior policy `b`, but the learner wants to evaluate or improve a different target policy `pi`. Importance sampling corrects for this mismatch by weighting observed actions or trajectories by how likely they are under `pi` relative to `b`.

For an action at state `s`:

```text
rho_t = pi(a_t | s_t) / b(a_t | s_t)
```

For a full trajectory segment, the correction is often a product of per-step ratios:

```text
rho = product_t pi(a_t | s_t) / b(a_t | s_t)
```

This makes returns collected under `b` usable for estimating values under `pi`. For example, if the behavior policy took actions that the target policy also strongly prefers, the return gets a high weight. If the behavior policy took actions the target policy would rarely take, the return gets downweighted.[^sutton-barto]

## Why It Matters

Without importance sampling, an off-policy estimate can answer the wrong question: it may estimate the value of the behavior policy while pretending to estimate the target policy. Importance sampling is one principled way to say, "Given that this data came from `b`, how much should it count for `pi`?"

The tradeoff is variance. Products of likelihood ratios can become very large or very small, especially over long horizons. Ordinary importance sampling can be unbiased but high variance; weighted or normalized variants reduce variance but can introduce bias that usually decreases with more data.[^sutton-barto]

## Relation To PPO And GRPO-Style Ratios

The probability ratios in [[ratio-clipping]] and [[grpo-loss]] are closely related to importance sampling. PPO-style objectives compare the current policy to the policy that generated the data, then clip the ratio to avoid overly large updates. The GRPO course transcript in this vault describes a policy-to-reference probability ratio and then clamps it before multiplying by the [[advantages|advantage]].[^grpo-transcript]

Strictly speaking, the GRPO transcript's policy-to-reference comparison is not a full off-policy trajectory correction by itself. It is a local token-probability ratio used inside a stabilized policy optimization loss. The shared idea is still the same: a likelihood ratio measures how much more or less the current/trainable policy favors the sampled token than the comparison policy.

## Related Pages

- [[grpo-loss]]
- [[ratio-clipping]]
- [[advantages]]
- [[policy-and-reference-models]]

## Sources

[^sutton-barto]: Richard S. Sutton and Andrew G. Barto, *Reinforcement Learning: An Introduction*, 2nd ed., section on off-policy prediction via importance sampling: `https://web.stanford.edu/class/psych209/Readings/SuttonBartoIPRLBook2ndEd.pdf`.
[^grpo-transcript]: Raw transcript: `../raw/courses/reinforcement_finetuning_llms_with_grpo/calculating-loss-in-grpo.md`.
