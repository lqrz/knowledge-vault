# Single Neurons and Layers

A single neuron takes an input vector and produces one scalar output.

Without a nonlinear activation, a neuron computes:

```text
z = w · x + b
```

where:

- `x` is the input vector
- `w` is the neuron's weight vector
- `b` is the bias
- `z` is the scalar pre-activation

This is an [[affine-transformations|affine transformation]] from a vector space to a one-dimensional output space. If `b = 0`, it is a strictly [[linear-transformations|linear transformation]].

## Effect of One Linear Neuron

If the neuron has no bias and no nonlinear activation:

```text
z = w · x
```

then the neuron measures how much the input vector aligns with the direction `w`.

Geometrically, the neuron projects the input onto one learned direction and returns a scalar coordinate-like score. It compresses the input vector into one number.

If the input is in `R^n`, then one linear neuron maps:

```text
R^n -> R
```

This loses information unless the original input was already one-dimensional. Many different input vectors can produce the same scalar output.

## Adding Bias

With bias:

```text
z = w · x + b
```

the neuron is affine rather than strictly linear. The bias shifts the score, moving the neuron's threshold away from the origin.

In classification intuition, the equation:

```text
w · x + b = 0
```

defines a [[hyperplanes|hyperplane]]. The neuron outputs a [[hyperplanes#Signed Score|signed score]] that measures which side of that hyperplane the input lies on and how far it is in the direction measured by `w`.

## Adding More Neurons

Multiple neurons compute multiple [[hyperplanes#Signed Score|signed scores]] from the same input.

For example:

```text
z1 = w1 · x + b1
z2 = w2 · x + b2
z3 = w3 · x + b3
```

Stacking these outputs gives a vector:

```text
z = [z1, z2, z3]
```

In matrix form:

```text
z = Wx + b
```

Each row of `W` is one neuron's weight vector. Each neuron measures the input along a different learned direction.

If there are `m` neurons and the input is in `R^n`, the layer maps:

```text
R^n -> R^m
```

So adding neurons changes the output dimension and lets the layer build a richer representation with multiple learned coordinates.

## Multiple Neurons as Multiple Hyperplanes

Each affine neuron defines one [[hyperplanes|hyperplane]]:

```text
wi · x + bi = 0
```

The weight vector `wi` is perpendicular to that hyperplane. The bias `bi` shifts the hyperplane away from the origin.

With multiple affine neurons, the layer defines multiple hyperplanes in the same input space:

```text
w1 · x + b1 = 0
w2 · x + b2 = 0
w3 · x + b3 = 0
```

The layer output:

```text
z = W · x + b
```

can be interpreted as a vector of [[hyperplanes#Signed Score|signed scores]], one per hyperplane:

```text
z = [z1, z2, z3]
```

Each coordinate is a [[hyperplanes#Signed Score|signed score]] saying which side of its neuron's hyperplane the input lies on, and how strongly in the direction measured by that neuron's weight vector.

Together, the hyperplanes partition the input space into regions. With no nonlinear activation, this partition is only an interpretation of the scores; the layer itself is still a single affine map. With nonlinear activations such as [[relu-activation|ReLU]], the regions can determine which neurons are active, letting later layers apply different effective affine maps in different regions.

## What More Neurons Change

Adding more linear neurons does not make the transformation nonlinear. A layer of linear neurons is still one linear or affine map.

More neurons change:

- the number of learned directions measured
- the dimensionality of the output representation
- how much information can be preserved, compressed, or expanded
- the coordinate system available to the next layer

More neurons do not, by themselves, create nonlinear expressivity. For that, the layer needs nonlinear [[activations/activation-functions|activation functions]].

## Linear Layers Still Collapse

If multiple layers use only linear neurons with no nonlinear activations, the layers collapse into one linear transformation:

```text
h = W1 · x
y = W2 · h
y = W2 · W1 · x
```

With biases, they collapse into one affine transformation:

```text
h = W1 · x + b1
y = W2 · h + b2
y = W2 · W1 · x + W2 · b1 + b2
```

This is why depth alone is not enough. Depth becomes expressive when affine transformations are composed with nonlinear activations.

## Related

- [[affine-transformations]]
- [[activations/activation-functions|activation-functions]]
- [[hyperplanes]]
- [[linear-transformations]]
- [[vector-spaces]]
- [[index]]

## Sources

- [[../../raw/personal-notes/linear-transformations-seed|Linear Transformations Seed]]
