---
title: Suporte de funcionalidades do Azure Cosmos DB para MongoDB | Microsoft Docs
description: Saiba mais sobre o suporte de funcionalidades que a API do Azure Cosmos DB MongoDB presta para MongoDB 3.4.
services: cosmos-db
author: alekseys
manager: kfile
ms.service: cosmos-db
ms.component: cosmosdb-mongo
ms.devlang: na
ms.topic: overview
ms.date: 11/15/2017
ms.author: alekseys
ms.openlocfilehash: 9202e8eb328f098f7ab68a18f4629a95ecc10991
ms.sourcegitcommit: 6116082991b98c8ee7a3ab0927cf588c3972eeaa
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/05/2018
ms.locfileid: "34796360"
---
# <a name="mongodb-api-support-for-mongodb-features-and-syntax"></a>Suporte de API do MongoDB para a sintaxe e funcionalidades do MongoDB

O Azure Cosmos DB é um serviço de bases de dados com vários modelos e distribuído globalmente da Microsoft. Pode comunicar com a API do Mongo DB da base de dados através de algum dos [controladores](https://docs.mongodb.org/ecosystem/drivers) do cliente de MongoDB em código aberto. A API do MongoDB permite a utilização dos controladores existentes do cliente através do respetivo [protocolo de transmissão](https://docs.mongodb.org/manual/reference/mongodb-wire-protocol) do MongoDB.

Ao utilizar a API do Azure Cosmos DB MongoDB, pode desfrutar dos benefícios das API do MongoDB a que está habituado, com todas as capacidades empresariais que o Azure Cosmos DB fornece: [distribuição global](distribute-data-globally.md), [fragmentação automática](partition-data.md), disponibilidade e garantias de latência, indexação automática de cada campo, encriptação em inatividade, cópias de segurança, etc.

## <a name="mongodb-query-language-support"></a>Suporte de linguagem de consulta do MongoDB

A API do Azure Cosmos DB MongoDB presta suporte abrangente para construções de linguagem de consulta do MongoDB. Abaixo encontra a lista detalhada de operações, operadores, fases, comandos e opções atualmente suportados.


## <a name="database-commands"></a>Comandos da base de dados

O Azure Cosmos DB suporta os seguintes comandos de base de dados em todas as contas de API do MongoDB. 

### <a name="query-and-write-operation-commands"></a>Comandos de operação de consulta e de escrita
- delete
- find
- findAndModify
- getLastError
- getMore
- insert
- update

### <a name="authentication-commands"></a>Comandos de autenticação
- logout
- authenticate
- getnonce

### <a name="administration-commands"></a>Comandos de administração
- dropDatabase
- listCollections
- drop
- criar
- filemd5
- createIndexes
- listIndexes
- dropIndexes
- connectionStatus
- reIndex

### <a name="diagnostics-commands"></a>Comandos de diagnóstico
- buildInfo
- collStats
- dbStats
- hostInfo
- listDatabases
- whatsmyuri

<a name="aggregation-pipeline"/>

## <a name="aggregation-pipelinea"></a>Pipeline de agregação</a>

O Azure Cosmos DB suporta pipeline de agregação na pré-visualização pública. Consulte o [blogue do Azure](https://aka.ms/mongodb-aggregation) para receber instruções sobre como integrar a pré-visualização pública.

### <a name="aggregation-commands"></a>Comandos de agregação
- aggregate
- count
- distinct

### <a name="aggregation-stages"></a>Fases de agregação
- $project
- $match
- $limit
- $skip
- $unwind
- $group
- $sample
- $sort
- $lookup
- $out
- $count
- $addFields

### <a name="aggregation-expressions"></a>Expressões de agregação

#### <a name="boolean-expressions"></a>Expressões booleanas
- $and
- $or
- $not

#### <a name="set-expressions"></a>Expressões de definição
- $setEquals
- $setIntersection
- $setUnion
- $setDifference
- $setIsSubset
- $anyElementTrue
- $allElementsTrue

#### <a name="comparison-expressions"></a>Expressões de comparação
- $cmp
- $eq
- $gt
- $gte
- $lt
- $lte
- $ne

#### <a name="arithmetic-expressions"></a>Expressões aritméticas
- $abs
- $add
- $ceil
- $divide
- $exp
- $floor
- $ln
- $log
- $log10
- $mod
- $multiply
- $pow
- $sqrt
- $subtract
- $trunc

#### <a name="string-expressions"></a>Expressões de cadeia
- $concat
- $indexOfBytes
- $indexOfCP
- $split
- $strLenBytes
- $strLenCP
- $strcasecmp
- $substr
- $substrBytes
- $substrCP
- $toLower
- $toUpper

#### <a name="array-expressions"></a>Expressões de matriz
- $arrayElemAt
- $concatArrays
- $filter
- $indexOfArray
- $isArray
- $range
- $reverseArray
- $size
- $slice
- $in

#### <a name="date-expressions"></a>Expressões de data
- $dayOfYear
- $dayOfMonth
- $dayOfWeek
- $year
- $month
- $week
- $hour
- $minute
- $second
- $millisecond
- $isoDayOfWeek
- $isoWeek

#### <a name="conditional-expressions"></a>Expressões condicionais
- $cond
- $ifNull

## <a name="aggregation-accumulators"></a>Acumuladores de agregação
- $sum
- $avg
- $first
- $last
- $max
- $min
- $push
- $addToSet

## <a name="operators"></a>Operadores

Os seguintes operadores são suportados com exemplos correspondentes da respetiva utilização. Considere este documento de exemplo utilizado nas consultas abaixo:

```json
{
  "Volcano Name": "Rainier",
  "Country": "United States",
  "Region": "US-Washington",
  "Location": {
    "type": "Point",
    "coordinates": [
      -121.758,
      46.87
    ]
  },
  "Elevation": 4392,
  "Type": "Stratovolcano",
  "Status": "Dendrochronology",
  "Last Known Eruption": "Last known eruption from 1800-1899, inclusive"
}
```

Operador | Exemplo |
--- | --- |
$eq | ``` { "Volcano Name": { $eq: "Rainier" } } ``` |  | -
$gt | ``` { "Elevation": { $gt: 4000 } } ``` |  | -
$gte | ``` { "Elevation": { $gte: 4392 } } ``` |  | -
$lt | ``` { "Elevation": { $lt: 5000 } } ``` |  | -
$lte | ``` { "Elevation": { $lte: 5000 } } ``` | | -
$ne | ``` { "Elevation": { $ne: 1 } } ``` |  | -
$in | ``` { "Volcano Name": { $in: ["St. Helens", "Rainier", "Glacier Peak"] } } ``` |  | -
$nin | ``` { "Volcano Name": { $nin: ["Lassen Peak", "Hood", "Baker"] } } ``` | | -
$or | ``` { $or: [ { Elevation: { $lt: 4000 } }, { "Volcano Name": "Rainier" } ] } ``` |  | -
$and | ``` { $and: [ { Elevation: { $gt: 4000 } }, { "Volcano Name": "Rainier" } ] } ``` |  | -
$not | ``` { "Elevation": { $not: { $gt: 5000 } } } ```|  | -
$nor | ``` { $nor: [ { "Elevation": { $lt: 4000 } }, { "Volcano Name": "Baker" } ] } ``` |  | -
$exists | ``` { "Status": { $exists: true } } ```|  | -
$type | ``` { "Status": { $type: "string" } } ```|  | -
$mod | ``` { "Elevation": { $mod: [ 4, 0 ] } } ``` |  | -
$regex | ``` { "Volcano Name": { $regex: "^Rain"} } ```|  | -

### <a name="notes"></a>Notas

Nas consultas $regex, as expressões ancoradas à esquerda permitem uma pesquisa de índice. No entanto, utilizar o modificador "i" (não sensível a maiúsculas e minúsculas) e o modificador "m" (multinha) faz a análise de coleção em todas as expressões.
Quando for necessário incluir "$" ou "|", é melhor criar duas (ou mais) consultas de regex. Por exemplo, dada a seguinte consulta original: ```find({x:{$regex: /^abc$/})```, tem de ser modificada como se segue: ```find({x:{$regex: /^abc/, x:{$regex:/^abc$/}})```.
A primeira parte irá utilizar o índice para restringir a pesquisa aos documentos que começam por ^abc e a segunda parte vai fazer a correspondência com as entradas exatas. O operador barra "|" atua como uma função "or" – a consulta ```find({x:{$regex: /^abc|^def/})``` faz a correspondência dos documentos cujo campo "x" tem valores que começam por "abc" ou "def". Para utilizar o índice, é recomendado dividir a consulta em duas consultas diferentes associadas pelo operador $or: ```find( {$or : [{x: $regex: /^abc/}, {$regex: /^def/}] })```.

### <a name="update-operators"></a>Operadores de atualização

#### <a name="field-update-operators"></a>Operadores de atualização de campo
- $inc
- $mul
- $rename
- $setOnInsert
- $set
- $unset
- $min
- $max
- $currentDate

#### <a name="array-update-operators"></a>Operadores de atualização de matriz
- $addToSet
- $pop
- $pullAll
- $pull (nota: $pull com condição não é suportado)
- $pushAll
- $push
- $each
- $slice
- $sort
- $position

#### <a name="bitwise-update-operator"></a>Operador de atualização bit a bit
- $bit

### <a name="geospatial-operators"></a>Operadores geoespaciais

Operador | Exemplo 
--- | --- |
$geoWithin | ```{ "Location.coordinates": { $geoWithin: { $centerSphere: [ [ -121, 46 ], 5 ] } } }``` | Sim
$geoIntersects |  ```{ "Location.coordinates": { $geoIntersects: { $geometry: { type: "Polygon", coordinates: [ [ [ -121.9, 46.7 ], [ -121.5, 46.7 ], [ -121.5, 46.9 ], [ -121.9, 46.9 ], [ -121.9, 46.7 ] ] ] } } } }``` | Sim
$near | ```{ "Location.coordinates": { $near: { $geometry: { type: "Polygon", coordinates: [ [ [ -121.9, 46.7 ], [ -121.5, 46.7 ], [ -121.5, 46.9 ], [ -121.9, 46.9 ], [ -121.9, 46.7 ] ] ] } } } }``` | Sim
$nearSphere | ```{ "Location.coordinates": { $nearSphere : [ -121, 46  ], $maxDistance: 0.50 } }``` | Sim
$geometry | ```{ "Location.coordinates": { $geoWithin: { $geometry: { type: "Polygon", coordinates: [ [ [ -121.9, 46.7 ], [ -121.5, 46.7 ], [ -121.5, 46.9 ], [ -121.9, 46.9 ], [ -121.9, 46.7 ] ] ] } } } }``` | Sim
$minDistance | ```{ "Location.coordinates": { $nearSphere : { $geometry: {type: "Point", coordinates: [ -121, 46 ]}, $minDistance: 1000, $maxDistance: 1000000 } } }``` | Sim
$maxDistance | ```{ "Location.coordinates": { $nearSphere : [ -121, 46  ], $maxDistance: 0.50 } }``` | Sim
$center | ```{ "Location.coordinates": { $geoWithin: { $center: [ [-121, 46], 1 ] } } }``` | Sim
$centerSphere | ```{ "Location.coordinates": { $geoWithin: { $centerSphere: [ [ -121, 46 ], 5 ] } } }``` | Sim
$box | ```{ "Location.coordinates": { $geoWithin: { $box:  [ [ 0, 0 ], [ -122, 47 ] ] } } }``` | Sim
$polygon | ```{ "Location.coordinates": { $near: { $geometry: { type: "Polygon", coordinates: [ [ [ -121.9, 46.7 ], [ -121.5, 46.7 ], [ -121.5, 46.9 ], [ -121.9, 46.9 ], [ -121.9, 46.7 ] ] ] } } } }``` | Sim

## <a name="additional-operators"></a>Operadores adicionais

Operador | Exemplo | Notas 
--- | --- | --- |
$all | ```{ "Location.coordinates": { $all: [-121.758, 46.87] } }``` | 
$elemMatch | ```{ "Location.coordinates": { $elemMatch: {  $lt: 0 } } }``` |  
$size | ```{ "Location.coordinates": { $size: 2 } }``` | 
$comment |  ```{ "Location.coordinates": { $elemMatch: {  $lt: 0 } }, $comment: "Negative values"}``` | 
$text |  | Não suportado. Em alternativa, utilize $regex 

### <a name="methods"></a>Métodos

São suportados os seguintes métodos:

#### <a name="cursor-methods"></a>Métodos de cursor

Método | Exemplo | Notas 
--- | --- | --- |
cursor.sort() | ```cursor.sort({ "Elevation": -1 })``` | Os documentos sem chave de ordenação não são devolvidos

## <a name="unique-indexes"></a>Índices exclusivos

O Azure Cosmos DB indexa cada campo nos documentos que são escritos na base de dados por predefinição. Os índices exclusivos garantem que um campo específico não tem valores duplicados em todos os documentos numa coleção, semelhante ao modo de preservação da exclusividade na chave "_id" predefinida. Agora pode criar índices personalizados no Azure Cosmos DB utilizando o comando createIndex, incluindo a restrição "unique".

Estão disponíveis índices exclusivos para todas as contas de API do MongoDB.

## <a name="time-to-live-ttl"></a>TTL

O Azure Cosmos DB suporta um TTL relativo com base no carimbo de data/hora do documento. O TTL pode ser ativado para coleções de API do MongoDB através do [portal do Azure](https://portal.azure.com).

## <a name="user-and-role-management"></a>Gestão de funções e utilizadores

O Azure Cosmos DB ainda não suporta utilizadores e funções. O Azure Cosmos DB suporta o controlo de acesso baseado em funções (RBAC) e palavras-passe/chave de leitura/escrita e apenas leitura que podem ser obtidas através do [portal do Azure](https://portal.azure.com) (página Cadeia de Ligação).

## <a name="replication"></a>Replicação

O Azure Cosmos DB suporta a replicação nativa e automática nas camadas inferiores. Esta lógica é expandida para conseguir também a replicação global de latência baixa. O Azure Cosmos DB não suporta comandos de replicação manuais.

## <a name="sharding"></a>Fragmentação

O Azure Cosmos DB suporta a fragmentação automática do lado do servidor. O Azure Cosmos DB não suporta comandos de fragmentação manuais.

## <a name="next-steps"></a>Passos seguintes

- Saiba como [utilizar Studio 3T](mongodb-mongochef.md) com uma API para base de dados do MongoDB.
- Saiba como [utilizar Robo 3T](mongodb-robomongo.md) com uma API para base de dados do MongoDB.
- Explore o Azure Cosmos DB com suporte de protocolo para [exemplos](mongodb-samples.md) do MongoDB.
