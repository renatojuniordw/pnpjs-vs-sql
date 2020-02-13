# PnPJs vs SQL

Documento criado, visando o melhor entendimento da biblioteca PNP JS, comparando-a com o SQL que é uma linguagem padrão de consulta muito usada.

  ## PnPJs, o que é ?
  

> O PnPjs é uma coleção de bibliotecas fluentes para consumir APIs
> REST do SharePoint, Graph e Office 365 de uma maneira segura. Você
> pode usá-lo no SharePoint Framework, Nodejs ou em qualquer projeto
> JavaScript. Esta é uma iniciativa de código aberto e incentivamos
> contribuições e feedback construtivo da comunidade

*Fonte: https://pnp.github.io/pnpjs/*

## Introdução

| SQL | PNP |
|--|--|
| Table | Lista/Biblioteca |
|Row|Object(JavaScript)|
|Column|Column(Attribute)|
|Primary Key|Primary Key (By default ID)|
| FOREIGN KEY|Lookup|
| Join|Expand()|
| From |getByTitle()|

### Comparando escalares em Perl vs SQL

| SQL | PNP |
|--|--|
| ASC| true |
|DESC|false|
|=|EQ|
|<>|NE|
| <|LT|
| >|GT|
| <=|LE|
| >=|GE|

## CRUD

### INSERT
Para ambos os casos é necessário informar a origem (lista/tabela), levando em consideração a estrutura já estabelecida. Diferente do SQL, no PNP não é obrigatório declarar todas as colunas, mesmo ela sendo obrigatórias.
|SQL| PNP |
|--|--|
| ```INSERT  INTO  users (user_id, age, NAME) VALUES  ("f62255a8259f", 30, “peter”)``` | ``` pnp.sp.web.lists.getByTitle("users").items.add({user_id: "f62255a8259f", age: 30, name: "Peter"}).then(function (itemAdicionado) { }) ``` |

