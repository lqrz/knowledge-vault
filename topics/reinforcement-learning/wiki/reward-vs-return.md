# Reward Vs Return

In reinforcement learning, a reward is the immediate scalar feedback received after a state, action, or transition. A return is the accumulated future reward from a point in time, usually discounted.[^sutton-barto]

In notation, `R_{t+1}` is the reward observed after taking action `A_t` in state `S_t`. The return `G_t` summarizes the rewards that follow:

```text
G_t = R_{t+1} + gamma R_{t+2} + gamma^2 R_{t+3} + ...
```

Here `gamma` is the discount factor. If `gamma` is close to `1`, later rewards matter almost as much as immediate rewards. If `gamma` is close to `0`, the return mostly reflects near-term reward.[^sutton-barto]

## Practical Difference

Reward answers: "What feedback did this step or outcome receive?"

Return answers: "How good was this trajectory from here onward?"

For example, in a game, a single move might receive `0` reward, but it can still have high return if it leads to a win later. Conversely, a move might get a small immediate reward while lowering the final trajectory return.

## Relation To LLM Post-Training

In LLM RL-style fine-tuning, especially prompt-completion training, reward is often assigned to an entire sampled completion by a reward model, verifier, or programmatic grader. That setting can blur the distinction because the "episode" may be just one generated answer, so the completion-level reward may also act like the trajectory return.

In [[topics/llm-post-training/wiki/grpo-loss|grpo-loss]], rewards are converted into [[topics/llm-post-training/wiki/advantages|advantages]] or group-relative signals that weight policy updates. The important idea is that the policy is not merely trained on isolated reward labels; it is updated according to how good sampled behavior was relative to a baseline or comparison group.[^grpo-transcript]

## Related Pages

- [[topics/llm-post-training/wiki/advantages|advantages]]
- [[topics/llm-post-training/wiki/grpo-loss|grpo-loss]]
- [[monte-carlo-methods/importance-sampling|importance-sampling]]
- [[prediction-and-control]]

## Sources

[^sutton-barto]: Richard S. Sutton and Andrew G. Barto, *Reinforcement Learning: An Introduction*, 2nd ed., definition of return: `../raw/books/sutton-barto-reinforcement-learning/monte-carlo-methods.pdf`.
[^grpo-transcript]: Raw transcript: `../../llm-post-training/raw/courses/reinforcement_finetuning_llms_with_grpo/calculating-loss-in-grpo.md`.
