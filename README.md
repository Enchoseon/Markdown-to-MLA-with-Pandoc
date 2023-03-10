# Markdown to MLA with Pandoc (and Neovim)!

This is a short and sweet demo of parsing markdown with Pandoc and turning it into an MLA-formatted PDF using [Ryan Aycock's MLA style for LaTeX](https://ctan.mirror.rafal.ca/macros/latex/contrib/mla-paper) and [Pandoc](https://pandoc.org/).

## Requirements

- `xelatex` 
- `pandoc`

## How-To

To turn markdown into MLA format, just run `pandoc pdf --pdf-engine=xelatex` in the directory with your Markdown file (or specify a path to a specific markdown file if you need to).
- See `example.md` to an example file. You can run `pandoc pdf --pdf-engine=xelatex` to get an MLA-formatted PDF.

If you type your markdown in Neovim, you can use the [vim-pandoc plugin](https://github.com/vim-pandoc/vim-pandoc) and do `:Pandoc pdf --pdf-engine=xelatex` from inside Vim, you could even bind it to a key or something.

> **Remember**: Be sure Pandoc can access `mla.sty` when you run it! You may want to consider hardcoding a path to it with a command-line argument.

If you really care about your experience being seamless (because wrapping your markdown in-between two lines of LaTeX and having 4 lines of YAML config at the top of you file ruins your minimalism) you can have Pandoc take your raw markdown and put it into a template before processing it.

Additionally, since this is using real LaTeX, we can even import our own MLA-friendly bibliography solutions if we want!
