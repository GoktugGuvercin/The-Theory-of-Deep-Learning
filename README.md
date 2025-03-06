
### How are tokenization and input embeddings proceeded in BERT ?

Input sequence given to the BERT can be single sentence or a pair of sentences. This provides a methodology to deal with question answering tasks, where SEP token is added between QA sentences. 

Additionally, each token is diversified with respect to sentence ownership. It specifies that the token belongs to the first sentence (question) or the second sentence (answer). In other words, input embeddings are not only the summation of token embeddings and positional embeddings, but also includes the addition of segment embeddings. Three embedding layers are operated to learn from positional, token and segment indices separately. The output of these embedding layers are summed up to generate input embeddings for the model. 

In BERT, WordPiece style tokenizer is used, and it has 30k token vocabulary. The first token of each input sequence is always classification token [CLS] to represent the entire sequence for classification tasks. 

### What is SentencePair ?

SentencePiece is a language-independent subword tokenizer. It treats the text given as input as a stream of characters; it does not rely on language-specific pre-tokenization tools, which are commonly used split the text into small entities like words by whitespaces and punctuation. SentencePiece processes raw text directly, treating spaces as regular characters, which makes it highly useful for the languages without clear word boundaries like Japanese and Chinese. 

In genome sequences, there is no exact definition of word and sentence. Besides, genome sequences are typically represented as a continuous string of nucleotide characters (A, C, G, T), so they don't require any kind of language-specific pre-tokenization. This makes SentencePiece quite suitable for genome. DNABERT-2 is one of the foundation models which specifically opted for this tokenizer.

SentencePiece features two subword segmentation algorithms, which are byte-pair encoding (BPE) and uni-gram language model. It partition the text into subword units, rather than focusing on whole words. This helps to balance vocabulary size and leverages token granularity. Rare or out-of-vocabulary (OOV) words would be handled; they are broken into more frequent subword components. 

### What is Byte-Pair Encoding ?

Byte-pair encoding (BPE) is a subword tokenization strategy used in pre-training GPT, GPT-2 and some BERT variations. It is also called as a compression algorithm used for word segmentation. It extracts unique set of words in the corpus and initialize the vocabulary by taking all unique symbols used to write those words. After getting base vocabulary, BPE started to search for the most frequent pair of existing tokens. Those two tokens are merged and added to vocabulary as a new token. This process is repeated until the desired vocabulary size is obtained by merges. 

For example, in DNABERT-2, DNA sequences are not in the form of linguistic sentences, so there is no word extraction. In that case, the base vocabulary is directly set to four nucleotides {A, T, G, C}. Then, BPE scans through the corpus and picks two tokens that repeats most to merge, as follows:

| Iteration  | Corpus | Operation | Vocabulary
|:----------:|:------:|:---------:|:----------:
| 0  | AACGCACTATATA  | Base Vocabulary | {A, T, C, G}
| 1  | A A C G C A C T A T A T A  | Merged TA | {A, T, C, G, TA}
| 2  | A A C G C A C TA TA TA  | Merged AC | {A, T, C, G, TA, AC}
| 3  | A AC G C AC TA TA TA  | Merged TATA | {A, T, C, G, TA, AC, TATA}


### What is the relationship between Protein-LLMs and Protein Families ?

Each protein family contains many different proteins, that share some conserved domains. For example, CDK1 and PLK2 comprise ***kinase domain***, located in 4-287 and 82-334 nucleotide ranges. This domain for its carrier proteins are not completely same, it actually differentiates. However, some common nucleotide motifs are located in that domain, which provides similar functionalities for those proteins. In that case, they can be involved in related biochemical tasks. This suggests that the proteins in the same family might possibly have common evolutionary origin. To detect this, sequence alignment methods and hidden Markov models are used. Protein language models started to be used to detect these motifs and domains for family classification tasks. 

- [Pfam Database](http://pfam.xfam.org)
- [EMBL-Protein Classification](https://www.ebi.ac.uk/training/online/courses/protein-classification-intro-ebi-resources/protein-classification/what-are-protein-families/#:~:text=A%20protein%20family%20is%20a,smaller%2C%20more%20closely%20related%20groups.)


### What is a protein domain ?

A protein can consists of single domain or multiple domains. Each domain is a distinct and independently folding unit. In other words, how a domain will be folded does not depend on other domains; that is why, the domains are generally called as a self-stabilizing unit. 

Each domain can form a three-dimensional structure, and it is observable in many different proteins. Molecular evolution utilizes protein domains as building blocks, different domains are recombined with each other to create new proteins with different functionalities. At this point, each domain is associated with a specific functionality.