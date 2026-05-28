# Coursera Sequence Models: Conditional Language Modeling and Search Video Transcript

Source: Coursera > Sequence Models. User-provided transcript about sequence-to-sequence machine translation as conditional language modeling and why decoding needs search rather than random sampling or greedy word selection.

## Transcript

There are some similarities between the sequence-to-sequence machine translation model and the language models that you worked with in the first week of this course, but there are some significant differences as well.

You can think of machine translation as building a conditional language model. In language modeling, the model estimates the probability of a sentence. It can also be used to generate novel sentences. The first input can be a vector of all zeros, and later inputs are the previous outputs being generated.

The machine translation model has an encoder network and a decoder network. The decoder looks similar to the language model. The difference is that, instead of always starting with a vector of all zeros, the decoder starts from a representation produced by the encoder from the input sentence.

That is why this is a conditional language model. Instead of modeling the probability of any sentence, it models the probability of an output English translation conditioned on an input French sentence. For example, it estimates the probability of "Jane is visiting Africa in September" conditioned on "Jane visite l'Afrique en septembre."

When applying this model to translation, given the French input sentence, the model assigns probabilities to different English translations. But the goal is not to sample outputs at random from $p(y \mid x)$. One sample might be a good translation, another might be awkward but acceptable, and another might be a bad translation.

For machine translation, the desired output is the English sentence $y$ that maximizes the conditional probability:

$$
\arg\max_y p(y \mid x)
$$

Developing a machine translation system therefore requires an algorithm that can find, or approximately find, the value of $y$ that maximizes this probability. The most common algorithm for this is beam search.

One possible alternative is greedy search. Greedy search generates the first word by picking the most likely first word according to the conditional language model, then picks the most likely second word given the first word, and continues one word at a time.

The actual objective is to pick the entire sequence $y_1, y_2, \ldots, y_{T_y}$ that maximizes the joint probability of the whole output sequence. Greedy search does not generally work well because choosing the locally best next word can lead to a globally worse sentence.

For example, "Jane is visiting Africa in September" may be a better translation than "Jane is going to be visiting Africa in September." But if the model has already chosen "Jane is", the word "going" may be locally more probable than "visiting", even though the full sentence that follows is less succinct and less optimal under the model.

The number of possible output sentences is exponentially large. If the vocabulary has 10,000 words and the system considers translations up to length 10, then there are $10000^{10}$ possible length-10 sentences. Exhaustive enumeration is impossible.

Therefore, translation uses an approximate search algorithm. It is not guaranteed to find the exact $y$ that maximizes $p(y \mid x)$, but it usually does well enough in practice.

To summarize, machine translation can be posed as conditional language modeling. A major difference from earlier language modeling examples is that instead of generating a random sentence, translation tries to find the most likely output sentence. Because the set of possible sentences is too large to enumerate, decoding uses a search algorithm such as beam search.
