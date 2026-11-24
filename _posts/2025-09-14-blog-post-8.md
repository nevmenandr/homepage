---
title: 'A markup language for poetry'
date: 2025-09-14
permalink: /posts/2025/09/blog-post-8/
tags:
  - digital-humanities
  - poetry
  - markup
---

VML
======

When a philologist meets a programmer, the conversation often comes down to data. The philologist wants to preserve the meaning, structure, and beauty of the text. The programmer wants that data to be processable, parsable, and measurable. For prose there is TEI markup, for scholarly articles there is LaTeX, for the web there is HTML. And for poetry?

Usually a poem in digital form is just a set of lines in a text file. But if we want to automatically find the author, the title, the epigraph, the date of composition, the stanzas, or the metrical scheme, plain text is useless. Everything is mixed together.

I have developed VML (Verse Markup Language) — a markup language that turns unstructured poetic text into a machine-readable structure while remaining lightweight and human-friendly. It is not another XML monster, nor a JSON that frightens humanists. It is a text format with short tags and a strict but intuitive hierarchy.

VML is already used in a real project — the Bashkir Poetic Corpus (http://web-corpora.net/bashcorpus/), which includes more than 10,000 poems. A monograph (Orekhov, 2019) and a number of articles on quantitative verse studies have been based on this corpus.

What does it look like?

Here is a minimal VML document:

```
<a> Александр Пушкин
<&> Я вас любил: любовь еще, быть может,
В душе моей угасла не совсем;
Но пусть она вас больше не тревожит;
Я не хочу печалить вас ничем.
```

Everything is simple:

- `<a>` — the author. It applies until the next author tag.
- `<&>` — the first line of the poem (the incipit). This is a mandatory tag; without it the document is invalid.

Already from this example one can extract the author, the first line, and the list of verse lines. They can be counted, compared, analyzed.

Adding structure

A poem is rarely just line after line. It has a title, an epigraph, stanzas.

- `<rm>` — a prose insertion (epigraph, dedication, stage direction). Each line requires a separate tag.
- `<*>` — the beginning of a stanza. Stanzas are separated from each other by repeating the tag.

Poetic cycles

If several poems are united by a common design, they can be placed in a cycle:

```
<nn> Стихи о Прекрасной Даме
<n> Вступление
<&> Отдых напрасен. Дорога крута.
Вечер прекрасен. Стучу в ворота.
<n> Второе стихотворение
<&> Вхожу я в храм и совершаю...
</nn>
```

The tag `<nn> ... </nn>` denotes a cycle. Inside it there may be several poems, each with its own title (`<n>`) and incipit.

What if there is an epigraph between poems?

Imagine: two poems in a row, and the second one has an epigraph before its incipit. How do you avoid attaching the epigraph to the first poem? VML has the tag `<&&>` for this — an explicit start of a new poem before the incipit.

For verse scholars: metrics

If you work in verse studies, you may need explicit markup of the metrical scheme. VML supports four systems of versification:

- Syllabic — `<m-S12>` (12-syllable verse)
- Syllabo-tonic — `<m-RЯ4м>` (iambic tetrameter with masculine ending)
- Tonic — `<m-A*2*2*1>` (stressed, 2 unstressed, stressed, 2 unstressed, stressed, 1 unstressed)
- Syllabo-metric (antique) — `<m-M*2*1*>`

Example:

```
<m-RЯ4ж> Мой дядя самых честных правил,
<m-RЯ4м> Когда не в шутку занемог...
```

Mayakovsky's "ladder"

The stepped notation of verse introduced by Mayakovsky is also supported. The tag `<l-NUM>` denotes the indentation level.

```
Я волком бы <l-2> выгрыз <l-3> бюрократизм.
```

Tools

The VML repository has two ready-made Python tools:

- A validator (vml_validator.py) — checks syntax, nesting hierarchy, and escaping of service sequences.
- A counter (vml_counter.py) — produces statistics: the number of authors, poems, lines, cycles.

Running:

```
python vml_validator.py poem.txt
python vml_counter.py poem.txt
```

Why VML and not something else?

Let us compare with existing formats:

- TEI — powerful but cumbersome. To mark up a single poem you have to write dozens of lines of XML. For corpus analysis it is excessive.
- JSON — excellent for machines but poorly readable for humans. A philologist is unlikely to want to edit JSON by hand.
- Markdown — good for documentation but not for strict markup of metadata and hierarchy.

VML is the golden mean. It is strict enough to be parsed automatically, yet light enough for a philologist to use without special training. The tags are short, the structure is intuitive, and learning takes 5–10 minutes.

Who is it for?

- For philologists who want to build their own poetic corpus but do not want to learn XML/JSON.
- For developers who need to extract metadata from poems for NLP tasks (author, date, meter, stanza structure).
- For Digital Humanities enthusiasts looking for a simple and open markup standard.

Where to get it?

- Full specification — https://github.com/nevmenandr/VML/blob/main/README.md
- Repository with code and examples — https://github.com/nevmenandr/VML
- DOI — 10.5281/zenodo.20100191
- License — Apache 2.0 (free use, modification, and distribution, including commercial applications)

Conclusion

The VML language was created not as an abstract standard but as a working tool. It has already been tested on a real corpus and has proven its effectiveness. If you work with poetry in a digital environment, try VML — it will take just a few minutes, and structured data will open up new possibilities for analysis and research.

Questions, suggestions, and criticism are welcome in the issues on GitHub. The more people use VML, the better both the language itself and the tools around it will become.
