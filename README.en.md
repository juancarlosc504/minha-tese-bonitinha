# My pretty thesis (English version)

Authorship: Juan Carlos Lamonica, MSc.

English translation of `README.md` (the Portuguese original is the reference version). A Claude skill and a LaTeX template folder with a formatting standard for theses and dissertations. **The skill is based on the canonical abnTeX2 model** (<https://www.abntex.net.br>): it starts from the `abntex2` class and the ABNT standards it implements, and adds its own formatting conventions (tables, figures, equations, citations, units, review markers). The repository contains the `SKILL.md` file (Portuguese) and `SKILL.en.md` (English), which teach Claude to write and edit documents in this standard and to create the template folder, and the `modelo/` folder, a canonical document with no filled-in data that can be used directly in TeXstudio or another editor, with or without Claude.

## Requirement: an installed TeX distribution

To compile the template, and any chapter written in this standard, a complete TeX distribution must be installed: **MacTeX** on macOS or **MiKTeX** on Windows (TeX Live on Linux). The template uses `latexmk`, `pdflatex` and `bibtex`, plus packages such as `memoir`, `microtype`, `isotope`, `nomencl`, `imakeidx`, `caption`, `tocloft`, `booktabs`, `pdfpages` and `listings`. Without the distribution, the `.tex` does not produce a PDF; Claude also depends on it to compile and check the result.

The `abntex2` class and the language files (`brazil.ldf`, `brazilian.ldf`, `portuges.ldf`) ship with the template in its own folder, copied from an abnTeX2 project, so they do not depend on the version installed on the system.

### Reference environment (author's computer)

The data below were extracted from the log of a real compilation and from the computer itself:

| Item | Value |
|---|---|
| System | macOS, MacBook Air (Apple Silicon, arm64) |
| Distribution | MacTeX / TeX Live, installed in `/usr/local/texlive` |
| Engine | pdfTeX 3.141592653-2.6-1.40.29 (TeX Live 2026) |
| LaTeX kernel | LaTeX2e 2026-06-01 |
| Base class | `memoir` 3.8.4b, with local `abntex2` v-1.9.7 |
| Build | `latexmk` (pdflatex + bibtex), `abntex2-alf` style |
| Editor | TeXstudio, default compiler `txs:///latexmk`, spell checker `pt_BR` |

### Installation

On **macOS**, install the full MacTeX (about 5 GB), by downloading from <https://tug.org/mactex/> or, with Homebrew, `brew install --cask mactex`. The lightweight alternative is BasicTeX (`brew install --cask basictex`), followed by the template packages:

```bash
sudo tlmgr update --self
sudo tlmgr install latexmk memoir microtype isotope nomencl imakeidx caption \
     tocloft booktabs pdfpages listings was enumitem multirow pgf lm \
     babel-portuges setspace relsize xcolor
```

The list covers the template packages; if compilation reports `File 'xxx.sty' not found`, install the corresponding package with `sudo tlmgr install xxx`. The full installation makes this step unnecessary.

On **Windows**, install MiKTeX (<https://miktex.org/download>), which downloads missing packages on demand at the first compilation. MiKTeX's `latexmk` also requires Perl (for example, Strawberry Perl).

On **Linux**, `sudo apt install texlive-full latexmk` (or the distribution's equivalent).

To check the installation, in a terminal:

```bash
pdflatex --version
latexmk -v
kpsewhich memoir.cls
```

## How to use the template folder

Copy the `modelo/` folder to your working location and rename it. Fill in the bracketed fields in `tese_modelo.tex` (title, author, advisors, institution, program, place, year, keywords), in `pretextual/resumo.tex` and `pretextual/abstract.tex`. Then always compile the master document:

```bash
cd modelo
latexmk -pdf tese_modelo.tex
```

In TeXstudio, open `tese_modelo.tex` and press F5, with the default compiler `txs:///latexmk`. Each file in `capitulos/`, `pretextual/` and `apendices/` starts with `% !TeX root = ../tese_modelo.tex`, which allows compiling the whole document from any chapter. The `capitulo_modelo.tex` chapter provides ready examples of table, figure, subfigures, equation, units, citations, cross-references and review markers. The `exemplo*.pdf` figures are only illustrative and can be removed.

Note: the template is a Brazilian (ABNT) document, so the typeset text it produces (chapter titles, "Figura", "Tabela", "Apêndice") and its placeholders are in Portuguese.

## How to use as a Claude skill

The `SKILL.md` file at the root is the skill (Portuguese); `SKILL.en.md` is its English translation. To install it, import the file in the Claude skills area. Afterwards, Claude applies the standard when writing or editing `.tex` and, when asked, creates the `Tese LaTeX - Modelo` folder in the connected folder, with the same contents as `modelo/`.

## Use together with the scientific figures skill

For the figures in the document, this skill can be used together with **scientific-figures**: <https://github.com/juancarlosc504/scientific-figures>. It generates plots, maps and diagrams in matplotlib at publication standard (Times + STIX, transparent background, decimal comma, no embedded title), always with a `gera_*.py` script that reproduces the figure and a checker (`checa_figura.py`). The division of tasks is simple: **scientific-figures** produces the figure file (PDF or PNG) in `figuras/`, and **minha-tese-bonitinha** takes care of how the figure enters the LaTeX (`figure` environment, caption, label, `\legend{Fonte: ...}` and `\autoref`).

Installing the figures skill in Claude Code:

```bash
claude plugin marketplace add juancarlosc504/scientific-figures
claude plugin install scientific-figures@scientific-figures
```

On claude.ai, download the `scientific-figures.skill` file from the repository's Releases page and upload it in *Settings, Capabilities, Skills*.

## Repository structure

```
minha-tese-bonitinha/
├── README.md                    Portuguese
├── README.en.md                 English
├── SKILL.md                     Claude skill (formatting rules and embedded template), Portuguese
├── SKILL.en.md                  Claude skill, English translation
└── modelo/                      canonical LaTeX folder
    ├── tese_modelo.tex          master document
    ├── referencias.bib          single bibliography
    ├── config/comandos.tex      review markers (\novo, \verde, \magenta, \preencher, \citar, \aref)
    ├── pretextual/              cover, title page, abstract (pt/en), lists
    ├── capitulos/               capitulo_modelo.tex
    ├── apendices/               apendice_modelo.tex
    ├── figuras/  graficos/
    └── abntex2.cls, abntex2cite.sty, *.bst, *.ldf   class and language files
```

## Note on licenses

The `abntex2*` and `*.bst` files belong to the abnTeX2 project and follow its original LPPL license (<https://www.abntex.net.br>).
