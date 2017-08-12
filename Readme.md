

# Selecionar schema
```javascript
use be-mean-pokemons
```

# Mostrar coleções
```javascript
show collections
```

# Criando uma coleção
```javascript
db.createCollection('pokemons')
```

# Removendo todos os documentos de uma coleção
```javascript
db.pokemons.remove({});
```

# Inserindo registros em uma coleção
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

# Comando find
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

# Operadores aritméticos
// <   ---  $lt
// <=  ---  $lte
// >   ---  $gt
// >=  ---  $gte

# Operadores lógicos
// $or
// $nor
// $and

```javascript
// Or
var query =  {$or: [{height: 0.4},{height: 0.3}]};

db.pokemons.find(query);
```

// NotOr
```javascript
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

# Update

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
```

