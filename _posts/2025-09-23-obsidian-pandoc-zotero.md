---
layout: post
title: "Academic writing in Markdown with Obsidian, Pandoc and Zotero"
date: 2025-09-23
categories: [blog, posts]
tags: [writing, markdown, pandoc, zotero]
---

# Everything markdown

I enjoy writing most things in [Markdown](https://www.markdownguide.org/), and I enjoy having all my data in on my own computers and servers in Markdown, and not some proprietary binary data somewhere on the cloud. This includes my academic writing. Whether it's a manuscript draft, a grant proposal, a review, or a thesis, the starting point is always a Markdown file in my favorite markdown editor.

Everything in Markdown is just plaintext (or, well, probably stored in [UTF-8 / RFC 3629](https://datatracker.ietf.org/doc/html/rfc3629)). Headlines, quotes, bold, italic, code blocks, tables, lists -- it is all right there at your fingertips, simple, easy, understandable. I don't want to delve into the mysteries of what a table is in Word, LibreOffice, or RTF, or EndNote. I just want a table. Here's what a Markdown table looks like:

```

| Variable      | Value         |
|---------------|---------------|
| Some variable | Some value    |
| Some variable | Some value    |

```

It is just text. Exactly how it is then rendered (if at all) is up to whatever software you use for rendering. It is a clear separation of conerns (see [Dijkstra](https://www.cs.utexas.edu/~EWD/transcriptions/EWD04xx/EWD447.html)): first authoring content, and then rendering content. For example, a Markdown table in a [Quarto](https://quarto.org/) document might be rendered as an HTML table using a certain [theme](https://quarto.org/docs/output-formats/html-themes.html), where the theme itself specifies table colors and such. Or it might be rendered as a table in a Word document. The output format does not matter: the table is just a table.

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

To insert a citation in the text, the inline citation would look like:

```

the work of Paulhan [@paulhan1887simultaneite] questioned whether...

```

You can of course insert the citekeys manually. However, the [Citations](https://github.com/hans/obsidian-citation-plugin) plugin makes this a lot easier. When writing an article, you can access your entire library via a hotkey (mine is set to `⌘ + ⌥ + C`) and fuzzy match title, author, or citekey.

<div class="row justify-content-center mt-3">
	{% include figure.liquid loading="eager" path="assets/img/citations_obsidian.gif" class="img-fluid rounded z-depth-1"%}
</div>
<div class="caption">
    Citations plugin in Obsidian
</div>

The citation format is the same as [Pandoc citations](https://pandoc.org/demo/example33/8.20-citation-syntax.html#citation-syntax) and [Quarto citations](https://quarto.org/docs/authoring/citations.html). This makes it handy for rendering anywhere.

# Pandoc handles conversion to any format

The second stage of the separation of concerns, rendering content, depends on desired output format. No matter the output format, though, [Pandoc](https://pandoc.org/index.html) will take the Markdown file and handle it. It will convert your document into HTML, LaTeX, docx, beamer, RTF, vimdoc, Haddock, and countless others. I mean look at the [input-output diagram](https://pandoc.org/diagram.svgz?v=20260131160627) on the website. That's a lot.

To convert your Markdown file into, for example, docx you would use a command as such:

```
pandoc input.md --output=document.docx --bibliography=references.bib --citeproc
```

or, for HTML:

```
pandoc input.md --output=document.html --bibliography=references.bib --citeproc
```

or, for producing a PDF via LaTeX (in this case using LuaLaTeX):

```
pandoc input.md \
  --output=document.pdf \
  --bibliography=references.bib \
  --citeproc \
  --pdf-engine=lualatex
```

To make it more convenient, you can specify all of these options in a preamble to your Markdown document. For example, I am using a [Templater](https://github.com/SilentVoid13/Templater) plugin in Obsidian which allows for adding pre-specified elements to any new file. I use it to add a preamble for conversion to various document types I use.

In Obsidian, the preamble looks like:

```
---
title: "My Document"
output:
  latex:
    output: document.tex
    pdf-engine: lualatex
    bibliography: references.bib
docx:
  output: document.docx
  bibliography: references.bib
  citeproc: true
---
```

This can then easily be processed using a simple tool like [Panrun](https://github.com/mb21/panrun) with the command:

```
panrun input.md
```

or, for only converting to one specific type:

```
panrun input.md -t docx
```

This will process the preamble and run the appropriate `pandoc` command with specified options.

Quarto uses the same principle. In Quarto, the preamble would look like:

```
---
title: "My Document"
bibliography: references.bib
---
```

Add a references heading at the end of the document and processed references will be placed there.

```
# References
```

To select citation style, point to a [Citation Style Language (CSL) file](https://citationstyles.org/) in the preamble. For example, using APA formatting:

```
---
title: "My Document"
output:
  latex:
    output: document.tex
    pdf-engine: lualatex
    bibliography: references.bib
    csl: apa.csl
---
```

# Other useful features

## Acronyms

When authoring long documents using abbreviations, it can be tricky to keep track of where an abbreviations was first introduced, and take care to not re-introduce them. This can be solved by packages like [pandoc-acro](kprussing.github.io/pandoc-acro/) or [pandoc-acronyms](https://gitlab.com/mirkoboehm/pandoc-acronyms), which are based on [Pandoc filters](https://pandoc.org/filters.html).

In pandoc-acro, an acronym is prepended with a plus sign (+), and an acronym definition list is defined in the document preamble.

Text:

```
Studies using +PET, +SPECT, and +EEG have found evidence of ...

Specifically, an +EEG study found that ...
```

Preamble:

```
---
title: "My Document"
output:
  latex:
    output: document.tex
    pdf-engine: lualatex
    bibliography: references.bib
    csl: apa.csl
    filter:
          - pandoc-acro
acronyms:
  PET:
    short: PET
    long: positron emission tomography
  SPECT:
    short: SPECT
    long: single-photon emission computed tomography
  EEG:
    short: EEG
    long: electroencephalography
---
```

This will produce the following text:

```
Studies using positron emission tomography (PET), single-photon emission computed tomography (SPECT), and electroencephalography (EEG) have found evidence of ...

Specifically, an EEG study found that ...
```

No need to keep track of where an abbreviation is used. When converting a file to `.tex` format, appropriate LaTeX acronym commands will be created, such as `\ac{EEG}`.

## Filters

[Pandoc filters](https://pandoc.org/filters.html) can be a powerful tool. For example, I have image assets inside my Obsidian storage folder under `img/`, but if I'm working in a LaTeX template with predefined asset paths, I would need to replace these paths to, for example, `includes/figures`. A custom filter can process the document and do this automatically. For example:

```
function Image(img)
  local new, n = img.src:gsub("^%.%.%/img/", "includes/figures/")
  if n > 0 then
    img.src = new
  end
  return img
end
```

This can be saved to a `.lua` file, for example `custom_filter.lua` and be added like any other filter.

```
filter:
      - pandoc-acro
      - custom_filter
```

## Diagrams

To produce simple diagrams, there is no need to even leave the Markdown environment. Many Markdown environments support [Mermaid](https://mermaid.js.org/), which is a language for producing graphs and diagrams.

```mermaid
quadrantChart
    title Interference effects
    x-axis Motor
    y-axis Cognitive
    quadrant-1 Mutual facilitation
    quadrant-2 Motor priority
    quadrant-3 Mutual interference
    quadrant-4 Cognitive priority
    P1: [0.31, 0.68]
    P2: [0.33, 0.11]
    P3: [0.48, 0.19]
    P4: [0.87, 0.65]
    P5: [0.31, 0.14]
    P6: [0.30, 0.75]
    P7: [0.68, 0.09]
```

In any rendering software that supports Mermaid, the resulting chart would look something like:

<div class="row justify-content-center mt-3">
	{% include figure.liquid loading="eager" path="assets/img/mermaid_quadrant.png" class="img-fluid rounded z-depth-1" zoomable=true %}
</div>
<div class="caption">
    Mermaid diagram
</div>
