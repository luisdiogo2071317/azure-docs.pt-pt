---
title: As chaves exclusivas do BD Azure Cosmos | Microsoft Docs
description: Saiba como utilizar chaves exclusivas na base de dados do Azure Cosmos DB.
services: cosmos-db
keywords: restrição de chave exclusiva, violação de restrição de chave exclusiva
author: rafats
manager: kfile
editor: monicar
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 03/21/2018
ms.author: rafats
ms.openlocfilehash: d12109efbb157b1e0c15b1a4c0d005fa98c44858
ms.sourcegitcommit: 1b8665f1fff36a13af0cbc4c399c16f62e9884f3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/11/2018
ms.locfileid: "35261105"
---
# <a name="unique-keys-in-azure-cosmos-db"></a>Chaves exclusivas do BD Azure Cosmos

Chaves exclusivas fornecem os programadores a capacidade de adicionar uma camada de integridade dos dados para a respetiva base de dados. Criar uma política de chave exclusiva quando é criado um contentor, certifique-se a exclusividade de um ou mais valores por [chave de partição](partition-data.md). Depois de criar um contentor com a política de chave exclusiva, impede a criação de quaisquer itens de novas ou atualizadas com valores duplicados valores especificados pela restrição de chave exclusiva.   

> [!NOTE]
> Chaves exclusivas são suportadas por versões mais recentes do [.NET](sql-api-sdk-dotnet.md) e [.NET Core](sql-api-sdk-dotnet-core.md) SDKs de SQL e o [MongoDB API](mongodb-feature-support.md#unique-indexes). A API de tabela e a Graph API não suportam chaves exclusivas neste momento. 
> 
>

## <a name="use-case"></a>Caso de utilização

Por exemplo, vamos ver como uma base de dados de utilizador associados a um [aplicação social](use-cases.md#web-and-mobile-applications) foi beneficiar de ter uma política de chave exclusiva nos endereços de correio eletrónico. Ao fazer uma chave exclusiva de endereços de correio eletrónico do utilizador, certifique-se de cada registo tem um endereço de e-mail exclusivo e não existem novos registos podem ser criados com endereços de correio eletrónico duplicado. 

Se pretende que os utilizadores para conseguir criar vários registos com o mesmo endereço, mas não o mesmo nome próprio, apelido de e-mail e endereço de e-mail, pode adicionar outros caminhos para a política de chave exclusiva. Por isso, em vez de criar uma chave exclusiva com base no endereço de correio eletrónico, pode criar uma chave exclusiva que é uma combinação do nome próprio, apelido e e-mail. Neste caso, cada combinação de três caminhos exclusivos é permitida, pelo que a base de dados pode conter os itens que têm os seguintes valores de caminho. Cada um destes registos deverá passar a política de chave exclusiva.  

**Valores de chave exclusiva do e-mail, firstName e lastName permitidos**

|Nome próprio|Apelido|Endereço de e-mail|
|---|---|---|
|Gaby|Duperre|gaby@contoso.com |
|Gaby|Duperre|gaby@fabrikam.com|
|Ivan|Duperre|gaby@fabrikam.com|
|    |Duperre|gaby@fabrikam.com|
|    |       |gaby@fabraikam.com|

Se a tentativa de inserir a outro registo com qualquer uma das combinações listadas na tabela acima, seria recebe um erro que indica que a restrição de chave exclusiva não foi cumprida. O erro devolvido de base de dados do Azure Cosmos é "Recurso com o id especificado ou o nome já existe." ou "Recurso com o id especificado, o nome ou o índice exclusivo já existe." 

## <a name="using-unique-keys"></a>Utilizar chaves exclusivas

Chaves exclusivas tem de ser definidas quando o contentor é criado e tem um âmbito a chave exclusiva para a chave de partição. Para criar no exemplo anterior, se a partição com base no código postal, pode ter os registos de tabela duplicado em cada partição.

Não é possível atualizar o contentor existente a utilizar chaves exclusivas.

Depois de um contentor é criado com uma política de chave exclusiva, não é possível alterar a política, a menos que recriar o contentor. Se tiver dados existentes que gostaria de implementar chaves exclusivas, criar o novo contentor e, em seguida, utilize a ferramenta de migração de dados adequada para mover os dados para o novo contentor. Para os contentores do SQL Server, utilize o [ferramenta de migração de dados](import-data.md). Para os contentores do MongoDB, utilize [mongoimport.exe ou mongorestore.exe](mongodb-migrate.md).

Um máximo de 16 valores do caminho (por exemplo /firstName /lastName, /address/zipCode, etc.) pode ser incluído em cada chave exclusiva. 

Cada política de chave exclusiva pode ter um máximo de 10 restrições de chave exclusivos ou combinações e os caminhos combinados de todas as propriedades de índice exclusivo não devem exceder 60 carateres. Por isso, o exemplo anterior, que utiliza o primeiro nome, apelido, endereço de correio eletrónico é apenas uma restrição, e utiliza três de 16 caminhos possíveis disponíveis. 

Pedir unidade encargos de criar, atualizar, e eliminar um item são ligeiramente superior quando existe uma política de chave exclusiva no contentor. 

As chaves exclusivas dispersas não são suportadas. Se os valores para algumas caminhos exclusivos estão em falta, são tratadas como um valor nulo especial, que demora a parte na restrição de exclusividade.

## <a name="sql-api-sample"></a>Exemplo de API do SQL Server

O exemplo de código seguinte mostra como criar um novo contentor do SQL Server com dois restrições de chave exclusivas. A restrição primeiro é o nome próprio, apelido, descrita no exemplo anterior de restrição de e-mail. A restrição segundo é o endereço/zipCode de utilizadores. Um ficheiro JSON de exemplo que utiliza os caminhos nesta política de chave exclusivo segue o exemplo de código. 

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

Documento JSON de exemplo.

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
> . Nome da chave exclusivo nota é sensível às maiúsculas e minúsculas. Conforme mostrado na acima exemplo, o nome exclusivo está definido para /address/zipcode. Se os seus dados tiver ZipCode, em seguida,-inserir "null" na chave exclusiva como zipcode não é igual ao ZipCode. E devido a esta sensibilidade a maiúsculas e minúsculas todos os outros registos com ZipCode não será capazes de ser inserido como duplicado "null" será viola a restrição de chave exclusiva.

## <a name="mongodb-api-sample"></a>Exemplo de API do MongoDB

O exemplo do comando seguinte mostra como criar um índice exclusivo no nome próprio, apelido e campos de correio eletrónico da coleção de utilizadores para a API do MongoDB. Isto garante a exclusividade para uma combinação de todos os três campos em todos os documentos na coleção. Para coleções de API do MongoDB, o índice exclusivo é criado a coleção é criada, mas antes de povoar a colecção.

```
db.users.createIndex( { firstName: 1, lastName: 1, email: 1 }, { unique: true } )
```
## <a name="configure-unique-keys-by-using-azure-portal"></a>Configurar as chaves exclusivas utilizando o Portal do Azure

As secções acima, irá encontrar exemplos de código que irão mostrar como pode definir restrições de chave exclusivas quando uma coleção é criada utilizando a API do SQL Server ou a API do MongoDB. Mas também é possível definir chaves exclusivas quando criar uma coleção através da IU da web no portal do Azure. 

- Navegue para o **Explorador de dados** na sua conta de base de dados do Cosmos
- Clique em **nova coleção**
- Nas chaves de secção exclusivo, * * pode adicionar as restrições de chaves exclusivas pretendidas clicando **chave exclusiva adicionar**

![Definir chaves exclusivas no Explorador de dados](./media/unique-keys/unique-keys-azure-portal.png)

- Se gostaria de criar uma restrição de chave exclusiva no caminho de lastName, adicionar `/lastName`.
- Se gostaria de criar uma restrição de chave exclusiva para a combinação de firstName lastName, adicionar `/lastName,/firstName`

Quando terminar clique **OK** para criar a coleção.

## <a name="next-steps"></a>Passos Seguintes

Neste artigo, aprendeu a criar chaves exclusivas para itens numa base de dados. Se estiver a criar um contentor pela primeira vez, reveja [a criação de partições de dados na base de dados do Azure Cosmos](partition-data.md) como chaves exclusivas e chaves de partição baseiam-se entre si. 


