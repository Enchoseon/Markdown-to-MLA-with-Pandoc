# Markdown to MLA with Pandoc (and Neovim)!

This is a short and sweet demo of parsing Markdown with Pandoc and turning it into a MLA-formatted PDF using [Ryan Aycock's MLA style for LaTeX](https://ctan.mirror.rafal.ca/macros/latex/contrib/mla-paper) and [Pandoc](https://pandoc.org/). (And, as a bonus, a really convenient way to do it in Neovim.)

# Quick Start

## Requirements

- `xelatex` 
- `pandoc`

## Instructions

1. Download and install [mla.sty](https://raw.githubusercontent.com/Enchoseon/Markdown-to-MLA-with-Pandoc/main/mla.sty).
```bash
mkdir -p $(kpsewhich -var-value=TEXMFHOME)/tex/latex/local/ && cp "<PATH_TO_DOWNLOADED_MLA.STY>" $( kpsewhich -var-value=TEXMFHOME )/tex/latex/local/
```
2. Create a new markdown file and paste this template:
```markdown
---
header-includes: \usepackage{mla}
pdf-engine: xelatex
---
\begin{mla}{FIRSTNAME}{LASTNAME}{PROFESSOR}{CLASS}{DATE}{TITLE}
Hello World!
\end{mla}
```
3. Do `:Pandoc pdf` in Neovim (see: [vim-pandoc plugin](https://github.com/vim-pandoc/vim-pandoc))
4. Done!

For more in-depth instructions (along with a faster Neovim workflow), see [Setup](#setup) and [Usage](#usage).

# Usage

## Terminal

1. Run `pandoc "<INPUT_FILE>" --pdf-engine=xelatex -o "<OUTPUT_FILE>"`
    - Be sure to include the .pdf file extension in `<OUTPUT_FILE>` *or* add `-w "pdf"`; otherwise Pandoc will default to exporting HTML

*Example Use: `pandoc "example.md" --pdf-engine=xelatex -o "example.pdf"`*

## Neovim

**Setup:** Install the [vim-pandoc plugin](https://github.com/vim-pandoc/vim-pandoc)

1. Run `:Pandoc pdf` from inside Vim.

*Example Use: `:Pandoc pdf`*

# Setup

## Part I: Installing [mla.sty](https://raw.githubusercontent.com/Enchoseon/Markdown-to-MLA-with-Pandoc/main/mla.sty)

[mla.sty](https://raw.githubusercontent.com/Enchoseon/Markdown-to-MLA-with-Pandoc/main/mla.sty) must be accessible to XeLaTeX, otherwise it will error out and complain that it can't find the MLA package.

You can provide [mla.sty](https://raw.githubusercontent.com/Enchoseon/Markdown-to-MLA-with-Pandoc/main/mla.sty) by doing any of the following:

**Option A**: Copy [mla.sty](https://raw.githubusercontent.com/Enchoseon/Markdown-to-MLA-with-Pandoc/main/mla.sty) into TeX home directory. This will make the style globally available and you won't have to do anything else.
  1. To find your TeX home directory, run `kpsewhich -var-value=TEXMFHOME`.
      - You may have to create the directory along with the TDS file structure inside of it (`/tex/latex/local`) if it doesn't exist
```bash
mkdir -p $( kpsewhich -var-value=TEXMFHOME )/tex/latex/local/
```
  2. Now that you've made/found your TeX directory, copy [mla.sty](https://raw.githubusercontent.com/Enchoseon/Markdown-to-MLA-with-Pandoc/main/mla.sty) into `<YOUR TEX HOME DIRECTORY>/tex/latex/local/`
```bash
cp "PATH_TO_YOUR_MLA.STY" $( kpsewhich -var-value=TEXMFHOME )/tex/latex/local/
```

**Option B**: Copy [mla.sty](https://raw.githubusercontent.com/Enchoseon/Markdown-to-MLA-with-Pandoc/main/mla.sty) into the same directory you run the Pandoc command

**Option C**: Hardcode the path to your [mla.sty](https://raw.githubusercontent.com/Enchoseon/Markdown-to-MLA-with-Pandoc/main/mla.sty) into your markdown file's YAML configuration.

## Part II: Formatting your Markdown

Your markdown file must have the following:
- `header-includes: \usepackage{mla}` in the YAML config
- `\begin{mla}{FIRSTNAME}{LASTNAME}{PROFESSOR}{CLASS}{DATE}{TITLE}` and `\end{mla}` surrounding your Markdown.

Add `pdf-engine: xelatex`  to the YAML config if you're using Neovim with [vim-pandoc](https://github.com/vim-pandoc/vim-pandoc) so that you don't need to do `--pdf-engine=xelatex`.

Altogether, this is what a skeleton file should look like:
```markdown
---
header-includes: \usepackage{mla}
pdf-engine: xelatex
---
\begin{mla}{FIRSTNAME}{LASTNAME}{PROFESSOR}{CLASS}{DATE}{TITLE}

Hello World!

\end{mla}
```

# Additional Notes

If you really care about your experience being seamless because wrapping your markdown in-between two lines of LaTeX and having a YAML config at the top of your file ruins your minimalist experience (or the LaTeX is screwing with your smart indent), you can have Pandoc take your raw markdown and put it into a template before processing it.

Additionally, since this is real LaTeX and not KaTeX, we can import our own packages! (e.g. a package for MLA-formatted bibliographies)
