# PnPJs vs SQL

Quase todos os sistemas desenvolvidos em SharePoint online envolvem PnP JS, existem diversas dúvidas de como usar essa biblioteca. Vendo a dificuldade de colegas de trabalho, ao usá-la, resolvi escrever este artigo colocando o PnP lado a lado do SQL, fazendo breves comparações entre ambos.
De um lado uma linguagem de consulta estruturada, utilizadas por milhões de programadores e do outro lado uma coleção de bibliotecas fluentes para consumir APIs REST do SharePoint, Graph e Office 365, usada por desenvolvedores de aplicações SharePoint.

## PnPJs, o que é ?

> O PnPjs é uma coleção de bibliotecas fluentes para consumir APIs
> REST do SharePoint, Graph e Office 365 de uma maneira segura. Você
> pode usá-lo no SharePoint Framework, Nodejs ou em qualquer projeto
> JavaScript. Esta é uma iniciativa de código aberto e incentivamos
> contribuições e feedback construtivo da comunidade

*Fonte: https://pnp.github.io/pnpjs/*

## Introdução
Entre o PnP e SQL existem lógicas semelhantes e a sintaxe diferente. Pensando neste cenário, uma breve comparação da sintaxe entre ambos.

| SQL | PNP |
|--|--|
| Table | Lista/Biblioteca |
|Row|Object(JavaScript)|
|Column|Column(Attribute)|
|Primary Key|Primary Key (By default ID)|
| FOREIGN KEY|Lookup|
| Join|Expand()|
| From |getByTitle()|

### Observações gerais
Dependendo da versão do PnP JS, devesse iniciar a chamada com o “$”(cifrão).    
Exemplo: ```$pnp.sp.web``` 


### Operadores lógicos e de comparações
O PnP utiliza o Perl como com operador, ou seja, operadores de String e não numéricos como o SQL.

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
Assim como o SQL o PnP conta com o seu CRUD (Create, Read, Update e Delete)

### INSERT
Para ambos os casos é necessário informar a origem (lista/tabela), levando em consideração a estrutura já estabelecida. Diferente do SQL, no PnP não é obrigatório declarar todas as colunas, mesmo que a coluna esteja configurada como obrigatória na configuração de sua lista.

Para adicionar item a lista é usado o método add().

|SQL| PNP |
|--|--|
|```INSERT  INTO  users (user_id, age, NAME) VALUES  ("f62255a8259f", 30, “peter”)``` |``` pnp.sp.web.lists.getByTitle("users").items.add({user_id: "f62255a8259f", age: 30, name: "Peter"}).then(function (itemAdicionado) { }) ``` |

### SELECT
Para ambos os casos é necessário informar a origem (lista/tabela), levando em consideração a estrutura já estabelecida.

#####  Consultar todos os itens, trazendo todas as colunas:
|SQL| PNP |
|--|--|
|```SELECT * FROM users``` | ```pnp.sp.web.lists.getByTitle("users").items.get().then(function (result) { }) ``` |

#####  Consultar todos os itens, especificando coluna(s):
|SQL| PNP |
|--|--|
|```SELECT _user_id, name_ FROM users``` |```pnp.sp.web.lists.getByTitle("users").items.select(“_user_id, name_”).get().then(function (result) { }) ``` |

|SQL| PNP |
|--|--|
|```SELECT * FROM users WHERE name = “Peter” and age = 30``` |```pnp.sp.web.lists.getByTitle("users").items.filter(“name eq ‘Peter’ and age eq 30”).get().then(function (result) { }); ``` |

#####  Consultar dados que determina se uma cadeia de caracteres específica corresponde a um padrão especificado
|SQL| PNP |
|--|--|
|```SELECT * FROM users WHERE name LIKE “P%”``` |```pnp.sp.web.lists.getByTitle("users").items.filter("substringof('P', name).get().then(function (result) { }); ``` |
 
#####  Ordenar retorno:
|SQL| PNP |
|--|--|
| ```SELECT * FROM users ORDER BY name DESC/ASC ``` |```pnp.sp.web.lists.getByTitle("users").items.orderBy(“name”, true/false).get().then(function (result) { }); ``` |
 
|SQL| PNP |
|--|--|
|```SELECT * FROM users WHERE name = "Peter” ORDER BY name DESC ``` |```pnp.sp.web.lists.getByTitle("users").items.filter(“name eq ‘Peter’”).orderBy(“name”, false).get().then(function (result) { }); ``` |

#####  Limitar retorno da consulta:
|SQL| PNP |
|--|--|
|```SELECT * FROM users LIMIT 5 ``` | ```pnp.sp.web.lists.getByTitle("users").items.top(5).get().then(function (result) { }); ``` |

##### Get() vs GetAll()
No PnP exitem dois métodos para obter dados de uma lista, são eles get() e o getAll(), todavia exitem diferenças entre eles

|Item| get() | getAll() |
|--|--|--|
|Quantidade de itens retornado |Até 100 itens |Mais de 100 itens |
|Funciona o orderBy() | Sim | Não |
|Funciona o top() | Sim | Não |

### UPDATE 
No update do PNP, o getById() serve como condição(where), ou seja, só será atualizado o item no qual o ID (da lista) foi informado.

|SQL| PNP |
|--|--|
|```UPDATE users SET age = 18 WHERE id = 10 ``` |```pnp.sp.web.lists.getByTitle("users").items.getById(10).update ({ age: 18 }).then(function (itemAtualizado) { })  ``` |

ATENÇÃO: Não execute o UPDATE sem o método getById(), pois caso contrário ira atualizar todos os itens da lista.

### DELETE 
Assim como no Update, o getById() serve como condição(where).

|SQL| PNP |
|--|--|
|```DELETE FROM users WHERE id = 10 ``` |```pnp.sp.web.lists.getByTitle("users").items.getById(10).delete().then(function(itemDeletado) { })  ``` |

ATENÇÃO: Não execute o DELETE sem o método getById().

### JOINS 
Para obter campos relacionados.

|SQL| PNP |
|--|--|
|```SELECT * FROM posts JOIN users ON posts.user_id = users.id ``` | ```pnp.sp.web.lists.getByTitle("posts").items.expand(“users”).filter(“ID eq 10 and users/id eq 10”).get().then(function (resultJoin) { })  ``` |

Documentação completa e oficial do PnP: https://pnp.github.io/pnpjs/sp/items/


##### By

Portfólio: http://bit.ly/portfoliorenatojunior
LinkedIn: https://www.linkedin.com/in/renato-bezerra
Medium: https://medium.com/@renatojuniordw/pnpjs-vs-sql-2406e6916e3e
