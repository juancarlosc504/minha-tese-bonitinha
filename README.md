# Minha tese bonitinha

**Português** | [English](README.en.md)

Autoria de Juan Carlos Lamonica, MSc. Versão em inglês: [README.en.md](README.en.md) e [SKILL.en.md](SKILL.en.md) (repositório bilíngue: português e inglês).

Skill do Claude e pasta LaTeX modelo com um padrão de formatação para teses e dissertações. **A skill é feita com base no modelo canônico do abnTeX2** (<https://www.abntex.net.br>): parte da classe `abntex2` e das normas ABNT que ele implementa e acrescenta convenções próprias de formatação (tabelas, figuras, equações, citações, unidades, marcadores de revisão). O repositório reúne o arquivo `SKILL.md`, que ensina o Claude a escrever e editar documentos nesse padrão e a criar a pasta modelo, e a pasta `modelo/`, um documento canônico, sem dados preenchidos, que pode ser usado diretamente no TeXstudio ou em outro editor, com ou sem o Claude.

## Requisito: distribuição TeX instalada

Para compilar o modelo, e qualquer capítulo escrito nesse padrão, é necessário ter uma distribuição TeX completa instalada no computador: **MacTeX** no macOS ou **MiKTeX** no Windows (no Linux, TeX Live). O modelo usa `latexmk`, `pdflatex` e `bibtex`, além de pacotes como `memoir`, `microtype`, `isotope`, `nomencl`, `imakeidx`, `caption`, `tocloft`, `booktabs`, `pdfpages` e `listings`. Sem a distribuição, o `.tex` não gera PDF; o Claude também depende dela para compilar e conferir o resultado.

A classe `abntex2` e os arquivos de idioma (`brazil.ldf`, `brazilian.ldf`, `portuges.ldf`) acompanham o modelo na própria pasta, copiados de um projeto abnTeX2, de modo que não dependem da versão instalada no sistema.

### Ambiente de referência (computador do autor)

Os dados abaixo foram extraídos do log de uma compilação real e do próprio computador:

| Item | Valor |
|---|---|
| Sistema | macOS, MacBook Air (Apple Silicon, arm64) |
| Distribuição | MacTeX / TeX Live, instalada em `/usr/local/texlive` |
| Motor | pdfTeX 3.141592653-2.6-1.40.29 (TeX Live 2026) |
| Núcleo LaTeX | LaTeX2e 2026-06-01 |
| Classe base | `memoir` 3.8.4b, com `abntex2` v-1.9.7 local |
| Compilação | `latexmk` (pdflatex + bibtex), estilo `abntex2-alf` |
| Editor | TeXstudio, compilador padrão `txs:///latexmk`, corretor `pt_BR` |

### Instalação

No **macOS**, instalar o MacTeX completo (cerca de 5 GB), por download em <https://tug.org/mactex/> ou, com Homebrew, `brew install --cask mactex`. A alternativa leve é o BasicTeX (`brew install --cask basictex`), seguido dos pacotes do modelo:

```bash
sudo tlmgr update --self
sudo tlmgr install latexmk memoir microtype isotope nomencl imakeidx caption \
     tocloft booktabs pdfpages listings was enumitem multirow pgf lm \
     babel-portuges setspace relsize xcolor
```

A lista cobre os pacotes do modelo; se a compilação acusar `File 'xxx.sty' not found`, instalar o pacote correspondente com `sudo tlmgr install xxx`. A instalação completa dispensa esse passo.

No **Windows**, instalar o MiKTeX (<https://miktex.org/download>), que baixa os pacotes ausentes sob demanda na primeira compilação. O `latexmk` do MiKTeX exige também o Perl (por exemplo, Strawberry Perl).

No **Linux**, `sudo apt install texlive-full latexmk` (ou o equivalente da distribuição).

Para conferir a instalação, em um terminal:

```bash
pdflatex --version
latexmk -v
kpsewhich memoir.cls
```

## Como usar a pasta modelo

Copiar a pasta `modelo/` para o local de trabalho e renomeá-la. Preencher os campos entre colchetes em `tese_modelo.tex` (título, autor, orientadores, instituição, programa, local, ano, palavras-chave), e em `pretextual/resumo.tex` (resumo e abstract no mesmo arquivo). Em seguida, compilar sempre o documento mestre:

```bash
cd modelo
latexmk -pdf tese_modelo.tex
```

No TeXstudio, abrir `tese_modelo.tex` e usar F5, com o compilador padrão `txs:///latexmk`. Cada arquivo de `capitulos/`, `pretextual/` e `apendices/` começa com `% !TeX root = ../tese_modelo.tex`, o que permite compilar o documento inteiro a partir de qualquer capítulo. O capítulo `capitulo_modelo.tex` traz exemplos prontos de tabela, figura, subfiguras, equação, unidades, citações, referências cruzadas e marcadores de revisão. As figuras `exemplo*.pdf` são apenas ilustrativas e podem ser removidas.

## Como usar como skill do Claude

Passo a passo completo, para o Claude e para o Claude Code, em [INSTALL.md](INSTALL.md).

O arquivo `SKILL.md` na raiz é a skill. Para instalá-la, importar o arquivo na área de skills do Claude. Depois disso, o Claude aplica o padrão ao escrever ou editar `.tex` e, quando solicitado, cria a pasta `Tese LaTeX - Modelo` na pasta conectada, com o mesmo conteúdo de `modelo/`.

## Uso em conjunto com a skill de figuras científicas

Para as figuras do documento, esta skill pode ser usada junto com a **scientific-figures**: <https://github.com/juancarlosc504/scientific-figures>. Ela gera gráficos, mapas e diagramas em matplotlib no padrão de publicação (Times + STIX, fundo transparente, vírgula decimal, sem título embutido), sempre com um script `gera_*.py` que reproduz a figura e um verificador (`checa_figura.py`). A divisão de tarefas é simples: a **scientific-figures** produz o arquivo da figura (PDF ou PNG) em `figuras/`, e a **minha-tese-bonitinha** cuida de como a figura entra no LaTeX (ambiente `figure`, legenda, rótulo, fonte dentro do `\caption[curto]{completo}` e `\autoref`).

Instalação da skill de figuras no Claude Code:

```bash
claude plugin marketplace add juancarlosc504/scientific-figures
claude plugin install scientific-figures@scientific-figures
```

No claude.ai, baixar o arquivo `scientific-figures.skill` na página de Releases do repositório e enviá-lo em *Settings, Capabilities, Skills*.

## Estrutura do repositório

```
minha-tese-bonitinha/
├── README.md
├── INSTALL.md                   guia de instalação (Claude e Claude Code)
├── SKILL.md                     skill do Claude (regras de formatação e modelo embutido)
├── LICENSE                      MIT (material do autor)
├── NOTICE.md                    camadas de licença (MIT e LPPL do abnTeX2)
└── modelo/                      pasta LaTeX canônica
    ├── tese_modelo.tex          documento mestre
    ├── referencias.bib          bibliografia única
    ├── config/comandos.tex      marcadores de revisão (\novo, \verde, \magenta, \preencher, \citar, \aref)
    ├── pretextual/              capa, folha de rosto, dedicatória, agradecimentos, epígrafe, resumo e abstract, listas
    ├── capitulos/               capitulo_modelo.tex
    ├── apendices/               apendice_modelo.tex
    ├── figuras/  graficos/
    └── abntex2.cls, abntex2cite.sty, *.bst, *.ldf   arquivos de classe e idioma
```

## Licença

O material de autoria de Juan Carlos Lamonica, MSc, está sob a licença MIT (arquivo `LICENSE`). Os arquivos do abnTeX2 (`abntex2*`, `*.bst`, `*.ldf`, `portuges.sty`) continuam sob a licença LPPL de origem (<https://www.abntex.net.br>) e não são relicenciados. A divisão completa está em `NOTICE.md`.
