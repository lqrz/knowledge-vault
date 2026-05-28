# Piecewise Linear Functions

A piecewise linear function is a function made by stitching together multiple linear pieces.

Instead of one linear rule applying everywhere, different linear rules apply in different regions of the input.

## Simple Example

The absolute value function is piecewise linear:

```text
f(x) = |x|
```

It can be written as:

```text
f(x) = -x   if x < 0
f(x) =  x   if x >= 0
```

Each piece is linear, but the full function is not a single linear transformation because the rule changes at `x = 0`.

## ReLU Example

[[../neural-networks/activations/relu-activation|ReLU]] is also piecewise linear:

```text
phi(z) = max(0, z)
```

which means:

```text
phi(z) = 0   if z <= 0
phi(z) = z   if z > 0
```

The negative side is a flat linear piece. The positive side is an identity linear piece. The nonlinearity comes from switching between pieces.

## Why It Is Not Globally Linear

A globally linear function must obey:

```text
T(u + v) = T(u) + T(v)
T(cu) = cT(u)
```

A piecewise linear function can be linear inside each region but fail to be linear across region boundaries.

For ReLU:

```text
ReLU(-1) = 0
ReLU(1) = 1
ReLU(-1 + 1) = ReLU(0) = 0
```

But:

```text
ReLU(-1) + ReLU(1) = 0 + 1 = 1
```

So:

```text
ReLU(-1 + 1) != ReLU(-1) + ReLU(1)
```

That is why ReLU is not a linear transformation even though each side is linear.

## Deep Learning Implication

Networks using ReLU often become piecewise linear functions. Within one activation pattern, the network behaves like an affine transformation. Across different activation patterns, different neurons turn on or off, so the effective affine rule changes.

This gives the network nonlinear expressivity while still using simple linear or affine pieces locally.

## Related

- [[linear-transformations]]
- [[affine-transformations]]
- [[../neural-networks/activations/activation-functions|activation-functions]]
- [[../neural-networks/activations/relu-activation|relu-activation]]
- [[../index|index]]

## Sources

- [[../../raw/personal-notes/linear-transformations-seed|Linear Transformations Seed]]
