

// Selecionar schema
use be-mean-pokemons

// Mostrar coleções
show collections

// Cria coleção
db.createCollection('pokemons')

// Remover todos os documentos da coleção
db.pokemons.remove({});

// Inserir registros na coleção
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


// O find aceita dois parâmetros
// 1 - clausula 2 - colunas

var query = {name:"Pikachu"};
db.pokemons.find(query)

// A cláusula é o where já as colunas são o select
// 1 -> true (vai retornar) ou 0 (duhhhh)

var query = {name:"Pikachu"};
var fields = {name:1,_id:0}

db.pokemons.find(query, fields);

// Operadores
// <   ---  $lt
// <=  ---  $lte
// >   ---  $gt
// >=  ---  $gte

// Lógicos
// $or
// $nor
// $and

// Or
var query =  {$or: [{height: 0.4},{height: 0.3}]};

db.pokemons.find(query);

// NotOr
var query =  {$nor: [{height: 0.4},{height: 0.3}]};

db.pokemons.find(query);

// And
var query =  {$and: [{height: 0.4},{height: {$gte: 0.3}}]};

db.pokemons.find(query);


// Operador existêncial -> $exists
var query =  {campoExistente: {$exists: true}};

db.pokemons.find(query);




