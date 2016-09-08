# eecs-111-ta-guide

This is an unofficial guide to being a TA for [EECS 111: Fundamentals of Computer Programming I](http://www.mccormick.northwestern.edu/eecs/courses/descriptions/111.html) at Northwestern University.

## Getting Started

You should have npm installed on your computer.

Clone the repo and install [Gitbook](https://toolchain.gitbook.com/setup.html):

```
$ npm install -g gitbook-cli
```

Then install Gitbook addons specified in `book.json`:

```
$ gitbook install
```

## Development

Serve the book during development:

```
$ gitbook serve
```

Build the static website:

```
$ gitbook build
```

Consult the [Gitbook Toolchain documentation](https://toolchain.gitbook.com/) for help.

## Editing Guidelines

Contributions should be written in [Markdown](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet).

Formatting conventions are as follows:

- One newline between blocks (headings, paragraphs, etc.)
- Headings should be hash-prefixed, with a space between the hashes and text
    + Good: `# Heading`
    + Bad: `#Heading`
    + Bad:
      ```
      Heading
      -------
      ```
- Use asterisks for bold and italics
    + Good: `**Bold**, *Italic*, ***Bold Italic***`
    + Bad: `__Bold__, _Italic_`
- Use fenced code blocks whenever possible, and the `scheme` language tag. (We pre-process using [Prism](http://prismjs.com), which doesn't support `racket`.)
