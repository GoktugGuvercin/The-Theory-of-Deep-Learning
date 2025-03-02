
### How are tokenization and input embeddings proceeded in BERT ?

The sequences given to BERT as input can be a single sentence or a pair of sentences. This provides specific methodology to deal with question answering tasks, where SEP token is added between QA sentences in token space. Additionally, each token is diversified with respect to the sentence ownership. In other words, input embeddings are not only the summation of token embeddings and positional embeddings, but also include the addition of segment embeddings. At this point, just like the others, segmentation embeddings are learned representations, but to specify whether the token belongs to first sentence or second sentence.
