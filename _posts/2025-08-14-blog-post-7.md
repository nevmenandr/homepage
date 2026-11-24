---
title: 'A vector model of Russian fiction'
date: 2025-08-14
permalink: /posts/2025/08/blog-post-7/
tags:
  - digital-humanities
  - corpus-linguistics
  - word2vec
---

w2v-russian-fiction
======

Imagine being able to look into the "consciousness" of all Russian fiction — from the classics to contemporary bestsellers. And to do it not with your eyes but mathematically, by measuring the semantic distances between words. That is exactly the opportunity opened up by the w2v-russian-fiction model, recently published on Hugging Face.

What is this model and why is it needed?

It is a large vector-semantic model (word2vec) trained on a colossal corpus of Russian-language literary texts with a total volume of 13 billion words. After lemmatization and removal of stop words, more than 7.3 billion word occurrences took part in training.

Why create a separate model for fiction? The point is that, as research shows, the semantics of words and their contextual connections depend strongly on genre. The way a word "lives" in news feeds or scholarly articles can differ radically from its use in prose or poetry. A model trained exclusively on fiction is therefore a specialized tool that captures the subtle semantic nuances characteristic of literary language.

How is the model built and how do you work with it?

The model is based on the CBOW (Continuous Bag of Words) algorithm with a vector dimensionality of 300 and a context window of 10 words. It operates not on words in their grammatical forms but on lemmas (dictionary forms) without part-of-speech tags, which simplifies working with it.

Working with the model is very simple. After loading it (with the gensim library), you can request the nearest "semantic neighbors" for any word:

```python
import gensim
model = gensim.models.Word2Vec.load("cbow_300_10.model")

words = ['слово', 'язык', 'речь']
for w in words:
    print(w)
    for similar_word, score in model.wv.most_similar(positive=[w], topn=10):
        print(f"  {similar_word} {score:.4f}")
    print()
```

What can the model tell us about a word? Some examples

The authors of the model give three illustrative cases in the description that perfectly demonstrate its capabilities.

1. The word "слово" (word) — the metalanguage of literature

It would seem that nothing could be simpler. But the model produces nearest neighbors that form a genuine portrait of the "word" as the basic unit of literary speech. They are:

- Synonyms and close concepts: фраза (0.7941), словечко (0.6602), слог (0.6322), реплика (0.6015), словосочетание (0.5928).
- More abstract linguistic terms: изречение (0.5818), высказывание (0.5800).
- Specifically literary-critical concepts: эпитет (0.5615), сентенция (0.5556).

Interestingly, in this list we see not merely linguistic terms but words that describe the quality and style of speech. "Epithet" and "sententia" are markers of a specifically artistic, figurative way of thinking about the word.

2. The word "язык" (language/tongue) — from linguistics to geography and culture

Here the model goes beyond dry theory and shows language as a living, multifaceted entity:

- Linguistic terms: наречие (0.6684), диалект (0.6095), грамматика (0.5027), идиома (0.4952).
- Specific languages: латынь (0.5892), суахили (0.4977), иврит (0.4950).
- The physical aspect: язычок (0.5698).
- Writing: алфавит (0.5039).
- Sound: произношение (0.4927).

Notice how "язык" is linked not only to other languages but also to the organ of speech (язычок) and to writing. The vector space captures this multidimensionality of meaning.

3. The word "речь" (speech) — the oral element and rhetoric

The values for "речь" paint an image of an oral, often expressive utterance:

- Forms of oral speech: монолог (0.6400), диалог (0.5477).
- Expressive and rhetorical genres: тирада (0.5900), спич (0.5914), проповедь (0.5334), филиппика (0.5184), декламация (0.5147).
- A synonym: разглагольствование (0.5193).

Here we see how subtly the model distinguishes different registers of speech — from dialogue to impassioned denunciatory speech (a philippic), which is very characteristic of many literary genres.

Why is this model a valuable tool?

For the humanities:
- Individual style and authorial semantics. The model makes it possible to compare how the same word (for example, "freedom," "soul," or "nature") is used in different literary epochs or by different authors.
- Genre analysis. One can study how the semantics of words change depending on genre (detective fiction vs. romance vs. philosophical prose).
- Historical semantics. Although the corpus is not annotated by year, it includes texts from different epochs, which allows preliminary observations about the dynamics of meaning.

For applied tasks (NLP):
- Improved search and clustering. A model trained on literary texts understands semantic queries in this domain better.
- Stylometry. Vector representations of words can serve as features for determining authorship or genre.
- Semantic analysis of characters. One can build portraits of characters based on the vector representations of the words they frequently use.

In lieu of a conclusion

The w2v-russian-fiction model is not just a set of numbers. It is a digitized "collective voice" of Russian literature, ready to answer our questions about language, meaning, and style. It opens new horizons for research: from testing intuitive literary hypotheses to creating intelligent systems capable of a deeper understanding of literary text.

And most importantly, it is available. Download it, experiment, and perhaps it is you who will make a new discovery about Russian letters with its help.

Bibliographic reference

Orekhov B. V. w2v-russian-fiction (Revision 34453d9) [Electronic resource] / B. V. Orekhov. – Hugging Face, 2025. – URL: https://huggingface.co/nevmenandr/w2v-russian-fiction (accessed: 21.08.2026). – DOI: 10.57967/hf/5921.
