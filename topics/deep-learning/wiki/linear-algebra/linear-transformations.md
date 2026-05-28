# Linear Transformations

A linear transformation is a structure-preserving function between [[vector-spaces|vector spaces]]. It takes a vector as input and returns another vector as output, while preserving two core operations: vector addition and scalar multiplication.

If `T` is linear, then:

- `T(u + v) = T(u) + T(v)`
- `T(cu) = cT(u)`

**These two properties mean the transformation does not arbitrarily bend the vector space. It can rotate, scale, shear, reflect, project, or collapse dimensions**, but it must do so in a way that keeps straight lines and the origin structurally consistent.

A linear transformation does not have to map a vector back into the same vector space. More generally, it maps from one vector space to another:

```text
T: V -> W
```

If the input and output spaces are the same, then:

```text
T: V -> V
```

This special case is often called a linear operator or endomorphism. Many geometric examples, such as rotating or shearing the plane, are of this form. Neural network layers often map between different vector spaces, such as from `R^784` image inputs to `R^128` hidden activations.

The key requirement is not that the output must live in the original space, but that linear structure is preserved between the input and output spaces:

```text
T(a*u + b*v) = a*T(u) + b*T(v)
```

## Vectors as Objects in a Space

A vector can be understood in several compatible ways:

- as an ordered list of numbers, such as `[2, -1, 3]`
- as an arrow from the origin
- as a point in a coordinate system
- as a weighted combination of basis directions

For deep learning, the most useful view is often: a vector is a representation. Inputs, hidden states, embeddings, weights, gradients, and activations can all be represented as vectors in some vector space.

## What Transformation Means

When a vector is transformed, its coordinates change according to a rule. In linear algebra, that rule is usually represented by a matrix.

For example, if:

```text
x = [x1, x2]
```

and:

```text
A = [[a, b],
     [c, d]]
```

then the transformed vector is:

```text
Ax = [a*x1 + b*x2,
      c*x1 + d*x2]
```

The matrix `A` defines how the input coordinates are recombined to produce the output coordinates. Each output coordinate is a weighted sum of the input coordinates.

This is the bridge to a single neuron: before applying a nonlinearity, a neuron computes a weighted sum of its inputs. A whole layer computes many weighted sums at once, which is exactly a matrix-vector transformation.

## Geometric Intuition

A linear transformation changes the space by moving the basis vectors. Once you know where the basis vectors go, you know where every vector goes.

In two dimensions, the standard basis vectors are:

```text
e1 = [1, 0]
e2 = [0, 1]
```

Any vector `[x, y]` can be written as:

```text
x*e1 + y*e2
```

If a transformation sends `e1` to one new direction and `e2` to another, then every vector moves according to the same combination of those transformed basis vectors.

This matters because a matrix can be read as a compact description of where basis vectors land. The columns of a matrix are the transformed basis vectors.

## What Linear Transformations Can Do

Linear transformations can:

- scale a vector
- rotate a vector
- reflect a vector
- shear the space
- project vectors onto a lower-dimensional subspace
- collapse many vectors into the same output
- change coordinates from one basis to another

They cannot, by themselves:

- curve the space
- create nonlinear decision boundaries
- condition behavior on thresholds
- represent interactions that require nonlinear bending

This limitation is central to deep learning. Stacking linear transformations without nonlinear [[../neural-networks/activations/activation-functions|activations]] is still equivalent to one linear transformation. Neural network layers usually add a bias term, which makes them [[affine-transformations|affine transformations]] rather than strictly linear transformations.

## Deep Learning Implication

A neural network layer transforms representations. Each layer maps vectors from one representation space into another:

```text
input vector -> linear/affine transformation -> activation -> new representation
```

The linear part mixes and reorients information. The affine version also shifts the result with a bias term. The nonlinear part changes what kinds of patterns can be represented. Deep learning is largely about learning useful sequences of these transformations.

## Open Questions

- How should vector spaces be interpreted when the coordinates are learned features rather than human-defined axes?
- What exactly does a neuron represent geometrically: a direction, a separator, a feature detector, or all of these depending on context?
- How do nonlinear activation functions change the geometry created by linear transformations?

## Related

- [[index]]
- [[../neural-networks/activations/activation-functions|activation-functions]]
- [[affine-transformations]]
- [[vector-spaces]]
- [[vector-space-basis]]

## Sources

- [[../../raw/personal-notes/linear-transformations-seed|Linear Transformations Seed]]
