# Affine Transformations

An affine transformation is a linear transformation followed by a translation.

It has the form:

```text
f(x) = Ax + b
```

The `Ax` part is linear. The `+ b` part is a translation or shift.

## Affine vs Linear

Strictly speaking, affine transformations are not [[linear-transformations|linear transformations]] unless their bias term is zero.

A strictly linear transformation must preserve addition and scalar multiplication, which implies that it must map the zero vector to the zero vector:

```text
T(0) = 0
```

For an affine transformation:

```text
f(0) = A0 + b = b
```

So if `b != 0`, the transformation does not send zero to zero, which means it is not strictly linear.

The special case:

```text
f(x) = Ax + 0
```

is linear.

## Geometric Intuition

The linear part changes directions, scales, mixes, rotates, shears, projects, or reflects vectors. The bias shifts the result.

This shift matters because it allows boundaries and representations to move away from the origin. Without a bias term, every strictly linear map must keep the origin fixed.

## Connection to Neural Network Layers

Neural network layers usually compute:

```text
y = Wx + b
```

The `Wx` part is linear. The `+ b` part is a bias term. Adding bias makes the transformation affine rather than strictly linear.

This distinction matters:

- `Wx` changes directions, scales, mixes, and projects input features.
- `b` shifts the result, moving decision [[hyperplanes|hyperplanes]] away from the origin.

## Connection to a Single Neuron

A single neuron receives an input vector `x` and has a weight vector `w`. See [[single-neurons-and-layers]] for a fuller treatment. It computes:

```text
z = w · x + b
```

The dot product `w · x` measures a weighted alignment between the input vector and the neuron's weight vector. The result `z` is a scalar pre-activation and can be interpreted as a [[hyperplanes#Signed Score|signed score]] relative to the neuron's hyperplane.

Conceptually:

- `x` is the input representation.
- `w` defines a direction in input space.
- `w · x` measures how much `x` aligns with that direction.
- `b` shifts the threshold.
- an activation function turns the affine score into a nonlinear response.

Without the activation function, the neuron is only computing an affine transformation from a vector to a scalar.

## Deep Learning Implication

A neural network layer transforms representations. Each layer maps vectors from one representation space into another:

```text
input vector -> affine transformation -> activation -> new representation
```

The affine part mixes, reorients, and shifts information. The nonlinear activation changes what kinds of patterns can be represented.

Stacking affine transformations without nonlinear [[../neural-networks/activations/activation-functions|activations]] is still equivalent to a single affine transformation. Deep networks become expressive because they alternate affine transformations with nonlinear activation functions.

## Related

- [[linear-transformations]]
- [[../neural-networks/activations/activation-functions|activation-functions]]
- [[single-neurons-and-layers]]
- [[hyperplanes]]
- [[vector-spaces]]
- [[vector-space-basis]]
- [[index]]

## Sources

- [[../../raw/personal-notes/linear-transformations-seed|Linear Transformations Seed]]
