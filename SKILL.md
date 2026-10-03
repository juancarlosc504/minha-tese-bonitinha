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
├── pretextual/              ← capa, folha_de_rosto, dedicatoria, agradecimentos, epigrafe, resumo (português e inglês), listas_pretextuais
├── capitulos/capitulo_modelo.tex
├── apendices/apendice_modelo.tex
└── figuras/  graficos/      ← vazias (com .gitkeep)
```

Os arquivos de classe e estilo (`abntex2.cls`, `abntex2cite.sty`, `*.bst`, `brazil.ldf` etc.) não são recriados: se o TeX do computador não os tiver, copiá-los de um projeto abnTeX2 existente. O `abntex2-options.bib` precisa da entrada `nbr10520-2023` (ver "Citações e bibliografia"); sem ela a compilação falha no BibTeX. Depois de criar, compilar uma vez com `latexmk -pdf tese_modelo.tex` e informar só o resumo (páginas e erros); o modelo deve sair sem erros e sem referências indefinidas.

## Converter um .docx em projeto LaTeX

Quando o autor pedir para formatar em LaTeX uma tese escrita em Word, gerar um projeto no mesmo padrão desta skill, com documento mestre `tese.tex` (a linha mágica de cada arquivo aponta para ele), um arquivo por capítulo em `capitulos/` (`01_introducao.tex` etc.) e os pré-textuais em `pretextual/`. O preâmbulo, a capa, a folha de rosto e os demais pré-textuais são os do modelo desta skill (ver "Documento e preâmbulo"), sem depender de nenhuma tese na pasta. O caminho que funcionou: extrair o `.docx` com `pandoc` para JSON (AST) e a mídia com `--extract-media`; percorrer a AST com um script Python próprio (e não com `python-docx`), escrevendo cada bloco já nas convenções abaixo; e aplicar uma camada final de expressões regulares para os resíduos. Pontos que exigem atenção: caracteres Unicode sem suporte (o apóstrofo `U+2032` vira `$'$`); `%` dentro de strings de formatação do Python deve ser escrito `%%`; citações autor-ano do Word viram `\cite` ou `\citeonline` casando com as chaves do `.bib` gerado a partir da lista de referências (conferir `et al.` em itálico, `\&`, `Jr.` e divergências de ano); localizadores como "p. 12" não podem virar decimais; números e unidades passam ao formato `$n$~unidade`; cada figura é gravada em `figuras/` com nome `figNN_descricao` (TIFF convertido para JPG; PNG só quando necessário); e a numeração manual de figuras, tabelas e equações do Word é trocada por `\label` e `\autoref`. Os dados de identificação vêm da capa do documento original, e a lista de siglas, da tabela correspondente. Itens ambíguos (referência sem correspondência, figura sem legenda, citação não casada) não são resolvidos em silêncio: registrar num relatório curto e reportar ao autor. O original `.docx` nunca é alterado; trabalhar numa cópia.

## Antes de editar

O autor pode editar os arquivos por fora (por exemplo, no TeXstudio) entre sessões. Antes de alterar qualquer `.tex`, `.bib` ou figura, reler a versão atual da pasta conectada e conferir o horário de modificação; só então editar, e gravar de forma a não sobrescrever alterações dele. Acumular as edições e compilar uma única vez ao final, reportando apenas o resumo do log (número de páginas e de erros). Verificação visual do PDF só quando o layout for crítico ou o autor pedir.

## Documento e preâmbulo

O documento mestre (`tese_modelo.tex` no modelo; compilar sempre ele) usa a classe `abntex2` com 12pt, `oneside`, `openright`, `a4paper`, `chapter=TITLE` (capítulos em caixa alta) e `sumario=abnt-6027-2012`. Idioma principal `brazil`, com `\frenchspacing`. Fonte Latin Modern (`lmodern`, `T1`, `utf8`) e `microtype` para justificação. O texto é sempre justificado; recuo de parágrafo `\setlength{\parindent}{1.3cm}` e `\setlength{\parskip}{0.2cm}`. Títulos de seção e subseção em `\normalsize`, negrito, fonte `lmr`. Figuras e tabelas numeradas por capítulo (`\counterwithin`). Citações com `abntex2cite` (`alf`, `bibjustif`, `abnt-etal-text=it`, `abnt-cite-style=nbr10520-2023`). Não alterar o preâmbulo sem pedido explícito; novos comandos vão em `config/comandos.tex`.

Este preâmbulo, o `config/comandos.tex` e os pré-textuais do modelo já incorporam a formatação da tese do próprio autor, que foi usada apenas para acertar o padrão: o que ela ensinou sobre formatação está nesta skill, e é daqui que parte qualquer projeto novo ou convertido, sem precisar de tese alguma na pasta. Ficam de fora, por serem específicos de um trabalho, os pacotes `lipsum`, `svg`, `tabu` e `perpage`, o `\tikzstyle`, os blocos `comment` e as inclusões do tema. Ao gerar um projeto, trocar apenas os dados de identificação, o `\graphicspath` e a lista de capítulos. A tese do autor, com seus dados, texto e figuras, não entra no modelo nem em repositório público. Os trabalhos convertidos (por exemplo, a partir de um .docx) servem para validar a skill, não para acrescentar regras novas.

Tamanhos e caixa resultantes, conferidos no PDF da tese do autor: corpo em 12 pt; título de capítulo em 14,4 pt, negrito e caixa alta; seções e subseções em 12 pt, negrito e caixa de título (só a inicial maiúscula, como digitado); legendas em 12 pt; texto das tabelas em `\small` (10,9 pt) com cabeçalho em negrito; capa inteira em 12 pt (ver "pretextual/capa.tex"). Se um trabalho convertido sair com outros tamanhos, o desvio está na capa ou num comando local, não no preâmbulo. O nome da lista de figuras sai "Lista de ilustrações" por padrão do abnTeX2; quando o trabalho pedir "Lista de figuras", acrescentar em `config/comandos.tex` a linha `\addto\captionsbrazil{\renewcommand{\listfigurename}{Lista de figuras}}`. O `\graphicspath` pode listar subpastas de `figuras/` quando o trabalho as usar.

## Arquivos e estrutura

Cada arquivo de `capitulos/`, `pretextual/` e `apendices/` começa com a linha mágica `% !TeX root = ../tese_modelo.tex` (ou o nome do documento mestre do projeto). Capítulo: `\chapter{Título}` seguido de `\label{ch:...}` na linha seguinte; seção: `\section{...}` e `\label{sec:...}` na linha seguinte. Prefixos de rótulo: `ch:`, `sec:`, `subsec:`, `fig:`, `tab:`, `eq:`, `ap:`. Rótulos em minúsculas, sem acento, descritivos (`fig:fluxo`, `tab:parametros`, `eq:principal`). Cada capítulo abre com um parágrafo de apresentação antes da primeira seção. Texto corrido em prosa; listas só quando o conteúdo for realmente enumerável.

## Referências cruzadas

Sempre `\autoref{...}` (nunca "Figura 3" digitado à mão), inclusive para equações e seções. Para apêndices usar `\aref{ap:...}`, que corrige o prefixo "Capítulo" para "Apêndice". Rótulo de `\section` de apêndice já resulta em "Seção".

## Citações e bibliografia

As citações seguem a **NBR 10520:2023**: o sobrenome sai em caixa alta e baixa tanto entre parênteses quanto no texto — "(Silva, 2020)" e "Silva (2020)", nunca "(SILVA, 2020)". A lista de referências continua pela NBR 6023:2018, com sobrenome em maiúsculas. No abnTeX2 isso se liga com a opção `abnt-cite-style=nbr10520-2023` do `abntex2cite`, que lê uma entrada própria acrescentada ao `abntex2-options.bib` do modelo. A opção de fábrica `abnt-cite-style=AuthorYEAR` **não funciona**: o BibTeX não diferencia maiúsculas nas chaves e descarta essa entrada como repetida da `abnt-cite-style=AUTHORYEAR`, e as citações continuam em maiúsculas sem aviso. Ao montar um projeto, copiar o `abntex2-options.bib` do `modelo/` (com a entrada) e, fora dela, não mexer nesse arquivo. Se o `abntex2-options.bib` vier de outro lugar, acrescentar ao fim dele:

```bibtex
@ABNT-options{abnt-cite-style=nbr10520-2023,
 abnt-cite-style="(Author, YEAR)",
 key="aaaa"}
```

Variações de citação do `abntex2cite`:

| Comando | Sai como | Uso |
|---|---|---|
| `\cite{chave}` | (Silva, 2020) | citação entre parênteses |
| `\cite[p.~12]{chave}` | (Silva, 2020, p. 12) | citação direta, com página |
| `\citeonline{chave}` | Silva (2020) | autor como sujeito da frase |
| `\citeauthoronline{chave}` | Silva | nome no fluxo do texto, com o ano na mesma frase |
| `\citeyear{chave}` | 2020 | só o ano |
| `\apud{a}{b}` / `\apudonline{a}{b}` | (Silva, 2000 apud Souza, 2020) / Silva (2000 apud Souza, 2020) | citação de citação |

Nunca digitar o nome do autor antes de um `\cite` ("Ogawa \cite{...}", "Devic et al. (2016)"): usar `\citeonline` ou `\citeauthoronline` + `\citeyear`. A ABNT pede o ano junto ao autor; o nome sozinho só quando o ano aparece na mesma frase.

Bibliografia única em `referencias.bib`, carregada por `\bibliography{referencias}`; novas referências entram sempre nesse arquivo, nunca em outro `.bib`. No campo `title` usar sempre chaves duplas (`title = {{Título do Artigo}}`), para manter a caixa alta e baixa como digitada; os modelos de todos os tipos de entrada estão na seção "Arquivos do modelo", em `referencias.bib`. Sobrenome com sufixo (Jr., Filho, Neto) entra todo entre chaves, como `{Sobrenome Jr.}, Nome`, para o estilo não tratar o sufixo como sobrenome e a citação sair correta.

## Números, unidades e símbolos

Vírgula decimal em modo matemático e espaço não separável antes da unidade: `$0{,}5$~cm`, `$5$~mm`, `$20$~kg`, `$2{,}5$~m`. Nunca ponto decimal, nunca número e unidade colados ou separados por espaço comum. Isótopos como `$^{14}$C`; graus como `$0^\circ$` (o `\degree` vem do `gensymb`). Percentuais como `$100\%$`. Variáveis e símbolos sempre em modo matemático e em itálico (`$x$`, `$y$`, `$\theta$`). Estrangeirismos e termos latinos em itálico (`\textit{software}`, `\textit{in loco}`). Aspas LaTeX (``assim''), nunca aspas retas. Travessão de aposto com `---` entre espaços.

## Equações

Ambiente `equation` com `\label{eq:...}` na linha seguinte, pontuação final dentro da equação (vírgula se a frase continua com "onde", ponto se encerra), indentação por tabulação, `\dfrac` em frações isoladas e `cases` com `\\[8pt]` para casos. Derivada temporal com `\dot{x}`. Definir cada símbolo logo após a equação, em prosa. Espaçamentos verticais já definidos no preâmbulo (`\abovedisplayskip` 5pt, `\belowdisplayskip` 12pt); não sobrescrever.

## Tabelas (padrão obrigatório)

Ambiente `table[h!]` centralizado, `\setlength{\tabcolsep}{8pt}`, `\renewcommand{\arraystretch}{1.2}`, `\small`, colunas centralizadas de largura fixa `M{<largura>}` (comando do preâmbulo; a largura é arbitrada caso a caso conforme o conteúdo, não existe largura padrão). Réguas apenas `booktabs` (`\toprule`, `\midrule`, `\bottomrule`), nunca `\hline` nem barras verticais. Cabeçalho em `\textbf`; quebra de linha em cabeçalho com `\shortstack[c]{linha1 \\ linha2}`. Legenda acima, sempre no formato `\caption[título curto]{legenda completa + fonte}` com `\label{tab:...}` (ver "Legendas de figuras e tabelas"); não usar `\legend{}`.

```latex
\begin{table}[h!]\centering
	\setlength{\tabcolsep}{8pt}
	\renewcommand{\arraystretch}{1.2}
	\caption[Título curto da tabela]{Legenda completa da tabela. Elaborado pelo autor.}\label{tab:rotulo}
	\small
	\begin{tabular}{M{1,8cm}M{4,2cm}M{2,4cm}}\toprule
		\textbf{Coluna A} & \textbf{Coluna B} & \textbf{Coluna C} \\
		\midrule
		... & ... & ... \\
		\bottomrule
	\end{tabular}
\end{table}
```

## Figuras

Ambiente `figure[h!]` com `\centering`, `\includegraphics[width=0.95\linewidth]{figuras/arquivo.pdf}` (o `\graphicspath` cobre `figuras/` e `graficos/`), `\caption[título curto]{legenda completa + fonte}` e `\label{fig:...}` (ver "Legendas de figuras e tabelas"); não usar `\legend{}`. Preferir PDF vetorial; PNG só para imagens rasterizadas. Figuras mais altas que largas (proporção altura/largura acima de cerca de 1,15) levam `\includegraphics[height=0.68\textheight,keepaspectratio]{...}` em vez de `width`, para não estourar a página e deixar espaço à legenda. Subfiguras: `subfigure[b]{0.49\linewidth}` com `\centering`, `\hfill` entre as duas colunas e `\\[1.5ex]` entre linhas, cada uma com sua `\caption{}` curta (sem título curto: subfigura não entra na lista), e a legenda geral ao final, no mesmo formato `\caption[curto]{completo + fonte}`.

## Legendas de figuras e tabelas (padrão obrigatório)

Toda figura e toda tabela usa `\caption[título curto]{legenda completa}`:

- `[título curto]` sempre presente: é o que vai para a Lista de ilustrações / Lista de tabelas. Só o título, sem ponto final, sem citação e sem marcador de revisão (`\novo` etc.).
- `{legenda completa}`: o título por extenso, a explicação necessária e, **no fim, a fonte**, dentro da própria legenda. Não usar `\legend{Fonte: ...}`.
- Formas da fonte: figura ou tabela própria, `Elaborado pelo autor.`; refeita a partir de dados de outro trabalho, `Elaborado pelo autor, com base em \citeonline{chave}.`; reproduzida ou adaptada, `Adaptado de \citeonline{chave}.`. A citação é sempre `\citeonline`/`\cite`, nunca nome digitado.
- Notas que antes iam no `\legend` (siglas, meio, observações) entram na legenda completa, depois da fonte.
- Com o título curto presente, a legenda completa pode ter `\autoref`, `\cite` e `\novo{}` sem quebrar a lista.

```latex
\caption[Arquitetura dos filmes radiocrômicos EBT2 e EBT3]{Desenho esquemático da diferença entre a arquitetura dos filmes radiocrômicos EBT2 e EBT3. Adaptado de \citeonline{devic2016reference}.}
```

Observação de norma: para tabelas, a NBR 14724 remete às Normas de Apresentação Tabular do IBGE, que põem a fonte no rodapé. Este padrão leva a fonte para o título por decisão do autor; avisar se a banca ou a biblioteca do programa exigir o rodapé.

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

`latexmk -pdf <documento mestre>.tex`. Erro `Undefined control sequence` com `\tempf@rtoc` indica `.toc` ou `.aux` corrompido, não erro de texto. Em pasta conectada que bloqueia exclusão, truncar apenas `.toc` e `.out` (`: > arquivo.toc`), nunca o `.aux`; se o bibtex acusar "no \citation commands", rodar `pdflatex`, `bibtex`, `pdflatex`, `pdflatex`. Em ambiente sem `abntex2.cls`, `lmodern` ou o pacote de português do babel (como o contêiner da nuvem), instalar os dois últimos pelo gerenciador de pacotes e clonar o repositório `github.com/abntex/abntex2` para o texmf local; compilar numa cópia fora da pasta do autor e só então gravar o PDF nela.

## Conferência final

Texto justificado e sem estouro de margem; unidades no formato `$n$~unidade` com vírgula decimal; todas as referências cruzadas via `\autoref`/`\aref`; tabelas sem `\hline`; figuras e tabelas com `\caption[curto]{completo + fonte}` e nenhum `\legend`; figuras altas com limite de altura; citações em caixa alta e baixa (NBR 10520:2023); citações em `referencias.bib` e com `\cite` ou `\citeonline` adequados; títulos do `.bib` com chaves duplas; capa em 12 pt com caixa alta; palavras-chave separadas por ponto e vírgula; texto novo em `\novo{}`; compilação sem erros e sem referências ou citações indefinidas.

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
\usepackage[alf,bibjustif,abnt-etal-text=it,abnt-etal-list=2,abnt-etal-cite=2,abnt-cite-style=nbr10520-2023]{abntex2cite}
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
% A instituição é digitada em caixa alta (a capa não a converte).
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
	% \include{pretextual/dedicatoria}   % opcional
	\include{pretextual/agradecimentos}
	\include{pretextual/epigrafe}
	\include{pretextual/resumo}   % resumo (português) e abstract (inglês)
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
% Opcional: "Lista de figuras" em vez de "Lista de ilustrações".
% \addto\captionsbrazil{\renewcommand{\listfigurename}{Lista de figuras}}
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
	\caption[Título curto da tabela]{Legenda completa da tabela. Elaborado pelo autor.}\label{tab:modelo}
	\small
	\begin{tabular}{M{2,4cm}M{4,2cm}M{2,4cm}}\toprule
		\textbf{Coluna A} & \textbf{Coluna B} & \textbf{Coluna C} \\
		\midrule
		$1{,}0$ & texto & $2{,}5$~kg \\
		\bottomrule
	\end{tabular}
\end{table}

\begin{figure}[h!]
	\centering
	\includegraphics[width=0.95\linewidth]{figuras/exemplo.pdf}
	\caption[Título curto da figura]{Legenda completa da figura. Elaborado pelo autor.}
	\label{fig:modelo}
\end{figure}

\begin{figure}[h!]
	\centering
	\begin{subfigure}[b]{0.49\linewidth}\centering
		\includegraphics[width=\linewidth]{figuras/exemplo_a.pdf}
		\caption{Painel A}\end{subfigure}\hfill
	\begin{subfigure}[b]{0.49\linewidth}\centering
		\includegraphics[width=\linewidth]{figuras/exemplo_b.pdf}
		\caption{Painel B}\end{subfigure}
	\caption[Título curto das subfiguras]{Legenda geral das subfiguras. Elaborado pelo autor.}
	\label{fig:modelo_sub}
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

A capa é própria (não usa `\imprimircapa` nem o ambiente `capa`): blocos centralizados em 12 pt, com espaços verticais fixos. Instituição (digitada em caixa alta em `\instituicao`, uma unidade por linha), autor em caixa alta, título em negrito e caixa alta, e local e ano na base, com o local em caixa alta. Sem logotipo, salvo pedido do autor. A folha de rosto segue o mesmo estilo: autor em caixa alta, título em negrito e caixa alta, preâmbulo alinhado à direita numa caixa de $7$~cm, orientador e coorientador centralizados, local e ano na base. Ficha catalográfica e folha de aprovação entram como PDF fornecido pela biblioteca e pela banca, quando existirem.

```latex
% !TeX root = ../tese_modelo.tex
% capa.tex
\begin{center}
	\singlespacing
	\imprimirinstituicao
\end{center}

\vspace{3.0cm}

\begin{center}
	\MakeTextUppercase{\imprimirautor}

	\vspace{4.5cm}

	\textbf{\MakeTextUppercase{\imprimirtitulo}}
\end{center}

\vfill
\begin{center}
	\singlespacing
	\MakeTextUppercase{\imprimirlocal} \\ \imprimirdata
\end{center}
```

```latex
% !TeX root = ../tese_modelo.tex
% folha_de_rosto.tex
\begin{center}
	\MakeTextUppercase{\imprimirautor}

	\vspace{4.0cm}

	\textbf{\MakeTextUppercase{\imprimirtitulo}}
\end{center}

\vspace{4.0cm}

\begin{minipage}[l]{15cm}
	\hfill \parbox[c]{7cm}{
		\singlespacing
		\imprimirpreambulo}
\end{minipage}

\vspace{2.5cm}

\begin{center}
	\imprimirorientadorRotulo \\ \imprimirorientador

	\vspace{0.8cm}
	\imprimircoorientadorRotulo \\ \imprimircoorientador
\end{center}

\vfill
\begin{center}
	\singlespacing
	\imprimirlocal \\ \imprimirdata
\end{center}
\newpage
```

### pretextual/dedicatoria.tex, agradecimentos.tex e epigrafe.tex

A dedicatória é opcional (linha comentada no documento mestre) e fica na parte inferior direita da página.

```latex
% !TeX root = ../tese_modelo.tex
% dedicatoria.tex
\begin{dedicatoria}
	\vspace*{\fill}
	\begin{flushright}
		[Texto da dedicatória.]
	\end{flushright}
\end{dedicatoria}
```

```latex
% !TeX root = ../tese_modelo.tex
% agradecimentos.tex
\begin{agradecimentos}
	\preencher{texto dos agradecimentos}
\end{agradecimentos}
```

```latex
% !TeX root = ../tese_modelo.tex
% epigrafe.tex
\begin{epigrafe}
	\vspace*{\fill}
	\begin{flushright}
		\textit{``[Texto da epígrafe.]''}

		[Autor da epígrafe]
	\end{flushright}
\end{epigrafe}
```

### pretextual/resumo.tex

Resumo e abstract ficam no mesmo arquivo, nesta ordem, com espaçamento de $18$~pt entre parágrafos (`\absparsep`). Palavras-chave separadas por ponto e vírgula, cada uma com inicial maiúscula, e ponto final. Em `pdfkeywords` do `\hypersetup` as palavras vão separadas por vírgula.

```latex
% !TeX root = ../tese_modelo.tex
% Resumo (português) e Abstract (inglês)

\setlength{\absparsep}{18pt}
\begin{resumo}

\noindent [Texto do resumo em um único parágrafo, justificado.]

\vspace{\onelineskip}

\noindent
\textbf{Palavras-chave}: [Palavra 1]; [Palavra 2]; [Palavra 3].
\end{resumo}

\begin{resumo}[Abstract]
\begin{otherlanguage*}{english}

	\noindent [Abstract text in a single paragraph.]

	\vspace{\onelineskip}

	\noindent
	\textbf{Keywords}: [Word 1]; [Word 2]; [Word 3].
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

Cada tipo de entrada abaixo tem um modelo no arquivo. Regras que valem para todos: **o título leva chaves duplas** (`title = {{Título do Artigo}}`), para o estilo manter a caixa alta e baixa exatamente como digitada; sem elas, siglas, nomes próprios e fórmulas (TG-43, EBT3, Monte Carlo) são convertidos para minúsculas. Para proteger só um termo, usar chaves simples em volta dele (`{EBT3}`). Autor pessoa em `Sobrenome, Nome and Sobrenome, Nome`; sobrenome com sufixo (Jr., Filho, Neto) entre chaves (`{Sobrenome Jr.}, Nome`); autor institucional entre chaves duplas e em caixa alta (`author = {{ASSOCIAÇÃO BRASILEIRA DE NORMAS TÉCNICAS}}`). Páginas com `--`. Documentos online com `url` e `urlaccessdate`. Única exceção: em `@proceedings` o título leva chaves simples, porque as duplas desbalanceiam o campo e geram erro. Em `@phdthesis` e `@mastersthesis`, `type` traz só a área (`Doutorado em Área`), pois o estilo já escreve "Tese" ou "Dissertação". Tipos cobertos: `article`, `book`, `inbook`, `incollection`, `inproceedings`, `proceedings`, `phdthesis`, `mastersthesis`, `techreport`, `manual`, `misc` (norma técnica e página de internet), `patent` e `unpublished`.

```bibtex
% ===================================================================
% referencias.bib --- modelos de entrada para o abntex2cite (estilo abntex2-alf)
%
% REGRAS GERAIS
%  1. Chave de citação: sobrenome do primeiro autor + ano + palavra
%     (minúsculas, sem acento, sem espaço), por exemplo silva2024exemplo.
%  2. TÍTULO COM CHAVES DUPLAS: title = {{Título do trabalho}}.
%     As chaves externas delimitam o campo; as internas protegem o título
%     e mantêm a caixa (alta ou baixa) exatamente como digitada. Sem elas,
%     o estilo converte parte do título para minúsculas e siglas, nomes
%     próprios e fórmulas (por exemplo TG-43, Monte Carlo, EBT3) perdem a
%     grafia. Para proteger só um termo dentro do título, usar chaves
%     simples em volta dele: title = {Estudo com {EBT3} e {Monte Carlo}}.
%  3. Autores: Sobrenome, Nome and Sobrenome, Nome (separados por "and").
%     Sobrenome com sufixo: author = {{Sobrenome Jr.}, Nome}.
%     Instituição como autor: author = {{ASSOCIAÇÃO BRASILEIRA DE NORMAS
%     TÉCNICAS}} (chaves duplas, para não ser lida como nome de pessoa, e
%     em caixa alta, como pede a ABNT para entidades).
%  4. Páginas com travessão duplo: pages = {1--10}.
%  5. Sites e documentos online: url + urlaccessdate (data de acesso).
%  6. Toda referência nova entra neste arquivo, nunca em outro .bib.
% ===================================================================

% ------------------------------------------------------------------
% Artigo de periódico
% ------------------------------------------------------------------
@article{chave2024exemplo,
  author  = {Sobrenome, Nome and Sobrenome, Nome},
  title   = {{Título do Artigo com a Caixa Alta e Baixa Mantida}},
  journal = {Nome da Revista},
  address = {Cidade},
  year    = {2024},
  volume  = {10},
  number  = {2},
  pages   = {1--10},
  doi     = {10.0000/exemplo}
}

% ------------------------------------------------------------------
% Livro (obra inteira)
% ------------------------------------------------------------------
@book{sobrenome2020livro,
  author    = {Sobrenome, Nome},
  title     = {{Título do Livro}},
  edition   = {2},
  address   = {Cidade},
  publisher = {Editora},
  year      = {2020}
}

% ------------------------------------------------------------------
% Parte de livro com autoria própria (capítulo do mesmo autor)
% ------------------------------------------------------------------
@inbook{sobrenome2020capitulo,
  author    = {Sobrenome, Nome},
  title     = {{Título do Livro}},
  chapter   = {3},
  pages     = {45--80},
  address   = {Cidade},
  publisher = {Editora},
  year      = {2020}
}

% ------------------------------------------------------------------
% Capítulo em livro organizado por outros (coletânea)
% ------------------------------------------------------------------
@incollection{sobrenome2019coletanea,
  author    = {Sobrenome, Nome},
  title     = {{Título do Capítulo}},
  booktitle = {{Título do Livro Organizado}},
  editor    = {Sobrenome, Nome},
  pages     = {101--120},
  address   = {Cidade},
  publisher = {Editora},
  year      = {2019}
}

% ------------------------------------------------------------------
% Trabalho publicado em anais de evento
% ------------------------------------------------------------------
@inproceedings{sobrenome2022anais,
  author    = {Sobrenome, Nome and Sobrenome, Nome},
  title     = {{Título do Trabalho Apresentado}},
  booktitle = {{Anais do Nome do Evento}},
  address   = {Cidade},
  year      = {2022},
  pages     = {1--8}
}

% ------------------------------------------------------------------
% Anais completos (evento como um todo). EXCEÇÃO: aqui o título leva
% chaves simples, porque o estilo abntex2-alf trunca o título deste tipo
% e as chaves duplas deixam o campo desbalanceado (erro de compilação).
% ------------------------------------------------------------------
@proceedings{evento2022anaiscompletos,
  title     = {Anais do Nome do Evento},
  address   = {Cidade},
  publisher = {Organizador},
  year      = {2022}
}

% ------------------------------------------------------------------
% Tese de doutorado
% ------------------------------------------------------------------
@phdthesis{sobrenome2023tese,
  author  = {Sobrenome, Nome},
  title   = {{Título da Tese}},
  school  = {Universidade, Unidade, Departamento},
  address = {Cidade},
  year    = {2023},
  type    = {Doutorado em Área}
}

% ------------------------------------------------------------------
% Dissertação de mestrado
% ------------------------------------------------------------------
@mastersthesis{sobrenome2021dissertacao,
  author  = {Sobrenome, Nome},
  title   = {{Título da Dissertação}},
  school  = {Universidade, Unidade, Departamento},
  address = {Cidade},
  year    = {2021},
  type    = {Mestrado em Área}
}

% ------------------------------------------------------------------
% Relatório técnico (inclui relatórios de organismos internacionais)
% ------------------------------------------------------------------
@techreport{organizacao2018relatorio,
  author      = {{NOME DA ORGANIZAÇÃO}},
  title       = {{Título do Relatório}},
  institution = {Nome da Organização},
  address     = {Cidade},
  year        = {2018},
  number      = {123},
  type        = {Relatório técnico}
}

% ------------------------------------------------------------------
% Manual, protocolo ou recomendação de organização
% ------------------------------------------------------------------
@manual{organizacao2017manual,
  author       = {{NOME DA ORGANIZAÇÃO}},
  title        = {{Título do Manual ou Protocolo}},
  organization = {Nome da Organização},
  address      = {Cidade},
  year         = {2017},
  edition      = {2},
  note         = {Relatório n. 00}
}

% ------------------------------------------------------------------
% Norma técnica (ABNT, IEC, ISO)
% ------------------------------------------------------------------
@misc{abnt2018norma,
  author  = {{ASSOCIAÇÃO BRASILEIRA DE NORMAS TÉCNICAS}},
  title   = {{NBR 0000: Título da Norma}},
  address = {Rio de Janeiro},
  year    = {2018}
}

% ------------------------------------------------------------------
% Página de internet, documento online ou banco de dados
% ------------------------------------------------------------------
@misc{organizacao2025site,
  author        = {{NOME DA ORGANIZAÇÃO}},
  title         = {{Título da Página}},
  year          = {2025},
  url           = {https://www.exemplo.org/pagina},
  urlaccessdate = {2 out. 2026}
}

% ------------------------------------------------------------------
% Artigo de periódico em meio eletrônico (com DOI e acesso online)
% ------------------------------------------------------------------
@article{sobrenome2025online,
  author        = {Sobrenome, Nome},
  title         = {{Título do Artigo Disponível Online}},
  journal       = {Nome da Revista},
  address       = {Cidade},
  year          = {2025},
  volume        = {5},
  number        = {1},
  pages         = {1--12},
  doi           = {10.0000/exemplo.online},
  url           = {https://doi.org/10.0000/exemplo.online},
  urlaccessdate = {2 out. 2026}
}

% ------------------------------------------------------------------
% Patente (o inventor sai na ordem direta: Nome Sobrenome)
% ------------------------------------------------------------------
@patent{sobrenome2016patente,
  author = {Nome Sobrenome},
  title  = {{Título da Patente}},
  year   = {2016},
  number = {BR 00 0000000-0},
  note   = {Depositante: Nome. Data de depósito: 1 jan. 2016}
}

% ------------------------------------------------------------------
% Trabalho não publicado (comunicação pessoal, manuscrito em preparação)
% ------------------------------------------------------------------
@unpublished{sobrenome2026inedito,
  author = {Sobrenome, Nome},
  title  = {{Título do Trabalho}},
  year   = {2026},
  note   = {Manuscrito em preparação}
}
```