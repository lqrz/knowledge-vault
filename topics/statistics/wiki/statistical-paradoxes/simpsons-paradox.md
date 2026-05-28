# Simpson's Paradox

Simpson's paradox occurs when a trend seen in aggregate data reverses after conditioning on a relevant subgroup variable.

The paradox is not a contradiction in probability. It is a warning that an aggregate comparison can mix together groups with different base rates.

## Pattern

Suppose treatment $B$ has a higher overall survival rate than treatment $A$.

After splitting patients by stone size, treatment $A$ can still have a higher survival rate for both small stones and large stones.

This can happen if treatment assignment is confounded with stone size. If treatment $B$ was disproportionately given to easier cases, its aggregate outcome can look better even when treatment $A$ is better within each severity group.

## Mechanism

The aggregate comparison weights subgroup outcomes by how many examples each treatment has in each subgroup.

When the subgroup distribution differs by treatment, the aggregate rate combines two effects:

- the treatment effect within each subgroup
- the baseline difficulty or risk mix of the subgroups

The second effect can dominate and reverse the aggregate conclusion.

## Practical Lesson

Do not interpret aggregate comparisons without checking whether important conditioning variables are imbalanced.

In causal or experimental settings, proper randomization helps prevent this failure mode by balancing confounders across treatment groups.

## Related

- [[../probability/conditional-probability|conditional-probability]]
- [[../probability/probability-distributions|probability-distributions]]
- [[../information-theory/mutual-information|mutual-information]]

## Sources

- [[../../raw/articles/colah/visual-information-theory|Visual Information Theory]] by Christopher Olah, 2015.
