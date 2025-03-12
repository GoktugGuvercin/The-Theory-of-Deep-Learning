
## How are tokenization and input embeddings proceeded in BERT ?

Input sequence given to the BERT can be single sentence or a pair of sentences. This provides a methodology to deal with question answering tasks, where SEP token is added between QA sentences. 

Additionally, each token is diversified with respect to sentence ownership. It specifies that the token belongs to the first sentence (question) or the second sentence (answer). In other words, input embeddings are not only the summation of token embeddings and positional embeddings, but also includes the addition of segment embeddings. Three embedding layers are operated to learn from positional, token and segment indices separately. The output of these embedding layers are summed up to generate input embeddings for the model. 

In BERT, WordPiece style tokenizer is used, and it has 30k token vocabulary. The first token of each input sequence is always classification token [CLS] to represent the entire sequence for classification tasks. 


## Can you describe the concept of connectivity in convolutional and FC layers ?

In fully-connected layers, every output unit interacts with every input unit. This is described by matrix multiplication between input and weight matrix. 

On the other hand, convolution layers have sparse connectivity, which is also called sparse interaction. An output unit only interacts with small part of the input. To accomplish this, a smaller kernel than input is chosen and this kernel is convolved with the corresponding part inside the input by sliding-window. This provides the following advantages:

- Smaller kernels help to store fewer parameters. In that way, memory complexity of the model is reduced. (Smaller Model, Lower memory complexity)

- The generation of output units requires fewer number of calculations. (Lower Computational Complexity)

## Do we have weight-sharing in FC and convolution layers ?

In a fully-connected layer, each neuron has its own weights. In that case, a different set of weights is used to produce each output unit. On the other hand, a convolution layer convolves same kernel values with different parts of the input to generate multiple units in output feature map. So, more than one output unit relies on same kernel values. 

- Each output unit relies on different set of parameters (FC Layer - No Weight Sharing).
- Multiple output units relies on same set of parameters (Conv Layer - Weight Sharing).

## Why is weight sharing so important ?

Weight sharing enables us to reduce the number of trainable parameters in the neural architecture. This allows us to use more number of layers in order to learn more complicated relationships in the data, and the model even dont get larger enough.

### Which factors decrease the number of parameters in CNNs ?

- Using smaller kernels than input map. Even though input gets larger, kernel size can remain same (This is not the case for FC layers).

- Weight sharing enables us to use same kernel values to generate multiple output units. So, each output unit does not rely on a new set of parameters.

### What are the possible solutions for vanishing gradients ?

- He/Glorot Initialization
- ReLU activation
- Residual Connections
- Layer-wise Pretraining

### What is SentencePiece ?

SentencePiece is a language-independent subword tokenizer. It treats the text given as input as a stream of characters; it does not rely on language-specific pre-tokenization tools, which are commonly used split the text into small entities like words by whitespaces and punctuation. SentencePiece processes raw text directly, treating spaces as regular characters, which makes it highly useful for the languages without clear word boundaries like Japanese and Chinese. 

In genome sequences, there is no exact definition of word and sentence. Besides, genome sequences are typically represented as a continuous string of nucleotide characters (A, C, G, T), so they don't require any kind of language-specific pre-tokenization. This makes SentencePiece quite suitable for genome. DNABERT-2 is one of the foundation models which specifically opted for this tokenizer.

SentencePiece features two subword segmentation algorithms, which are byte-pair encoding (BPE) and uni-gram language model. It partition the text into subword units, rather than focusing on whole words. This helps to balance vocabulary size and leverages token granularity. Rare or out-of-vocabulary (OOV) words would be handled; they are broken into more frequent subword components. 

- [SentencePiece Paper](https://arxiv.org/pdf/1808.06226)
- [Hugging Face](https://huggingface.co/docs/transformers/en/tokenizer_summary#sentencepiece)

### What is Byte-Pair Encoding ?

Byte-pair encoding (BPE) is a subword tokenization strategy used in pre-training GPT, GPT-2 and some BERT variations. It is also called as a compression algorithm used for word segmentation. It extracts unique set of words in the corpus and initialize the vocabulary by taking all unique symbols used to write those words. After getting base vocabulary, BPE started to search for the most frequent pair of existing tokens. Those two tokens are merged and added to vocabulary as a new token. This process is repeated until the desired vocabulary size is obtained by merges. 

For example, in DNABERT-2, DNA sequences are not in the form of linguistic sentences, so there is no word extraction. In that case, the base vocabulary is directly set to four nucleotides {A, T, G, C}. Then, BPE scans through the corpus and picks two tokens that repeats most to merge, as follows:

| Iteration  | Corpus | Operation | Vocabulary
|:----------:|:------:|:---------:|:----------:
| 0  | AACGCACTATATA  | Base Vocabulary | {A, T, C, G}
| 1  | A A C G C A C T A T A T A  | Merged TA | {A, T, C, G, TA}
| 2  | A A C G C A C TA TA TA  | Merged AC | {A, T, C, G, TA, AC}
| 3  | A AC G C AC TA TA TA  | Merged TATA | {A, T, C, G, TA, AC, TATA}

- [Hugging Face](https://huggingface.co/learn/nlp-course/en/chapter6/5)
- [DNABERT-2](https://arxiv.org/pdf/2306.15006)


### What are the main differences between RNNs and Transformers ?

* In transformer, sentences are processed as a whole rather than word by word. On the other hand,  recurrent architectures process the inputs sequentially, one time step at a time.

* RNNs process sequences sequentially. This is computationally expensive, and time-consuming. Due yo sequential nature of computation, training becomes quite slow. Additionally, this sequentially processing makes them difficult to be parallelized in training and inference stages. This is specified in “Attention is al you need” paper: “This inherently sequential nature precludes parallelization within training examples, which becomes critical at longer sequence lengths, as memory constraints limit batching across examples.” Transformers process the entire sequence at once. That is why they are so efficient, and can be parallelized on modern hardware. This leads to faster computation and better scalability. We can also observe this in “Attention is all you need” paper, where sequential operations and maximum path length complexities is O(1) in transformer, and O(n) in RNNs. 

* In machine translation and text generation tasks, both architectures are used as autoregressive model, so generation of each output token becomes dependent on the previous ones. However, their inherent approaches are different.

    * The design of RNN architectures is naturally sequential. It updates hidden state step by step. The tokens need to be processed in sequence. So, their recurrent nature makes them naturally suited for sequential prediction.

    * Transformers do not have such a design issue; it is capable of parallel processing. In inference stage, masking future tokens in self-attention leads transformer decoders to generate tokens one at a time. Nevertheless, in training stage, it processes entire entire sequence in parallel by teacher forcing.  


### What are the three main limitations that impact mRNA analysis in deep learning ?

- Many models utilize context windows that are too short to effectively process mRNA sequences. On the other hand, mRNAs are often extremely long. This makes it more difficult to model long-range dependencies.

- Tokenization techniques are simplified and more general; they are not specifically exclusive for biological analysis. That is why, they restricts learning some biological structures from the sequences. 

- Existing models designed for mRNA analysis do not account for all mRNA sequence, instead they focus on only specific mRNA regions. This decreases their generalizability and make them less flexible.

### Why is single nucleotide resolution used and entire mRNA sequence processed in Helix-mRNA model ?

Complete biological information tries to be preserved without any data loss. In addition to this, high granuality is leveraged in sequence modeling.


### What are the roles of mamba and attention layers in Helix-mRNA model ?

Mamba layers enable us to process long sequences in linear time complexity. At that point, they encode and summarize the context of the sequence. 

Attention layers help to focus on sub regions of the sequence. They provide in-context retention property. In that way, codin region structure is preserved. To additionally support codon separation, a new token is introduced. 


### What is the main advantage of mamba over attention systems in transformer ?

- Mamba is a state-space model. It maintains a hidden state, called as memory, and update this compressed state as processing the sequence given as input. It is specifically designed for processing long sequences because it has linear computational complexity.

- On the other hand, attention systems compute the importance of each token relative to all other tokens, which causes approximately $O(n^2)$ computational complexity, where $n$ refers to the sequence length. This makes attention systems computationally expensive for long sequences. The investigation of mRNA molecules is one of the extreme cases where we observe this.

### What is the relationship between Protein-LLMs and Protein Families ?

Each protein family contains many different proteins, that share some conserved domains. For example, CDK1 and PLK2 comprise ***kinase domain***, located in 4-287 and 82-334 nucleotide ranges. This domain for its carrier proteins are not completely same, it actually differentiates. However, some common nucleotide motifs are located in that domain, which provides similar functionalities for those proteins. In that case, they can be involved in related biochemical tasks. This suggests that the proteins in the same family might possibly have common evolutionary origin. To detect this, sequence alignment methods and hidden Markov models are used. Protein language models started to be used to detect these motifs and domains for family classification tasks. 

- [Pfam Database](http://pfam.xfam.org)
- [EMBL-Protein Classification](https://www.ebi.ac.uk/training/online/courses/protein-classification-intro-ebi-resources/protein-classification/what-are-protein-families/#:~:text=A%20protein%20family%20is%20a,smaller%2C%20more%20closely%20related%20groups.)


### What is a protein domain ?

A protein can consists of single domain or multiple domains. Each domain is a distinct and independently folding unit. In other words, how a domain will be folded does not depend on other domains; that is why, the domains are generally called as a self-stabilizing unit. 

Each domain can form a three-dimensional structure, and it is observable in many different proteins. Molecular evolution utilizes protein domains as building blocks, different domains are recombined with each other to create new proteins with different functionalities. At this point, each domain is associated with a specific functionality.