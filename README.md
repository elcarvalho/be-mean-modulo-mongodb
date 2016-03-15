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
