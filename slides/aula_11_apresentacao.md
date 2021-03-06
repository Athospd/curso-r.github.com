# Aula 10 - Pacotes e +
Curso de R: Do casual ao avançado  
2015-02-11  

# Criação de pacotes

## Criação de pacotes

Baseado no livro [r-pkgs](http://r-pkgs.had.co.nz/description.html) do Hadley

## Vantagens {.build}

- Economia de tempo para tarefas futuras
- Forma de organização pré-estabelecida
- Contribuir e aproveitar contribuições da comunidade

## Filosofia {.build}

- Tudo que pode ser automatizado, deve ser automatizado
- Utilização do pacote `devtools` como base para criação de pacotes
- Trabalhar menos com os detalhes (estrutura, etc.) e mais com funcionalidades (funções úteis, etc).
- Se for necessário trabalhar com coisas mais complexas, ler [Writing R extensions](cran.r-project.org/doc/manuals/R-exts.html#Creating-R-packages)

## Pré-requisitos

- Pacotes `devtools`, `roxygen2`, `testthat`, `knitr`
- **R** e **RStudio** atualizados (recomenda-se preview version do RStudio)
- Instalar versão `dev` do `devtools`


```r
devtools::install_github('hadley/devtools')
```

## Pré-requisitos

- No Windows, instalar o [Rtools](cran.r-project.org/bin/windows/Rtools)
- No Mac, instalar o [XCode](developer.apple.com/downloads)
- No linux, instalar o pacote de desenvolvimento `r-base-dev`. No Ubuntu, basta digitar


```r
sudo apt-get install r-base-dev
```

- Verifique se está tudo certo digitando `devtools::has_devel()`.

## Exemplo

- Crie um projeto pelo RStudio e selecione "R project".

## Estrutura {.build}

Essa é a estrutura mínima para criar um pacote.

- Tudo dentro de uma pasta
- `DESCRIPTION`: Metadados do pacote.
- `NAMESPACE`: Importante para jogar pacote no CRAN.
- `R/`: Pasta onde fica o código R
- `man/`: Pasta onde fica a documentação
- `xxx.Rproj`: Seu projeto (não é necessário).

## Tipos / estados dos pacotes

- Source (código fonte)
- Bundled (`.tar.gz`)
- Binary (binário, compactado)
- Installed (binário, descompactado numa pasta)
- In memory (depois de dar `library()` ou `require()`)

## Código R {.build}

- Todo o código em `R` fica aqui
- Tudo é baseado em funções. Crie objetos, principalmente funções, e não use coisas como `View()`
- Melhor _workflow_: Editar R -> Ctrl+Shift+L -> Teste no console -> Editar R -> ...
- Organizando funções: dividir arquivos por temas, e manter um padrão de títulos e conteúdos
- Não use `library()`, `require()` nem `source()`, `setwd()`, etc. Ao invés disso, coloque dependências na documentação.

## Documentação

### Arquivo `DESCRIPTION`

- Definir `Imports`, `Suggests`, e usar o `::`.
- `devtools::use_package()`
- Versões `(>= 0.3)`, `devtools::numeric_version()`
- `Depends` (versões de R).
- `Authors@R`
- [Licensas](https://choosealicense.com)

## Documentação

### Documentação dos objetos

- Ensina o usuário a usar o pacote
- Facilmente construído, colocando headers nas funções do R e usando `devtools::document()`
- Começar com `#'`
- _workflow_: Adicionar documentação em `roxygen` -> chamar `devtools::document()` -> visualizar documentação com `?` -> Adicionar documentação em `roxygen` -> ...
- Tags com `@tag` (ex: `@param`).
- Primeira sentença é o título. Segundo parágrafo é uma descrição. Os outos parágrafos vão para _Details_.

## Vignettes

- Útil para dar uma explicação geral de um pacote
- Facilmente construído usando RMarkdown
- Geralmente usado para pacotes mais complexos

## Testes

- Pacote `testthat`, do Hadley.
- `devtools::use_testthat()`
- Defina o que você quer testar (função e parâmetros), e o que você espera de resultado
- _workflow_: mude códigos -> `devtools::test()` -> repita. 


```r
library(stringr)
context("String Length")
test_that("str_length is a number of characters", {
  expect_equal(str_length('a'), 1)
  expect_equal(str_length('ab'), 2)
  expect_equal(str_length('abc'), 3)
})
```

## Namespace

- Só é necessário se preocupar com isso se você quiser colocar seu pacote no CRAN.
- `imports` e `exports`.
- Search path, load e attach
- `requireNamespace()` dá load e não attach.
- Geralmente também é criado usando `devtools::document()` e `roxygen2`.
- Use `@export` para fazer sua função ficar disponível para o usuário via `::`
- Use `@importFrom pkg fun` para importar funções no NAMESPACE (não recomendável)
- Use Depends se você quiser dar attach de um pacote e usar suas funções (no DESCRIPTION).

## Dados externos {.build}

- Três maneiras de incluir dados no pacote.
- Arquivos binários (`.RData`) na pasta `data/`. Utilizar `devtools::usedata()`
- Dados utilizados internamente pelas funções em `R/sysdata.rda`
- Dados em texto (csv, excel, etc), na pasta `inst/extdata`
- Documentar dados é semelhante a documentar funções, adicionando `@format` e `@source`
- Não é necessário usar `@export`

## Código compulado (C, C++, etc)

- Usar o pacote `RCpp`
- Programar em `C` e `C++` foge do escopo do curso
- Usando o RStudio e abrindo um novo arquivo, é possível visualizar um template.
- Usar Ctrl+Shift+B ao invés de `devtools::load_all()`

# Melhores práticas

## Git e GitHub

- Versionamento
- Colaboração
- Funciona como um website para seu pacote

<!-- ___________________________________________________________________________________________ -->


# Tópico extra: web crawling

## Filosofia

- Se não tem dados, faça o download dos dados
- Automatizar acessos a websites
- É necessário aprender a passear pelos sites (_crawling_) e trabalhar com os arquivos HTML (_scraping_)
- Podemos ter uma lista do que queremos inicialmente ou não

## Pacotes

- Pacotes `RCurl` e `XML` do Duncan.
- Pacotes `httr` e `rvest` do Hadley.
- Pacote `RSelenium` (não funciona muito bem ainda)

## Requisições GET e POST

- `GET`: Pede por informação do servidor
- `POST`: Envia infomações para o servidor
- Para mais detalhes, ver [aqui](http://www.w3schools.com/tags/ref_httpmethods.asp)

## Cookies, viewstate, etc.

- É bem comum que os sites não queiram que um computador acesse facilmente seus documentos
- Muitas vezes é preciso mostrar que o usuário "clicou" nos campos necessários para acessar um documento
- `ASP`, `ASPX`, `JSP` entre outros imprimem valores em `tags` na própria página acessada para minimizar acesso automatizado

## Captcha

- Atrapalha a vida dos web crawlers
- Muitas vezes é difícil de ser quebrado
- Geralmente são utilizados métodos de machine learning para extrair palavras de imagens
- Exemplo, serviço [deathbycaptcha](http://www.deathbycaptcha.com/user/login)


## Exemplo: Sabesp

- Exemplo no R

