
## How are tokenization and input embeddings proceeded in BERT ?

Input sequence given to the BERT can be single sentence or a pair of sentences. This provides a methodology to deal with question answering tasks, where SEP token is added between QA sentences. 

Additionally, each token is diversified with respect to sentence ownership. It specifies that the token belongs to the first sentence (question) or the second sentence (answer). In other words, input embeddings are not only the summation of token embeddings and positional embeddings, but also includes the addition of segment embeddings. Three embedding layers are operated to learn from positional, token and segment indices separately. The output of these embedding layers are summed up to generate input embeddings for the model. 

In BERT, WordPiece style tokenizer is used, and it has 30k token vocabulary. The first token of each input sequence is always classification token [CLS] to represent the entire sequence for classification tasks. 

## Can you describe classical pipeline of Retrieval Augmented Generation ?

When a query is given as input to a generative large language model, its context is enriched by retrieving relevant, up-to-date information from external resources. In that way, additional guidance would be provided beyond what the model have learnt during training. To achieve that, the context of the query is encoded by a representation model and its corresponding embedding is searched on vector databases. At this point, the most similar embeddings along with relavant documents or text chunks are retrieved.

Vector databases are built at the top of approximate nearest neighbor search to return the results as fast as possible. While this provides and advantage in speed, the retrieved results are not perfectly precise, where reranking enter the picture. Reranker models, at first, encode the query and retrieved passages and then compute pairwise proximity score between them. At the end, the most relevant top K passages are chosen to be included with query in an extended prompt. 

## Why do we split the documents into smaller chunks for vector databases ?

1. *Focused Semantic Search:* Large passages become more comprehensive, and thereby including more than one topic or headline. In contrast, smaller chunks allow for more focused and only one context for specific topic. This simplifies the search and help to find the most relevant, specific pieces of information that our query needs.

2. *Representation Quality:* Large text chunks tend to comprise multiple topics; in that case, its semantic representation extracted by embedding models might be too broad and thereby reducing the similarity to a specific query. In other words, the quality of embeddings degrade because they would try to encapsulate to much information. Spliting creates more distinct and semantically focused chunks; this improves the representation power of their embeddings and thereby making them easily matchable with our query content. This contributes to more accurate retrieval. 

3. *Constraint on Context Window:* Both embedding models and generative LLMs have a finite context window, meaning they can only process a limited amount of text at a time. Splitting documents into smaller chunks ensures that the retrieved information fits within this window.

4. *Resource Management:* Smaller chunks are always more advantageous in memory efficiency. This is directly related to memory footprint of our generative and embedding language models. Self-attention mechanism in transformer models require memory consumption that scale quadratically with sequence length. In that case, working with smaller chunks becomes a more pleased situation. Additionally, smaller, but many number of chunks are more suitable for parallelization. You can dispatch each chunk to one worker, and they all run concurrently. Since large chunks consume more memory, we might only be able to fit very few ones into GPU at a time, utilizing large batch size may not be possible.  

5. *Semantic Coherence:* Some text splitters divide the given text as input into sentences, group them into small parapraphs and then combine them into larger passages depending on their semantic meaning. Such an approach helps us to construct semantically coherent chunks, which amplifies the efficiency of semantic search on vector databases.

References: [Cohere](https://cohere.com/blog/chunking-for-rag-maximize-enterprise-knowledge-retrieval), [LangChain](https://python.langchain.com/docs/concepts/text_splitters/#:~:text=Optimizing%20computational%20resources%3A%20Working%20with,better%20parallelization%20of%20processing%20tasks.)

## Can you describe the concept of connectivity in convolutional and FC layers ?

In fully-connected layers, every output unit interacts with every input unit. In other words, all input units are consumed and processed to generate each output unit. This is described by matrix multiplication between input and weight matrices. 

On the other hand, convolution layers have sparse connectivity, which is also called sparse interaction. An output unit only interacts with small part of the input. A specific part of input map is used to generate an output score. To accomplish this, a smaller kernel than input is chosen and this kernel is convolved with the corresponding part inside the input by sliding-window. This provides the following advantages:

- Smaller kernel means fewer parameters, and sparse connectivity allows us to maintain this. Even input gets larger, kernel size can remain same. This reduces the size of the model, and thereby decreasing the memory complexity. (Smaller Model, Lower memory complexity)

- The generation of output units requires fewer number of calculations. (Lower Computational Complexity)

## What are the differences between RNNs and Transformer ?

* *Core Mechanism:* Recurrent neural networks process the inputs sequentially, dealing with one word or subword at a time. In each time step, they update internal hidden state based on the piece of input that they have just processed. Transformers, on the other hand, consume the input sequence as a whole rather than word by word and process it in parallel. While doing that, they do not rely on recurrence or sequentially updated hidden state, instead leverage attention mechanism. 

* *Parallelization and Training:* The recurrent nature of RNNs makes them computationally expensive: Each step in the sequence require processing the information from previous steps. This makes the steps are connected to each other; each one needs the result of previous one. That is why, its training cannot be parallelizable, which becomes more critical at longer sequences. In transformer layers, the computations for each position are independent from other ones, which facilliates simultaneous computions for all positions. That is why, it is parallelizable.

* *Long-Range Depedencies:* Recurrent neural networks struggle with long sequences. Information flow from early steps has to pass through many intermediate steps, potentially getting diluted or lost as a vanishing gradient problem. The path length for information flow grows linearly. In contrast, transformers are quite successful at capturing long-range dependencies. Self-attention mechanism allows any two tokens in the sequence to interact with directly, so maximum path length for information flow is constant as $O(1)$. 

* *Sequential Order:* The order of tokens is inherently preserved in RNNs, since its architecture scheme is organized in that way. Each time step relies on the output of previous one. Nevertheless, transformers do not have a built-in sense of order, so positional encoding is specifically added to input embeddings to provide information about the position of each token. 

## Do we have weight-sharing in FC and convolution layers ?

Weight sharing is a concept of using same parameters to generate more than one output unit. 

In a fully-connected layer, each neuron has its own weights. In that case, a different set of weights is used to produce each output unit. On the other hand, a convolution layer convolves same kernel values with different parts of the input to generate multiple units in output feature map. So, more than one output unit relies on same kernel values. 

- Each output unit relies on different set of parameters (FC Layer - No Weight Sharing).
- Multiple output units relies on same set of parameters (Conv Layer - Weight Sharing).

## What are conceptual idioms to describe there is no weight sharing in FC layers ?

- Each neuron to generate an output unit has its own weight vectors.
- Each element in weight matrix is used/visited only once to compute layer output. 

## Why is weight sharing so important ?

Weight sharing enables us to reduce the number of trainable parameters in the neural architecture. This allows us to use more number of layers in order to learn more complicated relationships in the data, and the model even dont get larger enough.

## Which factors decrease the number of parameters in CNNs ?

- Sparse connectivity, ssing smaller kernels than input map. Even though input gets larger, kernel size can remain same (This is not the case for FC layers).

- Weight sharing enables us to use same kernel values to generate multiple output units. So, each output unit does not rely on a new set of parameters.

## What are the possible solutions for vanishing gradients ?

- He/Glorot Initialization
- ReLU activation
- Residual Connections
- Layer-wise Pretraining
- Batch Normalization

## What is confusion matrix ?

- *Confusion Matrix:* Classification models classify the samples into predefined categories. To evaluate how well these classifiers perform, we use confusion matrix. It is actually tabular way of visualizing and assessing model performance. 

- *Structure of Confusion Matrix:* It shows how many times the model predicted each class compared to ground truth labels. While the rows represent the predictions, the columns refer to ground-truth labels. Each row and column are defined to denote a specific class index. 

- *Processing Confusion Matrix:* At first, all entries of this matrix are set to zero. Then, each prediction is compared with its true label, and their corresponding entry is incremented by one. Confusion matrix entries are used to calculate counting metrics like TP, FP, FN, and TN. 

## What is the main difference between semantic segmentation and instance segmentation ?

In semantic segmentation, all objects of same class are treated identically in semantic map. On the other hand, instance segmentation tries to differentiate and separate between unique instances of same object type.

## What are two main contributions of residual connections in deep neural architectures ?

- As the number of layers increases, optimization landscape becomes more complicated. This introduces so many local minima and saddle points. Residual connections smooths the loss topology, provides more stable landspace. In that way, non-convex structure of loss surface is alleviated.

- Residual connections provide highways for the gradients, which helps the gradient travel through many layers without being significantly diminished. In mathematical perspective, derivative terms are not only included in severe multiplicative chain of weights, but also maintained as separate units. This counteracts vanishing gradients problem.  

    Let's assume that $y = F(x) + x$ refers to a residual block. If we compute its derivative, $\frac{dy}{dx} = \frac{dF(x)}{dx} + 1$ occurs. Even if $\frac{dF(x)}{dx}$ is small, the overall gradient is increased by the additive $+1$ term, which also helps to preserve the gradient of the following layer as an identity multiplicative factor. 

## What is SentencePiece ?

SentencePiece is a language-independent subword tokenizer. It preserves the most repetitive words, but it splits the rare ones into the spepcific sub-parts that would appear more frequently in text corpus.   

Subword segmentation tools used in this kind of tokenizers generally assume that the input is pre-tokenized into the sequence of words. At this point, whitespaces and punctuations are taken into account. However, the main problem is the fact that this operation poses language-specific dependency. SentencePiece can extract the subwords from raw text, which it treats as a stream of characters. That is why it is called as language-independent tokenizer, which makes it useful for the languages without clear word boundaries like Japanese and Chinese. 

In genome sequences, there is no exact definition of word and sentence. Besides, genome sequences are typically represented as a continuous string of nucleotide characters (A, C, G, T), so they don't require any kind of language-specific pre-tokenization. This makes SentencePiece quite suitable for genome. DNABERT-2 is one of the foundation models which specifically opted for this tokenizer.

SentencePiece features two subword segmentation algorithms, which are byte-pair encoding (BPE) and uni-gram language model. It partition the text into subword units, rather than focusing on whole words. This helps to balance vocabulary size and leverages token granularity. Rare or out-of-vocabulary (OOV) words would be handled; they are broken into more frequent subword components. 

- [SentencePiece Paper](https://arxiv.org/pdf/1808.06226)
- [Hugging Face](https://huggingface.co/docs/transformers/en/tokenizer_summary#sentencepiece)

## What is Byte-Pair Encoding ?

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


## What are the main differences between RNNs and Transformers ?

* In transformer, sentences are processed as a whole rather than word by word. On the other hand,  recurrent architectures process the inputs sequentially, one time step at a time.

* RNNs process sequences sequentially. This is computationally expensive, and time-consuming. Due yo sequential nature of computation, training becomes quite slow. Additionally, this sequentially processing makes them difficult to be parallelized in training and inference stages. This is specified in “Attention is al you need” paper: “This inherently sequential nature precludes parallelization within training examples, which becomes critical at longer sequence lengths, as memory constraints limit batching across examples.” Transformers process the entire sequence at once. That is why they are so efficient, and can be parallelized on modern hardware. This leads to faster computation and better scalability. We can also observe this in “Attention is all you need” paper, where sequential operations and maximum path length complexities is O(1) in transformer, and O(n) in RNNs. 

* In machine translation and text generation tasks, both architectures are used as autoregressive model, so generation of each output token becomes dependent on the previous ones. However, their inherent approaches are different.

    * The design of RNN architectures is naturally sequential. It updates hidden state step by step. The tokens need to be processed in sequence. So, their recurrent nature makes them naturally suited for sequential prediction.

    * Transformers do not have such a design issue; it is capable of parallel processing. In inference stage, masking future tokens in self-attention leads transformer decoders to generate tokens one at a time. Nevertheless, in training stage, it processes entire entire sequence in parallel by teacher forcing.  


## List three main limitations impacting mRNA analysis in deep learning ?

- Many models utilize context windows that are too short to effectively process mRNA sequences. On the other hand, mRNAs are often extremely long. This makes it more difficult to model long-range dependencies.

- Tokenization techniques are simplified and more general; they are not specifically exclusive for biological analysis. That is why, they restricts learning some biological structures from the sequences. 

- Existing models designed for mRNA analysis do not account for all mRNA sequence, instead they focus on only specific mRNA regions. This decreases their generalizability and make them less flexible.

## Why is single nucleotide resolution used and entire mRNA sequence processed in Helix-mRNA model ?

Complete biological information tries to be preserved without any data loss. In addition to this, high granuality is leveraged in sequence modeling.


## What are the roles of mamba and attention layers in Helix-mRNA model ?

Mamba layers enable us to process long sequences in linear time complexity. At that point, they encode and summarize the context of the sequence. 

Attention layers help to focus on sub regions of the sequence. They provide in-context retention property. In that way, codin region structure is preserved. To additionally support codon separation, a new token is introduced. 


## What is the main advantage of mamba over attention systems in transformer ?

- Mamba is a state-space model. It maintains a hidden state, called as memory, and update this compressed state as processing the sequence given as input. It is specifically designed for processing long sequences because it has linear computational complexity.

- On the other hand, attention systems compute the importance of each token relative to all other tokens, which causes approximately $O(n^2)$ computational complexity, where $n$ refers to the sequence length. This makes attention systems computationally expensive for long sequences. The investigation of mRNA molecules is one of the extreme cases where we observe this.

## What is the relationship between Protein-LLMs and Protein Families ?

Each protein family contains many different proteins, that share some conserved domains. For example, CDK1 and PLK2 comprise ***kinase domain***, located in 4-287 and 82-334 nucleotide ranges. This domain for its carrier proteins are not completely same, it actually differentiates. However, some common nucleotide motifs are located in that domain, which provides similar functionalities for those proteins. In that case, they can be involved in related biochemical tasks. This suggests that the proteins in the same family might possibly have common evolutionary origin. To detect this, sequence alignment methods and hidden Markov models are used. Protein language models started to be used to detect these motifs and domains for family classification tasks. 

- [Pfam Database](http://pfam.xfam.org)
- [EMBL-Protein Classification](https://www.ebi.ac.uk/training/online/courses/protein-classification-intro-ebi-resources/protein-classification/what-are-protein-families/#:~:text=A%20protein%20family%20is%20a,smaller%2C%20more%20closely%20related%20groups.)


## What is a protein domain ?

A protein can consists of single domain or multiple domains. Each domain is a distinct and independently folding unit. In other words, how a domain will be folded does not depend on other domains; that is why, the domains are generally called as a self-stabilizing unit. 

Each domain can form a three-dimensional structure, and it is observable in many different proteins. Molecular evolution utilizes protein domains as building blocks, different domains are recombined with each other to create new proteins with different functionalities. At this point, each domain is associated with a specific functionality.