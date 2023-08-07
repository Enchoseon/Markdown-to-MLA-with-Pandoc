<div align="center">
<h1>Markdown to MLA with Pandoc (and Neovim)!</h1>
<p>This is a short and sweet demo of parsing Markdown with Pandoc and turning it into a MLA-formatted PDF using <a href="https://ctan.mirror.rafal.ca/macros/latex/contrib/mla-paper">Ryan Aycock's MLA style for LaTeX</a> and <a href="https://pandoc.org/">Pandoc</a> <i>(and, as a bonus, a really convenient way to do it in Neovim)</i></p>
</div>

# Table of Contents

### Quick Start
- [Quick Start](#quick-start)
  * [Requirements](#requirements)
  * [Instructions](#instructions)

### Non-Quick, In-Depth Start
- [Usage](#usage)
  * [Option A: Terminal](#option-a--terminal)
  * [Option B: Neovim](#option-b--neovim)
- [Setup](#setup)
  * Part I: Installing mla.sty
  * Part II: Formatting your Markdown

### Advanced Usage & Workflow
- [Additional Notes](#additional-notes)
- [My Workflow](#my-workflow)

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

## Option A: Terminal

1. Run `pandoc "<INPUT_FILE>" --pdf-engine=xelatex -o "<OUTPUT_FILE>"`
    - Be sure to include the .pdf file extension in `<OUTPUT_FILE>` *or* add `-w "pdf"`; otherwise Pandoc will default to exporting HTML

*Example Use: `pandoc "example.md" --pdf-engine=xelatex -o "example.pdf"`*

## Option B: Neovim

**Setup:** Install the [vim-pandoc plugin](https://github.com/vim-pandoc/vim-pandoc)
- Note: This plugin requires `pynvim` to be installed (Gentoo Package: `dev-python/pynvim`). You probably already have it, but it isn't working, this may be the culprit!

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
- I've adopted this approach myself (see: [My Workflow](#my-workflow))

Additionally, since this is real LaTeX and not KaTeX, we can import our own packages! (e.g. a package for MLA-formatted bibliographies)

The Mla.sty in this repo is a carbon copy of the original. It uses an outdated package for the Times-like font that you may want to replace with your system's Times New Roman font.
- (see: [My Workflow](#my-workflow) for my personal configuration/fix)

At the time of writing, vim-pandoc can only work with Pandoc version `2.x` and crashes with Pandoc version `3.x`. Hopefully support for 3.x will be added soon.
- On Gentoo, `app-text/pandoc` is on 2.x and `app-text/pandoc-bin` is on 3.x. The slot for 2.x was removed from `app-text/pandoc-bin`, so you'll need to compile Pandoc from `app-text/pandoc`. Unfortunately, Haskell's compilation process is a bit of a hot mess
    - My advice if you're getting problems is to enable`dev-lang/ghc`'s `binary` use flag. If that doesn't work, consider slamming your face against the keyboard whilst screaming.

# My Workflow

Since publishing this repo I've since streamlined my academic Markdown process into a couple of Neovim commands (e.g., `:PandocMarkdownToMla` for conversion and `:MlaPdfStats` for page and word count).

For example, this markdown—
```markdown
---
title: Markdown to MLA PDF Demo
professor: Professor Depressed Divorcee
class: Foo 1234
date: 20 April 2023
firstName: Joe
lastName: Mama
---

This is a demo of my Markdown $\to$ MLA-formatted PDF workflow in Neovim! Run `:PandocMarkdownToMla` to export this Markdown file to `/tmp/nvim-pandoc-mla.pdf`, or do `:PandocMarkdownToMlaLivePreview` to open `/tmp/nvim-pandoc-mla.pdf` in your default PDF reader and convert on every write (auto-refreshing is assumed to be done by the PDF reader). The tools used in the conversion are XeLaTeX, Pandoc, Neovim, and [vim-pandoc](https://github.com/vim-pandoc/vim-pandoc).

Need to know the page and word count? Run `:PandocMarkdownToMla` followed by `:MlaPdfStats`. [noice.nvim](https://github.com/folke/noice.nvim) will redirect the console echo output to the [nvim-notify](rcarriga/nvim-notify) notification manager for super-convenient viewing.

Special thanks to Ryan Aycock, Edward Z. Yang, and Teddy Bradford for their MLA package for LaTeX, which has been put into my dotfiles (`.config/yadm/xelatex/mla.sty`) with slight modifications to:
1. Use the system's Times New Roman font rather than an outdated package for a Times-like font, fixing bold, italic, and monospace text—but **only** if `mla.sty` is in the TeX home dir!
2. Provide the `title`, `professor`, `class`, `date`, `firstName`, and `lastName` variables, which can be set through a YAML header. *(Secret bonus variable: `mla.linespread`)*

\begin{workscited}
	\bibent
	Neil, Drew, \textit{Practical Vim: Edit Text at the Speed of Thought}, The Pragmatic Bookshelf, 2015.
\end{workscited}
```
—yields [this pdf](https://github.com/Enchoseon/dotfiles/blob/master/.config/nvim/pandoc/sample-mla.pdf) when I run `:PandocMarkdownToMla`. And I can further run `:MlaPdfStats` to get a little modal popup in the corner of the screen telling me my current page and word count to know if I'm reaching paper requirements.

Relevant parts of my dotfiles:
- Streamlined commands for export and preview: https://github.com/Enchoseon/dotfiles/blob/master/.config/nvim/plugin/export-and-preview.vim
- LaTeX template to use YAML variables to MLA output: https://github.com/Enchoseon/dotfiles/blob/master/.config/nvim/pandoc/markdown-to-mla.template
    - Additionally provides packages for strikethrough, bullet-points, non-MLA headings and subheadings, and using the current date when one isn't provided
- Modified mla.sty with Times New Roman fix (uses system's Times New Roman): https://github.com/Enchoseon/dotfiles/blob/master/.config/yadm/xelatex/mla.sty
