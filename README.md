
### How are tokenization and input embeddings proceeded in BERT ?

Input sequence given to the BERT can be single sentence or a pair of sentences. This provides a methodology to deal with question answering tasks, where SEP token is added between QA sentences. 

Additionally, each token is diversified with respect to sentence ownership. It specifies that the token belongs to the first sentence (question) or the second sentence (answer). In other words, input embeddings are not only the summation of token embeddings and positional embeddings, but also includes the addition of segment embeddings. Three embedding layers are operated to learn from positional, token and segment indices separately. The output of these embedding layers are summed up to generate input embeddings for the model. 

In BERT, WordPiece style tokenizer is used, and it has 30k token vocabulary. The first token of each input sequence is always classification token [CLS] to represent the entire sequence for classification tasks. 


### What is the relationship between Protein-LLMs and Protein Families ?

Each protein family contains many different proteins, that share sequence similarity. They generally contain some conserved domains and motifs. These domains and motifs might provide similar functionalities for those proteins. In that case, they can be involved in related biochemical tasks. This suggests that the proteins in the same family might possibly have common evolutionary origin. To detect this, sequence alignment methods and hidden Markov models are used. Protein language models started to be used to detect these motifs and domains for family classification tasks. 

- [Pfam Database](http://pfam.xfam.org)
- [EMBL-Protein Classification](https://www.ebi.ac.uk/training/online/courses/protein-classification-intro-ebi-resources/protein-classification/what-are-protein-families/#:~:text=A%20protein%20family%20is%20a,smaller%2C%20more%20closely%20related%20groups.)