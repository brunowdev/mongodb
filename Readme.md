# Repositório de estudos do MongoDB
Contém exercícios de cursos, artigos, etc.

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

## Deletando uma coleção ##
```javascript
db.pokemons.drop();
```

## Removendo todos os documentos de uma coleção ##
```javascript
// Sem filtros no modo estagiário
// Mas podem ser usados os mesmos filtros de busca igual lá em baixo
// Atenção para o remove -> Diferente do update, o remove é multi: true
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
## Arrays
```javascript
// Adicionando um campo do tipo array
db.pokemons.update({name: 'Pikachu'}, { $push: { moves: 'Choque do trovão' }});
```
```json
// Saída mais ou menos assim
{
	"_id" : ObjectId("598e60db2fc06a19a0fd7e3c"),
	"name" : "Pikachu",
	"description" : "Rato elétrico bem fofinho",
	"type" : "electric",
	"attack" : 620,
	"moves" : [ // Ó, criou e adicionou?
		"Choque do trovão"
	]
}
```
```javascript
// $pushAll
db.pokemons.update({name: 'Pikachu'}, { $pushAll: { attacks: ['choque elétrico', 'ataque rápido', 'bola elétrica'] }});
```
```json
// Saída mais ou menos assim
{
	"_id" : ObjectId("598e60db2fc06a19a0fd7e3c"),
	"name" : "Pikachu",
	"description" : "Rato elétrico bem fofinho",
	"type" : "electric",
	"attack" : 620,
	"moves" : [
		"Choque do trovão"
	],
	"attacks" : [
		"choque elétrico",
		"ataque rápido",
		"bola elétrica"
	]
}
```
```javascript
// $pull
db.pokemons.update({name: 'Pikachu'}, { $pull: { attacks: 'choque elétrico' }});
```
```json
// Saída mais ou menos assim
{
	"_id" : ObjectId("598e60db2fc06a19a0fd7e3c"),
	"name" : "Pikachu",
	"description" : "Rato elétrico bem fofinho",
	"type" : "electric",
	"attack" : 620,
	"moves" : [
		"Choque do trovão"
	],
	"attacks" : [ // Sumiu 1
		"ataque rápido",
		"bola elétrica"
	]
}
```
```javascript
// $pullAll -> inverso do $pushAll
db.pokemons.update({name: 'Pikachu'}, { $pullAll: { attacks: ['ataque rápido', 'bola elétrica'] }});
```
```json
// Saída mais ou menos assim
{
	"_id" : ObjectId("598e60db2fc06a19a0fd7e3c"),
	"name" : "Pikachu",
	"description" : "Rato elétrico bem fofinho",
	"type" : "electric",
	"attack" : 620,
	"moves" : [
		"Choque do trovão"
	],
	"attacks" : [ ] // Funcionou
}
```
### upsert ### 
Por padrão ele é false. Caso o objeto buscado não seja encontrado ele irá criá-lo.

### $setOnInsert ###
Um documento de parâmetro que pode ser passado com o upsert. Ideal para iniciar com valores padrões, caso o registro não exista.

```javascript
var query = { name: /Pokenotfind/i };
var changes = {
   $set: { active: true},
   $setOnInsert: {
     "name" : "NaoExisteMon",
     "description" : null,
     "type" : null,
     "attack" : null,
     "moves" : [ ]
   }
}
var options = { upsert: true };

db.pokemons.update(query, changes, options);
```
```json
// Saída mais ou menos assim
{
	"nInserted" : 0,
	"nUpserted" : 1, // Não achou e inseriu
	"nMatched" : 0,
	"nModified" : 0,
	"nRemoved" : 0,
	"lastOp" : {
		"ts" : Timestamp(1502559476, 1),
		"t" : 9
	}
}
// Veja o registro com os valores do $setOnInsert
{
	"_id" : ObjectId("598f3d7d418d4803b15ed4f7"),
	"active" : true,
	"name" : "NaoExisteMon",
	"description" : null,
	"type" : null,
	"attack" : null,
	"moves" : [ ]
}
```
### multi ###
Por padrão, o Mongo só altera um registro por vez. Se você quer fazer um update sem where numa sexta-feira, basta setar esse parâmetro para **true**.

### Write concerns ###

O Mongo retorna se o comando foi escrito com sucesso. Esse nível pode ser maior ou menor. Em alguns casos, para maior performance, pode ser interessante diminuir esse nível. Assim a query será executada com maior velocidade. Mais informações [aqui](https://docs.mongodb.com/manual/reference/write-concern/).


### $in ###

```javascript
// In
var query = { name: {$in: ['Pikachu']}};
db.pokemons.find(query);
```

### $nin ###
```javascript
// Not in
var query = { name: {$nin: ['Pikachu']}};
db.pokemons.find(query);
```

### $all ###
```javascript
// All representa um and
var query = { attacks: {$all: ['ataque rápido', 'bola elétrica']}};
db.pokemons.find(query);
```

### $ne ###

```javascript
// Ne ou NotEquals
var query = { name: {$ne: ['Pikachu']}};
db.pokemons.find(query);
```

### $not ###

```javascript
// Not
var query = { name: {$not: ['Pikachu']}};
db.pokemons.find(query);
```