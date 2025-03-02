
### How are tokenization and input embeddings proceeded in BERT ?

The sequences given to BERT as input can be a single sentence or a pair of sentences. This provides specific methodology to deal with question answering tasks, where SEP token is added between QA sentences in token space. Additionally, each token is diversified with respect to the sentence ownership. In other words, input embeddings are not only the summation of token embeddings and positional embeddings, but also include the addition of segment embeddings. At this point, just like the others, segmentation embeddings are learned representations, but to specify whether the token belongs to first sentence or second sentence.


### What is the relationship between Protein-LLMs and Protein Families ?

Each protein family contains many different proteins, that share sequence similarity. They generally contain some conserved domains and motifs. These domains and motifs might provide similar functionalities for those proteins. In that case, they can be involved in related biochemical tasks. This suggests that the proteins in the same family might possibly have common evolutionary origin. To detect this, sequence alignment methods and hidden Markov models are used. Protein language models started to be used to detect these motifs and domains for family classification tasks. 