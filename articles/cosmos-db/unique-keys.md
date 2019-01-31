---
title: Utilizar chaves exclusivas no Azure Cosmos DB
description: Saiba como utilizar chaves exclusivas na sua base de dados do Azure Cosmos DB
author: aliuy
ms.author: andrl
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 10/30/2018
ms.reviewer: sngun
ms.openlocfilehash: 73d4ba0c82f26a6249528f2dbef1fd30f99ccedb
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/31/2019
ms.locfileid: "55475878"
---
# <a name="unique-key-constraints-in-azure-cosmos-db"></a>Restrições de chave exclusivas no Azure Cosmos DB

Chaves exclusivas oferecem a capacidade de adicionar uma camada de integridade dos dados para um contentor do Cosmos. Criar uma política de chaves exclusivas durante a criação de um contentor do Cosmos. Com as chaves exclusivas, garantir a exclusividade de um ou mais valores dentro de uma partição lógica (pode garantir a exclusividade por [chave de partição](partition-data.md)). Depois de criar um contentor com uma política de chaves exclusivas, ele impede a criação de quaisquer itens duplicados novos (ou atualizados) dentro de uma partição lógica, como especificado pela restrição de chave exclusiva. A chave de partição combinada com a exclusividade de garantias de chave exclusivos de um item no âmbito do contentor.

Por exemplo, considere um contentor do Cosmos com endereço de e-mail como restrição de chave exclusiva e `CompanyID` como a chave de partição. Ao configurar uma chave exclusiva de endereços de correio eletrónico do utilizador, garantir que cada item tem um endereço de e-mail exclusivo dentro de um determinado `CompanyID`. Não não possível criar dois itens com endereços de e-mail duplicados e com o mesmo valor de chave de partição.  

Se quiser fornecer aos utilizadores a capacidade de criar vários itens com o mesmo endereço de e-mail, mas não o mesmo nome, o último nome e endereço de e-mail, poderia adicionar caminhos adicionais para a política de chaves exclusivas. Em vez de criar uma chave exclusiva com base no endereço de e-mail, também pode criar uma chave exclusiva com uma combinação do nome próprio, apelido e endereço (uma chave composta exclusivo) de e-mail. Neste caso, cada combinação exclusiva dos três valores dentro de um determinado `CompanyID` é permitido. Por exemplo, o contentor pode conter itens com os seguintes valores em que cada item é respeitar a restrição de chave exclusiva.

|CompanyID|Nome próprio|Apelido|Endereço de e-mail|
|---|---|---|---|
|Contoso|Gaby|Duperre|gaby@contoso.com |
|Contoso|Gaby|Duperre|gaby@fabrikam.com|
|Fabrikam|Gaby|Duperre|gaby@fabrikam.com|
|Fabrikam|Ivan|Duperre|gaby@fabrikam.com|
|Fabrkam|   |Duperre|gaby@fabraikam.com|
|Fabrkam|   |   |gaby@fabraikam.com|

Se tentar inserir outro item com as combinações listadas na tabela acima, receberá um erro que indica que a restrição de chave exclusiva não foi cumprida. Receberá a qualquer um dos "recursos com o ID especificado ou o nome já existe" ou "Recurso com o ID especificado, o nome ou índice exclusivo já existe" como uma mensagem de retorna.  

## <a name="defining-a-unique-key"></a>Definir uma chave exclusiva

Pode definir chaves exclusivas durante a criação de um contentor do Cosmos. Uma chave exclusiva é confinada a uma partição lógica. No exemplo anterior, se dividir o contentor com base em zipcode, acabará tendo duplicados itens em cada partição lógica. Durante a criação de chaves exclusivas, considere as seguintes propriedades:

* Não é possível atualizar um contentor existente para utilizar uma chave exclusiva diferente. Em outras palavras, uma vez que é criado um contentor com uma política de chave exclusiva, não é possível alterar a política.

* Se pretender definir chave exclusiva para um contentor existente, terá de criar um novo contentor com a restrição de chave exclusiva e utilizar a ferramenta de migração de dados apropriados para mover os dados de contentor existente para o novo contentor. Para contentores SQL, utilize o [ferramenta de migração de dados](import-data.md) para mover dados. Para contentores do MongoDB, utilize [mongoimport.exe ou mongorestore.exe](mongodb-migrate.md) para mover dados.

* Uma política de chaves exclusivas pode ter um máximo de 16 valores de caminho (por exemplo: /firstName, /lastName, / morada/CódigoPostal). Cada política de chaves exclusivas pode ter um máximo de 10 restrições de chave exclusivos ou combinações e os caminhos de combinado para cada restrição de índice exclusivo não devem exceder 60 bytes. No exemplo anterior, nome próprio, apelido e endereço de e-mail em conjunto são apenas uma restrição e, utiliza três de 16 caminhos possíveis.

* Quando um contentor tem uma política de chaves exclusivas (UR) de unidades de cobrança de pedido para criar, atualizar e eliminar um item são um pouco mais alta.

* Chaves exclusivas dispersas não são suportadas. Se faltam alguns valores de caminho exclusivo, são tratadas como valores nulos, o que faça parte da restrição de exclusividade. Por conseguinte, só pode existir um único item com valor nulo para satisfazer esta restrição.

* Nomes de chave exclusivos diferenciam maiúsculas de minúsculas. Por exemplo, considere um contentor com a restrição de chave exclusiva definida como /address/zipcode. Se os dados tiverem um campo chamado ZipCode, do Cosmos DB insere "null" como uma chave exclusiva porque "zipcode" não é igual a "ZipCode". Devido a este maiúsculas e minúsculas, todos os outros registos com ZipCode não não possível inserir uma vez que o duplicado "null" será violam a restrição de chave exclusiva.

## <a name="next-steps"></a>Passos Seguintes

* Saiba mais sobre [partições lógicas](partition-data.md)
