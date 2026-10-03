---
name: "my-pretty-thesis"
description: "LaTeX formatting standard for theses and dissertations, based on the canonical abnTeX2 model, with a template folder containing no filled-in data. Use when writing or editing .tex files in this standard or when asked for the template folder. Authorship: Juan Carlos Lamonica, MSc."
---

# My pretty thesis (English version)

Authorship: Juan Carlos Lamonica, MSc.

This is the English translation of `SKILL.md` (the Portuguese original, which is the reference version). The skill is based on the canonical abnTeX2 model (<https://www.abntex.net.br>): it starts from the `abntex2` class and the ABNT standards it implements, and adds the formatting conventions described below. It gathers LaTeX formatting conventions for theses and dissertations, plus a canonical LaTeX folder template with no filled-in data, ready to be completed. Apply it to any new or edited text in LaTeX projects following this standard. The skill covers form only; content, methodology and scientific choices are the decision of the author of the work.

Note on language: the template is a Brazilian (ABNT) document, so its typeset text (chapter titles, "Figura", "Tabela", "Apêndice", list names) stays in Portuguese and the placeholders in the template files are in Portuguese. The files of the template are identical in both versions; they are the ones in `modelo/` of the repository and in the section "Template files" of `SKILL.md`.

## Creating the template folder

When the author asks for the LaTeX template folder (or a new document in the same standard), create in the connected folder, through the shell of the author's computer, the tree below with the contents listed in `SKILL.md`, section "Arquivos do modelo". Default name `Tese LaTeX - Modelo`, next to the existing LaTeX project, unless the author indicates another. Before writing, check whether the folder already exists; if it does, do not overwrite anything and ask. Never alter an existing LaTeX project when creating the template.

The template is canonical: all identification data (title, author, advisors, institution, department, program, place, year, keywords) are left as bracketed placeholders, such as `[TÍTULO DO TRABALHO]`, for the author to fill in. Never fill the template with data from an existing work or with terms from its subject; content examples are neutral. The formatting, on the other hand, is that of the standard, complete.

```
Tese LaTeX - Modelo/
├── tese_modelo.tex          ← master document (always compile this one)
├── referencias.bib          ← single bibliography
├── config/comandos.tex      ← review markers and custom commands
├── pretextual/              ← capa, folha_de_rosto, dedicatoria, agradecimentos, epigrafe, resumo (pt and en), listas_pretextuais
├── capitulos/capitulo_modelo.tex
├── apendices/apendice_modelo.tex
└── figuras/  graficos/      ← empty (with .gitkeep)
```

Class and style files (`abntex2.cls`, `abntex2cite.sty`, `*.bst`, `brazil.ldf`, etc.) are not recreated: if the computer's TeX installation does not have them, copy them from an existing abnTeX2 project. After creating, compile once with `latexmk -pdf tese_modelo.tex` and report only the summary (pages and errors); the template must build with no errors and no undefined references. `abntex2-options.bib` needs the `nbr10520-2023` entry (see "Citations and bibliography"); without it BibTeX fails.

## Converting a .docx into a LaTeX project

When the author asks to typeset a thesis written in Word, generate a project in this skill's standard, with master document `tese.tex`, one file per chapter in `capitulos/` and the front matter in `pretextual/`. The preamble, cover, title page and other front matter are those of this skill's template (see "Document and preamble"), with no dependence on any thesis in the folder. What worked: extract the `.docx` with `pandoc` to JSON (AST) and the media with `--extract-media`; walk the AST with a custom Python script (not `python-docx`), writing each block already in these conventions; and apply a final regular-expression pass for leftovers. Watch for unsupported Unicode (the prime `U+2032` becomes `$'$`), `%` inside Python format strings (write `%%`), Word author-year citations (mapped to `\cite` or `\citeonline` against keys of the `.bib` built from the reference list; check `et al.`, `\&`, `Jr.` and year mismatches), page locators such as "p. 12" (must not become decimals), numbers and units (`$n$~unit`), figures (saved as `figNN_description`, TIFF converted to JPG) and manual numbering (replaced by `\label` and `\autoref`). Ambiguous items are never resolved silently: log them in a short report to the author. The original `.docx` is never altered.

## Before editing

The author may edit the files externally (for example, in TeXstudio) between sessions. Before changing any `.tex`, `.bib` or figure, re-read the current version from the connected folder and check the modification time; only then edit, and write in a way that does not overwrite the author's changes. Accumulate the edits and compile a single time at the end, reporting only the log summary (number of pages and errors). Visual check of the PDF only when the layout is critical or the author asks.

## Document and preamble

The master document (`tese_modelo.tex` in the template; always compile it) uses the `abntex2` class with 12pt, `oneside`, `openright`, `a4paper`, `chapter=TITLE` (chapter titles in capitals) and `sumario=abnt-6027-2012`. Main language `brazil`, with `\frenchspacing`. Latin Modern font (`lmodern`, `T1`, `utf8`) and `microtype` for justification. Text is always justified; paragraph indent `\setlength{\parindent}{1.3cm}` and `\setlength{\parskip}{0.2cm}`. Section and subsection titles in `\normalsize`, bold, `lmr` font. Figures and tables numbered by chapter (`\counterwithin`). Citations with `abntex2cite` (`alf`, `bibjustif`, `abnt-etal-text=it`, `abnt-cite-style=nbr10520-2023`). Do not change the preamble without an explicit request; new commands go in `config/comandos.tex`.

This template's preamble, `config/comandos.tex` and front matter already incorporate the formatting of the author's own thesis, which was used only to settle the standard: whatever it taught about formatting is in this skill, and any new or converted project starts from here, with no thesis needed in the folder. Left out, as specific to one work, are the packages `lipsum`, `svg`, `tabu` and `perpage`, the `\tikzstyle`, the `comment` blocks and topic-specific includes. When generating a project, change only the identification data, `\graphicspath` and the chapter list. The author's thesis, with its data, text and figures, never enters the template or any public repository. Converted works (for example, from a .docx) serve to validate the skill, not to add new rules.

The cover and title page are custom (no `\imprimircapa`): centred blocks at 12 pt, institution typed in capitals, author in capitals, title in bold capitals, place and year at the bottom. No logo unless the author asks. Abstract (Portuguese) and Abstract (English) live in the same `pretextual/resumo.tex`, with `\absparsep` at 18 pt and keywords separated by semicolons with a final period. Figures taller than wide (height/width above about 1.15) use `height=0.68\textheight,keepaspectratio`; surnames with a suffix (Jr., Filho, Neto) go entirely in braces in the `.bib`; and when the work calls for "Lista de figuras", add `\addto\captionsbrazil{\renewcommand{\listfigurename}{Lista de figuras}}` to `config/comandos.tex`.

## Files and structure

Each file in `capitulos/`, `pretextual/` and `apendices/` starts with the magic line `% !TeX root = ../tese_modelo.tex` (or the name of the project's master document). Chapter: `\chapter{Title}` followed by `\label{ch:...}` on the next line; section: `\section{...}` and `\label{sec:...}` on the next line. Label prefixes: `ch:`, `sec:`, `subsec:`, `fig:`, `tab:`, `eq:`, `ap:`. Labels in lowercase, without accents, descriptive (`fig:fluxo`, `tab:parametros`, `eq:principal`). Each chapter opens with an introductory paragraph before the first section. Running text in prose; lists only when the content is truly enumerable.

## Cross-references

Always `\autoref{...}` (never "Figure 3" typed by hand), including for equations and sections. For appendices use `\aref{ap:...}`, which changes the prefix "Capítulo" to "Apêndice". The label of an appendix `\section` already yields "Seção".

## Citations and bibliography

Citations follow **NBR 10520:2023**: the surname comes out in mixed case both in parentheses and in running text — "(Silva, 2020)" and "Silva (2020)", never "(SILVA, 2020)". The reference list still follows NBR 6023:2018, with surnames in capitals. In abnTeX2 this is switched on with the `abntex2cite` option `abnt-cite-style=nbr10520-2023`, which reads a dedicated entry added to the template's `abntex2-options.bib`. The stock option `abnt-cite-style=AuthorYEAR` **does not work**: BibTeX keys are case-insensitive, so that entry is discarded as a duplicate of `abnt-cite-style=AUTHORYEAR` and citations stay in capitals with no warning. When setting up a project, copy `abntex2-options.bib` from `modelo/` (with the entry) and otherwise do not touch that file. If `abntex2-options.bib` comes from elsewhere, append:

```bibtex
@ABNT-options{abnt-cite-style=nbr10520-2023,
 abnt-cite-style="(Author, YEAR)",
 key="aaaa"}
```

Citation variants in `abntex2cite`:

| Command | Output | Use |
|---|---|---|
| `\cite{key}` | (Silva, 2020) | parenthetical citation |
| `\cite[p.~12]{key}` | (Silva, 2020, p. 12) | direct quotation, with page |
| `\citeonline{key}` | Silva (2020) | author as subject of the sentence |
| `\citeauthoronline{key}` | Silva | name in running text, with the year in the same sentence |
| `\citeyear{key}` | 2020 | year only |
| `\apud{a}{b}` / `\apudonline{a}{b}` | (Silva, 2000 apud Souza, 2020) / Silva (2000 apud Souza, 2020) | secondary citation |

Never type the author's name before a `\cite` ("Ogawa \cite{...}", "Devic et al. (2016)"): use `\citeonline` or `\citeauthoronline` + `\citeyear`. ABNT requires the year next to the author; the name alone only when the year appears in the same sentence.

Single bibliography in `referencias.bib`, loaded by `\bibliography{referencias}`; new references always go in that file, never in another `.bib`. In the `title` field always use double braces (`title = {{Article Title}}`), so the capitalization stays exactly as typed; the models for every entry type are in `referencias.bib` (section "Arquivos do modelo" of `SKILL.md` and the `modelo/` folder). Exception: in `@proceedings` the title takes single braces. For `@phdthesis` and `@mastersthesis`, `type` holds only the field (`Doutorado em Área`), since the style already prints "Tese" or "Dissertação". Corporate authors go in double braces and in capitals.

## Numbers, units and symbols

Decimal comma in math mode and a non-breaking space before the unit: `$0{,}5$~cm`, `$5$~mm`, `$20$~kg`, `$2{,}5$~m`. Never a decimal point, never number and unit glued together or separated by an ordinary space. Isotopes as `$^{14}$C`; degrees as `$0^\circ$` (`\degree` comes from `gensymb`). Percentages as `$100\%$`. Variables and symbols always in math mode and in italics (`$x$`, `$y$`, `$\theta$`). Foreign words and Latin terms in italics (`\textit{software}`, `\textit{in loco}`). LaTeX quotes (``like this''), never straight quotes. Appositive dash with `---` between spaces.

## Equations

`equation` environment with `\label{eq:...}` on the next line, final punctuation inside the equation (comma if the sentence continues with "onde"/"where", period if it ends), indentation by tab, `\dfrac` for isolated fractions and `cases` with `\\[8pt]` for cases. Time derivative with `\dot{x}`. Define each symbol right after the equation, in prose. Vertical spacing already set in the preamble (`\abovedisplayskip` 5pt, `\belowdisplayskip` 12pt); do not override.

## Tables (mandatory standard)

Centered `table[h!]` environment, `\setlength{\tabcolsep}{8pt}`, `\renewcommand{\arraystretch}{1.2}`, `\small`, centered fixed-width columns `M{<width>}` (preamble command; the width is chosen case by case according to the content, there is no default width). Rules only from `booktabs` (`\toprule`, `\midrule`, `\bottomrule`), never `\hline` or vertical bars. Header in `\textbf`; line breaks in headers with `\shortstack[c]{line1 \\ line2}`. Caption above, always as `\caption[short title]{full caption + source}` with `\label{tab:...}` (see "Figure and table captions"); do not use `\legend{}`.

```latex
\begin{table}[h!]\centering
	\setlength{\tabcolsep}{8pt}
	\renewcommand{\arraystretch}{1.2}
	\caption[Short table title]{Full table caption. Elaborado pelo autor.}\label{tab:rotulo}
	\small
	\begin{tabular}{M{1,8cm}M{4,2cm}M{2,4cm}}\toprule
		\textbf{Column A} & \textbf{Column B} & \textbf{Column C} \\
		\midrule
		... & ... & ... \\
		\bottomrule
	\end{tabular}
\end{table}
```

## Figures

`figure[h!]` environment with `\centering`, `\includegraphics[width=0.95\linewidth]{figuras/file.pdf}` (the `\graphicspath` covers `figuras/` and `graficos/`), `\caption[short title]{full caption + source}` and `\label{fig:...}` (see "Figure and table captions"); do not use `\legend{}`. Prefer vector PDF; PNG only for raster images. Subfigures: `subfigure[b]{0.49\linewidth}` with `\centering`, `\hfill` between the two columns and `\\[1.5ex]` between rows, each with its own short `\caption{}` (no short title: subfigures do not go into the list), and the general caption at the end, in the same `\caption[short]{full + source}` format.

## Figure and table captions (mandatory standard)

Every figure and every table uses `\caption[short title]{full caption}`:

- `[short title]` always present: it is what goes into the List of figures / List of tables. Title only, no final period, no citation and no revision marker (`\novo` etc.).
- `{full caption}`: the full title, the needed explanation and, **at the end, the source**, inside the caption itself. Do not use `\legend{Fonte: ...}`.
- Source wording (kept in Portuguese in the document): own figure or table, `Elaborado pelo autor.`; redrawn from another work's data, `Elaborado pelo autor, com base em \citeonline{key}.`; reproduced or adapted, `Adaptado de \citeonline{key}.`. Always a `\citeonline`/`\cite`, never a typed name.
- Notes that used to go in `\legend` (acronyms, medium, remarks) go in the full caption, after the source.
- With the short title present, the full caption may contain `\autoref`, `\cite` and `\novo{}` without breaking the list.

```latex
\caption[Arquitetura dos filmes radiocrômicos EBT2 e EBT3]{Desenho esquemático da diferença entre a arquitetura dos filmes radiocrômicos EBT2 e EBT3. Adaptado de \citeonline{devic2016reference}.}
```

Note on the standard: for tables, NBR 14724 refers to the IBGE tabular presentation rules, which put the source in the table footer. This standard moves the source into the caption by the author's choice; warn if the committee or the program's library requires the footer.

For scientific figures in matplotlib, this skill can be used together with the `scientific-figures` skill (<https://github.com/juancarlosc504/scientific-figures>), which generates each figure with a reproducible `gera_*.py` script and a checker (`checa_figura.py`). This skill takes care of the LaTeX (environment, caption, label, source); `scientific-figures` takes care of the figure itself. In case of conflict about graphic content, `scientific-figures` prevails. Figures generated in Python (matplotlib) follow this standard, and the generating `.py` is always saved in `figuras/` (e.g. `gera_figura.py`):

```python
plt.rcParams.update({
    "font.family": "serif", "mathtext.fontset": "cm",
    "axes.edgecolor": "black", "axes.linewidth": 1.0,
    "legend.frameon": True, "legend.edgecolor": "black", "legend.framealpha": 1.0,
})
```

No title inside the image (the caption goes in `\caption`). Serif font, axis labels around 12 pt and bold, variables in italics via mathtext (`$x$`, `$\theta$`). Decimal comma (`r"$2{,}5$ m"`). Light gray dotted grid (`ls=":", color="0.82"`) on data plots; schematic diagrams without grid. Colors: blue `#1f5fa8` and red `#c0392b` for highlights; diagram blocks in light blue `#CFE2F3`, light green `#EAF7EA` and beige `#E8D8C3`. Labels with initial capital on key terms; symbol legend in the format "symbol - description", one line per symbol; numbering only when the text refers to it by number. Check for overlaps and text spilling over the edge (use `tight_layout` or limits with margin) and generate a PNG preview before overwriting the final PDF.

## Review markers (defined in config/comandos.tex)

`\novo{...}` marks newly inserted text in red; approving means changing the definition to `#1`, without touching the text. `\verde{...}` marks incorporations from a specific review, `\magenta{...}` marks suggested citations not yet evaluated, `\preencher{description}` marks pending gaps and `\citar` marks a pending citation. When inserting new text at the author's request, wrap the passage in `\novo{}` (new section titles included). Never remove existing markers on your own.

## Lists of acronyms and symbols

They live in `pretextual/listas_pretextuais.tex` (`\begin{siglas}` and `\begin{simbolos}`), in alphabetical order (symbols: Latin before Greek). English acronyms: full form in italics and translation in parentheses, in ABNT style. Updating is manual: do not review or repopulate on every edit, only when the author asks for a sweep.

## Parts and appendices

`\part{...}` groups chapters into parts, when needed. Appendices go in `apendices/`, inside `apendicesenv` with `\partapendices`, one file per appendix; reference them with `\aref`. Code listings and inputs use `lstlisting` with the `\lstset` already defined in the preamble.

## Compilation

`latexmk -pdf <master document>.tex`. An `Undefined control sequence` error with `\tempf@rtoc` indicates a corrupted `.toc` or `.aux`, not a text error. In a connected folder that blocks deletion, truncate only `.toc` and `.out` (`: > file.toc`), never the `.aux`; if bibtex reports "no \citation commands", run `pdflatex`, `bibtex`, `pdflatex`, `pdflatex`.

## Final check

Text justified and without margin overflow; units in the format `$n$~unit` with decimal comma; all cross-references via `\autoref`/`\aref`; tables without `\hline`; figures and tables with `\caption[short]{full + source}` and no `\legend`; citations in mixed case (NBR 10520:2023); citations in `referencias.bib` and with the proper `\cite` or `\citeonline`; new text in `\novo{}`; compilation with no errors and no undefined references or citations.

## Template files

The files of the template (`tese_modelo.tex`, `config/comandos.tex`, `capitulos/capitulo_modelo.tex`, `apendices/apendice_modelo.tex`, `pretextual/*.tex`, `referencias.bib`) are exactly those listed in `SKILL.md`, section "Arquivos do modelo", and available ready-made in the `modelo/` folder of the repository. All bracketed fields are for the author to fill in; nothing in them carries data from an existing work. Placeholder figures `figuras/exemplo*.pdf` are only illustrative: when creating the template, generate three minimal vector PDFs for testing, or comment out the `\includegraphics` lines, so the template compiles without error.
