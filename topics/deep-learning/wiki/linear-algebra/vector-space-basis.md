# Vector Space Basis

A basis is a minimal set of vectors that can generate every vector in a [[vector-spaces|vector space]] through linear combinations.

Basis vectors define the coordinate system used to describe vectors. Changing the basis does not necessarily change the underlying vector. It changes how the vector is described.

## Basis Vectors

A set of basis vectors must satisfy two conditions:

- spanning: every vector in the space can be built from them
- linear independence: none of the basis vectors is redundant

If vectors `b1` and `b2` form a basis for a two-dimensional space, then any vector `v` in that space can be written as:

```text
v = a*b1 + c*b2
```

The numbers `a` and `c` are the coordinates of `v` in that basis.

## Spanning

A set of vectors spans a space if linear combinations of those vectors can reach every vector in the space.

In two dimensions, the standard basis vectors span the plane:

```text
e1 = [1, 0]
e2 = [0, 1]
```

Any point in the plane can be reached by scaling and adding these two directions.

If two vectors point in the same direction, they do not span the plane. They only span a line.

## Linear Independence

A set of vectors is linearly independent if no vector in the set can be built from the others.

In two dimensions:

```text
[1, 0] and [0, 1]
```

are linearly independent, because neither one can be scaled to produce the other.

But:

```text
[1, 0] and [2, 0]
```

are linearly dependent, because `[2, 0]` is just `2 * [1, 0]`. The second vector adds no new direction.

## Why Basis Vectors Matter

This is central to understanding [[linear-transformations]]. A matrix can be understood by asking where it sends the basis vectors. Once the transformed basis vectors are known, every other transformed vector follows from linearity.

A layer transforms an input vector by recombining its coordinates. Informally, it learns new directions in representation space that are useful for the task.

For a single neuron:

```text
z = w · x + b
```

the weight vector `w` defines a direction in the input vector space. The dot product measures how much the input vector `x` aligns with that direction.

## Related

- [[vector-spaces]]
- [[linear-transformations]]
- [[index]]

## Sources

- [[../../raw/personal-notes/linear-transformations-seed|Linear Transformations Seed]]
