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

> mongoexport --db nome_do_database --collection nome_da_colecao --out minha_colecao.json

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