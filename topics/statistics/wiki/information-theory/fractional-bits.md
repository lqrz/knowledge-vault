# Fractional Bits

Information quantities can be fractional even though individual binary messages use whole bits.

The simplest reason is averaging. If half of messages are one bit and half are two bits, the average length is $1.5$ bits.

## Ideal Code Lengths

The ideal code length for an event with probability $p(x)$ is:

$$
L(x) = \log_2 \frac{1}{p(x)}
$$

This value is often not an integer. A single codeword must be rounded to a whole number of bits, which creates overhead.

## Block Coding Intuition

When many events are encoded together, fractional ideal lengths add across the block. Rounding overhead is spread over many events.

As the number of encoded events grows, the average bits per event can approach the [[entropy]] of the distribution.

This is the practical sense in which fractional bits are meaningful: they describe asymptotic average information, not necessarily the length of one standalone codeword.

## Related

- [[prefix-codes-and-entropy]]
- [[entropy]]
- [[cross-entropy]]

## Sources

- [[../../raw/articles/colah/visual-information-theory|Visual Information Theory]] by Christopher Olah, 2015.
