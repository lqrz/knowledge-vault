# Recursive Neural Networks

Recursive neural networks apply the same neural module over a tree or hierarchical structure.

They are related to [[recurrent-neural-networks|recurrent neural networks]] because both reuse parameters across multiple positions in a computation graph. The difference is the graph shape:

- recurrent neural networks reuse a cell along a sequence or time axis
- recursive neural networks reuse a cell across a tree or hierarchy

## Basic Idea

In a recurrent neural network, the hidden state is updated from the previous time step:

$$
h_t = F(x_t, h_{t-1})
$$

In a recursive neural network, a parent representation is computed from child representations:

$$
h_{\text{parent}} = F(h_{\text{left}}, h_{\text{right}})
$$

For a binary tree, the same function $F$ can be applied at every internal node. Leaf nodes start from input embeddings, and internal nodes compose those embeddings into larger phrase, expression, or subtree representations.

## Example: Syntax Tree

For a sentence parse tree, a recursive neural network can build:

- word representations at leaves
- phrase representations at internal nodes
- a sentence representation at the root

For example, the representation for "very good" could be computed from the representations of "very" and "good." Then that phrase representation could be composed with another phrase higher in the tree.

## Parameter Sharing

Like an RNN, a recursive neural network uses the same parameters at many places.

For a simple binary composition function:

$$
h_{\text{parent}} = \phi(W[h_{\text{left}}, h_{\text{right}}] + b)
$$

the same $W$ and $b$ are reused for every parent node in the tree.

This lets the model learn a general composition rule rather than separate parameters for each tree position.

## Recursive Versus Recurrent

| Property | Recurrent neural network | Recursive neural network |
|---|---|---|
| Computation shape | Chain | Tree |
| Typical input | Sequence | Hierarchy or parse tree |
| Reuse axis | Time or position | Tree nodes |
| Example task | Language modeling, tagging, time series | Syntax-aware composition, expression trees |
| State update | $h_t = F(x_t, h_{t-1})$ | $h_p = F(h_l, h_r)$ |

The names are easy to confuse. "Recurrent" usually means repeated over time. "Recursive" usually means repeated over a recursively defined structure.

## Training

Training uses backpropagation over the unrolled tree computation graph. This is analogous to [[backpropagation-through-time|backpropagation through time]], but the reverse pass follows tree edges instead of a time chain.

Every use of the shared composition parameters contributes to the same parameter gradient. Those contributions are summed before one optimizer update, just as shared recurrent weights are handled in BPTT.

## Limitations

Recursive neural networks require a tree or hierarchy. If the tree is supplied by an external parser, errors in that parser can affect the model.

They are also less natural for tasks where the relevant structure is not tree-like or where a model should learn its own flexible attention pattern over all tokens.

Modern transformer models often replace explicit recursive composition with attention-based token interactions, but recursive neural networks remain conceptually important for understanding tree-structured neural computation.

## Related

- [[recurrent-neural-networks]]
- [[backpropagation-through-time]]
- [[rnn-forward-propagation]]
- [[topics/deep-learning/wiki/linear-algebra/affine-transformations|affine-transformations]]
- [[topics/deep-learning/wiki/neural-networks/activations/activation-functions|activation-functions]]

## Sources

- [Deep Learning Book: Chapter 10, Sequence Modeling: Recurrent and Recursive Nets](https://www.deeplearningbook.org/contents/rnn.html)
