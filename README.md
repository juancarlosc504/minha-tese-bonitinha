# Minha tese bonitinha

Skill do Claude e pasta LaTeX modelo com o padrão de formatação da tese de doutorado de Juan Carlos Lamônica (UFMG, PPG em Ciências e Técnicas Nucleares), baseado na classe abnTeX2. O repositório reúne duas coisas: o arquivo `SKILL.md`, que ensina o Claude a escrever e editar a tese nesse padrão e a criar a pasta modelo, e a pasta `modelo/`, um documento canônico, sem dados preenchidos, que pode ser usado diretamente no TeXstudio ou em outro editor, com ou sem o Claude.

## Requisito: distribuição TeX instalada

Para compilar o modelo, e qualquer capítulo escrito nesse padrão, é necessário ter uma distribuição TeX completa instalada no computador: **MacTeX** no macOS ou **MiKTeX** no Windows (no Linux, TeX Live). O modelo usa `latexmk`, `pdflatex` e `bibtex`, além de pacotes como `memoir`, `microtype`, `isotope`, `nomencl`, `imakeidx`, `caption`, `tocloft`, `booktabs`, `pdfpages` e `listings`. Sem a distribuição, o `.tex` não gera PDF; o Claude também depende dela para compilar e conferir o resultado.

A classe `abntex2` e os arquivos de idioma (`brazil.ldf`, `brazilian.ldf`, `portuges.ldf`) acompanham o modelo na própria pasta, copiados da pasta de trabalho da tese, de modo que não dependem da versão instalada no sistema.

### Ambiente de referência (computador de Juan)

Os dados abaixo foram extraídos do log de compilação da tese (`tese_juan.log`) e do próprio computador:

| Item | Valor |
|---|---|
| Sistema | macOS, MacBook Air (Apple Silicon, arm64) |
| Distribuição | MacTeX / TeX Live, instalada em `/usr/local/texlive` |
| Motor | pdfTeX 3.141592653-2.6-1.40.29 (TeX Live 2026) |
| Núcleo LaTeX | LaTeX2e 2026-06-01 |
| Classe base | `memoir` 3.8.4b, com `abntex2` v-1.9.7 local |
| Compilação | `latexmk` (pdflatex + bibtex), estilo `abntex2-alf` |
| Editor | TeXstudio, compilador padrão `txs:///latexmk`, corretor `pt_BR` |
| Controle de versão | Git, com o script `commit.sh` na pasta da tese |

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

Copiar a pasta `modelo/` para o local de trabalho e renomeá-la. Preencher os campos entre colchetes em `tese_modelo.tex` (título, autor, orientadores, instituição, programa, local, ano, palavras-chave), em `pretextual/resumo.tex` e `pretextual/abstract.tex`. Em seguida, compilar sempre o documento mestre:

```bash
cd modelo
latexmk -pdf tese_modelo.tex
```

No TeXstudio, abrir `tese_modelo.tex` e usar F5, com o compilador padrão `txs:///latexmk`. Cada arquivo de `capitulos/`, `pretextual/` e `apendices/` começa com `% !TeX root = ../tese_modelo.tex`, o que permite compilar a tese inteira a partir de qualquer capítulo. O capítulo `capitulo_modelo.tex` traz exemplos prontos de tabela, figura, subfiguras, equação, unidades, citações, referências cruzadas e marcadores de revisão. As figuras `exemplo*.pdf` são apenas ilustrativas e podem ser removidas.

## Como usar como skill do Claude

O arquivo `SKILL.md` na raiz é a skill. Para instalá-la, importar o arquivo na área de skills do Claude. Depois disso, o Claude aplica o padrão ao escrever ou editar `.tex` da tese e, quando solicitado, cria a pasta `Tese LaTeX - Modelo` na pasta conectada, com o mesmo conteúdo de `modelo/`.

## Estrutura do repositório

```
minha-tese-bonitinha/
├── README.md
├── SKILL.md                     skill do Claude (regras de formatação e modelo embutido)
└── modelo/                      pasta LaTeX canônica
    ├── tese_modelo.tex          documento mestre
    ├── referencias.bib          bibliografia única
    ├── config/comandos.tex      marcadores de revisão (\novo, \verde, \magenta, \preencher, \citar, \aref)
    ├── pretextual/              capa, folha de rosto, resumo, abstract, listas
    ├── capitulos/               capitulo_modelo.tex
    ├── apendices/               apendice_modelo.tex
    ├── figuras/  graficos/
    └── abntex2.cls, abntex2cite.sty, *.bst, *.ldf   arquivos de classe e idioma
```

## Observação sobre licenças

Os arquivos `abntex2*` e `*.bst` pertencem ao projeto abnTeX2 e seguem a licença LPPL de origem (<https://www.abntex.net.br>).
