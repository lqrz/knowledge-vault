# Coursera Sequence Models: Beam Search Video Transcript

Source: Coursera > Sequence Models. User-provided transcript about the beam search algorithm for sequence decoding.

## Transcript

In this video, you learn about the beam search algorithm. For machine translation, given an input French sentence, you do not want to output a random English translation; you want to output the best and most likely English translation. The same is true for speech recognition: given an input audio clip, you do not want a random transcript, but the best, or most likely, text transcript.

Beam search is the most widely used algorithm for this. Consider the French sentence "Jane visite l'Afrique en Septembre", hopefully translated into "Jane visits Africa in September".

The first thing beam search does is pick possible first words of the English translation. Suppose the vocabulary has 10,000 words, ignoring capitalization. In the first step, the encoder reads the input sentence and the decoder evaluates:

$$
p(y_1 \mid x)
$$

Greedy search would keep only the single most likely first word. Beam search keeps multiple alternatives. Beam search has a parameter $B$, the beam width. If $B = 3$, beam search keeps the top three alternatives at a time. Suppose the top three first words are "in", "Jane", and "September". Beam search stores all three possibilities.

To perform this first step, run the input French sentence through the encoder, then run the first decoder step to produce a softmax over the 10,000 vocabulary items. Keep the top $B$ outputs.

In the second step, for each of the retained first-word choices, beam search considers every possible second word. If the first word was "in", it evaluates the probability of each possible second word given the input sentence and the first word "in":

$$
p(y_2 \mid x, y_1 = \text{"in"})
$$

The decoder input is hard-wired to the first chosen word. The same is done for the first-word candidates "Jane" and "September".

The goal at this step is not just the most likely second word. It is the most likely pair of first and second words:

$$
p(y_1, y_2 \mid x) = p(y_1 \mid x)p(y_2 \mid x, y_1)
$$

For each retained first word, beam search multiplies the saved probability of the first word by the conditional probability of the second word. With beam width $3$ and vocabulary size $10{,}000$, it evaluates $3 \times 10{,}000 = 30{,}000$ candidate two-word prefixes, then keeps the top three.

For example, the top three two-word candidates might be "in September", "Jane is", and "Jane visits". This means "September" by itself has been rejected as a first-word prefix, although the beam still keeps three total partial translations.

Because $B = 3$, each step uses three decoder copies or three partial hypotheses. Each copy evaluates a full softmax over the vocabulary. The algorithm does not instantiate 30,000 neural network copies; it uses the $B$ current hypotheses to efficiently evaluate the next-word distribution for each.

The third step repeats the same process. Given the retained prefixes "in September", "Jane is", and "Jane visits", beam search considers every possible third word for each prefix. For the prefix "in September", the decoder is conditioned on both words and evaluates:

$$
p(y_3 \mid x, y_1 = \text{"in"}, y_2 = \text{"September"})
$$

The same is done for "Jane is" and "Jane visits". Beam search again keeps the top three longer prefixes. Possible retained prefixes might include "in September Jane", "Jane is visiting", and "Jane visits Africa".

The process continues one word at a time until the decoder emits an end-of-sentence symbol. Ideally, beam search eventually returns a likely output sentence such as "Jane visits Africa in September."

If the beam width is $B = 1$, beam search becomes greedy search. By considering multiple possibilities, such as $B = 3$, $B = 10$, or another value, beam search usually finds a much better output sentence than greedy search.
