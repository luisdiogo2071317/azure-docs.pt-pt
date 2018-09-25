---
title: Resolução de conflitos de vários mestres no Azure Cosmos DB
description: Este artigo descreve as categorias de conflito e modos de resolução de conflito, como o último escritor-vence (LWW), personalizada – procedimento definidas pelo utilizador, personalizada – assíncrona no Azure Comsos DB com vários mestres.
services: cosmos-db
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/24/2018
ms.author: mjbrown
ms.reviewer: sngun
ms.openlocfilehash: d2f9c5dc8decfbe613d9fd3be2c37b45f1e4f2ea
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "46987633"
---
# <a name="multi-master-conflict-resolution-in-azure-cosmos-db"></a>Resolução de conflitos de vários mestres no Azure Cosmos DB 

Azure Cosmos DB com vários mestres fornece conflito global modos de resolução para garantir que dados são consistentes em todas as regiões em que são replicado.

## <a name="conflict-categories"></a>Categorias de conflito

Existem três categorias de conflitos que possam ocorrer ao trabalhar com dados do Azure Cosmos DB:

* **Inserir conflitos:** esse tipo de conflito ocorre quando o aplicativo insere dois ou mais documentos com o mesmo índice exclusivo (por exemplo, o ID) de duas ou mais regiões em simultâneo. Neste caso, todas as escritas podem ter êxito inicialmente, mas com base na política de resolução de conflito que escolher, apenas um documento com o ID original tem o compromisso.

* **Substitua conflitos:** esse tipo de conflito ocorre quando o aplicativo atualiza um único documento em simultâneo a partir de dois ou mais regiões.

* **Eliminar conflitos:** esse tipo de conflito ocorre quando a aplicação elimina um documento de uma região e atualiza-o partir de um ou mais regiões. 

## <a name="conflict-resolution-modes"></a>Modos de resolução de conflito

Existem três modos de resolução de conflito oferecidos pelo Azure Cosmos DB. Modos de resolução de conflito são definidos para cada coleção.

> [!NOTE]
> Os utilizadores da API de SQL podem escolher entre três modos de resolução de conflito diferentes. Para todos os outros API modelos (MongoDB, Cassandra, gráfico e tabela), são resolvidos os conflitos com o último escritor Wins.

### <a name="last-writer-wins-lww"></a>Último escritor-vence (LWW)

Último escritor-vence é o modo de resolução de conflito de predefinido. Neste modo, os conflitos são resolvidos com base num valor numérico passado numa propriedade do documento.

O fragmento de código seguinte é um exemplo de como configurar a política de resolução de conflito de última-escritor-Wins quando utilizar o SDK do .net. "ConflictResolutionPath" define o caminho para a propriedade que é utilizada para resolver o conflito. Neste exemplo, `/userDefinedId` é o caminho de resolução de conflito e o documento com o maior `userDefinedId` valor sempre será a vencedora o conflito. Para registar um modo de resolução de última-escritor-Wins, Aprovisione a coleção com o ConflictResolutionPolicy, conforme mostrado abaixo.

```csharp
DocumentCollection lwwCollection = await myClient.CreateDocumentCollectionIfNotExistsAsync(UriFactory.CreateDatabaseUri("myDatabase"), new DocumentCollection
{
   Id = "myCollection", 
   ConflictResolutionPolicy = new ConflictResolutionPolicy
   {
      Mode = ConflictResolutionMode.LastWriterWins,
      ConflictResolutionPath = "/userDefinedId"
   }
});
```

 Modo de resolução de última-escritor-Wins é aplicado às categorias de conflito de dados diferentes da seguinte forma:

* **Inserir e atualizar conflitos:** se dois ou mais documentos entrar em conflito no inserir ou substituem as operações, o documento que contém o maior valor para o caminho de resolução de conflito torna-se o vencedor (ou seja, userDefinedId). Se vários documentos tem o mesmo valor numérico para o caminho de resolução de conflito, o vencedor selecionado é determinística. No entanto, todas as regiões convergirão-se a um único vencedor.

* **Eliminar conflitos:** se há conflitos de eliminação envolvidas, a eliminação wins sempre através de outros conflitos de replace, independentemente do valor do caminho de resolução de conflito.

### <a name="custom--user-defined-procedure"></a>Personalizada – procedimento definido pelo utilizador

Neste modo, os controles de usuário entrar em conflito resolução registrando-se um utilizador definido procedimento (UDP) à coleção. Este UDP tem uma assinatura específica. Se selecionar esta opção, mas conseguir registar um UDP, ou se o UDP lança uma exceção em tempo de execução, está em conflito é escritos os conflitos de feed onde podem ser resolvidas individualmente.

Para registar um personalizado – o modo de resolução de conflito de procedimento definido pelo utilizador, Aprovisione a coleção com o ConflictResolutionPolicy, conforme mostrado abaixo.

```csharp
DocumentCollection udpCollection = await myClient.CreateDocumentCollectionIfNotExistsAsync(UriFactory.CreateDatabaseUri("myDatabase"), new DocumentCollection
{
  Id = "myCollection",
  ConflictResolutionPolicy = new ConflictResolutionPolicy
  {
     Mode = ConflictResolutionMode.Custom,
     ConflictResolutionProcedure = UriFactory.CreateStoredProcedureUri("myDatabase","myCollection","myUdpStoredProcedure").ToString()
  }
});
```

Em seguida, adicione o procedimento definido pelo utilizador para a coleção, conforme mostrado abaixo.

```csharp
StoredProcedure myUdpStoredProc = new StoredProcedure
{
   Id = "myUdpStoredProcedure",
   Body = myUdpStoredProcText
};
await myClient.UpsertStoredProcedureAsync(UriFactory.CreateDocumentCollectionUri("myDatabase", "myCollection"), myUdpStoredProc);
```

O procedimento armazenado registado com a coleção tem uma assinatura especial. Neste exemplo abaixo, o UDP usa uma propriedade, `UserDefinedId` para resolver conflitos. O documento com o maior valor wins o conflito.

```javascript
function myUdpStoredProcedure(incomingDocument, existingDocument, isDeleteConflict, conflictingDocuments){
    var collection = getContext().getCollection();

    if (!incomingDocument) {
        if (existingDocument) {

            collection.deleteDocument(existingDocument._self, {}, function(err, responseOptions) {
                if (err) throw err;
            });
        }
    } else if (isDeleteConflict) {
        // delete always wins.
    } else {
        var documentToUse = incomingDocument;

        if (existingDocument) {
            if (documentToUse.userDefinedId < existingDocument.userDefinedId) {
                documentToUse = existingDocument;
            }
        }

        var i;
        for (i = 0; i < conflictingDocuments.length; i++) {
            if (documentToUse.userDefinedId < conflictingDocuments[i].userDefinedId) {
                documentToUse = conflictingDocuments[i];
            }
        }

        tryDelete(conflictingDocuments, incomingDocument, existingDocument, documentToUse);
    }

    function tryDelete(documents, incoming, existing, documentToInsert) {
        if (documents.length > 0) {
            collection.deleteDocument(documents[0]._self, {}, function(err, responseOptions) {
                if (err) throw err;

                documents.shift();
                tryDelete(documents, incoming, existing, documentToInsert);
            });
        } else if (existing) {
            collection.replaceDocument(existing._self, documentToInsert,
                function(err, documentCreated) {
                    if (err) throw err;
                });
        } else {
            collection.createDocument(collection.getSelfLink(), documentToInsert,
                function(err, documentCreated) {
                    if (err) throw err;
                });
        }
    }
}

```

O procedimento tem quatro parâmetros:

* **incomingDocument:** Especifica a versão em conflito do documento. O conflito pode ser uma inserção, substituir ou um conflito de eliminação. Para um conflito de eliminação, esta será uma imagem de delete (exclusão) com nenhum conteúdo.

* **existingDocument:** Especifica a versão consolidada do documento, que tem o mesmo valor que o incomingDocument de "rid". Este parâmetro é definido como null para uma inserção e elimine o conflito.

* **isDeleteConflict:** sinalizador booleano que indica se o documento de entrada está em conflito com um documento excluído anteriormente. Quando este parâmetro estiver definido para verdadeiro, existingDocument também serão nulo.

* **conflictingDocuments:** Especifica uma coleção da versão consolidada de todos os documentos na base de dados, que estão em conflito com incomingDocument na coluna de ID ou quaisquer outros campos de índice exclusivo. Estes documentos terá o valor diferentes "eliminar" em comparação com o incomingDocument.

O procedimento definido pelo utilizador tem acesso total para a chave de partição do Cosmos DB e executa qualquer operação de armazenamento para resolver conflitos. Se o procedimento definido pelo utilizador não Consolide a versão de conflito, o sistema irá diminuir o conflito e será permanecem confirmado, a existingDocument. Se o procedimento definido pelo utilizador falha ou não existe, Azure Cosmos DB todos adicionará o conflito para os conflitos de só de leitura do feed onde ele pode processar de forma assíncrona como mostrado na [modo de resolução de conflito assíncrona](). 

### <a name="custom--asynchronous"></a>Personalizada – assíncrona  

Neste modo, o Azure Cosmos DB exclui todos os conflitos (inserir, substituir e delete) de ser consolidada e regista-os nos conflitos de só de leitura do feed para a resolução diferida pela aplicação do utilizador. A aplicação pode efetuar a resolução de conflitos de forma assíncrona e utilizar qualquer lógica ou consulte a qualquer origem externa, aplicação ou serviço para resolver o conflito.

Para registar um personalizado – o modo de resolução de conflito assíncrona, Aprovisione a coleção com o ConflictResolutionPolicy, conforme mostrado abaixo.

```csharp
DocumentCollection manualCollection = await myClient.CreateDocumentCollectionIfNotExistsAsync(UriFactory.CreateDatabaseUri("myDatabase"), new DocumentCollection
{
    Id = "myCollection",
    ConflictResolutionPolicy = new ConflictResolutionPolicy
    {
        Mode = ConflictResolutionMode.Custom
    }
});
```

Para ler e processar quaisquer conflitos no feed de conflitos, implemente o código mostrado abaixo. Dados armazenados no conflitos feed adiciona algum custo de armazenamento. Por isso, é recomendado para eliminar os dados armazenados em conflitos de feed depois são processados.

```csharp
FeedResponse<Conflict> response = await myClient.ReadConflictFeedAsync(myCollectionUri);
  foreach (Conflict conflict in response)
  {
      switch(conflict.OperationKind)
      {
         case OperationKind.Create:
         //Process Insert Conflicts.
         …
         case OperationKind.Replace:
         //Process Replace Conflicts
         …
         case OperationKind.Delete:
         //Process Delete Conflicts
         …
      }
  //Optionally delete the conflict after processed
  await myClient.DeleteConflictAsync(conflict.SelfLink);
  }
```

> [!NOTE]
> O feed de conflitos não inclui um serviço de escuta para enviar notificações para processamento a jusante, como a alteração de feed no Cosmos DB. Terá de implementar a lógica para consultar o feed de conflitos e determinar se existem conflitos.

## <a name="code-samples"></a>Exemplos de código

Seguem-se os aplicativos de exemplo que demonstram a resolução de conflitos para as APIs listadas. Cada exemplo gera conflitos dentro de um contêiner e, em seguida, demonstra como são resolvidos os conflitos para cada modo de resolução de conflito suportados.

|Modelo de API  | SDK |Sample |
|---------|---------|---------|
|SQL      | .NET    |[Azure-cosmos-DB-SQL-DotNet-multi-master](https://github.com/Azure-Samples/azure-cosmos-db-sql-dotnet-multi-master)  |
|MongoDB  | .NET    |[Azure-cosmos-DB-mongodb-DotNet-multi-master](https://github.com/Azure-Samples/azure-cosmos-db-mongodb-dotnet-multi-master)   |
|Tabela    | .NET    |[Azure-cosmos-DB-Table-DotNet-multi-master](https://github.com/Azure-Samples/azure-cosmos-db-table-dotnet-multi-master)       |
|SQL      | Nó    |[Azure-cosmos-js/exemplos/MultiRegionWrite /](https://github.com/Azure/azure-cosmos-js/tree/master/samples/MultiRegionWrite)  |
|SQL      | Java    |[Azure-cosmos-DB-SQL-Java-multi-master](https://github.com/Azure-Samples/azure-cosmos-db-sql-java-multi-master)  |

## <a name="next-steps"></a>Passos Seguintes

Neste artigo, aprendeu sobre os modos de resolução de conflito e categorias de conflito para o Azure Cosmos DB. Em seguida, veja os seguintes recursos:

* [Utilizar clientes do MongoDB com múltiplos principais](multi-master-oss-nosql.md)
