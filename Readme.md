# Repositório de estudos do MongoDB

## Selecionar schema ##

```javascript
use be-mean-pokemons
```

## Mostrar coleções ##

```javascript
show collections
```

## Criando uma coleção ##
```javascript

db.createCollection('pokemons')
```

## Removendo todos os documentos de uma coleção ##
```javascript
// Sem filtros no modo estagiário
// Mas podem ser usados os mesmos filtros de busca igual lá em baixo
db.pokemons.remove({});
```

## Inserindo registros em uma coleção ##
```javascript
db.pokemons.insert([
{
  "name": "Pikachu",
  "description": "Rato elétrico bem fofinho",
  "type": "electric",
  "attack": 100,
  "height": 0.4
},
{
  "name": "Bulbassauro",
  "description": "Chicote de trepadeira",
  "type": "grama",
  "attack": 49,
  "height": 0.4
},
{
  "name": "Caterpie",
  "description": "Larva lutadora",
  "type": "inseto",
  "attack": 30,
  "height": 0.3,
  "defense": 35
}]);
```

## Comando find ##
```javascript
// O find aceita dois parâmetros
// 1 - clausula 2 - colunas

var query = {name:"Pikachu"};
db.pokemons.find(query)

// A cláusula é o where já as colunas são o select
// 1 -> true (vai retornar) ou 0 (duhhhh)

var query = { name: "Pikachu" };
var fields = { name: 1, _id: 0 };

db.pokemons.find(query, fields);
```

## Operadores aritméticos ##
```javascript
// <   <->  $lt
// <=  <->  $lte
// >   <->  $gt
// >=  <->  $gte
```

## Operadores lógicos ( $or, $nor, $and ) ##

```javascript
// Or
var query =  {$or: [{height: 0.4},{height: 0.3}]};

db.pokemons.find(query);
```

```javascript
// NotOr
var query =  {$nor: [{height: 0.4},{height: 0.3}]};

db.pokemons.find(query);
```

```javascript
// And
var query =  {$and: [{height: 0.4},{height: {$gte: 0.3}}]};

db.pokemons.find(query); 
```

```javascript
// Operador existêncial -> $exists
var query =  {campoExistente: {$exists: true}};

db.pokemons.find(query);
```

## Update ##

```javascript
// Primeiro um save maroto
var poketest = {
  "name": "Poketest",
  "description": "Só um test duh",
  "type": "electric",
  "attack": 168,
  "height": 0.75
}

db.pokemons.save(poketest);

// Achando nosso cara
var queryMarota = { name: /Poketest/i }; // Um regex em js só pra exercitar
db.pokemons.find(queryMarota);

// Aqui o id do cara é -> 598f286e2fc06a19a0fd7e40

// Vamos buscar com o _id
var queryId = {"_id": ObjectId("598f286e2fc06a19a0fd7e40")};

// Fazer aqui uma query para alteração
var changes = { description: "Mudando as coisas..." };

// Primeiro o where, depois o update (sem maldade)
db.pokemons.update(queryId, changes);
```
```json
// Saída mais ou menos assim
{
	"nInserted" : 0,
	"nUpserted" : 0,
	"nMatched" : 1, // Achou aqui
	"nModified" : 1, // Modificou
	"nRemoved" : 0,
	"lastOp" : {
		"ts" : Timestamp(1502555927, 2),
		"t" : 9
	}
}
```
```javascript
// Vamos buscar o cara agora
var query = {"_id": ObjectId("598f286e2fc06a19a0fd7e40")};
db.pokemons.find(query);
```
```json
// Saída mais ou menos assim
{
	"_id" : ObjectId("598f286e2fc06a19a0fd7e40"),
	"description" : "Mudando as coisas..."
}
// Ihhh, acho que alguem deu um update zuado
// Basicamente, não se pode passar só alguns campos e achar q o mongo vai fazer um patch do HTTP duhhh
```
## Fazendo um update correto like a boss ##

```javascript
// Só usar o operador $set passando o campo ou os campos
db.pokemons.update({name: 'Pikachu'},  { $set: { attack: 120}});
```
```json
// Saída mais ou menos assim
{
	"nInserted" : 0,
	"nUpserted" : 0,
	"nMatched" : 1,
	"nModified" : 1,
	"nRemoved" : 0,
	"lastOp" : {
		"ts" : Timestamp(1502556815, 2),
		"t" : 9
	}
}
```
```javascript
// Buscando o maluco
db.pokemons.find({name: 'Pikachu'});
```
```json
// Saída mais ou menos assim
{
	"_id" : ObjectId("598e60db2fc06a19a0fd7e3c"),
	"name" : "Pikachu",
	"description" : "Rato elétrico bem fofinho",
	"type" : "electric",
	"attack" : 120,
	"height" : 0.4
}
// Não zuou nada :)
```
## Deletando campos do documento ( $unset )
```javascript
// Segue o mesmo padrão do find 0 e 1 nos campos -> 1 vai remover
db.pokemons.update({name: 'Pikachu'}, { $unset: { height: 1 }});
```
```json
// Saída mais ou menos assim
{
	"_id" : ObjectId("598e60db2fc06a19a0fd7e3c"),
	"name" : "Pikachu",
	"description" : "Rato elétrico bem fofinho",
	"type" : "electric",
	"attack" : 120,
}
```
## Incrementando um campos do documento ( $inc )
```javascript
// Passe o campo e por qual valor será incrementado
db.pokemons.update({name: 'Pikachu'}, { $inc: { attack: 500 }});
```
```json
// Saída mais ou menos assim
{
	"_id" : ObjectId("598e60db2fc06a19a0fd7e3c"),
	"name" : "Pikachu",
	"description" : "Rato elétrico bem fofinho",
	"type" : "electric",
	"attack" : 620
}
```
```javascript
// Para decrementar passe o valor negativo
db.pokemons.update({name: 'Pikachu'}, { $inc: { attack: -20 }});
```
```json
// Saída mais ou menos assim
{
	"_id" : ObjectId("598e60db2fc06a19a0fd7e3c"),
	"name" : "Pikachu",
	"description" : "Rato elétrico bem fofinho",
	"type" : "electric",
	"attack" : 600
}
```
