---
title: Saiba como gerir contas de base de dados no Azure Cosmos DB
description: Saiba como gerir contas de base de dados no Azure Cosmos DB
author: markjbrown
ms.service: cosmos-db
ms.topic: sample
ms.date: 11/10/2018
ms.author: mjbrown
ms.openlocfilehash: 97fb5c2558d55b3f80f2e771971faa109a930c5f
ms.sourcegitcommit: 1f9e1c563245f2a6dcc40ff398d20510dd88fd92
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/14/2018
ms.locfileid: "51626487"
---
# <a name="manage-indexing-in-azure-cosmos-db"></a>Gerir a indexação no Azure Cosmos DB

No Azure Cosmos DB, pode escolher se pretende que um contentor para indexar automaticamente todos os itens ou não. Por predefinição, todos os itens num contentor do Azure Cosmos são indexados automaticamente, mas pode desativar a indexação automática. Quando a indexação é desativada, os itens podem ser acedidos através de seus auto-ligações ou através de consultas utilizando o ID do item, como o id do documento. Pode explicitamente solicitar para servir os resultados sem usar o índice ao transmitir **x-ms-documentdb-enable-análise** cabeçalho na REST API ou a **EnableScanInQuery** pedir opção utilizando o. NET SDK.

Com a indexação automática desativada, pode ainda seletivamente adicionar itens específicos para o índice. Por outro lado, pode deixar a indexação automática ativada e seletivamente optar por excluir itens específicos. Indexação/desativar as configurações são úteis quando tem um subconjunto de itens que devem ser consultadas.  

Escreva unidades de débito e pedido são proporcionais ao número de valores que precisam ser indexados, que é especificado pelo conjunto incluído na política de indexação. Se tiver uma boa compreensão dos padrões de consulta, poderá escolher explicitamente o subconjunto de incluir/excluir de caminhos para melhorar o débito de escrita.

## <a name="manage-indexing-using-azure-portal"></a>Gerir a indexação com o portal do Azure

1. Inicie sessão no [portal do Azure](https://portal.azure.com/).

2. Criar uma nova conta do Cosmos do Azure ou selecione uma conta existente.

3. Abra o **Data Explorer** painel.

4. Selecione um contentor existente, expandi-lo e modificar os seguintes valores:

   * Abra o **dimensionamento e definições** janela.
   * Alteração **indexingMode** partir *consistente* para *none* ou incluir/excluir determinados caminhos de indexação.
   * Clique em **OK** para guardar as alterações.

   ![Gerir a indexação com o portal do Azure](./media/how-to-manage-indexing/how-to-manage-indexing-portal.png)

## <a name="manage-indexing-using-azure-sdks"></a>Gerir a indexação com SDKs do Azure

### <a id="dotnet"></a>SDK do .NET

O exemplo a seguir mostra como incluir um item explicitamente ao utilizar o [SDK de .NET API de SQL](sql-api-sdk-dotnet.md) e o [RequestOptions.IndexingDirective](/dotnet/api/microsoft.azure.documents.client.requestoptions.indexingdirective) propriedade.

```csharp
// To override the default behavior to exclude (or include) a document in indexing,
// use the RequestOptions.IndexingDirective property.
client.CreateDocumentAsync(UriFactory.CreateDocumentCollectionUri("myDatabaseName", "myCollectionName"),
    new { id = "myDocumentId", isRegistered = true },
    new RequestOptions { IndexingDirective = IndexingDirective.Include });
```

## <a name="next-steps"></a>Passos Seguintes

Leia mais sobre a indexação nos seguintes artigos:

* [Descrição geral de indexação](index-overview.md)
* [Política de indexação](index-policy.md)
* [Tipos de índice](index-types.md)
* [Caminhos de índice](index-paths.md)
