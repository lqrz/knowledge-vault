# Activation Functions

An activation function transforms a neuron's pre-activation value into an activation value.

For a single neuron:

```text
z = w · x + b
a = phi(z)
```

The affine part `w · x + b` produces a scalar [[../../linear-algebra/hyperplanes#Signed Score|signed score]] relative to the neuron's hyperplane. The activation function `phi` decides how that score becomes the neuron's output.

See [[../single-neurons-and-layers|single-neurons-and-layers]] for the effect of one linear neuron versus a layer with many neurons.

For a layer:

```text
z = Wx + b
a = phi(z)
```

where `phi` is usually applied elementwise to each coordinate of `z`.

## Linear Activations

A linear activation has the form:

```text
phi(z) = cz
```

The identity activation is the most common example:

```text
phi(z) = z
```

If every activation in a neural network is linear, then stacking layers does not create a genuinely deep nonlinear model.

For example:

```text
h = W1 · x
y = W2 · h
```

becomes:

```text
y = W2 · (W1 · x)
y = (W2 · W1) · x
```

So two linear layers collapse into one [[../../linear-algebra/linear-transformations|linear transformation]].

If the layers include biases:

```text
h = W1 · x + b1
y = W2 · h + b2
```

then:

```text
y = W2 · (W1 · x + b1) + b2
y = W2 · W1x + W2 · b1 + b2
```

So stacked [[../../linear-algebra/affine-transformations|affine transformations]] collapse into one affine transformation.

## Nonlinear Activations

A nonlinear activation does not preserve vector addition and scalar multiplication.

In general:

```text
phi(u + v) != phi(u) + phi(v)
phi(cu) != c phi(u)
```

This means a nonlinear activation is not a linear transformation of the vector space.

Common nonlinear activations include:

- [[relu-activation|ReLU]]: `max(0, z)`
- [[sigmoid-activation|sigmoid]]: `1 / (1 + exp(-z))`
- [[tanh-activation|tanh]]
- [[gelu-activation|GELU]]
- [[silu-swish-activation|SiLU or Swish]]

## What Happens to the Space

Linear and affine transformations preserve linear structure. They can rotate, scale, shear, project, mix, and shift representations, but they do not bend the representation space into nonlinear shapes.

Nonlinear activations change this. Applied after an affine transformation, they alter the geometry coordinate by coordinate. They can:

- clip regions
- [[activation-saturation-and-gradients|saturate values]]
- gate negative or low values
- bend straight decision boundaries into [[../../linear-algebra/piecewise-linear-functions|piecewise]] or smooth nonlinear boundaries
- make different regions of the input space follow different effective linear maps

For example, [[relu-activation|ReLU]]:

```text
phi(z) = max(0, z)
```

keeps positive values and sends negative values to zero. Geometrically, this folds or clips parts of the representation space against coordinate axes.

## Why Nonlinearity Matters

Without nonlinear activations, depth mostly adds parameterization, not expressivity. A stack of linear maps is still one linear map, and a stack of affine maps is still one affine map.

With nonlinear activations, each layer can change which directions and regions matter before the next affine transformation acts. This allows the network to build complex functions by composing simple transformations:

```text
input -> affine transformation -> nonlinear activation -> affine transformation -> nonlinear activation -> output
```

The **nonlinear activation is what prevents the whole network from collapsing into a single affine transformation**.

## Vector Space Caution

After a nonlinear activation, the output can still be represented as a vector, such as a list of activation values. But the activation function itself is not usually a vector-space-preserving map.

So it is useful to distinguish:

- the representation is still stored as a vector
- the operation that produced it may not be a linear transformation

This is central to deep learning: neural networks repeatedly move between vector representations using affine maps and nonlinear coordinatewise operations.

## Open Questions

- How exactly does each activation function change the geometry of the representation space?
- Why do some activations, such as [[relu-activation|ReLU]] or [[gelu-activation|GELU]], work better than [[sigmoid-activation|sigmoid]] in deep networks?
- How should activation choice be understood in terms of [[activation-saturation-and-gradients|optimization, gradients]], and representation geometry?

## Related

- [[../../linear-algebra/affine-transformations|affine-transformations]]
- [[activation-saturation-and-gradients]]
- [[../../linear-algebra/piecewise-linear-functions|piecewise-linear-functions]]
- [[relu-activation]]
- [[sigmoid-activation]]
- [[tanh-activation]]
- [[gelu-activation]]
- [[silu-swish-activation]]
- [[../single-neurons-and-layers|single-neurons-and-layers]]
- [[../../linear-algebra/linear-transformations|linear-transformations]]
- [[../../linear-algebra/hyperplanes|hyperplanes]]
- [[../../linear-algebra/vector-spaces|vector-spaces]]
- [[../../index|index]]

## Sources

- [[../../../raw/personal-notes/linear-transformations-seed|Linear Transformations Seed]]
