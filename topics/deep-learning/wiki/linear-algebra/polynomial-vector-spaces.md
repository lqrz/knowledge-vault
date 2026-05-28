# Polynomial Vector Spaces

Polynomials can form vector spaces even though each polynomial may take only a single scalar input.

This is possible because vector space dimension is not the same thing as input dimension.

## Why Polynomials Are Vectors

A vector is not defined by having multiple input dimensions. A vector is defined by belonging to a set where vectors can be added and multiplied by scalars while staying inside the same set.

For example:

```text
f(x) = x^2 + 1
g(x) = 3x
```

Polynomials can be added:

```text
f(x) + g(x) = x^2 + 3x + 1
```

Polynomials can also be scaled:

```text
2f(x) = 2x^2 + 2
```

So polynomials behave like vectors under the required vector space operations from [[vector-spaces]].

## Input Dimension vs Vector Space Dimension

The input dimension of a function is the dimension of the value it consumes. A polynomial like:

```text
f(x) = x^2 + 1
```

takes one scalar input `x`.

The vector space dimension is different. It is the number of [[vector-space-basis|basis]] directions needed to describe elements of the space.

For polynomials of degree at most 2, a basis is:

```text
1, x, x^2
```

Any polynomial of degree at most 2 can be written as:

```text
a*1 + b*x + c*x^2
```

For example:

```text
3 + 5x - 2x^2 = 3*1 + 5*x + (-2)*x^2
```

That space is 3-dimensional, even though each polynomial takes a single scalar input.

For all real polynomials, a basis is:

```text
1, x, x^2, x^3, ...
```

So the space of all real polynomials is infinite-dimensional.

## Dot Product Implication

The fact that polynomials form a vector space does not automatically provide a dot product.

One possible inner product for polynomials is:

```text
<f, g> = integral from -1 to 1 of f(x)g(x) dx
```

Another possible inner product could use a different interval or weighting function. The vector space alone does not choose between them.

## Key Distinction

```text
input dimension of a function != vector space dimension
```

A polynomial can take one scalar input, but as an object inside a vector space, it may require many or infinitely many basis components to describe.

## Related

- [[vector-spaces]]
- [[vector-space-basis]]

## Sources

- [[../../raw/personal-notes/linear-transformations-seed|Linear Transformations Seed]]
