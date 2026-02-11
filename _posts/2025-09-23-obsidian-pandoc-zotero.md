---
layout: post
title: "Academic writing in Markdown with Obsidian, Pandoc and Zotero"
date: 2025-09-23
categories: [blog, posts]
tags: [writing, markdown, pandoc, zotero]
---

# Everything markdown

I enjoy writing most things in [markdown](https://www.markdownguide.org/), and I enjoy having all my data in on my own computers and servers in markdown, and not some proprietary binary data somewhere on the cloud. This includes my academic writing. Whether it's a manuscript draft, a grant proposal, a review, or a thesis, the starting point is always a markdown file in my favorite markdown editor.

Everything in markdown is just plaintext. Headlines, quotes, bold, italic, code blocks, tables, lists -- it is all right there at your fingertips, simple, easy, understandable. I don't want to delve into the mysteries of what a table is in Word, LibreOffice, or RTF, or EndNote. I just want a table. Here's what a markdown table looks like:

```

| Variable      | Value         |
|---------------|---------------|
| Some variable | Some value    |
| Some variable | Some value    |

```

It is just text. Exactly how it is then rendered (if at all) is up to whatever software you use for rendering. It is a clear separation of conerns: first authoring content, and then rendering content. For example, a markdown table in a [Quarto](https://quarto.org/) document might be rendered as an HTML table using a certain [theme](https://quarto.org/docs/output-formats/html-themes.html), where the theme itself specifies table colors and such.

Currently the spot for my favorite editor goes to [Obsidian](https://obsidian.md/). Beautiful, sleek, with a simple folder structure with plaintext files for storage.

# Zotero integration

All my reference handling is done in the open-source [Zotero](https://www.zotero.org) reference manager. Zotero integrates well with the Markdown workflow. In Obsidian, I use the [Citations](https://github.com/hans/obsidian-citation-plugin) plugin to synchronize with my Zotero library. The plugin can watch a `.bib` file, which is generated and automatically updated with added references from Zotero via the [Better BibTeX plugin](https://retorque.re/zotero-better-bibtex/).

The `.bib` file is another plaintext file in the [BibTeX](https://www.bibtex.org/) format. An entry can look something like the following:

```

@article{paulhan1887simultaneite,
  title = {La Simultan\'eit\'e Des Actes Psychiques},
  author = {Paulhan, Fr},
  year = 1887,
  journal = {Revue Scientifique},
  volume = {13},
  pages = {684--689}
}

```

..where you have a citekey (@paulhan1887simultaneite in this case), along with the metadata about the article.

When writing an article, you can access your entire library

<div class="row justify-content-center mt-3">
	{% include figure.liquid loading="eager" path="assets/img/citations_obsidian.gif" class="img-fluid rounded z-depth-1"%}
</div>
<div class="caption">
    Citations plugin in Obsidian
</div>

# Pandoc handles everything

To be added.
