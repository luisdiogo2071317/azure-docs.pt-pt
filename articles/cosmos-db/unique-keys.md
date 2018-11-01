---
title: Chaves exclusivas no Azure Cosmos DB | Documentos da Microsoft
description: Saiba como utilizar chaves exclusivas na sua base de dados do Azure Cosmos DB.
services: cosmos-db
keywords: restrição de chave exclusiva, violação de restrição de chave exclusiva
author: rafats
manager: kfile
editor: monicar
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 08/08/2018
ms.author: rafats
ms.openlocfilehash: ff432de59e5a5fdfeaad4c3a5361554ee32e21b0
ms.sourcegitcommit: ae45eacd213bc008e144b2df1b1d73b1acbbaa4c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/01/2018
ms.locfileid: "50740013"
---
# <a name="unique-keys-in-azure-cosmos-db"></a>Chaves exclusivas no Azure Cosmos DB

Chaves exclusivas oferecem aos desenvolvedores a capacidade de adicionar uma camada de integridade dos dados para a base de dados. Ao criar uma política de chaves exclusivas quando é criado um contentor, garantir a exclusividade de um ou mais valores por [chave de partição](partition-data.md). Assim que tiver sido criado um contentor com uma política de chaves exclusivas, ele impede a criação de todos os itens novos ou atualizados com os valores que duplicam os valores especificados pela restrição de chave exclusiva.   

> [!NOTE]
> Chaves exclusivas são suportadas por versões mais recentes do [.NET](sql-api-sdk-dotnet.md) e [.NET Core](sql-api-sdk-dotnet-core.md) SDKs de SQL e o [API do MongoDB](mongodb-feature-support.md#unique-indexes). A API de tabela e a API do Gremlin não suportam chaves exclusivas neste momento. 
> 
>

## <a name="use-case"></a>Caso de utilização

Por exemplo, vamos analisar como uma base de dados de utilizador associadas com uma [aplicativo social](use-cases.md#web-and-mobile-applications) poderia se beneficiar de uma política de chaves exclusivas em endereços de e-mail. Fazendo uma chave exclusiva de endereços de correio eletrónico do utilizador, certifique-se de cada registo tem um endereço de e-mail exclusivo e não existem novos registos podem ser criados com endereços de e-mail duplicados. 

Se quis que os utilizadores possam criar vários registos com o mesmo endereço de e-mail, mas não o mesmo nome, sobrenome e endereço de e-mail, poderia adicionar outros caminhos para a política de chaves exclusivas. Então, em vez de criar uma chave exclusiva com base num endereço de e-mail, pode criar uma chave exclusiva que é uma combinação do nome próprio, apelido e e-mail. Neste caso, é permitida a cada combinação exclusiva dos três caminhos, para que a base de dados pode conter itens que tem os seguintes valores de caminho. Cada um desses registos é aprovados a política de chaves exclusivas.  

**Valores de chave exclusiva da firstName, lastName e e-mail permitidos**

|Nome próprio|Apelido|Endereço de e-mail|
|---|---|---|
|Gaby|Duperre|gaby@contoso.com |
|Gaby|Duperre|gaby@fabrikam.com|
|Ivan|Duperre|gaby@fabrikam.com|
|    |Duperre|gaby@fabrikam.com|
|    |       |gaby@fabraikam.com|

Se tentou insira outro registo com qualquer uma das combinações listadas na tabela acima, receberia um erro que indica que a restrição de chave exclusiva não foi cumprida. O erro devolvido do Azure Cosmos DB é "Recurso com o id especificado ou o nome já existe". ou "Recurso com o id especificado, o nome ou índice exclusivo já existe". 

## <a name="using-unique-keys"></a>Utilizar chaves exclusivas

Chaves exclusivas tem de ser definidas quando o contentor é criado e a chave exclusiva tem um âmbito para a chave de partição. Para criar no exemplo anterior, se dividir com base no CEP, poderia ter os registros da tabela duplicados em cada partição.

Não é possível atualizar o contentor existente para utilizar chaves exclusivas.

Depois de criar um contentor com uma política de chave exclusiva, não é possível alterar a política, a menos que recriar o contentor. Se tiver dados existentes que gostaria de implementar chaves exclusivas no, criar o contentor novo e, em seguida, utilize a ferramenta de migração de dados apropriados para mover os dados para o novo contentor. Para contentores SQL, utilize o [ferramenta de migração de dados](import-data.md). Para contentores do MongoDB, utilize [mongoimport.exe ou mongorestore.exe](mongodb-migrate.md).

Um máximo de 16 valores de caminho (por exemplo /firstName, /lastName, /address/zipCode, etc.) pode ser incluído em cada chave exclusiva. 

Cada política de chaves exclusivas pode ter um máximo de 10 restrições de chave exclusivos ou combinações e os caminhos combinados para todas as propriedades de índice exclusivo não devem exceder 60 carateres. Então, o exemplo anterior, que utiliza o primeiro nome, sobrenome e endereço de e-mail é apenas uma restrição e utiliza três dos 16 caminhos possíveis disponíveis. 

Pedir unidade cobra para criar, atualizar, e eliminar um item são um pouco mais altos quando existe uma política de chaves exclusivas no contentor. 

Chaves exclusivas dispersas não são suportadas. Se os valores de alguns caminhos exclusivos em falta, são tratadas como um valor nulo especial, o que faz parte da restrição de exclusividade.

## <a name="sql-api-sample"></a>Exemplo de API de SQL

O exemplo de código seguinte mostra como criar um novo contentor SQL com duas restrições de chave exclusivas. A primeira restrição é firstName, lastName, descrita no exemplo anterior de restrição de e-mail. A restrição segundo é o endereço de utilizadores/zipCode. Um ficheiro JSON de exemplo que utiliza os caminhos nesta política de chave exclusivos segue o exemplo de código. 

```csharp
// Create a collection with two separate UniqueKeys, one compound key for /firstName, /lastName,
// and /email, and another for /address/zipCode.
private static async Task CreateCollectionIfNotExistsAsync(string dataBase, string collection)
{
    try
    {
        await client.ReadDocumentCollectionAsync(UriFactory.CreateDocumentCollectionUri(dataBase, collection));
    }
    catch (DocumentClientException e)
    {
        if (e.StatusCode == System.Net.HttpStatusCode.NotFound)
        {
            DocumentCollection myCollection = new DocumentCollection();
            myCollection.Id = collection;
            myCollection.PartitionKey.Paths.Add("/pk");
            myCollection.UniqueKeyPolicy = new UniqueKeyPolicy
            {
                UniqueKeys =
                new Collection<UniqueKey>
                {
                    new UniqueKey { Paths = new Collection<string> { "/firstName" , "/lastName" , "/email" }}
                    new UniqueKey { Paths = new Collection<string> { "/address/zipcode" } },
          }
            };
            await client.CreateDocumentCollectionAsync(
                UriFactory.CreateDatabaseUri(dataBase),
                myCollection,
                new RequestOptions { OfferThroughput = 2500 });
        }
        else
        {
            throw;
        }
    }
```

Documento de JSON de exemplo.

```json
{
    "id": "1",
    "pk": "1234",
    "firstName": "Gaby",
    "lastName": "Duperre",
    "email": "gaby@contoso.com",
    "address": 
        {            
            "line1": "100 Some Street",
            "line2": "Unit 1",
            "city": "Seattle",
            "state": "WA",
            "zipcode": 98012
        }
    
}
```
> [!NOTE]
> . Nome da chave exclusivos nota diferencia maiúsculas de minúsculas. Como mostra acima de exemplo, o nome exclusivo é definido para /address/zipcode. Se os dados terão ZipCode, em seguida, ele irá inserir "nulo" na chave exclusiva como zipcode não é igual ao ZipCode. E, devido a essa distinção todos os outros registos com ZipCode não será possível a ser inserido como duplicado "null" será violam a restrição de chave exclusiva.

## <a name="mongodb-api-sample"></a>Exemplo de API do MongoDB

O exemplo de comando a seguir mostra como criar um índice exclusivo na firstName, lastName e campos de correio eletrónico da coleção de utilizadores para a API do MongoDB. Isto garante a exclusividade para uma combinação de todos os três campos todos os documentos na coleção. Para coleções de API do MongoDB, o índice exclusivo é criado a coleção é criada, mas antes de preencher a coleção.

> [!NOTE]
> O formato de chave exclusivo para contas de API do MongoDB é diferente das contas da API de SQL, onde não precisa especificar o caráter de barra (/) antes do nome de campo. 

```
db.users.createIndex( { firstName: 1, lastName: 1, email: 1 }, { unique: true } )
```
## <a name="configure-unique-keys-by-using-azure-portal"></a>Configurar chaves exclusivas com o Portal do Azure

Nas secções acima encontrará exemplos de código que irão mostrar como pode definir restrições de chave exclusivas quando é criada uma coleção com a API de SQL ou a API do MongoDB. Mas também é possível definir chaves exclusivas ao criar uma coleção através da IU da web no portal do Azure. 

- Navegue para o **Data Explorer** na sua conta do Cosmos DB
- Clique em **nova coleção**
- Em chaves exclusivas a seção, * * pode adicionar as restrições de chave exclusivas pretendidas clicando **chave exclusiva do Add**

![Definir chaves exclusivas no Data Explorer](./media/unique-keys/unique-keys-azure-portal.png)

- Se quiser criar uma restrição de chave exclusiva no caminho lastName, adicionar `/lastName`.
- Se quiser criar uma restrição de chave exclusiva para a combinação de firstName lastName, adicionar `/lastName,/firstName`

Quando terminar clique **OK** para criar a coleção.

## <a name="next-steps"></a>Passos Seguintes

Neste artigo, aprendeu a criar chaves exclusivas para itens numa base de dados. Se estiver a criar um contentor pela primeira vez, reveja [criação de partições de dados no Azure Cosmos DB](partition-data.md) como chaves exclusivas e chaves de partição dependem uns dos outros. 


