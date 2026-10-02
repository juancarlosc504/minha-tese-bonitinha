# Guia de instalação da skill

**Português** | [English](INSTALL.en.md)

Autoria de Juan Carlos Lamonica, MSc. Este guia mostra como instalar a skill `minha-tese-bonitinha` no Claude (claude.ai e aplicativo desktop) e no Claude Code. A skill é o arquivo `SKILL.md` da raiz do repositório; a versão em inglês é `SKILL.en.md`.

Antes de tudo, tenha uma distribuição TeX instalada (MacTeX, MiKTeX ou TeX Live), conforme o `README.md`. A skill orienta a escrita e a formatação, mas quem compila é o TeX do seu computador.

## 1. Obter os arquivos

```bash
git clone https://github.com/juancarlosc504/minha-tese-bonitinha.git
cd minha-tese-bonitinha
```

## 2. Instalar no Claude (claude.ai e aplicativo desktop)

A skill é enviada como um arquivo `.zip` que contém uma pasta com o `SKILL.md` dentro.

1. Montar a pasta da skill, só com os arquivos necessários:

```bash
mkdir -p /tmp/minha-tese-bonitinha
cp SKILL.md /tmp/minha-tese-bonitinha/SKILL.md
cd /tmp && zip -r minha-tese-bonitinha.zip minha-tese-bonitinha
```

2. No Claude, abrir *Settings* (Configurações), *Capabilities* (Recursos), seção *Skills*, e enviar o arquivo `minha-tese-bonitinha.zip`.
3. Ativar a skill na lista. Ela passa a ser usada sempre que a conversa envolver escrever ou editar `.tex` nesse padrão, ou quando você pedir a pasta modelo.

Para a versão em inglês, use `SKILL.en.md` copiado como `SKILL.md` dentro da pasta (nome da skill: `my-pretty-thesis`). Não instale as duas versões ao mesmo tempo, para não duplicar o gatilho.

Outra opção, sem arquivo: em uma conversa do Claude, peça "crie a skill minha-tese-bonitinha a partir deste SKILL.md" e anexe o arquivo; o Claude mostra um cartão de revisão e a skill é salva ao confirmar.

## 3. Instalar no Claude Code

O Claude Code carrega skills de pastas que contêm um `SKILL.md`. Há duas formas.

**Para todos os seus projetos (instalação pessoal):**

```bash
mkdir -p ~/.claude/skills
git clone https://github.com/juancarlosc504/minha-tese-bonitinha.git ~/.claude/skills/minha-tese-bonitinha
```

Para atualizar depois: `git -C ~/.claude/skills/minha-tese-bonitinha pull`.

**Só para o projeto da tese:**

```bash
cd /caminho/da/sua/tese
mkdir -p .claude/skills/minha-tese-bonitinha
cp /caminho/do/repositorio/SKILL.md .claude/skills/minha-tese-bonitinha/SKILL.md
```

Nesse caso, a pasta `.claude/skills/` pode ser versionada junto com a tese. Abra o Claude Code na pasta da tese (`claude`); a skill é carregada ao iniciar a sessão. Reinicie a sessão se a skill foi instalada com o Claude Code já aberto.

## 4. Usar pelo Claude Code

Abrir um terminal na pasta onde a tese vai ficar e iniciar o Claude Code:

```bash
cd ~/Documents/MinhaTese
claude
```

Exemplos de pedidos:

- "Crie a pasta LaTeX modelo da minha-tese-bonitinha." O Claude cria `Tese LaTeX - Modelo/` com todos os arquivos, compila uma vez e informa só o resumo (páginas e erros).
- "Escreva o capítulo 2 em `capitulos/` no padrão da skill, com o texto novo em `\novo{}`."
- "Adicione esta referência ao `referencias.bib`", colando o DOI ou os dados. O Claude usa o modelo de entrada correto e título com chaves duplas.
- "Revise as tabelas do capítulo 3 no padrão da skill."

Se preferir chamar a skill de forma explícita, digite `/minha-tese-bonitinha` no início do pedido.

No Claude Code, o Claude trabalha direto nos arquivos do seu computador e roda `latexmk`, de modo que não é necessário nenhum passo extra de cópia de arquivos.

## 5. Usar junto com a skill de figuras

Para as figuras, instale também a `scientific-figures` (<https://github.com/juancarlosc504/scientific-figures>):

```bash
claude plugin marketplace add juancarlosc504/scientific-figures
claude plugin install scientific-figures@scientific-figures
```

A `scientific-figures` gera a figura em matplotlib com um script reproduzível; a `minha-tese-bonitinha` cuida de como ela entra no LaTeX.

## 6. Conferir se funcionou

Na conversa, peça: "Quais regras de tabela você vai seguir para esta tese?". A resposta deve citar réguas `booktabs`, colunas `M{...}` centralizadas e `\legend{Fonte: ...}`. Se não citar, a skill não foi carregada: confira o nome da pasta, a presença do `SKILL.md` dentro dela e reinicie a sessão.

## 7. Enviar alterações ao GitHub (git push)

Para enviar alterações ao seu próprio repositório (um *fork* do original ou uma cópia sua), grave e envie as mudanças. O repositório original pertence a juancarlosc504; para propor melhorias a ele, abra um *pull request* a partir do seu fork, e, para guardar a sua versão, troque o endereço do `git clone` pelo do seu fork:

```bash
cd ~/.claude/skills/minha-tese-bonitinha   # a pasta do Claude Code, onde o repositório foi clonado na seção 3
git add -A
git commit -m "Descreva a alteração"
git push origin main
```

Se o Git pedir login, use o seu usuário do GitHub e, no lugar da senha, um token de acesso pessoal do GitHub (em *Settings, Developer settings, Personal access tokens*). Outra opção é rodar `gh auth login` uma vez, se o `gh` estiver instalado; depois disso o `git push` funciona sem pedir credenciais. Se você clonou o repositório em outra pasta, rode os mesmos comandos a partir dela.
