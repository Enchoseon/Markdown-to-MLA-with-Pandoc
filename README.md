# Markdown to MLA with Pandoc (and Neovim)!

This is a short and sweet demo of parsing markdown with Pandoc and turning it into an MLA-formatted PDF using [Ryan Aycock's MLA style for LaTeX](https://ctan.mirror.rafal.ca/macros/latex/contrib/mla-paper) and [Pandoc](https://pandoc.org/).

## Requirements

- `xelatex` 
- `pandoc`

## Setup

1. Copy `mla.sty` into `/usr/share/texmf/tex/latex`
  - If you don't want to do this, then you'll have to keep `mla.sty` in the same directory you run the Pandoc command or hardcode the path into your markdown file.

## Usage

1. Run `pandoc pdf --pdf-engine=xelatex` in the directory with your markdown file
  - Alternatively, you could specify a path to the markdown file with the `-i <PATH>` argument and run the command anywhere.
  
## Example

See `example.md` to an example file. You can run `pandoc pdf --pdf-engine=xelatex` to get an MLA-formatted PDF.

## Additional Notes

If you type your markdown in Neovim, you can use the [vim-pandoc plugin](https://github.com/vim-pandoc/vim-pandoc) and do `:Pandoc pdf --pdf-engine=xelatex` from inside Vim. You could even bind it to a key or something.

If you really care about your experience being seamless (because wrapping your markdown in-between two lines of LaTeX and having 4 lines of YAML config at the top of you file ruins your minimalist experience; and/or the LaTeX is screwing with your smart indent) you can have Pandoc take your raw markdown and put it into a template before processing it. I leave this as an exercise to the reader (or future me, who will then update this repo).

Additionally, since this is real LaTeX and not KaTeX or something, we can use LaTeX without doing any `$$` shenanigans and import our own packages! (e.g. a package for MLA-formatted bibliographies)
