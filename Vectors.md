<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [NLP Vectors](#nlp-vectors)
  - [Why We Need Them](#why-we-need-them)
  - [The Evolution](#the-evolution)
  - [How They Work (The Core Idea)](#how-they-work-the-core-idea)
  - [Key Properties](#key-properties)
  - [What They Enable](#what-they-enable)
  - [Semantic Vectors](#semantic-vectors)
  - [Syntactic Vectors](#syntactic-vectors)
  - [How They Relate](#how-they-relate)
  - [Other NLP Vectors](#other-nlp-vectors)
  - [By Granularity](#by-granularity)
  - [By Learning Method](#by-learning-method)
  - [By Context Awareness](#by-context-awareness)
  - [By Modality / Domain](#by-modality--domain)
  - [By Application](#by-application)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# NLP Vectors

An **[NLP](./NaturalLanguageProcessing.md) vector** (also called a **word 
embedding** or **text embedding**) is a numerical representation of a linguistic 
unit — a word, sentence, or document — as a list of real-valued numbers 
(e.g., `[0.25, -1.30, 0.87, …]`) in a continuous, high-dimensional space. The 
core idea: **words that appear in similar contexts get similar vectors**, so 
geometric proximity in this space reflects semantic and syntactic similarity.  

## Why We Need Them

Machine learning models can't process raw text — they need numbers. Vectors are
the bridge that lets us apply math (dot products, cosine similarity, neural
network operations) to language.

## The Evolution

The field progressed through roughly four phases:

| **Phase**                    | **Era**      | **Key Methods**                    | **Character**                                                    |
|:-----------------------------|:-------------|:-----------------------------------|:-----------------------------------------------------------------|
| Statistical / Sparse         | 1950s–2000s  | One-hot, Bag-of-Words, TF-IDF, LSA | High-dimensional, mostly zeros, no inherent relationships        |
| Static Dense                 | 2013–2018    | Word2Vec, GloVe, FastText          | Low-dimensional, dense, one fixed vector per word                |
| Contextual (Dynamic)         | 2018–present | ELMo, BERT, GPT, LLMs              | Different vector per occurrence, conditioned on surrounding text |
| Sentence/Document Embeddings | 2015–present | Doc2Vec, Sentence-BERT, CLIP       | Embed entire sentences or documents as single vectors            |

## How They Work (The Core Idea)

The foundational principle is the **distributional hypothesis** (Harris, 1954):
*"You shall know a word by the company it keeps."* If two words tend to appear
in similar contexts, they likely have similar meanings.

- **Word2Vec** (2013, Tomas Mikolov et al.) made this practical by training a
  shallow neural network to **predict a word from its neighbors** (CBOW) or *
  *predict neighbors from a word** (Skip-gram). The learned hidden-layer weights
  *are* the word vectors — a side effect of the prediction task.
- **GloVe** (2014) took a similar approach but used **global co-occurrence
  statistics** rather than local context windows.
- **Transformer-based models** (BERT, GPT) went further: they compute each
  word's vector by attending to *all* other words in the sequence, producing *
  *context-dependent** representations that solve polysemy ("bank" as river vs.
  financial institution).

## Key Properties

- **Dimensionality:** Typically 100–4096 dimensions  
  more dimensions = finer-grained distinctions, but more compute
- **Similarity:** Measured via **cosine similarity** or **dot product**, the
  angle between two vectors tells you how related the words are.
- **Linear structure:** Vectors capture relationships as *offsets* (e.g.,
  `king − man + woman ≈ queen`), enabling analogical reasoning.
- **Compositionality:** Vectors can be combined (averaged, summed, concatenated)
  to represent phrases and documents.

## What They Enable

Semantic search, machine translation, sentiment analysis, text classification,
question answering, recommendation systems, and — at the largest scale — the
foundation for **LLMs**, where the embedding layer is the first step in
transforming raw tokens into the representations that the rest of the network
operates on.

Semantic vs. syntactic distinction is one lens for understanding *what* these
vectors capture; the evolution above is the story of *how* we got from crude
one-hot codes to the rich, contextual representations that power modern NLP.

In NLP, **semantic vectors** and **syntactic vectors** are two kinds of
information that word embeddings can encode.

## Semantic Vectors

A **semantic vector** represents the *meaning* of a word or phrase in a
continuous vector space, where words with similar meanings are positioned close
together (typically measured by cosine similarity). This is grounded in the *
*distributional hypothesis**: words that appear in similar contexts tend to have
similar meanings.

Example: `king − man + woman ≈ queen` — the offset captures a *meaning-based*
analogy.

## Syntactic Vectors

A **syntactic vector** encodes the *grammatical/structural role* a word plays —
its part of speech, its position in a dependency tree, or its relation to
neighboring words.

Example: `quickly − quick + slow ≈ slowly` — the offset captures a *grammatical*
relation (adjective → adverb).

## How They Relate

The key insight from Word2Vec and similar models is that a **single learned
vector captures both** semantic and syntactic regularities simultaneously — they
are entangled in the same embedding space. The model doesn't explicitly separate
them; it simply learns to predict context, and both kinds of pattern emerge.

Research (e.g., DSS-VAE, VG-VAE) has explored ways to **disentangle** the two by
training separate representations:

- A *syntactic* component trained to predict word order / parse trees
- A *Semantic* component trained to predict the unordered set of words (i.e.,
  meaning without structure)

|                      | **Semantic vector**                | **Syntactic vector**                         |
|:---------------------|:-----------------------------------|:---------------------------------------------|
| **Captures**         | Meaning / sense                    | Grammatical role / structure                 |
| **Similarity means** | "These words mean similar things"  | "These words play similar grammatical roles" |
| **Typical use**      | Search, classification, clustering | Parsing, POS tagging, dependency extraction  |

In practice, modern contextual embeddings (BERT, GPT, etc.) blend both so
thoroughly that the distinction is mostly of theoretical interest — but it
remains useful for understanding *what* an embedding is doing when it succeeds
or fails on a given task.

## Other NLP Vectors

The semantic/syntactic distinction is just one axis. Vectors in NLP can be 
categorized along several other dimensions.

## By Granularity

| **Type**                | **Represents**   | **Examples**                            |
|:------------------------|:-----------------|-----------------------------------------|
| Word embeddings         | Individual words | Word2Vec, GloVe, FastText               |
| Sentence embeddings     | Full sentences   | Universal Sentence Encoder, SkipThought |
| Document embeddings     | Whole documents  | Doc2Vec, BERT [CLS] pooling             |

## By Learning Method

- **Frequency-based (sparse):** One-hot encoding, Bag-of-Words, TF-IDF,
  co-occurrence matrices — vectors are high-dimensional and mostly zeros.
- **Prediction-based (dense):** Word2Vec (CBOW / Skip-gram), GloVe, FastText —
  low-dimensional dense vectors learned by predicting context.

## By Context Awareness

- **Static:** One fixed vector per word regardless of sentence (Word2Vec,
  GloVe). "Bank" is the same vector in "river bank" and "savings bank."
- **Contextual:** A different vector for each occurrence, computed from
  surrounding text (ELMo, BERT, GPT). Solves polysemy.

## By Modality / Domain

- **Image embeddings** — CNN/ViT feature vectors for visual similarity
- **Audio embeddings** — acoustic feature vectors
- **Graph embeddings** — Node2Vec, GraphSAGE for structured data
- **Biovectors** — embeddings for DNA, RNA, or protein sequences
- **Cross-lingual embeddings** — aligned vector spaces across languages
- **Knowledge-enhanced embeddings** — incorporating external knowledge graphs

## By Application

- **User embeddings** — encode preferences/behavior (e.g., Netflix, Spotify)
- **Product embeddings** — encode product attributes for recommendation systems

The semantic vs. syntactic split you asked about earlier is really a question of
*what a given vector captures* — most modern embeddings (especially contextual
ones) blend multiple of these properties simultaneously.
