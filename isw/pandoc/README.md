# Pandoc and Reveal.js

Reveal.js is a framework for easily creating beautiful presentations using HTML.
For more information, see the top-level README in this repository or the
[Reveal.js home page](https://revealjs.com/#/).

[Pandoc](https://pandoc.org/) is a document converter.
In particular, it can convert Markdown to HTML, and it has built-in support
for Reveal.js.

The instructions below assume that you have Pandoc installed on your system.
If necessary, `brew install pandoc` if you work with a Mac and have
[Homebrew](https://brew.sh/) installed, or see Pandoc's
[installation instructions](https://pandoc.org/installing.html)
for more options.

## Quick start

Starting from the top-level directory of this repository,

```
cd pandoc
pandoc --standalone -t revealjs -o example.html example.md
cd ..
npm start
```

and then visit http://localhost:8000/pandoc/example.html in your web
browser.

The `npm start` command starts a web server in the top-level directory.
You could also use `php -S localhost:8000` or various other methods.

There is a simple `Makefile` in the `pandoc/` directory, so you can use `make`
instead of typing `pandoc` with all those options.
You can also use `make my-pres.html` to generate a presentation from the
source `my-pres.md`.

## Pandoc options

For more options, try `pandoc --help`.
For full details, `man pandoc`.
Here, we explain only the options that we use above.

### `--standalone`

By default, Pandoc creates fragments that can be pasted together.
This option tells Pandoc to create a complete HTML file, including `<head>`
and `<body>` tags. You can also use the short form `-s`.

### `-t revealjs`

Use the Reveal.js output format. Instead of `-t`, you can use `--to=`, `-w`,
or `--write=`.

### `-o example.html`

Write the output to the file `example.html`. By default, Pandoc writes to
`stdout`.
You can also use `--output=example.html`.

### `example.md`

This is the input file to read.
This is an argument, not an option.

## The example file

### Head matter

The example source file `example.md` starts with some header information in
YAML format:

```yaml
---
author: Benji Fisher
title: Example for Pandoc and Reveal.js
date: August 5, 2018
revealjs-url: '..'
theme: isovera
css:
  - 'https://fonts.googleapis.com/css?family=Roboto+Slab:700'
---
```

The `author`, `title`, and `date` keys are used to generate the title page
(first slide).

The `revealjs-url` key tells Pandoc where to look for the Reveal.js files.
Since the example file is inside the directory `pandoc/`, the value `..`
refers to the top level of this repository.

The `theme` key refers to one of the CSS files inside `css/theme/` (relative
to the root of the Reveal.js repository).
By default, Reveal.js will use the `black` theme.
We have added the custom `isovera.css` theme to this repository.

The `css` key is a YAML array of URLs of stylesheets to be linked.

The result of those three keys is that the following lines are added to the
`<head>` element of the output file:

```html
  <link rel="stylesheet" href="../css/theme/isovera.css" id="theme">
  <link rel="stylesheet" href="https://fonts.googleapis.com/css?family=Roboto+Slab:700"/>
```

### The rest of the file

The rest of the example file describes three or four slides, depending on how
you count:

```markdown
## About me

- Senior Drupal Developer at
  ![Isovera logo](../images/isovera_logo_reverse.svg){ style="border: 0; background: none; height: 50px; vertical-align: middle" }
  since 2016

![Isovera logo](../images/isovera_logo_reverse.svg){ style="border:0; background:none; height:50px;" }\ 

# About Isovera

## Nested slides

- Each slide begins with an `<h2>` element (`"## "` in markdown).
- Use an `<h1>` element (`"# "` in markdown) to group slides.

## Our slogan

> Design. Develop. Drupal.
```

Each slide starts with an `<h2>` element, and you can group slides with an
`<h1>` element.

The example illustrates some markdown features: unordered lists, block
quotations.
It also shows how to refer to the logo we added to this repository,
and how to create an image in its own paragraph.
Without the trailing `\ `, Pandoc will create a `<figure>` element, using the
alt text as the caption.

### Nested slides

One limitation: when you nest slides like this, the `<h1>` slide ("About
Isovera" in the example) does not have any text besides the title.

It is not necessarily bad, just a limitation.

For discussion and (ugly) work-arounds, see this
[Q&A on Stack Overflow](https://stackoverflow.com/questions/30988306/level-1-and-level-2-slides-in-reveal-js-using-pandoc).
