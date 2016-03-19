# BeMean - MongoDB

##Instalando MongoDB no Mac via brew

> brew install mongodb

Após a instalação vamos criar e dar permissão ao diretório utilizado pelo mongodb
```
sudo mkdir /data
sudo mkdir /data/db
sudo chmod 777 /data/db  *777 só para fins didáticos no maravilhoso mundo de localhost ;)
```
dã! >.<

##Comando para exportar uma coleção

> mongoexport --db nome_do_database --collection nome_da_colecao --out minha_colecao.json

- --db ou -d: especifica a database a ser usada/criada;
- --collection ou -c: especifica a coleção a ser usada/criada;
- --out: especifica qual arquivo receberá os dados.

##Comando para importar uma coleção

> mongoimport --db database --collection collection --drop --file data.json

- --db ou -d: especifica a database a ser usada/criada;
- --collection ou -c: especifica a coleção a ser usada/criada;
- --drop: apaga a coleção antes de inserir os novos dados;
- --file: especifica o caminho do arquivo a ser importado.

# Comandos básicos

## Para exibir a database selecionada

> db

## Para mudar de database

> use <nome_database>

## Listem de databases

> show dbs

## Criar database e collection

Para criar uma nova base de dados devemos selecionar a database e inserir algum registro nela

```
use be-mean-pokemons
db.pokemons.insert({"name":"Bulbasaur","description":"Bulbasaur can be seen napping in bright sunlight. There is a seed on its back. By soaking up the sun's rays, the seed grows progressively larger","attack":49, "defense":49, "height":0.7})
```
desta forma estaremos criando uma nova base chamada be-mean-pokemons e uma nova coleção chamada pokemons

## Listando dados de uma collection

> db.collection.find()

## Excluindo uma collection

> db.collection.drop()

## Alterando objeto na coleção

> findOne() diferente de find() retorna um objeto comum

```
var query = {"name":"Bulbasaur"}
var p = db.pokemons.findOne(query)
p.description = "descrição alterada"
db.pokemons.save(p)
```

# Operadores lógicos

## **$and**

### Sintaxe
> { $and: [ { <expression1> }, { <expression2> } , ... , { <expressionN> } ] }

### Exemplo
```
db.inventory.find( { $and: [ { price: { $ne: 1.99 } }, { price: { $exists: true } } ] } )
```

## **$or**

### Sintaxe
> { $or: [ { <expression1> }, { <expression2> }, ... , { <expressionN> } ] }

### Exemplo
```
db.inventory.find( { $or: [ { quantity: { $lt: 20 } }, { price: 10 } ] } )
```

## **$not**

### Sintaxe
> { field: { $not: { <operator-expression> } } }

### Exemplo
```
db.inventory.find( { price: { $not: { $gt: 1.99 } } } )
```

## **$nor**

### Sintaxe
> { $nor: [ { <expression1> }, { <expression2> }, ...  { <expressionN> } ] }

### Exemplo
```
db.inventory.find( { $nor: [ { price: 1.99 }, { sale: true } ]  } )
```

# Modificando documentos

## Para alterar um documento no mongoDB existem dois métodos:
 - save() *Para utilizar save() precisamos buscar o documento antes de alterá-lo*
 - update()

## A função **update()** recebe três parametros:
  - query
  - modificação
  - opções **opcional**

### Sintaxe
> db.collection.update(query, mod, options)

# Operadores de modificação

## O operador **$set** modifica um valor ou cria ele caso não exista

### Sintaxe
> { $set: { campo : valor } }

### Exemplo:
```
var query = {"name":"Pikachu"}
var mod = {$set: {"attack":120}}
db.pokemons.update(query, mod)
```

## O operador **$unset** é utilizado para remover campos

### Sintaxe
> { $unset: { campo : bool } }

### Exemplo
```
var query = {"name":/pikachu/i}
var mod = {$unset:{height:1}}
db.pokemons.update(query, mod)
```

## O operador **$inc** incrementa um valor no campo com a quantidade desejada.

*Caso o campo não exista, ele irá criar o campo e setar o valor.

Para decrementar, basta passar um valor negativo.*

### Sintaxe
> { $inc: { campo : valor } }

### Exemplo
```
var query = {"name":/pikachu/i}
var mod = {$inc:{attack:100}}
db.pokemons.update(query, mod)
```

# Operadores de array

## O operador **$push** adiciona um valor ao campo

*caso o campo seja um Array existente. Caso não exista irá criar o campo novo, do tipo Array com o valor passado no $push.

Caso o campo exista e não for um Array, irá retornar um erro.*

### Sintaxe
> { $push : { campo : valor } }

### Exemplo
```
var query = {"name":/pikachu/i}
var mod = {$push:{moves:'Thunder'}}
db.pokemons.update(query, mod)
```

## O operador **$pushAll** adiciona cada valor do [array_de_valores_informado]

### Sintaxe
> { $pushAll : { campo : [array_de_valores] } }

### Exemplo
```
var attacks = ["Thunder wave", "Thunderbolt", "Quick attack"]
var query = {"name":/pikachu/i}
var mod = {$pushAll:{moves:attacks}}
db.pokemons.update(query, mod)
```

## O operador **$pull** retira o valor do campo

### Sintaxe
> { $pull : { campo : valor } }

### Exemplo
```
var query = {"name":/pikachu/i}
var mod = {$pull: {moves: 'Quick attack'}}
db.pokemons.update(query, mod)
```

## O operador **$pullAll** retira cada valor do [array_de_valores_informado]

### Sintaxe
> { $pullAll : { campo : [array_de_valores] } }

### Exemplo
```
var attacks = ['Thunder', 'Thunder wave']
var mod = {$pullAll: {moves: attacks}}
db.pokemons.update(query, mod)
```

## Objeto options da função update(query, mod, **options**)
>O objeto options servirá para configurarmos alguns valores diferentes do padrão para o update.

### Sintaxe
> { upsert: boolean, multi: boolean, writeConcern: document }

## upsert
> O parâmetro **upsert** serve para caso o documento não seja encontrado pela query ele insira o objeto que está sendo passado como modificação.

## $setOnInsert
> Com esse operador você pode definir valores que serão adicionados apenas se ocorrer um upsert, ou seja, se o objeto for inserido pois não foi achado pela query.

### Exemplo
```
var query = {"name":/PokemonInexistente/i}
var mod = { $set: {"active":true}, $setOnInsert: {"name":"PokemonInexistente", "attack":null, "defense":null, "height":null, "description":"Sem informações"} }
var options = {upsert:true}
db.pokemons.update(query, mod, options)
```

## multi
> Por padrão o mongoDB não deixa você alterar multiplos documentos sem passar o campo como true

### Sintaxe
> { multi : true }

### Exemplo
```
var query = {}
var mod = {$set: {moves: ['investida']}}
var options = {multi: true}
db.pokemons.update(query, mod, options)
```

## O operador **$in** retorna o(s) documento(s) que possui(em) algum dos valores passados no [array_de_valores_informado].

### Sintaxe
> { campo : { $in : [array_de_valores] } }

### Exemplo
```
var query = {moves: {$in: [/thunder/i]}}
db.pokemons.find(query)
```

## **$nin** retorna documentos se nenhum dos valores for encontrado.

### Sintaxe
> { campo : { $nin : [array_de_valores] } }

### Exemplo
```
var query = {moves: {$nin: [/thunder/i]}}
db.pokemons.find(query)
```

## **$all* retorna documentos se todos os valores foram encontrados.

### Sintaxe
> { campo : { $all : [ array_de_valores ] } } )

### Exemplo
```
var query = {moves: {$all: ['Thunder', 'investida']}}
db.pokemons.find(query)
```

# Operadores de negação

## $ne Not Equal (não aceita regex)

### Sintaxe
> { campo : { $ne : valor} }

### Exemplo
```
var query = {type: {$ne: 'grama'}}
db.pokemons.find(query)
```

## $not

### Sintaxe
> { campo : { $not : valor} }

### Exemplo
```
var query = { name : { $not : /pikachu/i } }
db.pokemons.find(query)
```

# remove()

### Exemplo
```
var query = {name: \squirtle\i}
db.pokemons.remove(query)
```

# **count()**

### Sintaxe
> db.collection.count(query)

### Exemplo
```
var query = {"borough" : "Bronx"}
db.restaurantes.count(query)
```

# **distinct()**

### Sintaxe
> db.collection.distinct('valor')

### Exemplo
```
db.restaurantes.distinct("borough")
```

# **limit()**

### Sintaxe
> db.collection.find(query).limit(int)

## **skip()**

### Sintaxe
> db.collection.find(query).limit(int).skip(int)

### Exemplo
```
var qtd = 15
var query = {"cuisine":/american/i}
db.restaurantes.find(query).limit(qtd).skip(qtd * 0)
db.restaurantes.find(query).limit(qtd).skip(qtd * 1)
db.restaurantes.find(query).limit(qtd).skip(qtd * 2)
```

# **group()**

### Sintaxe
> db.collection.group({ key, reduce, initial [, keyf] [, cond] [, finalize] })
```
key - o(s) campo(s) para agrupar
reduce - Uma função de agregação que opera sobre os documentos durante a operação de agrupamento.
initial - Inicializa uma ou mais variaveis que poderão ser usadas dentro de reduce
cond - Define uma condição para o agrupamento
```

### Exemplo 1
```
db.pokemons.group(
{
    initial : {total : 0},
    cond : {defense : {$gt : 100}},
    reduce : function(curr, result){

      curr.types.forEach(function(type){
          if(result[type]) {
            result[type]++;
          } else {
            result[type] = 1;
          }

          result.total++;
      });

    }
})
```

### Exemplo 2
```
db.pokemons.group(
{
    initial : {total : 0, defense : 0, attack : 0},
    reduce : function(curr, result){
      result.total++;
      result.defense += curr.defense;
      result.attack += curr.attack;
    },
    finalize : function(result){
      result.avg_defense = result.defense / result.total;
      result.avg_attack = result.attack / result.total;
    }
})
```

# **aggregate()**

### Sintaxe
> db.collection.aggregate(pipeline, options)

### Exemplo 1
```
db.pokemons.aggregate({
    $group : {
      _id : {},
      defense_avg : { $avg : '$defense' },
      attack_avg : { $avg : '$attack' },
      defense : { $sum : '$defense'},
      attack : { $sum : '$attack'},
      total : { $sum : 1 }
    }
})
```

### Exemplo 2
```
db.pokemons.aggregate([
  {
    $match : { 'types' : 'fire' }
  },
  {
    $group : {
      _id : {},
      defense_avg : { $avg : '$defense' },
      attack_avg : { $avg : '$attack' },
      defense : { $sum : '$defense'},
      attack : { $sum : '$attack'},
      total : { $sum : 1 }
    }
  }
  ])
```
