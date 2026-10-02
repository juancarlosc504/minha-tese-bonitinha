---
name: "minha-tese-bonitinha"
description: "Padrão de formatação LaTeX para teses e dissertações, baseado no modelo canônico do abnTeX2, com pasta modelo sem dados preenchidos. Usar ao escrever ou editar .tex nesse padrão ou ao pedir a pasta modelo. Autoria de Juan Carlos Lamonica, MSc."
---

# Minha tese bonitinha

Autoria de Juan Carlos Lamonica, MSc.

Esta skill é feita com base no modelo canônico do abnTeX2 (<https://www.abntex.net.br>): parte da classe `abntex2` e das normas ABNT que ele implementa e acrescenta as convenções de formatação descritas abaixo. Reúne convenções de formatação LaTeX para teses e dissertações, mais um modelo canônico de pasta LaTeX, sem dados preenchidos, pronto para ser preenchido. Aplicar em todo texto novo ou editado em projetos LaTeX nesse padrão. A skill trata apenas de forma; conteúdo, metodologia e escolhas científicas são decisão do autor do trabalho.

## Criar a pasta modelo

Quando o autor pedir a pasta LaTeX modelo (ou um novo documento no mesmo padrão), criar na pasta conectada, pelo shell do computador dele, a árvore abaixo com o conteúdo da seção "Arquivos do modelo". Nome padrão `Tese LaTeX - Modelo`, ao lado do projeto LaTeX existente, salvo se ele indicar outro. Antes de gravar, verificar se a pasta já existe; se existir, não sobrescrever nada e perguntar. Nunca alterar um projeto LaTeX existente ao criar o modelo.

O modelo é canônico: todos os dados de identificação (título, autor, orientadores, instituição, departamento, programa, local, ano, palavras-chave) ficam como marcadores entre colchetes, como `[TÍTULO DO TRABALHO]`, para o autor preencher. Nunca preencher o modelo com dados de um trabalho existente nem com termos do tema dele; os exemplos de conteúdo são neutros. A formatação, por outro lado, é a do padrão, completa.

```
Tese LaTeX - Modelo/
├── tese_modelo.tex          ← documento mestre (compilar sempre este)
├── referencias.bib          ← bibliografia única
├── config/comandos.tex      ← marcadores de revisão e comandos próprios
├── pretextual/              ← capa, folha_de_rosto, resumo, abstract, listas_pretextuais
├── capitulos/capitulo_modelo.tex
├── apendices/apendice_modelo.tex
└── figuras/  graficos/      ← vazias (com .gitkeep)
```

Os arquivos de classe e estilo (`abntex2.cls`, `abntex2cite.sty`, `*.bst`, `brazil.ldf` etc.) não são recriados: se o TeX do computador não os tiver, copiá-los de um projeto abnTeX2 existente. Depois de criar, compilar uma vez com `latexmk -pdf tese_modelo.tex` e informar só o resumo (páginas e erros); o modelo deve sair sem erros e sem referências indefinidas.

## Antes de editar

O autor pode editar os arquivos por fora (por exemplo, no TeXstudio) entre sessões. Antes de alterar qualquer `.tex`, `.bib` ou figura, reler a versão atual da pasta conectada e conferir o horário de modificação; só então editar, e gravar de forma a não sobrescrever alterações dele. Acumular as edições e compilar uma única vez ao final, reportando apenas o resumo do log (número de páginas e de erros). Verificação visual do PDF só quando o layout for crítico ou o autor pedir.

## Documento e preâmbulo

O documento mestre (`tese_modelo.tex` no modelo; compilar sempre ele) usa a classe `abntex2` com 12pt, `oneside`, `openright`, `a4paper`, `chapter=TITLE` (capítulos em caixa alta) e `sumario=abnt-6027-2012`. Idioma principal `brazil`, com `\frenchspacing`. Fonte Latin Modern (`lmodern`, `T1`, `utf8`) e `microtype` para justificação. O texto é sempre justificado; recuo de parágrafo `\setlength{\parindent}{1.3cm}` e `\setlength{\parskip}{0.2cm}`. Títulos de seção e subseção em `\normalsize`, negrito, fonte `lmr`. Figuras e tabelas numeradas por capítulo (`\counterwithin`). Citações com `abntex2cite` (`alf`, `bibjustif`, `abnt-etal-text=it`). Não alterar o preâmbulo sem pedido explícito; novos comandos vão em `config/comandos.tex`.

## Arquivos e estrutura

Cada arquivo de `capitulos/`, `pretextual/` e `apendices/` começa com a linha mágica `% !TeX root = ../tese_modelo.tex` (ou o nome do documento mestre do projeto). Capítulo: `\chapter{Título}` seguido de `\label{ch:...}` na linha seguinte; seção: `\section{...}` e `\label{sec:...}` na linha seguinte. Prefixos de rótulo: `ch:`, `sec:`, `subsec:`, `fig:`, `tab:`, `eq:`, `ap:`. Rótulos em minúsculas, sem acento, descritivos (`fig:fluxo`, `tab:parametros`, `eq:principal`). Cada capítulo abre com um parágrafo de apresentação antes da primeira seção. Texto corrido em prosa; listas só quando o conteúdo for realmente enumerável.

## Referências cruzadas

Sempre `\autoref{...}` (nunca "Figura 3" digitado à mão), inclusive para equações e seções. Para apêndices usar `\aref{ap:...}`, que corrige o prefixo "Capítulo" para "Apêndice". Rótulo de `\section` de apêndice já resulta em "Seção".

## Citações e bibliografia

`\cite{chave}` para citação entre parênteses e `\citeonline{chave}` quando o autor é parte da frase ("conforme \citeonline{autor2024}"). Bibliografia única em `referencias.bib`, carregada por `\bibliography{referencias}`; novas referências entram sempre nesse arquivo, nunca em outro `.bib`. `abntex2-options.bib` é do template e não se mexe.

## Números, unidades e símbolos

Vírgula decimal em modo matemático e espaço não separável antes da unidade: `$0{,}5$~cm`, `$5$~mm`, `$20$~kg`, `$2{,}5$~m`. Nunca ponto decimal, nunca número e unidade colados ou separados por espaço comum. Isótopos como `$^{14}$C`; graus como `$0^\circ$` (o `\degree` vem do `gensymb`). Percentuais como `$100\%$`. Variáveis e símbolos sempre em modo matemático e em itálico (`$x$`, `$y$`, `$\theta$`). Estrangeirismos e termos latinos em itálico (`\textit{software}`, `\textit{in loco}`). Aspas LaTeX (``assim''), nunca aspas retas. Travessão de aposto com `---` entre espaços.

## Equações

Ambiente `equation` com `\label{eq:...}` na linha seguinte, pontuação final dentro da equação (vírgula se a frase continua com "onde", ponto se encerra), indentação por tabulação, `\dfrac` em frações isoladas e `cases` com `\\[8pt]` para casos. Derivada temporal com `\dot{x}`. Definir cada símbolo logo após a equação, em prosa. Espaçamentos verticais já definidos no preâmbulo (`\abovedisplayskip` 5pt, `\belowdisplayskip` 12pt); não sobrescrever.

## Tabelas (padrão obrigatório)

Ambiente `table[h!]` centralizado, `\setlength{\tabcolsep}{8pt}`, `\renewcommand{\arraystretch}{1.2}`, `\small`, colunas centralizadas de largura fixa `M{<largura>}` (comando do preâmbulo; a largura é arbitrada caso a caso conforme o conteúdo, não existe largura padrão). Réguas apenas `booktabs` (`\toprule`, `\midrule`, `\bottomrule`), nunca `\hline` nem barras verticais. Cabeçalho em `\textbf`; quebra de linha em cabeçalho com `\shortstack[c]{linha1 \\ linha2}`. Legenda acima (`\caption` com `\label{tab:...}`), fonte abaixo com `\legend{}`.

```latex
\begin{table}[h!]\centering
	\setlength{\tabcolsep}{8pt}
	\renewcommand{\arraystretch}{1.2}
	\caption{Legenda da tabela.}\label{tab:rotulo}
	\small
	\begin{tabular}{M{1,8cm}M{4,2cm}M{2,4cm}}\toprule
		\textbf{Coluna A} & \textbf{Coluna B} & \textbf{Coluna C} \\
		\midrule
		... & ... & ... \\
		\bottomrule
	\end{tabular}
	\legend{Fonte: elaborado pelo autor.}   % ou \legend{Fonte: \citeonline{chave}.}
\end{table}
```

## Figuras

Ambiente `figure[h!]` com `\centering`, `\includegraphics[width=0.95\linewidth]{figuras/arquivo.pdf}` (o `\graphicspath` cobre `figuras/` e `graficos/`), `\caption[título curto para a lista]{legenda completa}`, `\label{fig:...}` e, ao final, `\legend{Fonte: elaborado pelo autor.}` (ou `\citeonline{chave}` quando adaptada). Preferir PDF vetorial; PNG só para imagens rasterizadas. Subfiguras: `subfigure[b]{0.49\linewidth}` com `\centering`, `\hfill` entre as duas colunas e `\\[1.5ex]` entre linhas, cada uma com sua `\caption{}` curta, e a legenda geral ao final.

Para figuras científicas em matplotlib, esta skill pode ser usada em conjunto com a skill `scientific-figures` (<https://github.com/juancarlosc504/scientific-figures>), que gera cada figura com um script `gera_*.py` reprodutível e um verificador (`checa_figura.py`). Esta skill cuida do LaTeX (ambiente, legenda, rótulo, fonte); a `scientific-figures` cuida da figura em si. Em caso de conflito sobre o conteúdo gráfico, vale a `scientific-figures`. As figuras geradas em Python (matplotlib) seguem este padrão, e o `.py` gerador é sempre salvo em `figuras/` (ex.: `gera_figura.py`):

```python
plt.rcParams.update({
    "font.family": "serif", "mathtext.fontset": "cm",
    "axes.edgecolor": "black", "axes.linewidth": 1.0,
    "legend.frameon": True, "legend.edgecolor": "black", "legend.framealpha": 1.0,
})
```

Sem título dentro da imagem (a legenda fica no `\caption`). Fonte serifada, rótulos de eixo em torno de 12 pt e em negrito, variáveis em itálico via mathtext (`$x$`, `$\theta$`). Decimal com vírgula (`r"$2{,}5$ m"`). Grade cinza clara pontilhada (`ls=":", color="0.82"`) nos gráficos de dados; diagramas esquemáticos sem grade. Cores: azul `#1f5fa8` e vermelho `#c0392b` para destaques; blocos em diagramas com azul claro `#CFE2F3`, verde claro `#EAF7EA` e bege `#E8D8C3`. Rótulos com inicial maiúscula nos termos-chave; legenda de símbolos no formato "símbolo - descrição", uma linha por símbolo; numeração só quando o texto referencia por número. Conferir sobreposição e texto vazando da borda (usar `tight_layout` ou limites com folga) e gerar prévia PNG antes de sobrescrever o PDF definitivo.

## Marcadores de revisão (definidos em config/comandos.tex)

`\novo{...}` marca texto recém-inserido em vermelho; aprovar significa trocar a definição por `#1`, sem tocar no texto. `\verde{...}` marca incorporações de uma revisão específica, `\magenta{...}` marca citações sugeridas ainda não avaliadas, `\preencher{descrição}` marca lacunas pendentes e `\citar` marca citação pendente. Ao inserir texto novo a pedido do autor, envolver o trecho em `\novo{}` (títulos de seção novos incluídos). Nunca remover marcadores existentes por conta própria.

## Listas de siglas e símbolos

Ficam em `pretextual/listas_pretextuais.tex` (`\begin{siglas}` e `\begin{simbolos}`), em ordem alfabética (símbolos: latinos antes de gregos). Siglas em inglês: forma por extenso em itálico e tradução entre parênteses, no estilo ABNT. A atualização é manual: não revisar nem repopular a cada edição, apenas quando o autor pedir uma varredura.

## Partes e apêndices

`\part{...}` agrupa capítulos em partes, quando necessário. Apêndices ficam em `apendices/`, dentro de `apendicesenv` com `\partapendices`, um arquivo por apêndice; referenciá-los com `\aref`. Listagens de código e inputs usam `lstlisting` com o `\lstset` já definido no preâmbulo.

## Compilação

`latexmk -pdf <documento mestre>.tex`. Erro `Undefined control sequence` com `\tempf@rtoc` indica `.toc` ou `.aux` corrompido, não erro de texto. Em pasta conectada que bloqueia exclusão, truncar apenas `.toc` e `.out` (`: > arquivo.toc`), nunca o `.aux`; se o bibtex acusar "no \citation commands", rodar `pdflatex`, `bibtex`, `pdflatex`, `pdflatex`.

## Conferência final

Texto justificado e sem estouro de margem; unidades no formato `$n$~unidade` com vírgula decimal; todas as referências cruzadas via `\autoref`/`\aref`; tabelas sem `\hline`; figuras com `\legend{Fonte: ...}`; citações em `referencias.bib` e com `\cite` ou `\citeonline` adequados; texto novo em `\novo{}`; compilação sem erros e sem referências ou citações indefinidas.

## Arquivos do modelo

Todos os campos entre colchetes são para o autor preencher; nada abaixo traz dados de um trabalho existente.

### tese_modelo.tex

```latex
\documentclass[12pt,openright,oneside,a4paper,chapter=TITLE,english,french,spanish,brazil,sumario=abnt-6027-2012]{abntex2}
\DisemulatePackage{setspace}
\usepackage{setspace}
\usepackage{textcomp}
\usepackage{gensymb}
\DeclareUnicodeCharacter{00B0}{\degree}
\usepackage{lmodern}
\usepackage[T1]{fontenc}
\usepackage[utf8]{inputenc}
\usepackage{indentfirst}
\usepackage[dvipsnames]{xcolor}
\usepackage{graphicx}
\graphicspath{{figuras/}{graficos/}}
\usepackage{microtype}
\usepackage{enumitem}
\input{config/comandos}
\usepackage[alf,bibjustif,abnt-etal-text=it,abnt-etal-list=2,abnt-etal-cite=2]{abntex2cite}
\usepackage{multicol}
\usepackage{verbatim}
\usepackage{listings}
\lstset{basicstyle=\ttfamily\footnotesize,breaklines=true,breakindent=0pt,
        columns=fullflexible,keepspaces=true,frame=single,framerule=0.3pt,
        showstringspaces=false,xleftmargin=6pt,xrightmargin=6pt,breakautoindent=false}
\usepackage{imakeidx}
\usepackage{nomencl}
\makenomenclature
\usepackage{isotope}
\counterwithin{figure}{chapter}
\counterwithin{table}{chapter}
\usepackage{amsmath}
\usepackage{amssymb}
\usepackage{booktabs}
\usepackage{tikz}
\usepackage{array}
\usepackage{longtable}
\usepackage{multirow}
\usepackage{subcaption}
\usepackage{tocloft}
\usepackage{pdfpages}

% Colunas centralizadas de largura fixa (padrão de tabelas)
\newcolumntype{P}[1]{>{\centering\arraybackslash}p{#1}}
\newcolumntype{M}[1]{>{\centering\arraybackslash}m{#1}}

% ---------------------------------------------------------------
% DADOS DE IDENTIFICAÇÃO (capa e folha de rosto) --- PREENCHER
% ---------------------------------------------------------------
\titulo{[TÍTULO DO TRABALHO]}
\autor{[NOME COMPLETO DO AUTOR]}
\local{[CIDADE]}
\data{[ANO]}
\orientador{[Titulação e nome do orientador]}
\coorientador{[Titulação e nome do coorientador, se houver]}
\instituicao{%
	[UNIVERSIDADE]
	\par
	[UNIDADE ACADÊMICA]
	\par
	[DEPARTAMENTO]}
\tipotrabalho{[Tese (Doutorado) / Dissertação (Mestrado)]}
\preambulo{[Natureza do trabalho, programa de pós-graduação, departamento e instituição, e objetivo (por exemplo, requisito parcial para a obtenção do título de ...).]}

\definecolor{blue}{RGB}{41,5,195}
\makeatletter
\hypersetup{
	pdftitle={\@title}, pdfauthor={\@author}, pdfsubject={\imprimirpreambulo},
	pdfcreator={LaTeX with abnTeX2},
	pdfkeywords={[palavra-chave 1], [palavra-chave 2], [palavra-chave 3]},
	colorlinks=true, linkcolor=blue, citecolor=blue, filecolor=magenta, urlcolor=blue,
	bookmarksdepth=4
}
\setlength{\@fptop}{5pt}
\makeatother

% Quadros e lista de quadros
\newcommand{\quadroname}{Quadro}
\newcommand{\listofquadrosname}{Lista de quadros}
\newfloat[chapter]{quadro}{loq}{\quadroname}
\newlistof{listofquadros}{loq}{\listofquadrosname}
\newlistentry{quadro}{loq}{0}
\setfloatadjustment{quadro}{\centering}
\counterwithout{quadro}{chapter}
\renewcommand{\cftquadroname}{\quadroname\space}
\renewcommand*{\cftquadroaftersnum}{\hfill--\hfill}
\setfloatlocations{quadro}{hbtp}

% Espaçamentos e títulos
\setlength{\parindent}{1.3cm}
\setlength{\parskip}{0.2cm}
\renewcommand{\ABNTEXchapterfont}{\fontfamily{lmr}\fontseries{b}\selectfont}
\renewcommand{\ABNTEXsectionfont}{\fontfamily{lmr}\fontseries{b}\selectfont}
\renewcommand{\chaptitlefont}{\normalfont\large\bfseries}
\renewcommand{\cftsectionfont}{\bfseries}
\renewcommand{\cftsubsectionfont}{\normalfont}
\renewcommand{\cftsubsubsectionfont}{\normalfont}
\renewcommand{\cftparagraphfont}{\normalfont}
\renewcommand{\ABNTEXsectionfontsize}{\normalsize}
\renewcommand{\ABNTEXsubsectionfontsize}{\normalsize}
\renewcommand{\cftpartfont}{\cftchapterfont}
\renewcommand{\cftpartpagefont}{\cftchapterpagefont}
\renewcommand{\printparttitle}[1]{\parttitlefont\MakeTextUppercase{#1}}

\makeindex

\begin{document}
	\setlength{\abovedisplayskip}{5pt}
	\setlength{\abovedisplayshortskip}{0pt}
	\setlength{\belowdisplayskip}{12pt}
	\setlength{\belowdisplayshortskip}{12pt}
	\setlength{\afterchapskip}{18pt}

	\selectlanguage{brazil}
	\frenchspacing

	% --- Pré-textuais ---
	\include{pretextual/capa}
	\include{pretextual/folha_de_rosto}
	\include{pretextual/resumo}
	\include{pretextual/abstract}
	\include{pretextual/listas_pretextuais}

	% --- Textuais ---
	\textual
	\part{[Título da parte]}\label{part:um}
	\include{capitulos/capitulo_modelo}

	% --- Pós-textuais ---
	\postextual
	\bibliography{referencias}

	\begin{apendicesenv}
	\partapendices
	\include{apendices/apendice_modelo}
	\end{apendicesenv}

	\printindex
\end{document}
```

### config/comandos.tex

```latex
% !TeX root = ../tese_modelo.tex
\newcommand{\citar}{\textcolor{red}{[CITAR]}}
\newcommand{\preencher}[1]{\textcolor{red}{\textbf{[A PREENCHER:} \textit{#1}\textbf{]}}}
% \novo{}: texto recém-inserido (revisão). Aprovar = trocar \textcolor{red}{#1} por #1.
\newcommand{\novo}[1]{\textcolor{red}{#1}}
% \aref{ap:...} -> "Apêndice N" com hyperlink (corrige o \autoref em apêndices).
\newcommand{\aref}[1]{{\def\chapterautorefname{Apêndice}\autoref{#1}}}
% \verde{}: incorporações da revisão crítica. Aprovar = trocar por #1.
\newcommand{\verde}[1]{\textcolor{ForestGreen}{#1}}
% \magenta{}: citações sugeridas, a avaliar.
\newcommand{\magenta}[1]{\textcolor{magenta}{#1}}
```

### capitulos/capitulo_modelo.tex

```latex
% !TeX root = ../tese_modelo.tex
\chapter{[Título do Capítulo]}
\label{ch:modelo}

[Parágrafo de apresentação do capítulo: o que será tratado, por que importa para o trabalho e como o capítulo se organiza.] \cite{chave2024exemplo}.

\section{[Título da Seção]}
\label{sec:modelo_secao}

Texto corrido com valor numérico e unidade, como $1{,}5$~m de comprimento e $20$~kg de massa. Citação com autor na frase: conforme \citeonline{chave2024exemplo}. Referências cruzadas: \autoref{tab:modelo}, \autoref{fig:modelo}, \autoref{eq:modelo} e \aref{ap:modelo}.

\begin{equation}
	y = a\,x + b,
	\label{eq:modelo}
\end{equation}
onde $y$ é a variável dependente, $x$ a variável independente, $a$ o coeficiente angular e $b$ o coeficiente linear.

\begin{table}[h!]\centering
	\setlength{\tabcolsep}{8pt}
	\renewcommand{\arraystretch}{1.2}
	\caption{Legenda da tabela.}\label{tab:modelo}
	\small
	\begin{tabular}{M{2,4cm}M{4,2cm}M{2,4cm}}\toprule
		\textbf{Coluna A} & \textbf{Coluna B} & \textbf{Coluna C} \\
		\midrule
		$1{,}0$ & texto & $2{,}5$~kg \\
		\bottomrule
	\end{tabular}
	\legend{Fonte: elaborado pelo autor.}
\end{table}

\begin{figure}[h!]
	\centering
	\includegraphics[width=0.95\linewidth]{figuras/exemplo.pdf}
	\caption[Título curto]{Legenda completa da figura.}
	\label{fig:modelo}
	\legend{Fonte: elaborado pelo autor.}
\end{figure}

\begin{figure}[h!]
	\centering
	\begin{subfigure}[b]{0.49\linewidth}\centering
		\includegraphics[width=\linewidth]{figuras/exemplo_a.pdf}
		\caption{Painel A}\end{subfigure}\hfill
	\begin{subfigure}[b]{0.49\linewidth}\centering
		\includegraphics[width=\linewidth]{figuras/exemplo_b.pdf}
		\caption{Painel B}\end{subfigure}
	\caption{Legenda geral das subfiguras.}
	\label{fig:modelo_sub}
	\legend{Fonte: elaborado pelo autor.}
\end{figure}

\section{\novo{[Seção com Texto Novo]}}
\label{sec:modelo_novo}

\novo{Texto recém-inserido, em vermelho até a aprovação.} \preencher{informação pendente}
```

(Os arquivos `figuras/exemplo*.pdf` são placeholders: ao criar o modelo, gerar três PDFs vetoriais mínimos de teste, ou comentar os `\includegraphics`, para o modelo compilar sem erro.)

### apendices/apendice_modelo.tex

```latex
% !TeX root = ../tese_modelo.tex
\chapter{[Título do Apêndice]}
\label{ap:modelo}

[Texto do apêndice.] Listagens de código ou inputs:

\begin{lstlisting}
exemplo de listagem
\end{lstlisting}
```

### pretextual/capa.tex e folha_de_rosto.tex

```latex
% !TeX root = ../tese_modelo.tex
% capa.tex
\imprimircapa
```

```latex
% !TeX root = ../tese_modelo.tex
% folha_de_rosto.tex
\imprimirfolhaderosto*
\newpage
```

### pretextual/resumo.tex e abstract.tex

```latex
% !TeX root = ../tese_modelo.tex
% resumo.tex
\begin{resumo}
	[Texto do resumo em um único parágrafo, justificado.]

	\textbf{Palavras-chave}: [Palavra 1]. [Palavra 2]. [Palavra 3].
\end{resumo}
```

```latex
% !TeX root = ../tese_modelo.tex
% abstract.tex
\begin{resumo}[Abstract]
	\begin{otherlanguage*}{english}
		[Abstract text in a single paragraph.]

		\textbf{Keywords}: [Word 1]. [Word 2]. [Word 3].
	\end{otherlanguage*}
\end{resumo}
```

### pretextual/listas_pretextuais.tex

```latex
% !TeX root = ../tese_modelo.tex
\pdfbookmark[0]{\listfigurename}{lof}
\listoffigures*
\cleardoublepage

\pdfbookmark[0]{\listtablename}{lot}
\listoftables*
\cleardoublepage

% Siglas: ordem alfabética; inglês em itálico com tradução entre parênteses
\begin{siglas}
	\item[ABNT] Associação Brasileira de Normas Técnicas
	\item[{[SIGLA]}] \textit{[Forma por extenso em inglês]} ([tradução])
\end{siglas}

% Símbolos: latinos primeiro, depois gregos, cada grupo em ordem alfabética
\begin{simbolos}
	\item[$x$] [Descrição do símbolo latino]
	\item[$\alpha$] [Descrição do símbolo grego]
\end{simbolos}

\pdfbookmark[0]{\contentsname}{toc}
\tableofcontents*
\cleardoublepage
```

### referencias.bib

```bibtex
@article{chave2024exemplo,
  author  = {Sobrenome, Nome},
  title   = {Título do artigo},
  journal = {Revista},
  year    = {2024},
  volume  = {1},
  pages   = {1--10},
  doi     = {10.0000/exemplo}
}
```