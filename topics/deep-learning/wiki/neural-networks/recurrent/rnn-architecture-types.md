# RNN Architecture Types

Recurrent neural networks can be arranged according to the relationship between input length $T_x$ and output length $T_y$.

The same recurrent building block can support sequence labeling, sequence classification, sequence generation, and sequence-to-sequence translation-style tasks.

## One-to-One

One-to-one maps one input to one output:

$$
x \rightarrow y
$$

This is the ordinary feedforward neural-network setting. An RNN is usually unnecessary when there is no sequence structure.

## One-to-Many

One-to-many maps one input, or sometimes a null input, to a sequence:

$$
x \rightarrow \hat{y}_1, \hat{y}_2, \ldots, \hat{y}_{T_y}
$$

Example tasks:

- music generation conditioned on a genre or first note
- unconditional sequence generation with a zero or learned start input
- image captioning in encoder-decoder variants, where an image representation conditions a word sequence

In generation settings, the output from one step is often fed into the next step as an input:

$$
\hat{y}_{t-1} \rightarrow a_t \rightarrow \hat{y}_t
$$

This makes generation autoregressive: each generated item conditions later generated items.

## Many-to-One

Many-to-one maps an input sequence to one output:

$$
x_1, x_2, \ldots, x_{T_x} \rightarrow y
$$

Example tasks:

- sentiment classification for a movie review
- sequence-level classification
- assigning a single label to a time series

The RNN reads the sequence and uses the final hidden state, or a pooled representation of hidden states, to make one prediction.

In the simplest many-to-one design, the RNN still computes a hidden state at every input step, but the output head is applied only after the final relevant input:

$$
\hat{y} = g_y(W_y a_{T_x} + b_y)
$$

The network does not have to discover when to emit the output if the sequence boundary is provided by the data loader, padding mask, or end-of-sequence token. Training code knows which hidden state is the last real input state and computes the loss only from that prediction.

One could attach an output head at every time step and ignore all but the last output, but that is usually just an implementation variant. Conceptually, many-to-one means the loss is applied to one sequence-level prediction, not to every position.

## Many-to-Many With Equal Lengths

Many-to-many with equal lengths maps each input position to an output position:

$$
T_x = T_y
$$

$$
x_1, x_2, \ldots, x_T \rightarrow \hat{y}_1, \hat{y}_2, \ldots, \hat{y}_T
$$

Example tasks:

- named entity recognition
- part-of-speech tagging
- per-frame labels for a time series

This is the architecture used in the basic [[recurrent-neural-networks|RNN]] sequence-labeling example.

## Many-to-Many With Different Lengths

Some sequence tasks have input and output sequences with different lengths:

$$
T_x \neq T_y
$$

Example tasks:

- machine translation
- summarization
- speech recognition

A common RNN pattern is an encoder-decoder architecture:

- the encoder reads the input sequence and compresses it into a representation
- the decoder generates the output sequence from that representation

This lets the model read a source sequence first and then generate a target sequence whose length does not have to match the input length.

In machine translation, this architecture can be interpreted as a [[conditional-language-models|conditional language model]]: the decoder assigns probabilities to target-language sequences conditioned on the encoded source sentence. At inference time, the model then needs a [[sequence-decoding-search|decoding search]] algorithm such as [[beam-search]] to choose a high-probability output sequence.

## Attention Extension

Encoder-decoder RNNs can struggle when the encoder compresses the whole source sequence into a single fixed representation. Attention mechanisms address this by letting the decoder look back at different encoder states while generating each output step.

This is why attention-based architectures are often discussed as an extension of sequence-to-sequence RNNs rather than as one of the simple length-pattern diagrams.

## Related

- [[recurrent-neural-networks]]
- [[rnn-forward-propagation]]
- [[backpropagation-through-time]]
- [[lstm-networks]]
- [[conditional-language-models]]
- [[sequence-decoding-search]]
- [[beam-search]]

## Sources

- [[../../../raw/courses/coursera/sequence-models/types-of-rnn-video-transcript|Coursera Sequence Models: Types of RNN Video Transcript]]
- [[../../../raw/courses/coursera/sequence-models/conditional-language-modeling-and-search-video-transcript|Coursera Sequence Models: Conditional Language Modeling and Search Video Transcript]]
- [[../../../raw/courses/coursera/sequence-models/beam-search-video-transcript|Coursera Sequence Models: Beam Search Video Transcript]]
