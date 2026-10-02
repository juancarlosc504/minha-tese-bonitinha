# Skill installation guide

[Português](INSTALL.md) | **English**

Authorship: Juan Carlos Lamonica, MSc. This guide shows how to install the `minha-tese-bonitinha` skill in Claude (claude.ai and the desktop app) and in Claude Code. The skill is the `SKILL.md` file at the repository root; the English version is `SKILL.en.md`.

First, have a TeX distribution installed (MacTeX, MiKTeX or TeX Live), as described in `README.en.md`. The skill guides writing and formatting, but your computer's TeX does the compiling.

## 1. Get the files

```bash
git clone https://github.com/juancarlosc504/minha-tese-bonitinha.git
cd minha-tese-bonitinha
```

## 2. Install in Claude (claude.ai and desktop app)

The skill is uploaded as a `.zip` file containing a folder with `SKILL.md` inside.

1. Build the skill folder with only the required file:

```bash
mkdir -p /tmp/minha-tese-bonitinha
cp SKILL.md /tmp/minha-tese-bonitinha/SKILL.md
cd /tmp && zip -r minha-tese-bonitinha.zip minha-tese-bonitinha
```

2. In Claude, open *Settings*, *Capabilities*, *Skills* section, and upload `minha-tese-bonitinha.zip`.
3. Turn the skill on in the list. It will be used whenever the conversation involves writing or editing `.tex` in this standard, or when you ask for the template folder.

For the English version, copy `SKILL.en.md` as `SKILL.md` into the folder (skill name: `my-pretty-thesis`). Do not install both versions at the same time, to avoid a duplicated trigger.

Another option, with no zip: in a Claude conversation, ask "create the minha-tese-bonitinha skill from this SKILL.md" and attach the file; Claude shows a review card and the skill is saved when you confirm.

## 3. Install in Claude Code

Claude Code loads skills from folders that contain a `SKILL.md`. There are two ways.

**For all your projects (personal install):**

```bash
mkdir -p ~/.claude/skills
git clone https://github.com/juancarlosc504/minha-tese-bonitinha.git ~/.claude/skills/minha-tese-bonitinha
```

To update later: `git -C ~/.claude/skills/minha-tese-bonitinha pull`.

**Only for the thesis project:**

```bash
cd /path/to/your/thesis
mkdir -p .claude/skills/minha-tese-bonitinha
cp /path/to/repository/SKILL.md .claude/skills/minha-tese-bonitinha/SKILL.md
```

In this case, the `.claude/skills/` folder can be versioned together with the thesis. Open Claude Code in the thesis folder (`claude`); the skill is loaded when the session starts. Restart the session if the skill was installed while Claude Code was already open.

## 4. Using it from Claude Code

Open a terminal in the folder where the thesis will live and start Claude Code:

```bash
cd ~/Documents/MyThesis
claude
```

Example requests:

- "Create the minha-tese-bonitinha LaTeX template folder." Claude creates `Tese LaTeX - Modelo/` with all the files, compiles once and reports only the summary (pages and errors).
- "Write chapter 2 in `capitulos/` following the skill standard, with the new text in `\novo{}`."
- "Add this reference to `referencias.bib`", pasting the DOI or the data. Claude uses the right entry model and a double-braced title.
- "Review the tables in chapter 3 against the skill standard."

To call the skill explicitly, type `/minha-tese-bonitinha` at the start of the request.

In Claude Code, Claude works directly on the files of your computer and runs `latexmk`, so no extra file-copying step is needed.

## 5. Using it together with the figures skill

For figures, also install `scientific-figures` (<https://github.com/juancarlosc504/scientific-figures>):

```bash
claude plugin marketplace add juancarlosc504/scientific-figures
claude plugin install scientific-figures@scientific-figures
```

`scientific-figures` generates the figure in matplotlib with a reproducible script; `minha-tese-bonitinha` takes care of how it enters the LaTeX.

## 6. Check that it worked

In the conversation, ask: "Which table rules will you follow for this thesis?" The answer should mention `booktabs` rules, centered `M{...}` columns and `\legend{Fonte: ...}`. If it does not, the skill was not loaded: check the folder name, that `SKILL.md` is inside it, and restart the session.
