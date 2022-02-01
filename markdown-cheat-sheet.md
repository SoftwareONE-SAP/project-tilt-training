# Markdown Cheatsheet

**Github Flavoured Markdown** (GFMD) is based on [Markdown Syntax Guide](http://daringfireball.net/projects/markdown/syntax) with some overwriting as described [here](http://github.github.com/github-flavored-markdown/).

This cheatsheet for Centiq is a simplified version of [this document](https://docs.github.com/en/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax).

Note that Markdown does have rules. Mostly, what you type will be rendered as you would expect. However when things end up looking unexpected, it's probably because you "broke a rule". I've attempted to explain the rules in this cheatsheet, but to aid their understanding you will have installed a "Markdown linting" extension which flags rule-breaking as you type.

## Text Writing

It is easy to write in GFMD. Just type simple text and use the below heading styles to mark the text and you are good to go!

Text with no blank lines between is "joined together".
To specify a paragraph, leave a blank line between the end of the first paragraph and the beginning of the next.

## Headings

```text
# Sample H1

## Sample H2

### Sample H3
```

will produce

# Sample H1

## Sample H2

### Sample H3

Headings introduce paragraph breaks automatically. However, you should still put a blank line before and a blank line after a heading.

Note that there should only be one `H1` and it should be the first line in the markdown file. The `H1` style is typically used as a document title.

Heading styles which need to move "upwards" to a higher number, should move one value at a time (`H1` to `H2`, for example, but not `H1` to `H3`). There are no constraints moving "downwards". `H6` is the biggest number (smallest sized heading).

---

## Horizontal Rules

A horizontal rule is created using three dashes (`---`) on a line by itself and surrounded by blank lines.

---

## Coding - Block

<pre>
```bash
declare ENV=${4:-sandbox}

case "$1 $2 $3" in

  'build manifests for')
    if [[ ! $(git status | tee /dev/tty | grep 'nothing to commit, working tree clean') ]]; then
```
</pre>

will produce

```bash
declare ENV=${4:-sandbox}

case "$1 $2 $3" in

  'build manifests for')
    if [[ ! $(git status | tee /dev/tty | grep 'nothing to commit, working tree clean') ]]; then
```

Note: You can specify alternative syntax highlighting based on the coding language, eg. `ruby`, `bash`, `php`, etc. by marking the *first set* of backticks as shown. If do not want syntax highlighting, use the \`\`\`text marker.

Note: You should leave blank lines before the first and after the final markers.

## Coding - In-line

You can produce inline-code by wrapping the code fragment in single backticks:

```text
This is some code: `echo something`
```

will produce

This is some code: `echo something`

---

## Text Formatting

**Bold Text** is produced using `**Bold Text**`

*Italic Text* is produced using `*Italic Text*`

***Bold and Italic text*** is produced using `***Bold and Italic text***`.

~~Struck through text~~ is produced using `~~Struck through text~~`.

---

## Hyperlinks

GFMD will automatically detect URLs and convert them to links like this https://www.softwareone.com/. However, this also is a rule-breaking form. It's preferable to use the following:

```text
This form is preferred: [Corporate Website](https://www.softwareone.com/)
```

This form is preferred: [Corporate Website](https://www.softwareone.com/)

---

## Escape Sequence

You can escape characters which would otherwise be actioned by preceding each actionable character with a backslash (`\`). For example \\\`

---

## Creating a List

### Bullet

Adding a `-` will change it into a bullet list:

```text
- Item 1
- Item 2
  - Item 2a
- Item 3
```

will produce

- Item 1
- Item 2
  - Item 2a
- Item 3

### Numbered

Adding a `1.` will change it into a numbered list:

```text
1. Item 1
1. Item 2
   1. Item 2a
1. Item 3
```

will produce

1. Item 1
1. Item 2
   1. Item 2a
1. Item 3

Note that indented bullets/numbers must be aligned with the preceding line's start of text. Numbers are permitted to run consecutively if you wish, but reordering a list is easier if all numbers are set to `1.`

Blank lines between list items are not required. If used they will be ignored. However, you may want to include "hanging paragraphs", i.e. which follow on from the bulleted/numbered paragraph, are indented nt the same amount and do not disturb the number sequencing (if always using `1.`).

```text
1. Item 1
1. Item 2

   This is a hanging paragraph. It begins at the same position as the start of text in the preceding (numbered) item. It should be wrapped in blank lines.

1. Item 3
```

will produce

1. Item 1
1. Item 2

   This is a hanging paragraph. It begins at the same position as the start of text in the preceding (numbered) item. It should be wrapped in blank lines.

1. Item 3

---

## Quoting

You can create a quote using `>`:

``` text
> This is a quote
```

will produce

> This is a quote

## Tables

Tables are a pain, but can be mastered.

```text
| Atomic Number | Element | Symbol |
|-|-|-|
| 1 | Hydrogen | H |
| 2 | Helium | He |
| 3 | Lithium | Li |
| 4 | Beryllium | Be |
```

| Atomic Number | Element | Symbol |
|-|-|-|
| 1 | Hydrogen | H |
| 2 | Helium | He |
| 3 | Lithium | Li |
| 4 | Beryllium | Be |

Columns may be aligned left (default), centre or right using `:` markers in the separator line. In addition lines may be padded for easier reading:

```text
| Atomic Number | Element   | Symbol |
|--------------:|:----------|:------:|
| 1             | Hydrogen  |   H    |
| 2             | Helium    |   He   |
| 3             | Lithium   |   Li   |
| 4             | Beryllium |   Be   |
```

| Atomic Number | Element   | Symbol |
|--------------:|:----------|:------:|
|             1 | Hydrogen  |   H    |
|             2 | Helium    |   He   |
|             3 | Lithium   |   Li   |
|             4 | Beryllium |   Be   |

When using VS Code, intalling [Markdown Table Prettifier](https://marketplace.visualstudio.com/items?itemName=darkriszty.markdown-table-prettify) makes it easy to automatically unify the appearance by highlighting and reformatting.

## Adding Images

Images may be added using either URLs to a web-hosted image, or via linking to image files included in the repo.

### Web-hosted Images

These are introduced very similarly to the way hyperlinks are included, with the difference being an inital `!` to mark an image hyperlink:

```text
![description](URL-of-image "optional hover text")
```

For example

```text
![Optiq Logo](https://monitiq.com/client/images/optiq-monitoring-login-page.png)
```

Results in:

![Optiq Logo](https://monitiq.com/client/images/optiq-monitoring-login-page.png)

### Image File

```text
![description](path/to/image-file "optional hover text")
```

For example:

```text
![Optiq Logo](images/optiq-logo.png "Optiq")
```

Results in:

![Optiq Logo](images/optiq-logo.png "Optiq")
