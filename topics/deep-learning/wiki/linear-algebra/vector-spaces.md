# Vector Spaces

A vector space is a collection of objects called vectors, together with rules for adding vectors and multiplying them by scalars.

The important idea is not that vectors must be arrows or lists of numbers. The important idea is that the objects behave like vectors: they can be added together, scaled up or down, and still remain inside the same space.

## Required Operations

A vector space must support two operations:

- vector addition: `u + v`
- scalar multiplication: `c v`

These are the operations required by the definition of a vector space. Other operations can be defined on vectors, but they are additional structure rather than part of the minimal vector space definition.

For a set of vectors to form a vector space, addition and scalar multiplication must be well-behaved. For example:

- adding two vectors in the space must produce another vector **in the same space**
- scaling a vector must produce another vector **in the same space**
- there must be a zero vector
- every vector must have an additive inverse
- addition and scaling must follow consistent algebraic rules

## Dot Products Are Extra Structure

Not every vector space has a dot product as part of its definition. A dot product is **not required for something to be a vector space**. It is an **additional operation** that turns a vector space into an inner product space.

The dot product takes two vectors and returns a scalar:

```text
u · v = scalar
```

Once a vector space has a dot product, new geometric ideas become available:

- length or norm
- angle
- orthogonality
- projection
- similarity or alignment

For deep learning, dot products are extremely important, but they are not what makes a vector space a vector space. They are extra structure layered on top of the basic vector-space operations.

Some vector spaces can be given a dot product, but there may be more than one possible choice. The dot product is therefore not automatic from the vector space alone; it is extra structure that must be specified.

Saying that a vector space "does not have a dot product" usually means: the vector space definition has not specified an inner product operation. The vectors can still be added and scaled, but there is no defined rule yet for taking two vectors and producing a scalar that represents length, angle, or alignment.

For example, the set of polynomials with real coefficients is a vector space. Polynomials can be added and scaled:

```text
(x^2 + 1) + (3x) = x^2 + 3x + 1
2(x^2 + 1) = 2x^2 + 2
```

But there is no single automatic dot product for polynomials. Different inner products can be defined on the same polynomial vector space. See [[polynomial-vector-spaces]] for the distinction between a polynomial's scalar input and its vector space dimension.

## Coordinates and Basis

A vector can be written as coordinates, such as:

```text
v = [3, 2]
```

But the coordinate list is not the deepest meaning of the vector. Coordinates describe the vector relative to a chosen [[vector-space-basis|basis]].

For example, in the standard two-dimensional basis:

```text
e1 = [1, 0]
e2 = [0, 1]
```

the vector `[3, 2]` means:

```text
3*e1 + 2*e2
```

So the coordinates `[3, 2]` are instructions: move 3 units in the `e1` direction and 2 units in the `e2` direction.

## Deep Learning Implication

In deep learning, inputs, weights, embeddings, activations, and gradients are usually represented as vectors in vector spaces. The model learns to move and reshape these representations through transformations.

In a neural network, vectors often live in learned representation spaces. The axes of those spaces are not always human-interpretable, but the same linear algebra still applies.

## Related

- [[vector-space-basis]]
- [[polynomial-vector-spaces]]
- [[linear-transformations]]
- [[index]]

## Sources

- [[../../raw/personal-notes/linear-transformations-seed|Linear Transformations Seed]]
