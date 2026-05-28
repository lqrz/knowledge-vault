# On-Policy Monte Carlo Control

On-policy Monte Carlo control evaluates and improves the same policy that is used to generate behavior.

To avoid relying on [[monte-carlo-methods/exploring-starts|exploring-starts]], on-policy Monte Carlo control usually keeps the policy soft: every action has nonzero probability in every state. A common form is epsilon-greedy action selection, where the policy usually selects a greedy action but sometimes samples randomly.

## Epsilon-Soft Improvement

The policy-improvement step shifts probability toward actions with high estimated action value while retaining exploration probability for all actions.

This changes the goal slightly:

- The agent does not learn a fully deterministic greedy policy while it must keep exploring.
- Instead, it improves toward the best policy within the chosen epsilon-soft policy class.

## Tradeoff

On-policy Monte Carlo control handles exploration without a separate behavior policy, but its learned policy remains exploratory. This is a compromise between learning good behavior and collecting enough action-value data.

## Related

- [[monte-carlo-methods/control|monte-carlo-control]]
- [[monte-carlo-methods/exploring-starts|exploring-starts]]
- [[monte-carlo-methods/off-policy-control|off-policy-monte-carlo]]

## Sources

- [[topics/reinforcement-learning/raw/books/sutton-barto-reinforcement-learning/monte-carlo-methods.pdf|Sutton and Barto, Reinforcement Learning: An Introduction, Chapter 5, Monte Carlo Methods]]
- [Sutton and Barto, Reinforcement Learning: An Introduction, Chapter 5 online copy](https://web.stanford.edu/class/psych209/Readings/SuttonBartoRL.pdf): Section 5.4 explains on-policy Monte Carlo control without exploring starts and the role of epsilon-soft policies; see lines 5641-5660 and 5843-5850 in the indexed PDF text.
