
### How are tokenization and input embeddings proceeded in BERT ?

Input sequence given to the BERT can be single sentence or a pair of sentences. This provides a methodology to deal with question answering tasks, where SEP token is added between QA sentences. 

Additionally, each token is diversified with respect to sentence ownership. It specifies that the token belongs to the first sentence (question) or the second sentence (answer). In other words, input embeddings are not only the summation of token embeddings and positional embeddings, but also includes the addition of segment embeddings. Three embedding layers are operated to learn from positional, token and segment indices separately. The output of these embedding layers are summed up to generate input embeddings for the model. 

In BERT, WordPiece style tokenizer is used, and it has 30k token vocabulary. The first token of each input sequence is always classification token [CLS] to represent the entire sequence for classification tasks. 


### What is the relationship between Protein-LLMs and Protein Families ?

Each protein family contains many different proteins, that share some conserved domains. For example, CDK1 and PLK2 comprise ***kinase domain***, located in 4-287 and 82-334 nucleotide ranges. This domain for its carrier proteins are not completely same, it actually differentiates. However, some common nucleotide motifs are located in that domain, which provides similar functionalities for those proteins. In that case, they can be involved in related biochemical tasks. This suggests that the proteins in the same family might possibly have common evolutionary origin. To detect this, sequence alignment methods and hidden Markov models are used. Protein language models started to be used to detect these motifs and domains for family classification tasks. 

- [Pfam Database](http://pfam.xfam.org)
- [EMBL-Protein Classification](https://www.ebi.ac.uk/training/online/courses/protein-classification-intro-ebi-resources/protein-classification/what-are-protein-families/#:~:text=A%20protein%20family%20is%20a,smaller%2C%20more%20closely%20related%20groups.)


### What is a protein domain ?

A protein can consists of single domain or multiple domains. Each domain is a distinct and independently folding unit. In other words, how a domain will be folded does not depend on other domains; that is why, the domains are generally called as a self-stabilizing unit. 

Each domain can form a three-dimensional structure, and it is observable in many different proteins. Molecular evolution utilizes protein domains as building blocks, different domains are recombined with each other to create new proteins with different functionalities. At this point, each domain is associated with a specific functionality.